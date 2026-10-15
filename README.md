# Tentcomm Ops Desk

One dashboard for the whole company: social media, sales and leads, tasks, people (HR) and finance.

This is the **UI prototype**. It is a single static file with no build step. Open `dashboard/index.html` in a browser.

All numbers are **sample data**, produced by small generator functions so the layout can be reviewed before any integration exists.

## What's in it

The goal is one tool that replaces ClickUp, the HR software, the social media scheduler, the influencer tool, the accounting/invoicing software and Slack.

| Section | What it does | Replaces |
|---|---|---|
| Dashboard | Today so far, performance chart, health meters, publishing today, client health, a "Needs attention" list, and quick actions | — |
| Clients | Every client as a card with health, retainer, margin and next post. Each client has its own hub: overview, social, content, tasks, campaigns, chat and billing | CRM / spreadsheets |
| Client portal | A private, read-only page per client: their numbers, posts waiting for their approval, campaign reports and invoices | Report decks, email approvals |
| Content | Publishing calendar for Tent Communications and every client, a post composer, and an approval flow (draft → client approval → scheduled → published) | Social media posting tool |
| Influencers | Creator directory with contacts and rates, campaigns with deliverables and payments, campaign reports, work history per creator, and a shareable report link | Influencer management tool |
| Tasks & projects | Workload per person, project progress, and a task board linked to clients | ClickUp |
| Messages | Slack-style team spaces, a space per client (client contacts join as guests) and direct messages | Slack |
| Social media | Metrics for Tent Communications or any client, by platform | Native analytics |
| Sales & leads | Leads today, by source, funnel, latest leads and team leaderboard | CRM reports |
| People | Attendance, who is out, leave approvals and attendance trend | HR software |
| Finance & invoicing | Cash, money in and out, invoices (create, send, record payment), bills and spending | Accounting / invoicing software |
| Team & access | Invite people, set roles, tick which sections each person can open, preview the app as them, and control client portal and chat access | — |

The date range control (Today, 7 days, 30 days, Custom) drives every period metric. Deltas compare against the previous period of the same length.

## Building it for real

Everything you need to replace is in the `SAMPLE DATA` block of the `<script>` in `dashboard/index.html`. Each generator maps to one data source:

| Generator / constant | Replace with |
|---|---|
| `PLATFORMS`, `socialDay(platform, day)` | Meta Graph API (Facebook, Instagram), LinkedIn, TikTok, YouTube Analytics, X APIs, or an aggregator |
| `salesDay(day)`, `SOURCES`, `REPS` | Your CRM (lead counts by source, stage counts, won revenue) |
| `TASKS`, `TEAM`, `PROJECTS` | Your task tool (ClickUp, Notion, Asana…) or your own tasks table |
| `DEPTS`, `OUT_TODAY`, `LEAVES` | Attendance / HR system |
| `ACCOUNTS`, `financeDay(day)`, `RECEIVABLES`, `PAYABLES`, `INVOICES` | Your own invoices table, plus bank feeds |
| `CLIENTS`, `POSTS`, `INFLUENCERS`, `CAMPAIGNS`, `CHANNELS`, `USERS`, `GUESTS` | Your own database tables (Supabase). Posting goes out through each platform's publishing API |

`CONFIG` at the top of the script sets the company name, the currency symbol and the weekend days (Fri and Sat by default).

A suggested build: a Next.js (or similar) app with one API route per section. Each route returns the same shapes the generators return. A scheduled sync job caches the external APIs into a database so the dashboard loads quickly and stays the single source of truth.
