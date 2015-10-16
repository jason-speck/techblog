---
layout: post
title:  "IMAP: under the hood"
date:   2015-10-08 07:41:00
categories: imap mail
---

Sometimes when configuring or debugging your mail client, it's useful to
have an understanding of what is occuring in the underlying IMAP
protocol.  To explore the IMAP protocol, you can use the ncat tool.  You
can also use netcat, or telnet, but ncat allows you to connect to both
plain-text and secure IMAP servers with a minimum of hassle.

##Installing ncat

Yup, you gotta install it.

##Connecting to the server

To connect to a plain-text IMAP server, execute the following:

    ncat -C my.mailserver.com 143

To connect to a secure IMAP server, execute the following:

    ncat -C --ssl my.mailserver.com 993

If your server is running on a nonstandard port, change the last
argument, which is the port number.

Once you connect you'll see something like:

    * OK Kerio Connect 8.5.1 IMAP4rev1 server ready

##Communicating with the server

All IMAP commands are prefixed with a short alphanumeric string called a
"tag".  You can use whatever you like here.  The tag is followed by a
command and a number of arguments.

The server will then respond.

When you're ready to disconnect, use `CTRL-D`.

##Authenticating with the server

Before running any other commands, you must first log in.  You use the
LOGIN command for this:

    tag LOGIN myusername mypassword

To which the server responds:

    tag OK LOGIN completed

##Exploring your mailboxes

You can list all your mailboxes with the LIST command:

    
    tag LIST "" "*"

server response:

    * LIST (\Inbox) "/" "INBOX"
    * LIST () "/" "INBOX/current"
    * LIST () "/" "INBOX/customers"
    * LIST () "/" "INBOX/customers/bigco"
    * LIST () "/" "INBOX/customers/littleco"
    * LIST () "/" "INBOX/customers/medco"
    * LIST () "/" "INBOX/reply"
    * LIST (\Trash) "/" "Deleted Items"
    * LIST (\Drafts) "/" "Drafts"
    * LIST (\Junk) "/" "Junk E-mail"
    * LIST (\Sent) "/" "Sent Items"
    * LIST (\Noselect) "/" "Public Folders"
    tag OK LIST completed

To examine an individual mailbox, use EXAMINE to "enter" the mailbox,
then FETCH to get information about the messages:

    tag examine INBOX
    * FLAGS (\Deleted \Seen \Answered \Draft \Flagged $MDNSent $Forwarded $AutoJunk $AutoNotJunk $Junk $NotJunk)
    * 19 EXISTS
    * 1 RECENT
    * OK [UNSEEN 18] Message 18 is first unseen
    * OK [UIDVALIDITY 1421969253] UID validity
    * OK [UIDNEXT 218717] Predicted next UID
    * OK [PERMANENTFLAGS (\Deleted \Seen \Answered \Draft \Flagged $MDNSent $Forwarded $AutoJunk $AutoNotJunk $Junk $NotJunk)] Permanent flags
    tag OK [READ-ONLY] SELECT completed

    tag FETCH 18 FAST
    * 18 FETCH (FLAGS (\Recent) INTERNALDATE "09-Oct-2015 10:02:04 -0700" RFC822.SIZE 21307)
    * 20 EXISTS
    * 2 RECENT
    tag OK FETCH completed

See RFC 3501 for more examples

To delete a mail:
    tag STORE 8 +FLAGS (\Deleted)
    * 8 FETCH (FLAGS (\Deleted))
    tag OK Store completed.


##Special mailboxes

You'll notice in the initial LIST example that some of the mailboxes
have a special backslashed value in parenthesis, such as `\Inbox` or
`\Trash`.  These provide hints to clients about where special folders
are located.  The exact behavior may differ between servers though.  To
see the special mailboxes:

    tag list (SPECIAL-USE) "" "*"
    * LIST (\Inbox) "/" "INBOX"
    * LIST (\Trash) "/" "Deleted Items"
    * LIST (\Drafts) "/" "Drafts"
    * LIST (\Junk) "/" "Junk E-mail"
    * LIST (\Sent) "/" "Sent Items"
    * 20 EXISTS
    * 1 RECENT

If the server doesn't all you to use the (SPECIAL-USE) argument, you'll
need to just look at the full output and find your folders of concern.

The behavior of these special mailboxes can differ between servers.  The
above is Kerio.

This is gmail (filtered, since it doesn't support SPECIAL-USE):
    * LIST (\Drafts \HasNoChildren) "/" "[Gmail]/Drafts"
    * LIST (\HasNoChildren \Sent) "/" "[Gmail]/Sent Mail"
    * LIST (\HasNoChildren \Junk) "/" "[Gmail]/Spam"
    * LIST (\HasNoChildren \Trash) "/" "[Gmail]/Trash"



###Trash

In the Kerio IMAP server, deleting a message automatically moves it to
the "Deleted Items" folder.  When you delete a message in "Deleted
Items", it is automatically expunged.

Gmail does no magic.  When you delete a message, it just removes the
label, meaning it only shows in the "All" folder.  You need to make your
IMAP client move the message to Trash.

Gandi does no magic.  When you delete, it sets delete flag.  When you
expunge it is truly expunged.  You need to make your IMAP client move
the message to the Trash.

###Drafts

I don't think there's any magic here.  You should check what it is,
though to configure your mail client correctly

###Junk

Again, no magic here.  This just tells your client where to check for
the spam folder in case the client treats it specially.

###Sent

In Kerio,there's no magic here.  But you want to know this value to set
mutt's `record` variable correctly.

In gmail, this gets populated automatically by the SMTP server, so no
need to do anything.  You need to set record to "" though.

In gandi, no magic.   Use mutt's record variable.
