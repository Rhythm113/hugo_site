---
date: '2026-02-11T17:02:39-05:00'
draft: false
title: 'How a Phishing Campaign Died Overnight: A Real Incident Response'
---
It was September 2025. I had just finished packing.

The next morning, I was supposed to head back to my hometown for a short holiday. Everything was calm until a friend texted me out of nowhere.

He said he’d received an email from our university’s IT Director about a special enhanced version of RDS, our student portal.

I laughed.

“Why would you get a special version of RDS?”

He replied that he was a scholarship holder, and that’s why. He also added that the email looked legit.

Out of curiosity, I asked him to forward it to me.

---

## Something Felt Off

At first glance, the email was convincing. It referenced our IT Director, used familiar language, and even had “director.it” in the sender name.

But then I noticed the domain.

Our real university domain was:

```
northsouth.edu
```

The email came from:

```
northsouth.info
```

That small  difference changed everything.

Immediately, alarm bells went off.

Still, I wondered. Why target him? He wasn’t exactly a high-profile person.

Before I could think too deeply, another old classmate sent me the same email asking if it was real.

That’s when I knew this wasn’t a coincidence.

It was active phishing.

---

## Analysis time

The email contained a Google Drive link with a browser extension and a PDF explaining how to install it.

First thing I did: download a copy.

Second thing: report the extension to Google as malicious.

The attacker hadn’t even packaged it properly. The extension was extremely basic, probably AI-generated. Classic AI slop.

The JavaScript was obfuscated, but deobfuscating JS these days is trivial.

Within minutes, I understood exactly what it was doing.

It was a *cookie stealer*.

Every stolen browser cookie was being shipped directly to a Slack channel via the Slack API.

Even funnier, the extension also had a crude “AI feature,” complete with a hardcoded Hugging Face API key.

That part genuinely made me smile.

---

## A little fun

Instead of stopping at analysis, I decided to disrupt the operation.

I wrote a simple script that spammed their Slack webhook with:


Hello hacker nice to meet u.


Then I:

- Logged into my VPS 
- Routed traffic through Cloudflare
- Let the script run wild

Within hours, their Slack channel was drowning in *millions of messages* - burying any real cookie data.

Soon after, Google replied to my report.

The Drive link was gone.

Strike one.

Before going to sleep, I added another layer.

I wrote a second Python script that generated fake cookie values and sent those to Slack as well mixed in with my spam.

Now the attacker had:

- Endless junk messages  
- Fake cookies  
- No usable data  

Perfect chaos.

---

## Taking Down the Domain

I looked up the phishing domain and found it was registered through Hostinger.

I submitted an abuse report and went to sleep.

The next morning, while sitting on the train heading home, I checked again.

The domain was already offline. The slack server also gone (The attacker probably got depressed and deleted it)

Less than 24 hours.

A coordinated phishing campaign, gone.

After returning from holiday, I sent a full incident report to the IT Director and received an appreciation email.

---

## How Did They Get Student Emails?

Simple OSINT.

Our Financial Aid Office publicly posts scholarship recipient lists on the university website complete with student IDs.

If you have access to the NSU mailbox system, you can just plug those IDs into Gmail search and instantly discover student email addresses.

No hacking. Just some simple data hygiene.

---

## Why I Didn’t Trace the Attacker

Attribution requires:

- Legal notices  
- Provider cooperation  
- Serious time and money  

For what?

Sometimes, fast containment is the real victory.

Shutting it down quickly protects users.

That’s already a win.

---

## Final Thoughts

Incident response isn’t always like movies.

Sometimes it’s:

- Spotting a fake domain  
- Reverse-engineering sloppy malware  
- Filing abuse reports  
- And flooding a Slack channel with “Hello hacker nice to meet u.”

No legal drama.

No cinematic takedown.

Just practical action.

And honestly?

That’s enough.

Because sometimes, stopping the damage matters more than knowing who pulled the trigger. Students were saved and that was the goal.
