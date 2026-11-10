# Lesson 2 Homework — Slack Alert Scripts

I ran seven Python scripts from my EC2 instance. Each one sent an alert to `#balerica-alerts` in https://balericaai.slack.com through a webhook. The webhook URL is stored only in `.env`.

## Run details
- **Instance ID:** i-0fb77b43b6c4d1aa8
- **Instance type:** t3.micro
- **AWS zone:** us-east-1c
- **Run from:** `/opt/balerica-bank`, with the `venv` active

## Results

| Script | Incident | Severity | Terminal result | Proof |
|---|---|---|---|---|
| `hw1_hello_slack.py` | HW-001 | INFO | Delivered | [before](screenshots/hw1_hello_slack.py/hw1_hello_slack.py_ec2_befor.jpg), [after](screenshots/hw1_hello_slack.py/hw1_hello_slack.py_ec2_after.jpg), [Slack](screenshots/hw1_hello_slack.py/hw1_hello_slack.py_ec2_slack.jpg) |
| `hw2_severity_tour.py` | HW-002 | INFO, WARNING, ERROR, CRITICAL | All four delivered | [terminal](screenshots/hw2_severity_tour.py/hw2_severity_tour.py_ec2.jpg), [INFO](screenshots/hw2_severity_tour.py/hw2_severity_tour.py_slack_info.jpg), [WARNING](screenshots/hw2_severity_tour.py/hw2_severity_tour.py_slack_warning.jpg), [ERROR](screenshots/hw2_severity_tour.py/hw2_severity_tour.py_slack_error.jpg), [CRITICAL](screenshots/hw2_severity_tour.py/hw2_severity_tour.py_slack_critical.jpg) |
| `hw3_server_time.py` | HW-003 | INFO | Delivered at 04:10:47 UTC | [terminal](screenshots/hw3_server_time.py/hw3_server_time.py_ec2.jpg), [Slack](screenshots/hw3_server_time.py/hw3_server_time.py_slack.jpg) |
| `hw4_disk_watch.py` | HW-004 | INFO | Delivered, disk 25.4% used | [terminal](screenshots/hw4_disk_watch.py/hw4_disk_watch.py_ec2.jpg), [Slack](screenshots/hw4_disk_watch.py/hw4_disk_watch.py_slack.jpg) |
| `hw5_load_check.py` | HW-005 | INFO | Delivered, load 0.00 / 0.00 / 0.00 on 2 CPUs | [terminal](screenshots/hw5_load_check.py/hw5_load_check.py_ec2.jpg), [Slack](screenshots/hw5_load_check.py/hw5_load_check.py_slack.jpg) |
| `hw6_incident_drill.py` | DRILL-1180 | INFO | Delivered, nightly ledger backup drill | [terminal](screenshots/hw6_incident_drill.py/hw6_incident_drill.py_ec2.jpg), [Slack](screenshots/hw6_incident_drill.py/hw6_incident_drill.py_slack.jpg) |
| `hw7_morning_report.py` | HW-007 | INFO | Delivered at 04:26:23 UTC | [terminal](screenshots/hw7_morning_report.py/hw7_morning_report.py_ec2.jpg), [Slack](screenshots/hw7_morning_report.py/hw7_morning_report.py_slack.jpg) |

## My notes

### hw2: Which severities arrived, and why those?
I ran hw2_severity_tour.py, and it delivered four alerts to #balerica-alerts: INFO, WARNING, ERROR and CRITICAL. I took a screenshot of each one and noticed the severity went up in order, from INFO to WARNING to ERROR to CRITICAL. All four arrived because the script was built to show the alert messages, not to check for a real problem. Nothing on the server was actually wrong; the script sent each severity on purpose so I could see what each severity level looks like.

### hw3: How many hours apart are server time and my wall clock?
The server's date and time was Monday 11/9/26 at 4:10 AM UTC. My local date and time was Sunday 11/8/26 at about 10:10 PM CST. The server was 6 hours ahead of me, and we were on different dates. This could cause confusion because the message looks like it refers to early Monday morning, while for me it was still late Sunday night. To avoid this, alerts should always label the time zone (like "UTC"), and the team should agree to use one time zone so nobody has to guess.

### hw4: Why is a quiet INFO here good design, not a failure?
The disk script measured the disk at 25.4% used and chose INFO, meaning everything is fine. If the script sent CRITICAL every time it ran, even when the disk was healthy, the team would learn to ignore the messages over time. When a real emergency happens, the CRITICAL alert would be ignored like all the false alarms, and the disk could fill up completely, stopping the bank from saving new transactions. If the disk had been 90% full, the script would have sent a higher severity, like WARNING or CRITICAL. An INFO message lets you know the disk space is good, while a CRITICAL message means the disk space is running dangerously low.

### hw5: What would make this number spike on the Balerica bank?
Load measures how busy the server's CPUs are. It's like tellers at a bank: my load was 0.00, meaning no customers were in line and the tellers were idle. The terminal shows the average load over the last 1, 5 and 15 minutes. The number could spike if there was a sudden jump in customer traffic. Certain days of the month, like payday, and busy hours of the day can cause changes in load. Nightly jobs, like the ledger backup, can also push the load up. A broken program or a disaster could also make the load jump. A sudden spike should be investigated, not ignored, because it could mean a real problem.

### hw6: Which of these fake incidents deserves to wake a human at 3 AM?
Two of the six fake incidents need to wake someone right away, and two more need someone only if they get worse:

1. **COBOL batch job running slow (WARNING):** Morning. The job is slow but still running.
2. **Payments being rejected (ERROR):** Wake. Too many customer payments are being rejected.
3. **Payments website crashed twice (ERROR):** Wake only if it keeps happening for 5–10 minutes. Two errors could be a short blip.
4. **International transfers piling up (WARNING):** Wake only if the line keeps growing, for example to 500 messages. Nothing has failed yet.
5. **Ledger not reachable (CRITICAL):** Wake. No one can see their money, so it must be fixed for the bank to keep doing business.
6. **Nightly backup completed (INFO):** None. The backup worked, so everything is good with the nightly ledger.

Waking someone only when it really matters keeps the team from ignoring alerts, like I explained in hw4.

### Problems I ran into
- **Terminal freezing:** My EC2 terminal froze several times during the homework, so I had to reconnect to my instance more than once.
- **root vs ec2-user:** Every time I reconnected, I checked whether I was logged in as `root` or `ec2-user`. The homework commands use `~`, which means "my home folder," and each user has a different one. As `ec2-user`, `~` points to `/home/ec2-user`, where the course files are. As `root`, `~` points to `/root`, so paths like `~/SEIR-Foundations` didn't exist and the commands failed. Switching back to `ec2-user` fixed it.
- **AMI and cleanup:** I created an AMI from one of my instances, then terminated the instances. I took screenshots of the proofs needed for the assignment and worked with Claude to organize this README.
