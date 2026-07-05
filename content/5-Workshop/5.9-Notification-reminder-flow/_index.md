---
title: "Automated Notifications, Reminders & Data Analytics"
date: 2026-06-29
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

---

This section covers two modules built by the same team member:

- **NotificationFunction** — sends registration confirmation emails and automated reminders via Amazon SES
- **AnalyticsFunction** — provides system-wide statistics and per-event details

Both modules are declared in the infrastructure template and deployed automatically via SAM, not created manually through the AWS Console.

---

# PART 1: Notifications & Automated Reminders

---

This section demonstrates two independent notification flows, both handled by a single Lambda function (NotificationFunction), but triggered through two different mechanisms and automatically classified based on the JSON structure of the incoming event — no separate route is needed for each trigger type.

This logic reads data from the Events, Tickets, and Users tables, logs results to the Notification Log table, and sends emails via Amazon SES.

---

## Prerequisites

Before testing any flow, make sure the sending email address has been verified in Amazon SES. If the email isn't verified, the entire email-sending flow will fail with a MessageRejected error from SES, regardless of whether the code is correct.

Checking verification status:

1. Go to AWS Console → Amazon SES → Verified identities
2. Confirm that khacanh204@gmail.com shows a Verified status
3. If not yet verified: select Create identity → Email address, enter the email, and AWS will send a confirmation link to that inbox — click the link to complete verification

> ⚠️ **Note:** If the AWS account is in Sandbox mode (the default), SES only allows sending to verified email addresses. Both the sending address (khacanh204@gmail.com) and the recipient address must be verified while in Sandbox mode. To send to any email address, Production Access must be requested in SES.

---

## Input Event Classification Mechanism

The main handler function receives input as JSON and automatically determines the calling source based on its structure:

```text
Has an "httpMethod" field        → a request from API Gateway (GET /admin/notifications)
Has a "detail-type" field
  = "Scheduled Event"        → invoked by EventBridge Scheduler (reminder flow)
  = "TicketRegistered"       → invoked by an EventBridge Rule (registration confirmation flow)
```

This is a design pattern that combines 3 types of triggers (API, custom event, and scheduled event) into a single Lambda handler, instead of splitting them into 3 separate Lambda functions.

---

## Flow 1: Sending a Confirmation Email Upon Ticket Registration

1. After the ticket registration function successfully creates a ticket (section 5.7), it publishes an event to the default event bus with source eventmanagement.ticket and detail-type TicketRegistered.
2. The EventBridge rule that matches this pattern automatically invokes NotificationFunction, passing along the detail payload containing the ticket information (email, full name, event title, time, location).
3. The handler for this flow reads the detail payload, builds the registration confirmation email content, and sends it via SES from the khacanh204@gmail.com address.
4. The send result (success or failure) is logged to the Notification Log table with type RegistrationConfirmed, status Sent or Failed, along with an error message if applicable.

---

## Flow 2: Sending a Reminder Email Before an Event

1. EventBridge Scheduler (running once every hour) automatically invokes NotificationFunction with an event of type "Scheduled Event".
2. The reminder handler scans the Events table to find events whose start time falls within the next 24 hours from the current time.
3. For each upcoming event, the Lambda retrieves the list of Confirmed tickets for that event.
4. For each ticket, the Lambda builds the reminder email content and sends it to the corresponding user's email address.
5. Every send is logged individually to the Notification Log table with type EventReminder.

> ⚠️ **Note on timeout:** the Lambda is configured with a 30-second timeout. If the number of Confirmed tickets is large, the reminder flow may time out before finishing sending all the emails. This doesn't matter in a test environment with little data.

> ⚠️ **Note on frequency:** since it runs once every hour, an event may receive reminders multiple times if it's still within the 24-hour window across consecutive scans — don't expect exactly one reminder email per ticket.

---

## Step 1: Test the Registration Confirmation Flow (via a Real Ticket Registration)

Register a new ticket following the exact steps from section 5.7:

```powershell
Invoke-RestMethod -Uri "$baseUrl/events/$eventId/register" -Method Post -Headers $headers
```

After a few seconds (since the flow runs asynchronously through EventBridge), check the notification log:

```powershell
Invoke-RestMethod -Uri "$baseUrl/admin/notifications" -Method Get -Headers $headers
```

Expected response (abbreviated, showing only the newest record):

```json
{
  "notificationId": "<GUID auto-generated for each send>",
  "eventId": "<EventId of the just-registered event>",
  "email": "<Email of the User who just registered>",
  "type": "RegistrationConfirmed",
  "status": "Sent",
  "errorMessage": null,
  "sentAt": "<Actual send time, UTC ISO 8601>"
}
```

---

## Step 2: Verify the EventBridge Rule Invoked NotificationFunction

Open `Amazon EventBridge → Rules`, select the rule that listens for the ticket registration event, go to the Monitoring tab, and confirm that the Invocations count increased by exactly 1 after the registration in Step 1.

---

## Step 3: Check the Lambda Logs for the Registration Confirmation Flow

Open `CloudWatch → Log groups → the log group for NotificationFunction`, find the log stream from the exact test time, and confirm there are log lines corresponding to the ticket registration handler and the result of the SES call.

---

## Step 4: Test the Reminder Flow by Simulating a Scheduled Event

Since the reminder flow only runs automatically once an hour, it can be actively tested by invoking the Lambda directly with a simulated payload matching the structure that EventBridge Scheduler sends.

Create a file named test-reminder.json with the following content:

```json
{
  "version": "0",
  "id": "test-event-id",
  "source": "aws.scheduler",
  "account": "<AccountId>",
  "region": "ap-southeast-2",
  "detail-type": "Scheduled Event",
  "detail": {}
}
```

Then invoke the Lambda using the AWS CLI:

```powershell
aws lambda invoke `
  --function-name NotificationLambda `
  --payload file://test-reminder.json `
  --cli-binary-format raw-in-base64-out `
  response.json

Get-Content response.json
```

> ⚠️ **Note:** use a JSON file rather than an inline string to avoid encoding issues on PowerShell. For actual reminder data to show up, make sure there's at least one event with a start time within the next 24 hours that already has a Confirmed ticket.

---

## Step 5: Check the Results of the Reminder Flow

```powershell
Invoke-RestMethod -Uri "$baseUrl/admin/notifications" -Method Get -Headers $headers
```

Confirm that new records with type EventReminder now appear, with a count exactly matching the number of Confirmed tickets for events occurring within the next 24 hours.

---

## Step 6: Check the Data in DynamoDB

Open `DynamoDB → Tables → the Notification Log table → Explore table items`, and directly compare the items against the responses from Step 1 and Step 5.

---

## Expected Outcome (Part 1)

After completing the Notification section, the practitioner should be able to:

- Understand how a single Lambda classifies and handles 3 different trigger types (API Gateway, EventBridge custom event, EventBridge Scheduler).
- Confirm that khacanh204@gmail.com has been verified in SES and can successfully send emails.
- Test the registration confirmation email flow, verifying it through the notification log and the actual email received in the inbox.
- Test the reminder email flow by simulating a Scheduled Event via the AWS CLI with a JSON file.
- Cross-check notification log data between the API response and the actual items in DynamoDB.
- Confirm that the EventBridge rule's invocation count matches the number of ticket registrations tested.

---

# PART 2: Data Analytics & Statistics

---

This section demonstrates two independent statistics flows, both handled by a single Lambda function (AnalyticsFunction), triggered via API Gateway and automatically routed based on the path parameter.

This logic reads data from the Events, Tickets, Attendance, and Notification Log tables, as well as the certificates S3 bucket — aggregating it into two types of reports: a system-wide overview and per-event details.

---

## Routing Mechanism

The main handler function receives requests from API Gateway and automatically determines which flow to run based on the path parameter:

```text
No "eventId" path parameter   → runs the Dashboard flow (system-wide overview)
Has an "eventId" path parameter    → runs the per-event analytics flow
```

---

## Flow 1: System-Wide Overview Statistics

The Dashboard handler aggregates metrics from multiple tables and from S3, returning the following information:

- Total events — counted using a Scan with count-only mode on the Events table
- Total registrations, confirmed, and waiting — scans the entire Tickets table page by page (pagination), classifying by status: Waiting, Cancelled (not counted), and everything else as Confirmed
- Total check-ins — counted using a Scan with count-only mode on the Attendance table
- Average attendance rate — calculated as the number of check-ins divided by the number confirmed, times 100, rounded to 1 decimal place
- Number of emails sent and number failed — scans the Notification Log table by Sent or Failed status
- Number of certificates issued — counts actual PDF files in S3 under the certificates/ prefix, without reading from DynamoDB, to ensure accuracy

---

## Flow 2: Per-Event Detailed Statistics

The per-event analytics handler receives an eventId from the path parameter, checks that the event exists, and then returns the following information:

- Total registrations, confirmed count, and waiting count — scans the Tickets table filtered by EventId
- Check-in count — uses a Query (not a Scan) on the Attendance table with EventId as the Partition Key — more efficient than a Scan since it doesn't read the entire table
- The event's own attendance rate — calculated as the check-in count divided by the confirmed count, times 100

---

## Step 1: Test the Dashboard Overview API

```powershell
Invoke-RestMethod -Uri "$baseUrl/admin/analytics/dashboard" -Method Get -Headers $headers
```

Expected response:

```json
{
  "totalEvents": 5,
  "totalRegistrations": 20,
  "confirmedRegistrations": 18,
  "waitingRegistrations": 2,
  "totalCheckIns": 10,
  "averageAttendanceRate": 55.6,
  "emailSentCount": 18,
  "emailFailedCount": 0,
  "certificatesIssuedCount": 10
}
```

---

## Step 2: Test the Per-Event Analytics API

```powershell
Invoke-RestMethod -Uri "$baseUrl/admin/analytics/events/$eventId" -Method Get -Headers $headers
```

Expected response:

```json
{
  "eventId": "<EventId of the event>",
  "eventTitle": "<Event name>",
  "totalRegistrations": 5,
  "confirmedCount": 5,
  "waitingCount": 0,
  "checkInCount": 3,
  "attendanceRate": 60.0
}
```

---

## Step 3: Cross-Check the Data Against DynamoDB

Open `DynamoDB → Tables`, and check each of the following in turn:

- The Tickets table — count the items with status Confirmed and Waiting
- The Attendance table — count the items and compare against the total check-in count in the Dashboard
- The Notification Log table — count the items with status Sent and Failed

Confirm these numbers match the API responses from Step 1 and Step 2.

---

## Step 4: Check the Lambda Logs

Open CloudWatch → Log groups → the log group for AnalyticsFunction, find the log stream from the test time, and confirm there are no errors and that all the queries/scans ran successfully.

---

## Expected Outcome (Part 2)

After completing the Analytics section, the practitioner should be able to:

- Understand how a single Lambda handles 2 different types of requests based on the path parameter.
- Understand the difference between a Scan (reads the entire table) and a Query (uses a key, more efficient) in DynamoDB — specifically, the per-event analytics flow uses a Query for the Attendance table instead of a Scan.
- Confirm the Dashboard data matches the actual data in the DynamoDB tables and S3.
- Confirm that the certificates-issued count is counted directly from S3, independent of DynamoDB.
- Have the Analytics data ready to be summarized and presented in the final report.