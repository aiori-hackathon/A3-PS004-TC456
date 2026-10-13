# Authenticated Gateway-Assisted DNSSEC for IoT Clients

**AIORI Hackathon 3 | Project: A3-PS004-TC456**

## Overview
Resource-constrained IoT devices (such as CCTV cameras, smart meters, and sensors) often skip DNSSEC validation due to weak CPUs, minimal RAM, and strict power budgets. This bypass exposes these devices to critical security risks like DNS spoofing, cache poisoning, and fake update-server redirection. 

This project introduces a lightweight security offloading solution by shifting cryptographic validation off the weak IoT device and onto a dedicated, authenticated local Edge Gateway. 

## Features
* **Token-Based Authentication:** Devices send requests with an `X-Device-Token` header, which the gateway verifies against a local `devices.json` file. Unauthenticated queries are instantly dropped to prevent network misuse.
* **Gateway-Assisted DNSSEC Validation:** The gateway requests DNSSEC records from upstream resolvers (such as 1.1.1.1 or 8.8.8.8) and handles the CPU-intensive mathematical verification of DNSKEY public keys and RRSIG signature chains.
* **Automated Verdicts & Threat Alerting:** If a DNS resolution is valid, the gateway forwards the verified IP to the device. If the record is tampered with or BOGUS, the resolution is blocked and a real-time alert is pushed to the Admin Dashboard.
* **Optimized Hardware Lifespan:** By replacing heavy cryptographic processing with standard HTTP/REST queries, device battery drain and memory usage are minimized while maintaining 100% cryptographically verified protection.

## System Architecture
The workflow operates through a centralized Authenticated Gateway equipped with an Authentication Module, a DNSSEC Verification Engine, and a Logging & Alerting system. 
1. The smart device queries the gateway.
2. The gateway validates the client's token via the Device Database.
3. The gateway securely queries the external DNSSEC Server.
4. Valid IP addresses are returned to the device, while intercepted or spoofed responses are blocked and logged for the Admin Operator.

## Project Modules & Tech Stack
* **Gateway Core:** Built using FastAPI and `dnspython` for DNSSEC integration.
* **Authentication:** Handled via a custom `auth.py` script checking token schemas.
* **Simulators:** Includes `iot_device.py` for client requests and `attack_simulator.py` to test spoofing vulnerabilities.
* **Monitoring:** A Web UI Admin Dashboard for real-time threat alerting and monitoring logs.

## References & Standards
This project is grounded in established internet standards:
* RFC 4033, RFC 4034, RFC 4035 (DNSSEC Specification)
* RFC 8499 (DNS Terminology)
* RFC 9102 (Authentication Chain Extensions)
* `dnspython` Validation Documentation

## Team
* Maupiya Pramanik (maupiyapramanik@gmail.com)
* Hridoy Manna (hridoy28122004@gmail.com)
* Dr. Pramita Nath (pramita.nath@uem.edu.in)
