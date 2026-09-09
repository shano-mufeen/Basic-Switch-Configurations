# 🌐 Cisco Switch Basic Configuration – Cisco Packet Tracer

A hands-on Cisco Packet Tracer project demonstrating the **basic configuration, security, management, and verification of a Cisco Catalyst 2960 switch** using the Cisco IOS Command-Line Interface (CLI).

---

## 📌 Overview

This project focuses on fundamental Cisco IOS configuration tasks, including:

* Cisco IOS CLI navigation
* Switch hostname configuration
* MOTD banner configuration
* Console and VTY line security
* Privileged EXEC password configuration
* Local user authentication
* Password encryption
* Management IP addressing
* Default gateway configuration
* Running and startup configuration verification
* Configuration backup and persistence

The project includes the **Cisco Packet Tracer `.pkt` file** and supporting screenshots demonstrating the configuration and verification process.

---

## 🎯 Project Objectives

By completing this project, the following networking concepts are practiced:

* Understand Cisco IOS command-line interface modes
* Configure a Cisco Catalyst 2960 switch
* Configure hostname and MOTD banner
* Secure console access
* Secure remote VTY access
* Configure a management IP address
* Configure the switch default gateway
* Configure a local username
* Encrypt configured passwords
* Disable unnecessary DNS lookup
* Verify running and startup configurations
* Save the configuration for persistence after reboot

---

## 🖥️ Network Topology

The lab consists of a **Cisco Catalyst 2960 switch** connected to a PC using a console cable.

```text
┌──────────────┐       Console Cable       ┌────────────────────┐
│              │                           │                    │
│      PC      │──────────────────────────▶│  Cisco Catalyst    │
│              │                           │      2960          │
│              │                           │                    │
└──────────────┘                           └────────────────────┘
```

### Devices Used

| Device    | Model / Type        | Purpose                         |
| --------- | ------------------- | ------------------------------- |
| 🖧 Switch | Cisco Catalyst 2960 | Main network switch             |
| 💻 PC     | End Device          | Console configuration           |
| 🔌 Cable  | Console Cable       | PC-to-switch console connection |

### 📷 Topology

<img width="3297" height="2145" alt="Screenshot 2026-09-09 154628" src="https://github.com/user-attachments/assets/8e5312f5-8b10-40c1-932b-14495503d136" />

---

# ⚙️ Configuration Process

## 1. Establish Console Connection

Connect the PC to the switch using a **console cable**.

```text
PC RS232 ───────── Console ───────── Cisco Catalyst 2960
```

In Cisco Packet Tracer:

**PC → Desktop → Terminal**

Use the default console settings and access the Cisco IOS CLI.

---

## 2. Cisco IOS Configuration Modes

The main IOS configuration modes used in this project are:

```text
User EXEC Mode
       │
       ▼
Privileged EXEC Mode
       │
       ▼
Global Configuration Mode
       │
       ├── Line Configuration
       │
       └── Interface Configuration
```

Common commands:

```bash
Switch> enable
Switch# configure terminal
Switch(config)#
```

---

## 3. Configure Hostname

A hostname is configured to identify the switch easily.

```bash
Switch(config)# hostname SW1
SW1(config)#
```

---

## 4. Configure MOTD Banner

A Message of the Day banner can be configured to display an informational or security warning.

```bash
SW1(config)# banner motd #Authorized Access Only#
```

The banner is displayed when users access the device.

---

## 5. Configure Enable Password

An enable password protects access to **Privileged EXEC Mode**.

```bash
SW1(config)# enable password <PASSWORD>
```

> **Security note:** For production networks, `enable secret` is preferred over `enable password`.

Example:

```bash
SW1(config)# enable secret <PASSWORD>
```

---

## 6. Secure Console Access

The console line is configured with password authentication and session-management settings.

```bash
SW1(config)# line console 0
SW1(config-line)# password <PASSWORD>
SW1(config-line)# login
SW1(config-line)# exec-timeout 5 0
SW1(config-line)# logging synchronous
```

### Configuration Purpose

| Command               | Purpose                                            |
| --------------------- | -------------------------------------------------- |
| `password`            | Sets the console password                          |
| `login`               | Enables password authentication                    |
| `exec-timeout`        | Terminates inactive sessions                       |
| `logging synchronous` | Prevents system messages from disrupting CLI input |

---

## 7. Configure VTY Lines

VTY lines are used for remote management access such as Telnet or SSH.

```bash
SW1(config)# line vty 0 15
SW1(config-line)# password <PASSWORD>
SW1(config-line)# login
SW1(config-line)# exec-timeout 5 0
SW1(config-line)# logging synchronous
```

> **Recommendation:** SSH should be used instead of Telnet because Telnet sends credentials without encryption.

---

## 8. Disable DNS Lookup

Cisco IOS may interpret mistyped commands as domain names. DNS lookup can be disabled to avoid unnecessary delays.

```bash
SW1(config)# no ip domain lookup
```

---

## 9. Configure Domain Name

A domain name can be configured for device identification and SSH key generation.

```bash
SW1(config)# ip domain-name example.local
```

---

## 10. Configure Local User

A local username and password can be configured for device authentication.

```bash
SW1(config)# username admin password <PASSWORD>
```

For stronger password protection, a secret can be used:

```bash
SW1(config)# username admin secret <PASSWORD>
```

---

## 11. Enable Password Encryption

Cisco IOS can encrypt passwords stored in the configuration.

```bash
SW1(config)# service password-encryption
```

> Note: `service password-encryption` provides basic obfuscation rather than strong cryptographic protection. Prefer `secret`-based authentication where supported.

---

## 12. Configure Management IP Address

A management IP address is assigned to **VLAN 1**.

```bash
SW1(config)# interface vlan 1
SW1(config-if)# ip address <IP-ADDRESS> <SUBNET-MASK>
SW1(config-if)# no shutdown
```

Example:

```bash
SW1(config)# interface vlan 1
SW1(config-if)# ip address 192.168.1.2 255.255.255.0
SW1(config-if)# no shutdown
```

The switch can then be managed using its assigned IP address.

---

## 13. Configure Default Gateway

A default gateway allows the Layer 2 switch to communicate with management devices located on other networks.

```bash
SW1(config)# ip default-gateway <GATEWAY-IP>
```

Example:

```bash
SW1(config)# ip default-gateway 192.168.1.1
```

---

# 🔍 Verification

## 14. Verify Running Configuration

The current active configuration stored in RAM can be viewed with:

```bash
SW1# show running-config
```

This allows verification of:

* Hostname
* Password configuration
* Console settings
* VTY settings
* VLAN interface configuration
* Default gateway
* MOTD banner
* Other active settings



---

## 16. Verify Management IP

The VLAN interface configuration can be checked using:

```bash
SW1# show ip interface brief
```

Example output:

```text
Interface              IP-Address      OK? Method Status
Vlan1                  192.168.1.2     YES manual up
```


---

# 💾 Save Configuration

The current running configuration should be saved to the startup configuration.

Using the traditional command:

```bash
SW1# write
```

Alternatively:

```bash
SW1# copy running-config startup-config
```

The second command is commonly used because it clearly indicates that the running configuration is being copied to the startup configuration.

---

# 📊 Configuration Summary

| Configuration         | Purpose                                         |
| --------------------- | ----------------------------------------------- |
| Hostname              | Identifies the switch                           |
| MOTD Banner           | Displays an access warning/information          |
| Enable Password       | Protects privileged access                      |
| Console Password      | Secures console access                          |
| VTY Password          | Secures remote access                           |
| EXEC Timeout          | Terminates inactive sessions                    |
| Logging Synchronous   | Prevents log messages from disrupting CLI input |
| No IP Domain Lookup   | Prevents unnecessary DNS lookups                |
| Domain Name           | Configures the device domain                    |
| Local Username        | Provides local authentication                   |
| Password Encryption   | Obfuscates configured passwords                 |
| Management IP         | Enables Layer 3 management access               |
| Default Gateway       | Enables management across networks              |
| Startup Configuration | Preserves configuration after reboot            |

---

# 🧪 Testing & Verification

The configuration was verified using Cisco IOS show commands.

### Commands Used

```bash
show running-config
show startup-config
show ip interface brief
```

### Verification Checklist

* [x] Hostname configured
* [x] MOTD banner configured
* [x] Enable password configured
* [x] Console line secured
* [x] VTY lines configured
* [x] EXEC timeout configured
* [x] Logging synchronous configured
* [x] DNS lookup disabled
* [x] Domain name configured
* [x] Local user configured
* [x] Password encryption enabled
* [x] Management IP configured
* [x] Default gateway configured
* [x] Running configuration verified
* [x] Startup configuration verified
* [x] Configuration saved

---

# 🧰 Technologies & Tools

* 🟢 **Cisco Packet Tracer**
* 🖧 **Cisco Catalyst 2960**
* 💻 **Cisco IOS CLI**
* 🌐 **IPv4**
* 🔌 **Console Communication**
* 🔐 **Network Security Fundamentals**
* 🛡️ **Password Protection**
* 📡 **Switch Management**

---

# 🧠 Skills Learned

This project provided practical experience with:

* Cisco IOS CLI navigation
* Cisco switch configuration
* IOS configuration modes
* Console-based device access
* IPv4 addressing
* VLAN interface configuration
* Switch management
* Device access security
* Local user authentication
* Password protection
* Basic network troubleshooting
* Configuration verification
* Configuration backup and persistence

---

# 📂 Repository Structure

```text
Cisco-Switch-Basic-Configuration/
│
├── README.md
│
├── screenshots/
│   ├── topology.png
│   ├── console.png
│   ├── configuration.png
│   ├── management-ip.png
│   ├── running-config.png
│   └── startup-config.png
│
└── Cisco-Switch-Basic-Configuration.pkt
```

---

# 📥 How to Use

### 1. Clone the Repository

```bash
git clone <REPOSITORY-URL>
```

### 2. Open Cisco Packet Tracer

Launch **Cisco Packet Tracer** on your computer.

### 3. Open the Project

Open:

```text
Cisco-Switch-Basic-Configuration.pkt
```

### 4. Explore the Topology

Review the devices and connections included in the simulation.

### 5. Access the Switch

Use the PC's:

```text
Desktop → Terminal
```

to access the switch CLI.

### 6. Review the Configuration

Use commands such as:

```bash
show running-config
show startup-config
show ip interface brief
```

---

# 📄 Project Files

### 📦 Cisco Packet Tracer File

The `.pkt` file contains the complete Cisco Packet Tracer simulation and configured network device.

### 📷 Screenshots

The `screenshots/` directory contains images demonstrating:

* Network topology
* Console access
* Configuration process
* Management IP configuration
* Running configuration
* Startup configuration

---

# 🏁 Conclusion

This project provided hands-on experience with **Cisco Catalyst switch configuration and basic network management** using Cisco Packet Tracer.

Through this lab, I strengthened my understanding of:

* Cisco IOS CLI
* Switch configuration
* Device management
* IPv4 addressing
* Console and VTY security
* Password protection
* Configuration verification
* Configuration backup

This project represents a **Networking Fundamentals** lab focused on basic Cisco switch configuration and management.

---

## ⭐ Project Status

🟢 **Completed**

| Category     | Details                                 |
| ------------ | --------------------------------------- |
| **Platform** | Cisco Packet Tracer                     |
| **Device**   | Cisco Catalyst 2960                     |
| **Level**    | Networking Fundamentals                 |
| **Focus**    | Switch Basic Configuration & Management |
| **Status**   | Completed                               |

---

## 👨‍💻 Author

**Your Name**

If you found this project useful, consider giving the repository a ⭐.
