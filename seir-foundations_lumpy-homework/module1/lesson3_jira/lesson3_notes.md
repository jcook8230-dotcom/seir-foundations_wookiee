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
- Completed the Step 5 checklist in `jira_install.md`.
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

- Screenshots: `screenshots/BAM_5.3.jpg` (terminal), `screenshots/BAM_5.3_id.jpg` (ID line), `screenshots/BAM_5.3_jira.jpg`, `screenshots/BAM_5.3_slack_channel.jpg` (both alerts in #balerica-alerts)

### Troubleshooting check performed
My first `jira.py --verify` failed with `Jira HTTP 404. Check JIRA_SITE_URL and JIRA_PROJECT_KEY`. I confirmed the site URL and project key in `.env` matched the browser exactly (using `grep ... | cat -A` to rule out hidden characters). I then called the Jira `myself` endpoint, which returned **401**, and `project/search` returned an empty list, which showed Jira was treating me as anonymous, so the 404 was really a login problem. Using `grep -c` checks that print only counts (never the secret values), I found the token was fine (192 characters, starts with `ATATT`, no hidden whitespace) but `JIRA_EMAIL` did not match my Atlassian login because it had been cut off when pasted. After fixing the email, `--verify` succeeded.
