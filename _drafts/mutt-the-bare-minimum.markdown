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




