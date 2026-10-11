---
title:  "Writing Incident Response Reports: A Primer for CIRCUS Competitors and New DFIR Analysts"
author: Turb0Yoda
date:   2026-10-10 21:20:00 -0700
tags: [DFIR,Reporting,Forensics,Legal,Incident Response]
comments: true
---

# Introduction:

This blog initially started out as a late semi-short post for students who are competing in CIRCUS - the Collegiate Incident Response Competition for Undergrad Students. 
While writing this post, I realized that it could serve as a good look into how we do reporting for incident response - primarily for those looking to get into incident response, those who are very new to the field, and of course, the competing students. As far as I’m aware, no one really teaches how to write an incident response report in undergrad. I’ve taken my own undergrad forensics course and sat in on others that cover forensic report writing. Those reports tend to be in excruciating detail, and for good reason: they are frequently prepared for use in criminal cases Incident Response reports are - with a few small exceptions - the exact opposite.

I don’t intend to get too deep into every single aspect of reporting, but I hope that this blog post provides a balanced overview of what can be covered in an incident response report.

# A word on Privilege and Confidentiality

Now that I have the attention of legal counsel with those words, it’s time to discuss something that students should at least be aware of. Most engagements are delivered under _some_ level of privilege and confidentiality. This is a baseline, near-universal request by either internal or external counsel to limit the spread of information related to the incident. In the consulting world, it’s almost always assumed that there is a level of both privilege and confidentiality involved no matter the type of engagement, and this is always explicitly ironed out before the engagement starts. Because of this, it is almost impossible to find any publicly available Incident Response Reports.

This can be worded several ways based on what counsel desires. This is a non-exhaustive list, but the three most common ones I see:

* [Privileged and Confidential]
* [Privileged and Confidential: Attorney-Client Work Product]
* [Privileged and Confidential: Prepared at the Direction of Counsel]

The primary reason why privilege and confidentiality markings are used is to help protect documentation from discovery should things go to trial. However, there are ways to waive that protection. It’s fairly rare and I am not a lawyer, so I won’t be delving into that. Below is an example from the Capital One breach in 2019 where the court ordered the Mandiant incident response report produced in discovery, rejecting Capital One's work-product claim.

* [Capital One Breach - Document 490](https://storage.courtlistener.com/recap/gov.uscourts.vaed.456165/gov.uscourts.vaed.456165.490.0.pdf)

Here are some references to the nuances between privilege, confidentiality, and related concepts. The two are **not** the same, but almost always come as an intertwined pair.

* [Purdue video on Legal Ethics](https://youtu.be/AxVcQxAQxRs)
* [Canadian Bar Association - Privacy and Ethics](https://web.archive.org/web/20260825025502/https://cba.org/resources/practice-tools/privacy-and-ethics-a-toolkit-for-lawyers/difference-between-privilege-and-confidentiality/)

# Types of Reports:

DFIR reports are typically separated into two deliverable categories: an executive summary and a technical report. The in-between option is a Key Facts Summary.

All of the report types will be bookended by these two things:

1. A Table of Contents
2. Appendices

There’s not much to cover about the table of contents - that’s self-explanatory. The appendices will contain a variety of things based on how your organization or team chooses to format them. Typically, a full list of indicators(host- or network-based),methodology-related items (CrowdStrike has an excellent methodology section and is seen in one of the reports below), or other engagement-specific items (e.g., in-depth malware technical breakdowns) can be included here. 

## Executive Summary

An executive summary is aimed at executives and internal or external stakeholders, and contains a high-level overview with few technical references. By technical references, I mean line-by-line readouts of the incident. Timestamps and some events will always be referenced. These tend to be at most, one or two pages.

## Key Facts Summary

The Key Facts Summary is a step above the executive summary in detail, where major events are referenced directly, typically as individual bullet points. The best example ,and the use case I've written them for most are for Business Email Compromises, or BECs. Here, high-level events like specific account takeovers and data exfiltration are covered, but not as much of the nitty-gritty. These rarely run longer than three or four pages.

## Full Technical Report

Lastly, a full, fat technical report. These can be extremely long, and tend to be a blow-by-blow of the incident, and can include excruciating detail on the events. As such, there are (basically) no public examples. You are not expected to transplant every single event in your Spreadsheet of Doom (SOD), but you are expected to include every critical event. For example, if an attacker dumps NTDS.DIT, dumps LSASS memory, and runs SharpHound on an endpoint, leaving the output files on disk - every single file creation/modification event does not need to be written down. Those three items may have ~50 entries in the SOD, but 3-4 sentences covering it all should suffice.

Full reports will typically include an executive summary before the technical portion, so knowing how to write a full technical report will, by default, let you write both an executive summary and a Key Facts Summary with relative ease.

For all of the above, please prepare a vat of red ink to be used during quality control/review by your team, the client, and counsel.  

# Defensible Language:

One of the major failures I noticed when judging reports from the 2025 CIRCUS competition was the lack of defensible language. There’s no one person or thing to blame, as I haven’t seen any course or lesson teaching this. It is something you will want to be aware of if/when you have to write your own IR report. The language used must be factual and  defensible, and able to hold up in court in the absolute worst-case scenario. The easiest (and extremely repetitive) way to do this is to structure sentences related to evidence/actions like this:

```On September 16, 2025, at 14:55:12 UTC*, the Windows Security Event Log recorded the “rickybobby” user account authenticating to DC-01. ```

* *You don’t have to go full ISO 8601 on timestamps - I prefer this variation on writing timestamps in reports due to it being easier on the eyes. Just make sure you are consistent with formatting across the entire report.* 

There is no inference, guessing, or hunches in that statement. It is backed by evidence from the image/triage data.

- Is it extremely repetitive? Yes
- Will it make your legal counsel happy? Yes

Using that alone will help stop the world’s supply of red ink from being depleted so quickly, and spares everyone involved in the reporting process a few grey hairs.

Another key statement is also made when there is no data or evidence to help support a conclusion. Oftentimes, logs will rotate out due to age, or there will be data unavailable for various reasons. This statement will often be seen in a report when that happens.

```The Forensics team was unable to determine access to the FILESERV-04 Server within the evidence available.```

It is semi-rare for these reports to surface in legal or government proceedings, however, it does happen. The Suffolk County Legislature's report on the 2022 ALPHV/BlackCat ransomware attack is one example. This report references incident response report(s) from investigation(s) conducted by Palo AltoNetworks’s Unit 42 Incident Response Team. The report(s) are not public insofar as I could find, but this is an example of IR reports being referenced in official  documents.

* [Cyber Attack on Suffolk County](https://web.archive.org/web/20260113191746/https://www.scnylegislature.us/DocumentCenter/View/118502/09122024-Report-On-The-2021-2022-Cyber-Attack-On-Suffolk-County-PDF) 

# Example Reports:
I was able to scrounge up some key facts and executive-style reports. Unsurprisingly, and as mentioned above, publicly released technical reports are basically unheard of. A deep dive into security failures and the client’s environment as a whole is not exactly something anyone wants to publish. For CIRCUS, and most likely any other CTF or competition where a DFIR report is needed, a full technical report is expected. The reports listed are still a good starting point for seeing how sentences are written and how to format the executive summary of a report.

Somewhat ironically, the bulk of the reports posted are from places where I have worked and currently work. The views expressed here are my own and do not represent those of my current or former employers. All reports linked are publicly available and were not obtained through my employment.


## 2012 Mandiant Report

This is most analogous to a key-findings report. It's a bit special as it's made specifically for the public eye. Most reports that are made for the publci will be a bit similar. Take note of the background section - this is pretty much how all reports will be written like. 

* A sentence stating when the client noticed the breach and some details regarding known extent or concerns
* A sentence stating when the client(or counsel on behalf of client) engaged the incident response team
* A list of objectives(or the scope of the engagement) prescribed to the responding team
    * These typically remain pretty static, but can morph based on the incident
* An extremely high-level overview of what was done
* A timeframe of the investigation/engagement

* [OAG Report](https://web.archive.org/web/20260731120658/https://oag.ca.gov/system/files/Mandiant%20Report_0.pdf)


## Mid 2024 CrowdStrike Report

A jump to the future- this report is a (Preliminary) public summary made for counsel on behalf of the client. You may note that in this case, the report's language states that the incident response team(firm) is assisting counsel, versus the client directly. This is a bit of an extended executive summary.

* [Snowflake Report](https://www.snowflake.com/en/resources/learn/snowflake-security-hub/crowdstrike/)


## Late 2024 CrowdStrike Report

This is the cloest to a key-facts summary that is public. High-level events are numbered out in order of when they happened. The Appendix items are also included. This is a glimpse into the tools used and how the analysis was carried out. This typically remains static engagement to engagement, but can also be adjusted as needed- e.g. if there was no Azure environment in play, this section would not exist.

* [PowerSchool Report](https://web.archive.org/web/20260526190531/https://pages.powerschool.com/rs/387-SBG-541/images/PowerSchool%20-%20CrowdStrike%20Final%20Report.pdf)


# Crypocurrency Reports

This wasn't intended to be a stand-alone section, but ended up coming to fruition by accident. North Korea has been stealing crypto for a while, and this year has been fruitfil for them. Crypto companies also seem very happy to have public reporting available. A lot of the below material is "First-party", and published/created by the victim and not the investigating firm. As such the language used differs vastly from the reports above. They are also the closest thing I could find to published technical incident response reports at the time of the initial write up of this post. All of these are what I would consider odd-ball, but still good reading.

## 2026 BitGet

This set of reports are preliminary investigation reports published by Mandiant and Slowmist regarding a(at the time of writing) crypto heist by suspected DPRK Threat Actors. There’s not really much in the way of details here, being a preliminary report- but this is often used by companies in order to let downstream clients/etc know that there is an active investigation going on. These reports are published by the investigating firm.

Personal commentary - the narrative-style of writing used by slowmist is something I am not a fan of. 

* [Mandiant Preliminary Report](https://web.archive.org/web/20261005120734/https://img.bgstatic.com/multiLang/events/MFR26-1029_Status_Update_Bitget_0930.pdf)

* [SlowMist Preliminary Report](https://github.com/slowmist/Knowledge-Base/blob/master/open-report-V2/incident-response/SlowMist%20Investigation%20Progress%20Report%20-%20Bitget_en-us.pdf)


## 2026 Drift Protocol Report - now known as Velocity

* [Post on Twitter/X](https://x.com/velocitydex/status/2040611161121370409?s=46)

Yes, it’s a tweet. A full narrative of the heist, and there are no hard references to forensic artifacts or details of the investigation. Any specific technical conclusions must be drawn up by the reader.

## 2026 LayerZero KelpDAO report

* [KelpDAO Incident](https://layerzero.network/publications/kelpdao-incident-report.pdf)

This public-facing report is primarily penned by LZ- but skipping to section 2, you can see pieces of a technical report that have been thrown in here - primarily the malware and carved out implants. 


# One Last Resource
Lastly, CrowdStrike released an IR Tracker for public use some time ago. I’ve tried several case management systems, and nothing seems to beat a good ole’ Spreadsheet of Doom. Keep this properly filled out during your investigation, and reporting writing becomes much easier.

* [CrowdStrike IR Tracker](https://www.crowdstrike.com/en-us/blog/crowdstrike-releases-digital-forensics-and-incident-response-tracker)


