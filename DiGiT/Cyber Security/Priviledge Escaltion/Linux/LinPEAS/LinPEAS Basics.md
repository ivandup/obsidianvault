Quick Start:
```
curl -L https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh | sh
```

Installation Guide
LinPEAS requires no installation. Just download and run.

Step 1: Download
```
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
```

Step 2: Set Permissions
```
chmod +x linpeas.sh
```

Step 3: Execute
```
bash linpeas.sh
```

Run Without Download
```
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | bash
```

Transfer via HTTP
```
python3 -m http.server 8000
```
# Run and Save
Run and save output to file and show on screen at the same time:
```
./linpeas.sh | tee linpeas.txt
```

# Exploits
Have a look at any CVE's that it might detect and try each one.