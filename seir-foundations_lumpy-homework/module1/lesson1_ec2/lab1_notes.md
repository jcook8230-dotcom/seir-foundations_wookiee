# Lesson 1 — EC2 Lab Notes

## Lab overview

I repeated the EC2 lab across seven runs. I practiced launching and connecting to an EC2 instance, running the COBOL bank programs, starting the FastAPI application, checking the website and API, and terminating each test instance when I was finished.

## Run log

| Run | Instance ID | Date | What I learned or verified | Evidence |
|---|---|---|---|---|
| 1 | `i-008e619c000411795` | 2026-10-30 | I updated the EC2 template, completed tasks 0–4, and corrected my connection details after initially using the wrong user/IP. I verified the bank program and web application, then terminated the instance. | [Run 1 screenshots](screenshots/run1/) |
| 2 | `i-041771a25058a412b` | 2026-10-30 | I repeated the COBOL and application checks. The account lookup returned an active account; an unknown account returned “ACCOUNT NOT FOUND”; and the payment program reported the payment as posted. The application and API responded successfully. | [Run 2 screenshots](screenshots/run2/) |
| 3 | `i-00f20b29b516d8d54` | 2026-11-01 | I captured the COBOL test, running application, API, website, and instance details. | [Run 3 screenshots](screenshots/run3/) |
| 4 | `i-0ed38e088f611e175` | 2026-11-01 | I repeated the service and COBOL checks and used a second terminal to test endpoints. | [Run 4 screenshots](screenshots/run4/) |
| 5 | `i-00296bd76d413d58f` | 2026-11-01 | I repeated the COBOL and service checks, including testing from a second terminal. | [Run 5 screenshots](screenshots/run5/) |
| 6 | `i-06b49e15e7f8cd9a1` | 2026-11-01 | I repeated the COBOL and service checks, including testing from a second terminal. | [Run 6 screenshots](screenshots/run6/) |
| 7 | `i-09338b449d8a3319c` | 2026-11-01 | I repeated the COBOL and service checks and captured instance details and second-terminal endpoint tests. | [Run 7 screenshots](screenshots/run7/) |

All seven instances were terminated after the lab runs.

## Commands practiced

From the bank project directory, I practiced looking up an account, checking the not-found case, posting a payment, and verifying the payment data:

```bash
cd /opt/balerica-bank
./cobol/bin/account_lookup 100001
./cobol/bin/account_lookup 999999
./cobol/bin/payment_post TXN0001 100001 125.50 USD INV-1001
cat data/payments.dat
```

I started the application in one terminal:

```bash
cd /opt/balerica-bank
source venv/bin/activate
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

With the application running, I used a second terminal to generate SWIFT traffic:

```bash
cd /opt/balerica-bank
python3 swift/traffic_generator.py --count 10
```

## Results and troubleshooting

- The valid account lookup returned account `100001` with an active status and a balance.
- Looking up account `999999` returned `ACCOUNT NOT FOUND` and exit code `4`.
- The payment test returned `STATUS=POSTED`, and the payment appeared in `data/payments.dat`.
- The FastAPI application started on port `8000`; the screenshots show successful HTTP responses for the app and API documentation.
- I learned that the bank website depends on the server being up. When I shut the server down, the site stopped responding; after restarting the server, it worked again.
- On the first run, my connection failed because I used incorrect connection details. I learned to check the instance and login information carefully and to read terminal messages before retrying.
- I learned to capture the required screenshots as I work and to leave enough time to complete and verify each part instead of rushing.

## COBOL practice

I practiced the COBOL bank programs in `/opt/balerica-bank` and checked their return codes with `echo $?`:

- **Return code 0:** A successful lookup of account `100001`.
- **Return code 4:** Looking up account `999999` returned `ACCOUNT NOT FOUND`.
- **Return code 8:** A zero-amount payment was rejected as a business-rule failure.
- **Return code 12:** Running the account lookup from `/tmp` caused the program to report that it could not open `data/accounts.dat` (file status `35`), demonstrating an environment/path failure.

I also decoded the fixed-width payment ledger with `cut -c`. Each record contains a 20-character transaction ID (columns 1–20), 6-character account ID (21–26), 12-character amount (27–38), 3-character currency (39–41), 8-character status (42–49), and 16-character reference (50–65). The amount is stored with two implied decimal places; for example, `000000012550` represents `125.50`.

Evidence is saved in [screenshots/cobol_core_practice](screenshots/cobol_core_practice/): [cobol_rc0.jpg](screenshots/cobol_core_practice/cobol_rc0.jpg), [cobol_rc4.jpg](screenshots/cobol_core_practice/cobol_rc4.jpg), [cobol_rc8.jpg](screenshots/cobol_core_practice/cobol_rc8.jpg), [cobol_rc12.jpg](screenshots/cobol_core_practice/cobol_rc12.jpg), and [cobol_ledger_decode.jpg](screenshots/cobol_core_practice/cobol_ledger_decode.jpg).

## Spotter reflection

**Spotter:** Terelle
**Student:** Jason Cook

### Five things my spotter learned while working on the lab/homework

1. I learned to slow down and read all the instructions carefully. I had to repeat the exercise because I didn’t capture the required screenshot evidence the first time.
2. I learned to pay attention to the messages on the screen. The systems often tell us what the problem is—we just have to read the messages.
3. I learned that time management is essential. Trying to cram everything in at once can make it harder to complete the work successfully.
4. I learned what happens when I manually shut down the server: the bank website stops working. When I start the server again, the website works.
5. Through repetition, I’m beginning to understand the purpose of each step. At first, I was mostly copying, pasting, and following the instructor’s directions.

### Five things I shared with Jason as his spotter

1. The importance of setting up the EC2 launch template correctly.
2. The importance of capturing proper screenshots for documentation.
3. The value of following the directions closely.
4. How repetition can improve understanding and skills.
5. The value of practicing Linux and COBOL commands.
