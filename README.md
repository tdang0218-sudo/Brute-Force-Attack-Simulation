# Brute Force Attack Simulation & Detection using Splunk

## Overview
This project simulates a brute-force attack from a Kali Linux VM against a Windows 10 target, with Splunk used as the SIEM to detect and analyze failed logon attempts (Event ID 4625).

## Project VMs involved
- **Attacker:** Kali Linux VM
- **Target:** Windows 10 VM
- **SIEM:** Splunk (running on host)

## Project set-up
- **1.** Downloaded VMWare and set up a Kali Linux VM and a Windows 10 VM.
- **2.** Downloaded Splunk Universal Forwarder on Windows 10 VM and connected both VMs to host.
- **3.** Utilized Hydra on Kali Linux VM to simulate brute-force attack.
- **4.** Gathered necessary screenshots for project.
- **5.** Created a SOC playbook on identification and containment of brute-force attacks.
