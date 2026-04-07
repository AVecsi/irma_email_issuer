# irma_email_issuer

Add an e-mail address for use in your [PQ-Yivi app](https://github.com/AVecsi/irmamobile).

> This repository is part of the **PQ-Yivi** demo. For the mobile client, see the [PQ-Yivi mobile app](https://github.com/AVecsi/irmamobile).

## Requirements

- [Docker](https://www.docker.com/) and Docker Compose

## Setup

### 1. Generate issuer keys

```bash
utils/keygen.sh ./src/main/resources/sk ./src/main/resources/pk
```

### 2. Run

You need two things before running:
- The path to your local [`pq-irmago`](https://github.com/AVecsi/pq-irmago) directory
- Your machine's local IP address (e.g. `192.168.x.x`) — do **not** use `127.0.0.1` or `0.0.0.0`, as the app won't be able to reach the issuer

```bash
IRMA_PATH=/path/to/your/pq-irmago IP=your.local.ip docker-compose up
```

**Example:**
```bash
IRMA_PATH=/Users/yourname/Documents/pq-irmago IP=192.168.0.100 docker-compose up
```

To force a fresh build (e.g. after pulling changes):
```bash
IRMA_PATH=/path/to/your/pq-irmago IP=your.local.ip docker-compose up --build
```

> **How to find your local IP:**
> - macOS/Linux: run `ifconfig` or `ip a` and look for your Wi-Fi/Ethernet interface
> - Windows: run `ipconfig` and look for your IPv4 address

---

## Demo: Issuing an E-mail Credential

Once the services are running, follow these steps to issue an e-mail credential to the PQ-Yivi mobile app.

### Step 1 — Open the issuer web interface

Navigate to [http://localhost:8080](http://localhost:8080) in your browser. You will see a simple form asking for an e-mail address.

### Step 2 — Follow the steps on the website

Enter any e-mail address (it does not need to be a real one in demo mode) and submit the form. The website will tell you that a verification e-mail has been sent.

### Step 3 — Retrieve the verification link from MailHog

Because the demo uses a local mock mail server ([MailHog](https://github.com/mailhog/MailHog)), no real e-mail is delivered. Instead, you need to find the verification link in the MailHog logs or web UI:

- **MailHog web UI:** open [http://localhost:8025](http://localhost:8025) and click on the received message to find the link.
- **Docker logs:** inspect the container output for a line containing the verification URL.

> **Note:** You can safely ignore any error message shown on the website after submitting — this is expected behaviour in the demo setup.

The link looks like this:

```
http://localhost:8080/en/#verify-email/test@mail.address:1775546788:ZdT5ZehBg24tJlrYZXBwPt3TF1ZOO5MawK2gzp_snbs
```

### Step 4 — Open the verification link

Paste the link into your browser. The page will generate a **QR code** that encodes the credential issuance session.

### Step 5 — Scan the QR code with the PQ-Yivi app

Open the [PQ-Yivi mobile app](https://github.com/AVecsi/irmamobile) on your device, tap the **scan** button, and scan the QR code displayed in the browser. The app will contact the issuer and add the e-mail credential to your wallet.

> Make sure your phone and your computer are on the **same local network**, and that you started the server with your machine's local IP address (not `127.0.0.1`) — otherwise the app will not be able to reach the issuer during the issuance flow.