---
title: "Manage Exam Seats"
url: /order-exams-in-bulk/manage-exam-seats/
weight: 1
description: "Describes how an exam admin in an organization can manage exam seats."
---

## Introduction

An organization can place bulk orders to purchase Mendix certification exam seats. When Mendix processes the order, an exam admin is designated per order. 

After the order is processed, the designated exam admin receives an email and notification to manage the order. The exam admin can oversee the details of the order and assign the purchased exam seats to individuals within the organization. People who are assigned exam seats will be granted free access to take the exam.

This how-to describes how you can manage exam seats as an exam admin.

## Prerequisites

To manage exam seats, you must be an exam admin.

## Inviting Students to Take an Exam

1. In Mendix Academy, go to the [Overview](https://academy.mendix.com/link/examadmin) page for exam administration. In the **Available Exams** table, you can see the remaining seats per order and per exam type in the **Remaining Seats** column.

    {{< figure src="/attachments/community-tools/order-exams-in-bulk/manage-exam-seats/overview.png" >}}

2. Click **Invite Student**. The **Add Student** dialog box opens.
3. Enter the email addresses of the students who you want to invite. If you enter multiple email addresses, separate them by commas.

    {{< figure src="/attachments/community-tools/order-exams-in-bulk/manage-exam-seats/add-student-email-addresses.png" max-width=75% >}}

4. Click **Add to Invite**. 

5. Set an expiry date to the invitation. 

   {{< figure src="/attachments/community-tools/order-exams-in-bulk/manage-exam-seats/expiry-date.png" >}}

   {{% alert color="info" %}}The student must register for the exam before the expiry date. If the student fails to register before the expiry day, the seat will be set back to the **Remainin Seats** and can be reassigned.{{% /alert %}}

6. Click **Send Invites**.

The invitation is now visible in the table in the **Invited Students** section. Once the invitation is sent, the student will receive an email with a link that redirects them to the exam registration page.

## Checking the Status of an Invitation

In the **Invited Students** section, you can check the status of all the invitations that have been sent. The statuses are displayed in the **Status** column of the table. An invitation can have one of the following statuses:

- **NOT ACCEPTED YET**: The student has not accepted the invitation. You can still [remind the student](#remind-student) or [withdraw the invitation](#withdraw-invitation).
- **REGISTERED**: The student has registered but has not yet completed the exam. 
- **UNDISCLOSED**: The student has taken the exam but has not shared the result. A student can choose not to share the result with the inviter when they register for the exam.
- **CERTIFIED**: The student has passed the exam and is certified. 
- **FAILED**: The student has taken the exam and failed. 

## Reminding Students to Accept an Invitation {#remind-student}

If the student has not accepted the invitation, you can send a reminder email to encourage the student to register for the exam. To do so, click **Remind Student** in the table in the **Invited Students** section.

 {{< figure src="/attachments/community-tools/order-exams-in-bulk/manage-exam-seats/remind-student.png" >}}

## Withdrawing an Invitation {#withdraw-invitation}

You can withdraw an invitation that has not been accepted. To do so, click **Withdraw Invite** in the table in **Invited Students** section.

 {{< figure src="/attachments/community-tools/order-exams-in-bulk/manage-exam-seats/withdraw-invite.png" >}}
