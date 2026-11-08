# Lesson 2 — Slack Alerts

## Workspace and channel
- Workspace: https://balericaai.slack.com
- Channel: #balerica-alerts

## Proof
![Slack alert](screenshots/slack_alert.jpg)
![Terminal success](screenshots/slack_test_success.jpg)

## Workspace URL vs Webhook URL
- Workspace URL: https://balericaai.slack.com
- Webhook URL: starts with `https://hooks.slack.com/services/` (the rest is secret and stored only in `.env`)

- The workspace URL is used by me to sign in to my Slack app.
- The webhook URL is used by my EC2 instance to send messages to my Slack app.
- If someone got the workspace URL, they would still need to sign in with an account in my workspace before they could read or post anything.
- If someone got the webhook URL, they could post to my channel without logging in.

## Claude Code question and how I verified it
- **Question I asked:** Why did `python slack.py --test` give `ModuleNotFoundError: No module named 'requests'` even though `(venv)` was in my prompt?
- **What I learned:** I got a `ModuleNotFoundError`. I was in the right environment, but `requests` hadn't been installed yet. Closing and reopening the venv didn't help. Claude helped me see that I needed to install it. I ran `pip install` for `requests`, and the script ran.
- **How I verified it:** I ran `python slack.py --test` and got back `SUCCESS: Chewbacca approves!`. I then went to my Slack channel, `#balerica-alerts`, and checked for a message. The message I got was: `TEST: Slack is connected. RRRRAAAWWWRRR!`
