# MTA
In Linux, inbox is a special location in the file system that is inaccessible to other non root users and stores the user's personal email messages.
 
 Incoming messages are added to the user's inbox by an MTA; Mail Transafer Agent.

 A mail outbox, also referred to as an email outbox, is a temporary storage space or folder in an email client or email server where outgoing email messages are held momentarily before being sent to their intended recipients

 Email Client: A software application that allows users to access, compose, and manage email accounts. Examples include Microsoft Outlook, Mozilla Thunderbird, and Apple Mail. Email clients typically run on local devices (e.g., desktops, laptops, or mobile phones) and connect to email servers to retrieve and send emails.

Email Server: A service that manages and stores email messages, providing access to users through protocols like SMTP, POP3, and IMAP. Email servers can be self-hosted (e.g., iRedMail, hMailServer) or hosted by email providers (e.g., Gmail, Outlook.com).

 MTA is a program running as a system service which collects messages sent by other local acounts as well as messages received from the netwwork , sent from remote user accounts and delivers it to a destination address.
 Mail Transfer Agents (MTAs) are software applications responsible for transferring email messages between mail servers and clients

 There are several examples of MTAs, such as Sendmail, Postfix, and Exim.

 MTAs query the DNS service for the corresponding MX record of the domain section of the email address to locate the destination emial address. The DNS Mx record contains the IP of the MTA handling the email for that domain

  If the same domain has more than one MX record specified in the DNS, the MTA should try to contact them according to their priority values.
  If the recipient’s address does not specify a domain name or the domain does not have an MX record, then the part after the @ symbol will be treated as the host of the destination MTA.

The command nc — a networking utility which reads and writes generic data across the network — can be used to send SMTP commands directly to the MTA

The mail command is used to send emails from the Linux terminal. The basic syntax is:

```sh
mail [-s subject] [-c cc] [-b bcc] [-a file] recipient@example.com
```
Here:

-s specifies the subject of the email.
-c specifies the carbon copy (CC) recipients.
-b specifies the blind carbon copy (BCC) recipients.
-a specifies an attachment file.
recipient@example.com is the email address of the recipient.

To specify the email body from a file, use redirection (<) instead of entering it interactively:
```sh 
mail -s "Body from File" recipient@example.com < email_body.txt
```
To send an email to multiple recipients, separate their email addresses with commas:
```sh
mail -s "Multiple Recipients" recipient1@example.com,recipient2@example.com,recipient3@example.com
```
To use an SMTP server instead of the default MTA (Mail Transfer Agent), specify the -t option followed by the SMTP server address:
```sh
mail -t smtp.example.com -s "SMTP Example" recipient@example.com
```
the sendmail command is also used to send mails
```sh
sendmail der@der
```
mialq is used to managed jobs queue
```sh
mailq
```
Use postsuper: The postsuper command is used to manage the Postfix mail queue. Run the command:
```sh
postsuper -d ALL
```

This will delete all messages from the queue. To delete a specific message, use:
```sh
postsuper -d <mail_id>
```
important protocols

SMTP (Simple Mail Transfer Protocol)
it's function is to deliver Outgoing emails
it Sends emails from a client (e.g., email client or mail server) to a mail server or another email server
Typically uses port 25 (or 587 for submission)


POP3 (Post Office Protocol version 3)
retrieves incoming emails
Downloads emails from a mail server to a client device (e.g., email client or desktop)
Typically uses port 110 (or 995 for SSL/TLS)

IMAP (Internet Message Access Protocol)
Incoming email management and synchronization
Allows clients to access and manage emails on a mail server, with synchronization across multiple devices
Typically uses port 143 (or 993 for SSL/TLS)


SMTP is used for sending emails, while POP3 and IMAP are used for receiving emails.
POP3 downloads emails to a client device and deletes them from the mail server, whereas IMAP keeps emails on the mail server and synchronizes changes across devices.
IMAP provides more advanced features, such as folder organization and search capabilities, compared to POP3.
