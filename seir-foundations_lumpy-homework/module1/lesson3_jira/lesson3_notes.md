# Lesson 3 Notes — Jira (Extra Credit)

## BAM

| Item | Value |
|---|---|
| EC2 Instance ID | `i-0e92090f44af04eec` |
| Working folder on EC2 | `~/SEIR-Foundations/module1/lesson2_slack` |
| Jira project | `BAI` (Balerica AI Incident Management) |
| Manual test incident | `BAI-2` |
| Integration test incident | `BAI-3` |

### BAM_5.1 — Jira config saved privately
- Saved site URL, email, project key, test issue key, and issue type in a private note (not in this repo).
- Created the manual test incident `BAI-2` ("TEST — EC2 application error") in Step 3.
- Step 5 checklist from `jira_install.md`:
  - [x] I created my own Atlassian account.
  - [x] I can open my Jira Cloud site.
  - [x] I selected or confirmed the Free plan.
  - [x] I created the Balerica AI Incident Management project.
  - [x] I know my **actual** project key. (`BAI`)
  - [x] I created and opened a manual test incident. (`BAI-2`)
  - [x] I saved the site URL, email, project key, and test issue key privately.
  - [x] I have **not** shared any passwords or API tokens.
- Screenshot: `screenshots/BAM_5.1.jpg`

### BAM_5.2 — API token + `jira.py --verify`
- Created an Atlassian API token (without scopes) and stored it only in the EC2 `.env` as `JIRA_API_TOKEN`.
- Copied `jira.py` and `incident.py` into `lesson2_slack` next to `slack.py`.
- Added the Jira variables to `.env` alongside the existing Slack settings.
- `python jira.py --verify` returned: `Connected to Jira project: BAI (Balerica AI Incident Management)`
- Screenshot: `screenshots/BAM_5.2.jpg` (Thu Nov 12 21:47:50 CST 2026)

### BAM_5.3 — Incident integration test

First, I activated the virtual environment and ran `python3 slack.py --test` to verify that my EC2 instance was connected to Slack. The terminal printed `SUCCESS: Chewbacca approves!`, and an alert appeared in my #balerica-alerts Slack channel.

Next, I ran `python3 jira.py --verify` to verify that my instance was connected to Jira. The terminal printed `Connected to Jira project: BAI (Balerica AI Incident Management)`, confirming the connection.

Then I ran `python3 incident.py --dry-run`. The terminal printed:

    EC2 agent -> incident.py -> Jira ticket -> Slack alert (includes Jira link)
    No API calls made. Run --test when .env is configured.

This shows that the Jira ticket is created first, so Slack has the ticket URL to include in its message.

Finally, I ran `python3 incident.py --test` once. The terminal printed:

    {
      "incident_id": "SEIR-TEST-20261113T162834687539",
      "jira_key": "BAI-3",
      "jira_url": "https://jcook8230.atlassian.net/browse/BAI-3",
      "jira_created": true,
      "slack_status": "sent"
    }

This created ticket BAI-3 in Jira and sent a message to Slack that links to the ticket. The same incident ID appears in the terminal, the Slack alert, and the Jira ticket, so one incident can be traced across all three.

**Time proof / timeline.** After the test, I ran the ID line in the same SSH session without rebooting:

    whoami && TZ='America/Chicago' date && ec2-metadata -i && uptime -s
    ec2-user
    Fri Nov 13 11:59:08 CST 2026
    instance-id: i-0e92090f44af04eec
    2026-11-13 15:50:16

`date` shows Central time because I set `TZ`, but `uptime -s` shows the server's default time zone, UTC (6 hours ahead of CST in November). Converting everything to CST:

| Event | UTC | CST |
|---|---|---|
| Instance booted (`uptime -s`) | 15:50 | 9:50 AM |
| `incident.py --test` ran (incident ID `...T162834...`, Jira "Created 10:28 AM") | 16:28 | 10:28 AM |
| ID line run | 17:59 | 11:59 AM |

Because the boot time is earlier than the test and there was no reboot in between, the test came from instance `i-0e92090f44af04eec`.

- Screenshots: `screenshots/BAM_5.3.jpg` (terminal), `screenshots/BAM_5.3_id.jpg` (ID line)

### BAM_5.4 — Slack alert links to Jira
- The `incident.py --test` alert in #balerica-alerts includes a clickable link to BAI-3. I clicked it and it opened the correct issue.
- Screenshot: `screenshots/BAM_5.4.jpg` (computer clock 12:09 PM 11/13/2026)

### BAM_5.5 — The Jira issue exists
- BAI-3 in the BAI project, with its key, title, and project visible. Created November 13, 2026 at 10:28 AM.

When I ran `incident.py --test`, `incident.py` acted as the coordinator: it built the incident and then called functions from the other two scripts. First it called `create_incident` from `jira.py`, which used my Jira API token to create ticket BAI-3 and returned the ticket key and URL. Then it called `send_alert` from `slack.py`, which used my Slack webhook to post an alert to #balerica-alerts. Jira went first because Slack needed the URL of the ticket to include in its message; if Slack went first, the alert would have no link to follow. I verified it worked in three places: the terminal printed `jira_key: BAI-3` and `slack_status: sent`, the Slack alert had a clickable BAI-3 link that opened the correct issue, and BAI-3 showed up in my Jira BAI project. The same incident ID appears in all three, so one incident can be traced from the terminal to Slack to Jira.

- Screenshot: `screenshots/BAM_5.5.jpg` (computer clock 12:13 PM 11/13/2026)

### BAM_5.7 — One-day sprint
- **Space:** `SEIR-Saga_JasonCook` (`SSJ`), Scrum, story points
- **Sprint dates:** Mon 11/16/2026, 9:05 AM to 11/17 12:00 AM (custom duration)
- **Committed:** 138 points (9 stories + 34 tasks under epic SSJ-11)
- **Completed:** 138 points. The red line reached 0 around 11:45 PM, and I completed the sprint.
- **What didn't get done:** Nothing. The day went as planned.

**Burndown chart.** The red line stayed above the gray guideline for most of the day because most of my higher-point tasks were concentrated later in the day. Everything before my 2 PM shift (Morning, Study, Prep, and Commute) adds up to only 32 of 138 points, about a quarter of the work, while about a third of the sprint time had passed. The remaining 106 points, including all of the 8-point items, fell in my 2 PM–10:30 PM shift and at closing, so the red line didn't catch up until the end of the night.

**Unplanned work.** My day had no unplanned work, but in a real shift a broken-down truck could show up at 3 PM that isn't on the board. I would stop working on SSJ-29, create a new work order, and assign the truck to a technician. That slows down my planned work, which is what the reading means by unplanned work. Because interruptions like this happen often in my job, I gave SSJ-29 8 points so the estimate leaves room for them.

- Screenshot: `screenshots/BAM_5.7.jpg`

### Troubleshooting check performed
My first `jira.py --verify` failed with `Jira HTTP 404. Check JIRA_SITE_URL and JIRA_PROJECT_KEY`. I confirmed the site URL and project key in `.env` matched the browser exactly (using `grep ... | cat -A` to rule out hidden characters). I then called the Jira `myself` endpoint, which returned **401**, and `project/search` returned an empty list, which showed Jira was treating me as anonymous, so the 404 was really a login problem. Using `grep -c` checks that print only counts (never the secret values), I found the token was fine (192 characters, starts with `ATATT`, no hidden whitespace) but `JIRA_EMAIL` did not match my Atlassian login because it had been cut off when pasted. After fixing the email, `--verify` succeeded.
