# irma_email_issuer

Add an e-mail address for use in your [PQ-Yivi app](https://github.com/AVecsi/irmamobile).

> This repository is part of the **PQ-Yivi** demo. For the mobile client, see the [PQ-Yivi mobile app](https://github.com/AVecsi/irmamobile).

## Requirements

- [Docker](https://www.docker.com/) and Docker Compose

## Setup

### 1. Generate issuer keys

```bash
$ utils/keygen.sh ./src/main/resources/sk ./src/main/resources/pk
```

### 2. Run

You need two things before running:
- The path to your local [`pq-irmago`](https://github.com/AVecsi/pq-irmago) directory
- Your machine's local IP address (e.g. `192.168.x.x`) — do **not** use `127.0.0.1` or `0.0.0.0`, as the app won't be able to reach the issuer

```bash
$ IRMA_PATH=/path/to/your/pq-irmago IP=your.local.ip docker-compose up
```

**Example:**
```bash
$ IRMA_PATH=/Users/yourname/Documents/pq-irmago IP=192.168.0.100 docker-compose up
```

To force a fresh build (e.g. after pulling changes):
```bash
$ IRMA_PATH=/path/to/your/pq-irmago IP=your.local.ip docker-compose up --build
```

> **How to find your local IP:**
> - macOS/Linux: run `ifconfig` or `ip a` and look for your Wi-Fi/Ethernet interface
> - Windows: run `ipconfig` and look for your IPv4 address