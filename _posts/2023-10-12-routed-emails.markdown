---
layout: post
title:  "Sending Emails with Gmail through a Cloudflare-routed Address"
date:   2023-10-12 17:00:00 -0400
categories: email cloudflare gmail
---

A friend who was seeing his free email service being deprecated asked me if I could suggest an alternative for him. I was already using Cloudflare's email routes for a few domain so I decided to explore a solution to leverage Gmail's SMTP to not only receive the mails, but also send messages from that address.

## Cloudflare Email Routing

To send emails, you'll first need to route messages to a destination address through Cloudflare's email routing. When you activate the routing on your managed domain, Cloudflare will offer to create the associated MX records, it's the easiest way to get started. (See the DNS Records section for more details)

Custom adresses can be anything and emails they receive can either be forwarded to a destination address, to a Cloudflare worker, or simply dropped. 

The catch-all address option will apply an action to all other messages that do not fit one of the defined routes.

![Cloudflare email routing](/assets/img/2023-10-12-routed-emails/cloudflare-email-routing.png)

## Google App Password

I'll be using the Gmail as a client to connect to our Google account and since MFA is enabled (you too right?) we'll need an app password which bypasses the multifactor auth. 
App passwords give the same access as your own password so I will not be saving it anywhere and just use it at the configuration step.

To create an app password:
1. Go to your Google Account.
2. Select Security.
3. Under "Signing in to Google," select 2-Step Verification.
4. At the bottom of the page, select App passwords.
5. Enter a name that helps you remember where you’ll use the app password.
6. Select Generate.

(https://myaccount.google.com/apppasswords) 

![Google app passwords](/assets/img/2023-10-12-routed-emails/google-app-passwords.png)

## Gmail account Configuration

Now that I have an app password, I can configure Gmail to send emails with the routed address:
1. ![Google app passwords](/assets/img/2023-10-12-routed-emails/gmail-cog.png) Settings cog 
2. See all settings (https://mail.google.com/mail/u/0/#settings/general)
3. Accounts and Import tab
4. In the `Send mail as` sub-section, click `Add another email address`
5. In the window that pops up, enter your information. **Uncheck** the "treat as an alias" box
![Add another email address you own](/assets/img/2023-10-12-routed-emails/another-address.png)
6. In the next step, configure the SMTP with your Gmail account and app password
![SMTP Gmail](/assets/img/2023-10-12-routed-emails/smtp-settings.png)
7. Gmail will ask you to validate that you indeed own the email address by sending an email and asking you to click on a link it contains. Click the link and you're all set on Google's side
![Confirm you own the email](/assets/img/2023-10-12-routed-emails/confirm-email.png)

## DNS Records
Since I'm using Gmail's SMTP to send my emails I'll include Google's servers to the SPF TXT record that Cloudflare previously created in my DNS zone.

The TXT record contains:
`"v=spf1 include:_spf.mx.cloudflare.net include:_spf.google.com ~all"`

And the zone looks like this:

![Cloudflare DNS](/assets/img/2023-10-12-routed-emails/dns-zone.png)

## Results

With this all setup, I should be able to send emails with my custom address, and they should be delivered properly.

So I can now select my custom address in the `From` dropdown menu:

![Custom address send from](/assets/img/2023-10-12-routed-emails/send-from.png)


And the email I send are also delivered properly

![Custom address mail delivery](/assets/img/2023-10-12-routed-emails/email-delivery.png)

As expected, the email shows as being sent `via gmail.com` (which is both true and fair, considering we're leveraging a free service.)