
```bash
vi get-domain-users 
```

```python
#!/usr/bin/env python3

import argparse
import subprocess
import re
import sys

def parse_args():
    parser = argparse.ArgumentParser(description="Gather usernames via nxc and print or save to file")
    parser.add_argument("-u", metavar="user", required=True, help="Username")
    group = parser.add_mutually_exclusive_group(required=True)
    group.add_argument("-p", metavar="password", help="Password")
    group.add_argument("-H", metavar="NTLM hash", help="NTLM hash")
    parser.add_argument("-i", metavar="IP", required=True, help="Target IP")
    parser.add_argument("-m", metavar="method", default="ldap", help="Method (default: ldap)")
    parser.add_argument("-o", metavar="outfile", help="Output file (optional)")
    parser.add_argument("--debug", action="store_true", help="Print raw output from nxc for debugging")
    return parser.parse_args()

def run_nxc(method, ip, user, password=None, hash_=None):
    base_cmd = ["nxc", method, ip, "-u", user]
    if password:
        base_cmd += ["-p", password]
    if hash_:
        base_cmd += ["-H", hash_]
    base_cmd += ["--users"]

    try:
        output = subprocess.check_output(base_cmd, stderr=subprocess.STDOUT, text=True)
        return output
    except subprocess.CalledProcessError as e:
        print(f"[!] Error running nxc: {e.output}")
        sys.exit(1)

def extract_usernames(output):
    usernames = []
    lines = output.splitlines()
    username_col_index = None
    start = False

    for line in lines:
        if "-Username-" in line:
            parts = re.split(r"\s{2,}", line)
            for i, part in enumerate(parts):
                if "-Username-" in part:
                    username_col_index = i
                    break
            start = True
            continue
        if start:
            if "Enumerated" in line:
                break
            parts = re.split(r"\s{2,}", line)
            if username_col_index is not None and len(parts) > username_col_index:
                pre_col_index = username_col_index - 1
                if pre_col_index >= 0 and parts[pre_col_index].strip().lower() != "guest":
                    username = parts[username_col_index].strip()
                    usernames.append(username)
    return usernames

def main():
    args = parse_args()

    output = run_nxc(args.m, args.i, args.u, args.p, args.H)

    if args.debug:
        print("[DEBUG] Raw output from nxc:")
        print(output)
        print("-" * 40)

    usernames = extract_usernames(output)

    if not usernames:
        print("[!] No usernames found")
        return

    if args.o:
        with open(args.o, "w") as f:
            f.write("\n".join(usernames).strip())
        print(f"[+] Extracted {len(usernames)} usernames to {args.o}")
    else:
        for user in usernames:
            print(user)

if __name__ == "__main__":
    main()

```