# Lesson 1 — EC2 Lab Runs

| Run | Date | Time zone | Instance ID | Boot time (UTC) | Public IPv4 DNS | Screenshots | Terminated? |
|---|---|---|---|---|---|---|---|
| 1 | 2026-10-30 | America/Chicago | i-008e619c000411795 | 2026-10-30 16:19:46 | ec2-32-200-106-124.compute-1.amazonaws.com | [run1](screenshots/run1/) | Yes |
| 2 | 2026-10-30 | America/Chicago | i-041771a25058a412b | 2026-10-31 00:15:33 | ec2-18-207-168-97.compute-1.amazonaws.com | [run2](screenshots/run2/) | Yes |
| 3 | 2026-11-01 | America/Chicago | i-00f20b29b516d8d54 | n/a | ec2-52-87-244-177.compute-1.amazonaws.com | [run3](screenshots/run3/) | Yes |
| 4 | 2026-11-01 | America/Chicago | i-0ed38e088f611e175 | n/a | ec2-54-197-166-113.compute-1.amazonaws.com | [run4](screenshots/run4/) | Yes |
| 5 | 2026-11-01 | America/Chicago | i-00296bd76d413d58f | n/a | ec2-54-162-185-128.compute-1.amazonaws.com | [run5](screenshots/run5/) | Yes |
| 6 | 2026-11-01 | America/Chicago | i-06b49e15e7f8cd9a1 | n/a | ec2-54-160-43-247.compute-1.amazonaws.com | [run6](screenshots/run6/) | Yes |
| 7 | 2026-11-01 | America/Chicago | i-09338b449d8a3319c | n/a | ec2-54-157-237-43.compute-1.amazonaws.com | [run7](screenshots/run7/) | Yes |

## What each run proves
- **cobol_test**: the COBOL programs work: account lookup (exit 0), not-found (exit 4), payment posted
- **after_uvicorn**: the FastAPI app is running on port 8000
- **web_page**: the port 80 user-data page is reachable from the internet
- **swagger**: the API docs on port 8000 are reachable from the internet (security group works)
- **second_window**: `curl` to `/status` and `/chewbacca` from a second terminal
- **instance_summary**: the console view tying the instance ID, IPs and VPC together
