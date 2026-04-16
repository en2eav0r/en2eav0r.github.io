---
title: "Gophish: Setting Up a Phishing Simulation Campaign"
date: 2025-03-24 00:00:00 +0000
categories: [Projects]
tags: [gophish, phishing, social-engineering, red-team, smtp, landing-page, awareness, security-assessment]
image:
  path: /assets/images/gophish/gophish-logo-official.png
  alt: Gophish logo
---

<img src="/assets/images/gophish/gophish-logo.png" alt="Gophish logo" style="border-radius: 10px; width: 40%; display: block; margin: 0 auto;" />

Gophish is an open-source platform for running phishing simulations. You send realistic phishing emails to a defined set of targets, track who opens them, who clicks the link, and who submits credentials, then use that data to work on security awareness inside an organization.

This post walks through a full campaign: installation, configuration, execution, and results.

---

## Attack scenario

The fictional company **SNOW**, a software solutions firm, wants to test how its employees react to phishing attempts. The IT team runs an internal campaign using Gophish to simulate a realistic attack.

The target is **Jean LOU**, a financial analyst with access to sensitive data. That kind of profile is exactly what a real attacker would go after.

<img src="/assets/images/gophish/target-email.png" alt="Target email address (redacted)" style="border-radius: 10px; width: 60%;" />

---

## Installing Gophish on Windows

**Download Gophish** from the official GitHub releases page:

[https://github.com/gophish/gophish/releases](https://github.com/gophish/gophish/releases)

<img src="/assets/images/gophish/gophish-download.png" alt="Gophish GitHub releases page" style="border-radius: 10px; width: 100%;" />

**Extract the archive:**

<img src="/assets/images/gophish/gophish-extract.png" alt="Extracting the Gophish archive" style="border-radius: 10px; width: 100%;" />

**Launch from the command prompt.** The console prints your admin credentials and the local dashboard URL. Write them down before doing anything else.

<img src="/assets/images/gophish/gophish-console.png" alt="Gophish console showing credentials and URL" style="border-radius: 10px; width: 100%;" />

---

## Logging in and navigating the interface

Open the URL from the console and log in with the credentials shown there. First login forces a password change.

<img src="/assets/images/gophish/gophish-login.png" alt="Gophish login page" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/gophish/gophish-interface.png" alt="Gophish main interface" style="border-radius: 10px; width: 100%;" />

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

<img src="/assets/images/gophish/users-groups-section.png" alt="Users and Groups section" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/gophish/new-group-form.png" alt="New group form" style="border-radius: 10px; width: 100%;" />

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

<img src="/assets/images/gophish/email-templates-section.png" alt="Email Templates section" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/gophish/new-template-form.png" alt="New Template form" style="border-radius: 10px; width: 100%;" />

For this campaign, the email impersonates Facebook and tells the target to reset their password. The more believable the email, the more likely the target clicks.

The form fields:

1. **Template name:** `TEMPLATE SNOW`
2. **Subject:** `Facebook: Reset your password`. The subject line has to match the email body to look credible.
3. **Format:** HTML is selected. This lets you build a styled, formatted email that looks like it came from a real service.
4. **HTML content:** the body of the phishing email.

Make sure the call-to-action link uses `{{.URL}}`. Gophish substitutes this with the real landing page URL when the campaign runs.

<img src="/assets/images/gophish/url-placeholder.png" alt="URL placeholder in template" style="border-radius: 10px; width: 100%;" />

Click **Source** to preview the raw HTML:

<img src="/assets/images/gophish/source-preview.png" alt="HTML source preview" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/gophish/html-preview.png" alt="HTML rendered preview" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/gophish/email-preview.png" alt="Email preview" style="border-radius: 10px; width: 100%;" />

Click **Save Template**.

---

### Landing Page

The landing page is the phishing page: where the target arrives after clicking and where credentials get captured.

Go to **Landing Pages** and click **New Page**.

<img src="/assets/images/gophish/landing-pages-section.png" alt="Landing Pages section" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/gophish/new-landing-page-form.png" alt="New landing page form" style="border-radius: 10px; width: 100%;" />

Configuration steps:

1. Give the landing page a name.
2. Paste the HTML code. Usually a cloned version of the site being impersonated.
3. Check **Capture Submitted Data** to record what the target submits.
4. Check **Capture Passwords** to store the password field as well.

Preview with the **Source** tab:

<img src="/assets/images/gophish/source-preview.png" alt="Landing page source preview" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/gophish/landing-page-preview.png" alt="Landing page rendered preview" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/gophish/landing-page-view.png" alt="Final landing page view" style="border-radius: 10px; width: 100%;" />

Click **Save Page**.

---

### Sending Profile

A sending profile is the SMTP configuration Gophish uses to deliver emails.

Go to **Sending Profiles**.

<img src="/assets/images/gophish/sending-profiles-section.png" alt="Sending Profiles section" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/gophish/smtp-config.png" alt="SMTP configuration form" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/gophish/smtp-config-2.png" alt="SMTP configuration details" style="border-radius: 10px; width: 100%;" />

The fields to fill in:

1. **Profile name:** a label to identify this configuration.
2. **From address (SMTP From):** the sender address shown to the target. Needs to be a valid account.
3. **SMTP host:** the outgoing mail server. Here: `smtp.gmail.com:465`.
4. **Username:** the Gmail address used to authenticate.
5. **Password:** for Gmail, use an **App Password**. Regular account passwords no longer work for direct SMTP connections.

Before saving, test the configuration. Enter a valid email address and send a test message:

<img src="/assets/images/gophish/test-email-send.png" alt="Test email field" style="border-radius: 10px; width: 100%;" />

If the configuration is correct, a confirmation message appears:

<img src="/assets/images/gophish/test-email-success.png" alt="Test email success message" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/gophish/sending-profile-saved.png" alt="Sending profile overview" style="border-radius: 10px; width: 100%;" />

Click **Save Profile**.

---

## Launching the campaign

Go to **Campaigns** and create a new one.

<img src="/assets/images/gophish/campaign-creation.png" alt="Campaign creation form" style="border-radius: 10px; width: 100%;" />

Six fields to fill in:

1. Campaign name
2. Email template (the one created above)
3. Landing page (the phishing page)
4. URL (the IP address or hostname of the Gophish server, this is what `{{.URL}}` resolves to)
5. Sending profile
6. Target group (containing Jean LOU)

<img src="/assets/images/gophish/campaign-launch.png" alt="Campaign launch button" style="border-radius: 10px; width: 100%;" />

Click **Launch Campaign**.

---

## Results

<img src="/assets/images/gophish/campaign-overview.png" alt="Campaign live overview" style="border-radius: 10px; width: 100%;" />

After launching, Gophish tracks activity in real time. A few moments later, the dashboard updates:

<img src="/assets/images/gophish/campaign-results.png" alt="Campaign results summary" style="border-radius: 10px; width: 100%;" />

- **Email Sent:** one phishing email delivered to the target
- **Email Opened:** the target opened the email (tracking pixel worked)
- **Clicked Link:** the target clicked the link in the email
- **Submitted:** the target filled in and submitted the form on the landing page

The detailed view shows exactly what Jean LOU entered:

<img src="/assets/images/gophish/captured-credentials.png" alt="Captured credentials from the phishing page" style="border-radius: 10px; width: 100%;" />

His Facebook credentials were captured. The simulation worked end to end.

---

## Takeaway

The full attack chain ran without any technical exploit. No CVE, no zero-day. Just an email that looked credible enough to click. The weakness here is not the software stack, it's the person reading their inbox.

That's the point of running this kind of simulation: show people what a phishing attack looks like from the inside, then work on fixing the behavior rather than the system.

Jean LOU now knows.
