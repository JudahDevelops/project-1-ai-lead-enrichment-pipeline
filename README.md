# Project 1: AI-Powered Lead Enrichment Pipeline

## What This Project Is

This workflow automates the process of finding potential clients, verifying their contact
details, and writing a personalized opening line for each one — all without any manual work.

In sales, this process is called **lead enrichment**. Normally you would do this by hand:
search Apollo for prospects, copy their details into a spreadsheet, verify each email manually,
then spend hours writing a unique opening line for each person. This workflow does all of that
automatically, on a schedule, every day.

---

## The Problem It Solves

8cast Corporation sells fractional CFO services to founders and small business owners. To get
clients, someone needs to:

1. Find founders and CEOs at small companies
2. Confirm their email addresses are real (bad emails hurt your sending reputation)
3. Write something personal enough that they'll actually read it

This pipeline does steps 1, 2, and 3 automatically, and drops the results into a Google Sheet
ready for a campaign tool like Instantly.ai to send.

---

## How It Works — Plain English

Every weekday morning at 9AM, the workflow wakes up and does this:

1. **Searches Apollo.io** for 25 founders, CEOs, and CFOs at small companies (1–50 employees)
2. **Groups them into batches of 10** so it doesn't overwhelm the APIs
3. **Sends each email to Hunter.io** to check if it's real and deliverable
4. **For valid emails:** asks an AI (OpenAI) to write a personalized one-liner based on the
   person's job title, company, and industry
5. **For invalid emails:** marks them as "unverified" and skips them (no wasted outreach)
6. **Saves every result** to a Google Sheet with all enriched data
7. **Posts a Slack summary** — "Enriched 18 leads, 7 skipped" — so you know it ran

---

## The Workflow — Node by Node

```
[Schedule Trigger]
      |
      v
[HTTP Request → Apollo API]        ← pulls 25 raw leads
      |
      v
[Code Node: Normalize + Dedupe]    ← cleans the data, removes duplicates
      |
      v
[Split In Batches: 10 at a time]   ← prevents rate limit errors
      |
      v
[HTTP Request → Hunter.io]         ← verifies each email address
      |
      v
[IF Node: email valid?]
      |                   |
     YES                  NO
      |                   |
      v                   v
[AI Transform]      [Set Node: mark "unverified"]
(write icebreaker)
      |                   |
      +-------------------+
                |
                v
[Google Sheets: append row]        ← saves enriched lead to spreadsheet
                |
                v
[Slack: post daily summary]        ← notifies you it ran
```

---

## Tech Stack

| Tool | Purpose | Cost |
|---|---|---|
| **n8n** | Orchestrates the entire workflow | Already have it |
| **Apollo.io** | Source of prospect data (name, title, company, email) | Free tier: 50 exports/month |
| **Hunter.io** | Email verification — checks if address is real | Free tier: 25 verifications/month |
| **OpenAI API** | Writes the personalized icebreaker per lead | Pay-per-use (~$0.01 per lead) |
| **Google Sheets** | Stores the enriched lead list | Free |
| **Slack** | Daily summary notification | Free |

---

## What the Output Looks Like

Each row in the Google Sheet will have these columns:

| Column | Example Value |
|---|---|
| name | Amara Okafor |
| title | CEO |
| company | Paystack |
| industry | Fintech |
| email | amara@paystack.com |
| email_status | valid |
| employee_count | 35 |
| icebreaker | Scaling a fintech team to 35 means your burn rate story needs to be airtight for Series A — that's exactly where we help. |
| enriched_at | 2026-05-28T09:14:22Z |

---

## What You'll Tell the Interviewer

This project proves you can:

- **Replace Clay** — Clay charges $800+/month to do this enrichment. This pipeline does the
  same thing for ~$5/month in API costs using n8n + free tiers.
- **Handle data hygiene** — the Hunter verification step means no bad emails enter the system.
  This directly addresses their requirement: "no duplicates, no stale records, no gaps."
- **Personalize at scale** — the AI icebreaker step shows you understand that volume alone
  doesn't book calls — relevance does.
- **Build infrastructure, not just connect apps** — the batching, error handling, and Slack
  alerting show you think beyond a simple 3-step Zapier zap.

---

## Resume Bullet (copy this)

> Built n8n lead enrichment pipeline pulling 25 prospects/run from Apollo API, verifying
> emails via Hunter.io, and generating AI-personalized icebreakers per lead using OpenAI —
> replacing manual Clay enrichment at ~$5/month vs $800+/month per-seat cost.

---

## Portfolio Screenshot Checklist

Before considering this project done, capture:

- [ ] The full n8n canvas (zoom out to show all nodes connected)
- [ ] The Apollo HTTP Request node — showing the API call config
- [ ] The IF node — showing the true/false branch logic
- [ ] The AI Transform node — showing the prompt you wrote
- [ ] The Google Sheet — showing 10+ enriched rows with all columns filled
- [ ] The Slack message — showing the daily summary
- [ ] The n8n execution log — showing a successful run (green checkmarks on all nodes)

---

## Build Steps (follow in order)

1. Sign up for Apollo.io (free) → get API key
2. Sign up for Hunter.io (free) → get API key
3. Set up Google Sheet with correct column headers
4. Add credentials in n8n: Apollo, Hunter, OpenAI, Google Sheets, Slack
5. Build workflow node by node (guided step-by-step)
6. Test with real data
7. Screenshot everything

---

## Files in This Folder

| File | What it is |
|---|---|
| `README.md` | This file — full project explanation |
| `workflow.json` | The importable n8n workflow (created during build) |
| `sample-output.csv` | Example enriched lead data (created after first test run) |
