---
layout: post
title:  "mutt: the bare mimimum"
date:   2015-10-08 07:41:00
categories: mutt mail
---

I've been using mutt for several years now, and I've somewhat haphazardly
accumulated configuration settings, helper scripts, and other badly-documented
magic.

I'd like to clean this up and bring some order to it.  In the process I hope to
write the series of articles that I wish I had when I first started using mutt.

There are a lot of great articles on mutt out there, but most spend too much
time on fancy features.  Here, I'd like to detail the bare minimum you need to
get up and running with mutt.  I.e., what do you need to do nothing more than to
receive, compose, and send email?

The approach I generally take when learning a new tool is to start with the most
minimal configuration, and do everything manually so I can understand what's
going on.  Only after I understand do I add configuration or automation.

The assumptions I wil make in this article are:

* you're comfortable at the command line
* you know how to use vim
* you know how to use your distributions's package manager, and/or how to install software from source.
* you have an IMAP email account

##Starting up mutt

The very first thing you need to do, of course, is install mutt either with your
distro's package manager, or from source.  Mutt has been fairly feature-stable
for the last few years, so every distro I've seen has a recent-enough version of
mutt.

Once you have it installed, let's start it up with no configuration:

You'll start on the "index" interface.  The index is one of the primary mutt
interfaces.  This is the interface used for browsing messages in a folder.  As
you can see, your index is empty, because we haven't yet told mutt where to find
mail.

##Connecting to your imap account

You need to tell mutt where your mail is.  The setting which controls
this is `folder`.  If you use an imap URL for `folder`, mutt will connect to
your imap server.

To set configuration variables in mutt, first enter `:`, which brings you to the
mutt command line.  You then can enter your command.  So, to set your imap
folder:

    :set folder = "imaps://my.imapserver.com"

Substitute your user name and mail server.  If you're using plain-text imap
rather than secure imap, use `imap:` rather than `imaps:` in the URL.

Then set your imap user name

    :set imap_user = "my.imap.name"

Depending on your mail server, your user name may or may not include the
domain.  You may need to consult your mail provider's IMAP
documentation.

To see your IMAP folders, hit the `c` key, which is bound to the mutt
`change-folder` function.  Then hit `?` to list your folders.  This brings up
the "browser" interface, another of mutt's interfaces.  To navigate the browser,
use the `j` and `k` keys to move up and down, `ENTER` to navigate the subfolders
of a folder, and `SPACE` to browse the messages in a folder.

You're now looking at the index interface which displays the messages in your
folder.  To navigate the messages, use `j` and `k`.  To view the highlighted
message, use `ENTER` or `SPACE`.   This brings up the "pager" interface, which
is the mutt interface for viewing mail.

Within the pager, use `SPACE` to page down, and `-` to page up.  You can also
use `j` and `k` to go to the next and previous messages.  To exit back to the
index interface, use `i` or `q`.

##Making your settings persistent

Having to manually enter your imap settings each time would be quite annoying.
Mutt's configuration file, `.muttrc` allows you to configure mutt at startup.
Anything you can enter at the mutt command line can also be placed in the config
file.

Go ahead and exit mutt.

Now create a file in your home directory called `.muttrc`

Add the following three lines:

    set imap_user = "my.imap.name"
    set folder = "imaps://my.imapserver.com"
    set spoolfile = "+INBOX"

The `folder` variable is the same as what we had set manually before.

The `spoolfile` variable defines the folder for which mutt should display the
index at startup.  `+` is a special value in mutt variable assignments that will
be substituted with the `folder` value.  So `+INBOX` is equivalent to `imaps://user.name@my.imapserver.com/INBOX`

##Sending mail

The IMAP settings only cover receiving mail.  You also need to configure the SMTP settings.

Add the following to your .muttrc:

    set smtp_url="smtps://myaddress@mydomain.com@my.smtpserver.com"
    set realname="Firstname Lastname"
    set from="myaddress@mydomain.com"

Start mutt again.  You should now be able to send email.  There are two ways to send mail.

#Replying to a message

To reply to a message, press `r` within the index or pager view.   Mutt prompts
you with a suggested `To:` header.  Press `Enter` to accept this.  It then
prompts you with a suggested `Subject:`.  Again, press `Enter`.

Mutt then launches your editor (vi by default), where you can compose
your message.

#Composing a new message

To compose a new message, press `m` in the index view.  Mutt will prompt
you for the `To:` and `Subject:` headers.  Enter the recipient and
subject here.  

Mutt then launches your editor, where you can compose the message.

#Sending the message

Regardless of whether you are replying or composing a new message, you
are given an opportunity to review the headers before sending.  If
everything looks good, press `y` to send.

If this is the first message you send during this session, you'll be
prompted for your SMTP password.

#Special folders

There are two special IMAP folders you may need to deal with when
sending mails, the "Sent" folder and the "Drafts" folder.  Different
IMAP servers call them different things.  The drafts folder is almost
always called "Drafts".  The sent folder is commondly called "Sent" or "Sent
Items".  You may need to check with your IMAP provider if it's not
obvious what folder to user here.  The "Sent" and "Draft" folder are set
by the `record` and `postponed` variables in mutt.  

Add the following to your muttrc:

    set record="+Sent Items"
    set postponed="+Drafts"

As in your existing `spoolfile` setting, the `+` substitutes the value
of `folder`.

If you're using Gmail's imap interface, you don't need to set a "Sent"
folder, as outgoing mail is automatically added here.

#Alternate names

Often, you will have multiple addresses that will reach the same
mailbox.  For example, at many companies, both
firstname.lastname@company.com and firstname@company.com will go to the
same person.  If this is the case, mutt can get confused, and will add
your alternate address as a `Cc:` when resopnding.  To handle this
situation, use the mutt `alternates` command.  This defines alternate
addresses under which you may receive mail.  You only need to list the
addresses that are different than your `from` value.

For example, if I can receive mail as joe.schmoe@bigco.com or
joe@bigco.com, and my `from` setting is:
    
    set from=`joe.schmoe@bigco.com`

then I should add the following line to define my alternate address:

    alternates joe@bigco.com





##Managing folders

##Dealing with attachments

##Playing well with others





