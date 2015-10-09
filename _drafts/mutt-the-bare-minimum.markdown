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

Starting up mutt
================

The very first thing you need to do, of course, is install mutt either with your
distro's package manager, or from source.  Mutt has been fairly feature-stable
for the last few years, so every distro I've seen has a recent-enough version of
mutt.

Once you have it installed, let's start it up with no configuration:

You'll start on the "index" interface.  The index is one of the primary mutt
interfaces.  This is the interface used for browsing messages in a folder.  As
you can see, your index is empty, because we haven't yet told mutt where to find
mail.

Connecting to your imap account
===============================
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

Making your settings persistent
===============================
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

Sending mail
============

Managing folders
================

Dealing with attachments
========================

Playing well with others
========================





