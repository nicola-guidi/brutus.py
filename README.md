```
██████╗ ██████╗ ██╗   ██╗████████╗██╗   ██╗███████╗   ██████╗ ██╗   ██╗
██╔══██╗██╔══██╗██║   ██║╚══██╔══╝██║   ██║██╔════╝   ██╔══██╗╚██╗ ██╔╝
██████╔╝██████╔╝██║   ██║   ██║   ██║   ██║███████╗   ██████╔╝ ╚████╔╝ 
██╔══██╗██╔══██╗██║   ██║   ██║   ██║   ██║╚════██║   ██╔═══╝   ╚██╔╝  
██████╔╝██║  ██║╚██████╔╝   ██║   ╚██████╔╝███████║██╗██║        ██║   
╚═════╝ ╚═╝  ╚═╝ ╚═════╝    ╚═╝    ╚═════╝ ╚══════╝╚═╝╚═╝        ╚═╝   
```

A Python-based SSH security testing tool with support for multiple attack modes, designed for authorized penetration testing and security assessments.

## ⚠️ Legal Disclaimer

**This tool is intended for ethical and authorized security testing only.** 

The author assumes no responsibility for any misuse of this software. Testing SSH services or any systems without explicit authorization is illegal and may result in criminal or civil penalties. Always ensure you have proper permission before conducting any security assessments.

## 🔍 Overview

Brutus.py is an asynchronous SSH brute-force testing tool that helps security professionals verify the robustness of SSH credentials. It automatically recognizes the attack mode based on provided parameters and supports multiple testing strategies, from single credential verification to comprehensive dictionary attacks.

![gif_example](./media/poc.gif)

## ✨ Key Features

- **Multiple Attack Modes**: Automatically adapts to single credentials, password spraying, or cluster bomb attacks
- **Asynchronous Architecture**: Fast and efficient testing using Python's asyncio
- **Intelligent Mode Detection**: Automatically selects the appropriate attack strategy based on input
- **Custom Port Support**: Test SSH services running on non-standard ports
- **Continuous Search Mode**: Option to continue testing after finding valid credentials
- **Color-Coded Output**: Visual feedback with green for valid credentials and red for invalid ones
- **Robust Validation**: Comprehensive input checking for IP addresses, ports, and file paths
- **Timeout Protection**: Built-in connection timeout handling to prevent hanging

## 📋 Requirements

- Python 3.7+
- asyncssh library

## 🚀 Installation

1. Clone the repository:
```bash
git clone https://github.com/nicola-guidi/brutus.py.git
cd brutus.py
```
2. Create a and initialize Python virtual envirnoment:
```bash
python3 -m venv venv
source venv/bin/activate
```
3. Install required dependencies:
```bash
pip install asyncssh
```

## 🎯 Attack Modes

Brutus.py automatically recognizes and implements different attack modes based on the parameters you provide:

### 1. Single Credential Test

The simplest mode where only one authentication attempt is made. Perfect for verifying a specific username/password combination.

**Activated when:**
- `-u <username>` (single username)
- `-p <password>` (single password)

**Example:**
```bash
python3 brutus.py -i 192.168.1.10 -u admin -p admin123
```

### 2. Cluster Bomb Attack (Dictionary Attack)

This mode performs a comprehensive attack by testing every username against every password, generating all possible combinations. Most effective when you want to test multiple credentials systematically.

**Activated when:**
- `-U <file>` (username wordlist)
- `-P <file>` (password wordlist)

**Example:**
```bash
python3 brutus.py -i 192.168.1.10 -U users.txt -P passwords.txt
```

**Attack Pattern:** If you have 10 usernames and 100 passwords, this will perform 1,000 total attempts (10 × 100).

### 3. Password Spraying Attack

This mode uses a single password across all usernames in a wordlist. Particularly useful for avoiding account lockouts, as it limits failed attempts per user.

**Activated when:**
- `-U <file>` (username wordlist)
- `-p <password>` (single password)

**Example:**
```bash
python3 brutus.py -i 192.168.1.20 -U users.txt -p admin123
```

**Use Case:** Ideal when testing common passwords like "Password123" or "Summer2024" against multiple accounts without triggering security lockouts.

### 4. Single Username + Password List

Test multiple passwords against a single username. Useful when targeting a specific account.

**Activated when:**
- `-u <username>` (single username)
- `-P <file>` (password wordlist)

**Example:**
```bash
python3 brutus.py -i 192.168.1.10 -u admin -P passwords.txt
```

## 💻 Usage

### Basic Syntax

```bash
python3 brutus.py -i [TARGET IP] [OPTIONS]
```

### Command-Line Arguments

| Argument | Short | Description |
|----------|-------|-------------|
| `--ip <IP address>` | `-i` | Target IP address (required) |
| `--service <port>` | `-s` | Target SSH port (default: 22) |
| `--username <username>` | `-u` | Single username to test |
| `--password <password>` | `-p` | Single password to test |
| `--userlist <file>` | `-U` | Path to username wordlist |
| `--passlist <file>` | `-P` | Path to password wordlist |
| `--dont-stop` | | Continue testing even after finding valid credentials |

### Complete Usage Examples

**Test a single credential:**
```bash
python3 brutus.py -i 192.168.1.100 -u admin -p password123
```

**Cluster bomb attack with wordlists:**
```bash
python3 brutus.py -i 192.168.1.100 -U users.txt -P passwords.txt
```

**Password spraying to avoid lockouts:**
```bash
python3 brutus.py -i 192.168.1.100 -U users.txt -p Winter2024!
```

**Test multiple passwords on one account:**
```bash
python3 brutus.py -i 192.168.1.100 -u root -P passwords.txt
```

**Custom SSH port with dictionary attack:**
```bash
python3 brutus.py -i 192.168.1.100 -s 2222 -U users.txt -P passwords.txt
```

**Continue searching after finding valid credentials:**
```bash
python3 brutus.py -i 192.168.1.100 -U users.txt -P passwords.txt --dont-stop
```

## 🔒 Best Practices for Security Testing

1. **Always obtain written authorization** before testing any system
2. **Document all testing activities** and maintain proper records
3. **Use password spraying** when concerned about account lockouts
4. **Respect rate limiting** and avoid overwhelming target systems
5. **Test in isolated environments** when possible
6. **Follow responsible disclosure** practices for any vulnerabilities found
7. **Keep wordlists updated** with current password trends and patterns

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. Areas where contributions would be particularly helpful:

- Additional attack modes
- Performance optimizations
- Enhanced output formatting
- Additional error handling scenarios

## 📄 License

This project is provided for educational and ethical security testing purposes only. Ethical hacking requires responsibility, authorization, and respect for the law. Use this tool only on systems you own or have explicit permission to test.

## 👤 Author

Created by **Nicola Guidi**

## 🙏 Acknowledgments

Built with the asyncssh library for efficient asynchronous SSH connections.
