# Tentcomm Ops Desk

One dashboard for the whole company: social media, sales and leads, tasks, people (HR) and finance.

This is the **UI prototype**. It is a single static file with no build step. Open `dashboard/index.html` in a browser.

All numbers are **sample data**, produced by small generator functions so the layout can be reviewed before any integration exists.

## What's in it

| Section | What it shows |
|---|---|
| Overview | Today so far (leads, tasks done, attendance, leave requests, cash collected, new followers), one card per department, and a "Needs attention" list |
| Social media | Followers, new followers, reach, impressions, engagement rate and posts for all platforms or a single one. Includes reach by platform, share of reach, a card per platform, new followers and top posts |
| Sales & leads | Leads today, leads by source over time, pipeline funnel, the latest leads and a sales team leaderboard |
| Tasks & projects | Done today, overdue and due today, workload per person, project progress, and a task board where you can create and move tasks |
| People | Present, absent, late and on leave today, a breakdown by department, who is out, leave applications you can approve or decline, and the attendance rate trend |
| Finance | Cash on hand by account, money coming in (receivables), money owed (payables), cash in vs out, cash balance and spending by category |

The date range control (Today, Last 7 days, Last 30 days, Custom) drives every period metric. Deltas compare against the previous period of the same length. Today's numbers compare against yesterday at the same time.

## Building it for real

Everything you need to replace is in the `SAMPLE DATA` block of the `<script>` in `dashboard/index.html`. Each generator maps to one data source:

| Generator / constant | Replace with |
|---|---|
| `PLATFORMS`, `socialDay(platform, day)` | Meta Graph API (Facebook, Instagram), LinkedIn, TikTok, YouTube Analytics, X APIs, or an aggregator |
| `salesDay(day)`, `SOURCES`, `REPS` | Your CRM (lead counts by source, stage counts, won revenue) |
| `TASKS`, `TEAM`, `PROJECTS` | Your task tool (ClickUp, Notion, Asana…) or your own tasks table |
| `DEPTS`, `OUT_TODAY`, `LEAVES` | Attendance / HR system |
| `ACCOUNTS`, `financeDay(day)`, `RECEIVABLES`, `PAYABLES` | Accounting software and bank feeds |

`CONFIG` at the top of the script sets the company name, the currency symbol and the weekend days (Fri and Sat by default).

A suggested build: a Next.js (or similar) app with one API route per section. Each route returns the same shapes the generators return. A scheduled sync job caches the external APIs into a database so the dashboard loads quickly and stays the single source of truth.
