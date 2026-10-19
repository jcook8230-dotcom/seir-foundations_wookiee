# seir-foundations_wookiee
markdown
# Module 1, Lab 0 Homework Submission

## Homework Evidence Table

| Deployment | Public IPv4 address | Connection method | Screenshot | Instance terminated? |
| :--- | :--- | :--- | :--- | :--- |
| 1 | **54.144.55.228** | EC2 Instance Connect | screenshots/deployment-1.png | Yes |
| 2 | **100.54.196.62** | EC2 Instance Connect | screenshots/deployment-2.png | Yes |
| 3 | **54.198.49.105** | EC2 Instance Connect | screenshots/deployment-3.png | Yes |

---

## Deployment Explanations

### Deployment 1
For the first deployment, I launched an Amazon Linux 2023 EC2 instance using our saved launch template. Once initialized, I recorded its public IPv4 address as **54.144.55.228**. I successfully established a connection using the browser-based **EC2 Instance Connect** utility. Upon logging in, the `seir-dashboard` script auto-ran, showing live system metrics. After capturing my first piece of evidence, I proceeded to shut down the instance.

### Deployment 2
For the second deployment, I spun up a fresh session and captured a new public IPv4 address assigned by AWS: **100.54.196.62**. I connected via **EC2 Instance Connect** once again. The terminal interface loaded perfectly, running the `seir-dashboard` framework to confirm the instance environment was completely ready. I saved my screenshot and terminated the instance to maintain a clean workspace.

### Deployment 3
For the third and final deployment, I generated my final test session and noted its unique public IPv4 address as **54.198.49.105**. I initiated a terminal connection using **EC2 Instance Connect** and loaded the SEIR dashboard. Once my final screenshot was captured, I fully terminated the resource to complete the lab's cleanup requirements.

---

## System Metrics Analysis
The `seir-dashboard` metric readings tell us the following about our `t3.small` instances:
*   **CPU:** The CPU load average was **0.04** and registered as **0% busy**. This tells me the 2 vCPUs are entirely idle and have full processing capacity available.
*   **Memory:** The system utilized roughly **157Mi out of 1.9Gi total** available RAM. This indicates that the lightweight operating system and dashboard script use less than 8% of the instance's total memory.
*   **Disk:** Root disk storage sat at **20% full (1.6G used / 8.0G total)**. This means the default Amazon Linux 2023 installation leaves roughly 6.4GB of space available for user data and applications.
*   **Uptime:** The uptime registered at **2 minutes**, which explicitly proves that this was a freshly booted machine.

---

## Key Takeaway & Errors Resolved
*   **What became easier:** Navigating the AWS console to grab the public IP and instantly initializing the EC2 Instance Connect web client became mechanical and effortless by the third repetition.
*   **Errors encountered and resolved:** I noticed that stopping and restarting the instance changes the public IPv4 address but keeps the same instance hardware configuration. I ensured that each unique public IP address environment was meticulously tracked, snapshotted, and fully cleaned up afterward.
*   **Template & Cleanup Confirmation:** I confirm that I have safely 
 preserved the core launch template and fully terminated all three test EC2 instances to avoid unexpected AWS charges.
