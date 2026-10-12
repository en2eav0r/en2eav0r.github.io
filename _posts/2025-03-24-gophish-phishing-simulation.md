---
title: "Gophish: Setting Up a Phishing Simulation Campaign"
date: 2025-03-24 00:00:00 +0000
categories: [Projects]
tags: [gophish, phishing, social-engineering, red-team, smtp, landing-page, awareness, security-assessment]
image:
  path: /assets/images/gophish/gophish-dashboard-cover.png
  alt: Gophish dashboard
---

<img src="/assets/images/gophish/gophish-logo.png" alt="Gophish logo" class="media-compact" />
<small class="media-caption">The Gophish logo used for this lab.</small>

Gophish is an open-source platform for running phishing simulations. You send realistic phishing emails to a defined set of targets, track who opens them, who clicks the link, and who submits credentials, then use that data to work on security awareness inside an organization.

This post walks through a full campaign: installation, configuration, execution, and results.

---

## Attack scenario

The fictional company **SNOW**, a software solutions firm, wants to test how its employees react to phishing attempts. The IT team runs an internal campaign using Gophish to simulate a realistic attack.

The target is **Jean LOU**, a financial analyst with access to sensitive data. That kind of profile is exactly what a real attacker would go after.

<img src="/assets/images/gophish/target-email.png" alt="Target email address (redacted)" class="media-compact" />
<small class="media-caption">The target address is hidden here because it belongs to a real person.</small>

---

## Installing Gophish on Windows

**Download Gophish** from the official GitHub releases page:

[https://github.com/gophish/gophish/releases](https://github.com/gophish/gophish/releases)

<img src="/assets/images/gophish/gophish-download.png" alt="Gophish GitHub releases page" />
<small class="media-caption">The Windows archive is available from the official Gophish releases page.</small>

**Extract the archive:**

<img src="/assets/images/gophish/gophish-extract.png" alt="Extracting the Gophish archive" />
<small class="media-caption">The downloaded archive contains the executable and the files needed to start Gophish.</small>

**Launch from the command prompt.** The console prints your admin credentials and the local dashboard URL. Write them down before doing anything else.

<img src="/assets/images/gophish/gophish-console.png" alt="Gophish console showing credentials and URL" />
<small class="media-caption">The first launch prints the local dashboard address and temporary admin credentials.</small>

---

## Logging in and navigating the interface

Open the URL from the console and log in with the credentials shown there. First login forces a password change.

<img src="/assets/images/gophish/gophish-login.png" alt="Gophish login page" />
<small class="media-caption">The login page is served locally by the Gophish instance.</small>

<img src="/assets/images/gophish/gophish-interface.png" alt="Gophish main interface" />
<small class="media-caption">The main interface groups campaign setup, targets, templates, landing pages, and SMTP profiles.</small>

The interface has five sections:

**Campaigns** is where you run and monitor phishing campaigns. You pick a target group, an email template, a landing page, and a sending profile, then launch.

**Users & Groups** is where you manage targets. Add users manually or import them from a CSV file.

**Email Templates** is where you write the phishing email the target will receive. The email needs to be convincing enough to prompt an action: clicking a link or entering credentials. Templates support HTML, dynamic variables, and a `{{.URL}}` placeholder that Gophish replaces with the actual phishing link at send time.

**Landing Pages** is the page the target reaches after clicking. You typically clone a real site here. Gophish can capture submitted usernames and passwords directly from this page.

**Sending Profiles** is the SMTP configuration. This is where you connect a mail account so Gophish can actually deliver emails.

---

## Practical phase: campaign configuration

### Users & Groups

Go to **Users & Groups** and click **New Group**.

<img src="/assets/images/gophish/users-groups-section.png" alt="Users and Groups section" />
<small class="media-caption">Users & Groups lists the target sets already configured.</small>

<img src="/assets/images/gophish/new-group-form.png" alt="New group form" />
<small class="media-caption">A new group can contain one target or an entire imported list.</small>

Follow these steps:

1. Enter a name for the group.
2. Fill in the user details for each target:
   - First name: **Jean** (optional)
   - Last name: **LOU** (optional)
   - Email address: hidden here for security reasons (required)
   - Position: optional
3. Click **Add**. You can add as many users as needed.
4. Click **Save Changes** when done.

---

### Email Template

Go to **Email Templates** and click **New Template**.

<img src="/assets/images/gophish/email-templates-section.png" alt="Email Templates section" />
<small class="media-caption">Email Templates stores the messages available to campaigns.</small>

<img src="/assets/images/gophish/new-template-form.png" alt="New Template form" />
<small class="media-caption">The template editor combines the subject, message body, and tracking link.</small>

For this campaign, the email impersonates Facebook and tells the target to reset their password. The more believable the email, the more likely the target clicks.

The form fields:

1. **Template name:** `TEMPLATE SNOW`
2. **Subject:** `Facebook: Reset your password`. The subject line has to match the email body to look credible.
3. **Format:** HTML is selected. This lets you build a styled, formatted email that looks like it came from a real service.
4. **HTML content:** the body of the phishing email.

Make sure the call-to-action link uses `{{.URL}}`. Gophish substitutes this with the real landing page URL when the campaign runs.

<img src="/assets/images/gophish/url-placeholder.png" alt="URL placeholder in template" />
<small class="media-caption">The URL placeholder is replaced with the campaign link when the email is sent.</small>

Click **Source** to preview the raw HTML:

<img src="/assets/images/gophish/source-preview.png" alt="HTML source preview" />
<small class="media-caption">Source view gives direct access to the HTML behind the message.</small>

<img src="/assets/images/gophish/html-preview.png" alt="HTML rendered preview" />
<small class="media-caption">The rendered preview shows how the HTML will appear to the recipient.</small>

<img src="/assets/images/gophish/email-preview.png" alt="Email preview" />
<small class="media-caption">The final email preview should be checked before the template is saved.</small>

Click **Save Template**.

---

### Landing Page

The landing page is the phishing page: where the target arrives after clicking and where credentials get captured.

Go to **Landing Pages** and click **New Page**.

<img src="/assets/images/gophish/landing-pages-section.png" alt="Landing Pages section" />
<small class="media-caption">Landing Pages lists the pages that can be attached to a campaign.</small>

<img src="/assets/images/gophish/new-landing-page-form.png" alt="New landing page form" />
<small class="media-caption">The page editor accepts cloned HTML and controls what submitted data Gophish records.</small>

Configuration steps:

1. Give the landing page a name.
2. Paste the HTML code. Usually a cloned version of the site being impersonated.
3. Check **Capture Submitted Data** to record what the target submits.
4. Check **Capture Passwords** to store the password field as well.

Preview with the **Source** tab:

<img src="/assets/images/gophish/source-preview.png" alt="Landing page source preview" />
<small class="media-caption">The cloned page can be reviewed and adjusted directly in source view.</small>

<img src="/assets/images/gophish/landing-page-preview.png" alt="Landing page rendered preview" />
<small class="media-caption">The preview reveals whether the imported page still looks correct inside Gophish.</small>

<img src="/assets/images/gophish/landing-page-view.png" alt="Final landing page view" />
<small class="media-caption">This is the page the target sees after following the link in the email.</small>

Click **Save Page**.

---

### Sending Profile

A sending profile is the SMTP configuration Gophish uses to deliver emails.

Go to **Sending Profiles**.

<img src="/assets/images/gophish/sending-profiles-section.png" alt="Sending Profiles section" />
<small class="media-caption">Sending Profiles keeps the SMTP configurations used to deliver campaign emails.</small>

<img src="/assets/images/gophish/smtp-config.png" alt="SMTP configuration form" />
<small class="media-caption">The first part of the form identifies the profile and the visible sender.</small>

<img src="/assets/images/gophish/smtp-config-2.png" alt="SMTP configuration details" />
<small class="media-caption">The remaining fields connect Gophish to the outgoing mail server.</small>

The fields to fill in:

1. **Profile name:** a label to identify this configuration.
2. **From address (SMTP From):** the sender address shown to the target. Needs to be a valid account.
3. **SMTP host:** the outgoing mail server. Here: `smtp.gmail.com:465`.
4. **Username:** the Gmail address used to authenticate.
5. **Password:** for Gmail, use an **App Password**. Regular account passwords no longer work for direct SMTP connections.

Before saving, test the configuration. Enter a valid email address and send a test message:

<img src="/assets/images/gophish/test-email-send.png" alt="Test email field" />
<small class="media-caption">A test message checks the SMTP settings before the campaign is launched.</small>

If the configuration is correct, a confirmation message appears:

<img src="/assets/images/gophish/test-email-success.png" alt="Test email success message" />
<small class="media-caption">This confirmation means the test message was accepted for delivery.</small>

<img src="/assets/images/gophish/sending-profile-saved.png" alt="Sending profile overview" />
<small class="media-caption">The working SMTP profile is now available for campaign setup.</small>

Click **Save Profile**.

---

## Launching the campaign

Go to **Campaigns** and create a new one.

<img src="/assets/images/gophish/campaign-creation.png" alt="Campaign creation form" />
<small class="media-caption">The campaign form brings together the template, landing page, sender, URL, and target group.</small>

Six fields to fill in:

1. Campaign name
2. Email template (the one created above)
3. Landing page (the phishing page)
4. URL (the IP address or hostname of the Gophish server, this is what `{{.URL}}` resolves to)
5. Sending profile
6. Target group (containing Jean LOU)

<img src="/assets/images/gophish/campaign-launch.png" alt="Campaign launch button" />
<small class="media-caption">Gophish asks for confirmation before sending the campaign.</small>

Click **Launch Campaign**.

---

## Results

<img src="/assets/images/gophish/campaign-overview.png" alt="Campaign live overview" />
<small class="media-caption">The live view starts recording events as soon as the campaign is launched.</small>

After launching, Gophish tracks activity in real time. A few moments later, the dashboard updates:

<img src="/assets/images/gophish/campaign-results.png" alt="Campaign results summary" />
<small class="media-caption">The summary records delivery, opening, link clicks, and form submissions.</small>

- **Email Sent:** one phishing email delivered to the target
- **Email Opened:** the target opened the email (tracking pixel worked)
- **Clicked Link:** the target clicked the link in the email
- **Submitted:** the target filled in and submitted the form on the landing page

The detailed view shows exactly what Jean LOU entered:

<img src="/assets/images/gophish/captured-credentials.png" alt="Captured credentials from the phishing page" />
<small class="media-caption">The detailed event shows the data submitted through the simulated landing page.</small>

His Facebook credentials were captured. The simulation worked end to end.

---

## Takeaway

The full attack chain ran without any technical exploit. No CVE, no zero-day. Just an email that looked credible enough to click. The weakness here is not the software stack, it's the person reading their inbox.

That's the point of running this kind of simulation: show people what a phishing attack looks like from the inside, then work on fixing the behavior rather than the system.

Jean LOU now knows.
