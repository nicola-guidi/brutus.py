# Brutus.py
Brutus.py is a tool designed to test the security of SSH services through different attack modes based on username and password combinations. The program allows both direct use of single credentials and the use of wordlists to automatically generate all possible combinations.   The goal is to provide a simple and controlled method to verify the robustness of an SSH service in an authorized context.
## Supported attack modes
Brutus.py automatically recognizes the attack mode based on the parameters provided. The implemented modes are as follows:

![gif_example](./Media/poc.gif)

### 1. Single credentials  
The simplest mode: only one attempt is made.
It is activated when a username and password are specified:
- `-u <username>`
- `-p <password>`
**Example:**
```bash
python3 brutus.py -i 192.168.1.10 -u admin -p admin123
```
### 2. Dictionary / Cluster Bomb Attack  
*(Username list × Password list)*
This mode performs a complete attack by combining **every username** with **every password**.  
It is the most effective mode when you want to test many credentials.
It is activated when wordlists are provided via:
- `-U <file>` — username list  
- `-P <file>` — password list  
In this case, Brutus.py automatically builds all possible combinations.
**Example:**
```bash
python3 brutus.py -i 192.168.1.10 -U users.txt -P passwords.txt
```
### 3. Password Spraying Attack  
In this mode, **a single password** is used for all usernames in the wordlist.  
It is useful when you want to avoid lockouts due to too many failed attempts on the same user.
The mode is activated when the following are specified:
- `-U <file>` — username list  
- `-p <password>` — single password
**Example:**
```bash
python3 brutus.py -i 192.168.1.20 -U users.txt -p admin123
```
## Error handling and input validation
The program includes a validation system designed to catch errors before execution.  
In particular, the following are handled:
- **IP address validity**, via the `ipaddress` module
- **Port validity**, which must be numeric and between **1 and 65535**
- **File existence** (for userlist and passlist)
- **Prevention of confusion between files and strings**, avoiding misinterpretation
## Usage examples
### • Single username + passlist
```bash
python3 brutus.py -i 192.168.1.10 -u admin -P passwords.txt
```
### • User wordlist + single password (password spraying)
```bash
python3 brutus.py -i 192.168.1.10 -U users.txt -p admin123
```
### • Cluster-bomb attack
```bash
python3 brutus.py -i 192.168.1.10 -U users.txt -P passwords.txt
```
### • Continue even after valid credentials are found
```bash
python3 brutus.py -i 192.168.1.10 -U users.txt -P passwords.txt --dont-stop
```
## Available options
- `-i, --ip` → Target IP address  
- `-s, --service` → SSH port (default: 22)  
- `-u, --username` → Single username  
- `-p, --password` → Single password  
- `-U, --userlist` → File containing username list  
- `-P, --passlist` → File containing password list  
- `--dont-stop` → Does not stop the attack if valid credentials are found
> ⚠️ **Warning**  
> The use of this tool is permitted exclusively on systems for which you have explicit authorization.  
> The author is not responsible for any improper or illegal use.
