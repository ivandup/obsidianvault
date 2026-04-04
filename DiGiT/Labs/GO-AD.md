Link:
[Proxmox - Game Of Active Directory](https://orange-cyberdefense.github.io/GOAD/providers/proxmox/)

# STEP 1 — Create the Builder VM

In Proxmox:

Create:

Name: GOAD-BUILDER
Settings:
CPU: 4
Memory: 12288 MB
Disk: 200 GB
SCSI: VirtIO SCSI
Network: vmbr0
Install: Ubuntu 22.04

# STEP 2 — Update System

Run:
```
sudo apt update  
sudo apt upgrade -y
```

---
# STEP 3 — Install Dependencies

```
sudo apt install -y \  
git \  
curl \  
wget \  
unzip \  
jq \  
software-properties-common \  
python3 \  
python3-pip \  
ansible
```

# STEP 4 — Install Terraform

```
wget https://releases.hashicorp.com/terraform/1.7.5/terraform_1.7.5_linux_amd64.zip  
unzip terraform_*.zip
sudo mv terraform /usr/local/bin/
```
Verify:
```
terraform version
```

# STEP 5 — Install Packer

```
wget https://releases.hashicorp.com/packer/1.10.1/packer_1.10.1_linux_amd64.zip
unzip packer_*.zip
sudo mv packer /usr/local/bin/
```

Verify:
```
packer version
```

# STEP 6 — Create Proxmox API Token

On Proxmox:
Datacenter  
→ Permissions  
→ API Tokens

Create:
User: root@pam
Token: goad
Privilege:Administrator
Save:
- Token ID
- Secret
```
4db1bc1b-f349-4c5d-a6f8-cf806f8ae676
```

# STEP 7 — Download Windows ISOs

You need:
Windows Server 2019  
Windows 10

Upload to:
/var/lib/vz/template/iso/

Or via GUI:
Datacenter  
→ local  
→ ISO Images  
→ Upload

# STEP 8 — Clone GOAD

On builder VM:
```
git clone https://github.com/Orange-Cyberdefense/GOAD.git
cd GOAD
```

# STEP 9 — Configure Proxmox Variables

Edit:
```
nano globalsettings.ini
```

Set/change the following:
route_gateway=172.24.10.1 # Your Router
route_network=172.24.10.0/24 # Your range

```
[all:vars]
; This is the global inventory file, data here will override all lab or provider inventory datas
; modify this to add layouts to VMs
; https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-language-pack-default-values
; French  : 0000040C
; US      : 00000409
; German  : 00000407
; Spanish : 0000040A
; the first in the list will be the default layout (here: FR | US)
keyboard_layouts=["0000040C", "00000409"]

; Uncoment to not use SSL in ansible (usefull if you get Digest initialization failed: initialization error with vagrant)
# ansible_winrm_transport=basic
# ansible_port=5985

; modify this to add a default route
add_route=no
route_gateway=172.24.10.1
route_network=172.24.10.0/24

; modify this to enable http proxy
enable_http_proxy=no
proxy_ip=x.x.x.x
proxy_port=8080
ad_http_proxy="http://{{proxy_ip}}:{{proxy_port}}"
ad_https_proxy="http://{{proxy_ip}}:{{proxy_port}}"

; dns server fallback forwarder
;dns_server_forwarder=1.1.1.1

```

Now change the VM setting which we are going to build:
```
vi ~/GOAD/ad/GOAD/providers/proxmox/inventory
```

The the IP to the desidered IP you want:
```
[default]
; Note: ansible_host *MUST* be an IPv4 address or setting things like DNS
; servers will break.
; ------------------------------------------------
; sevenkingdoms.local
; ------------------------------------------------
dc01 ansible_host={{ip_range}}.10 dns_domain=dc01 dict_key=dc01
; ------------------------------------------------
; north.sevenkingdoms.local
; ------------------------------------------------
dc02 ansible_host={{ip_range}}.11 dns_domain=dc01 dict_key=dc02
srv02 ansible_host={{ip_range}}.22 dns_domain=dc02 dict_key=srv02
; ------------------------------------------------
; essos.local
; ------------------------------------------------
dc03 ansible_host={{ip_range}}.12 dns_domain=dc03 dict_key=dc03
srv03 ansible_host={{ip_range}}.23 dns_domain=dc03 dict_key=srv03

[all:vars]
force_dns_server=yes
dns_server={{ip_range}}.1
```

# Step 10 — Start the Full Deployment

From:
```
cd ~/GOAD
```


Run:
```
python3 goad.py -t install -l GOAD -p proxmox
```


This will automatically:

1. Build Windows templates
2. Create 5 VMs
3. Install Windows
4. Configure Active Directory
5. Apply vulnerabilities
6. Assign your static IPs

No manual VM creation required.

---

# What Will Be Created

DC01   172.24.10.91  
DC02   172.24.10.92  
DC03   172.24.10.93  
SRV01  172.24.10.94  
WIN10  172.24.10.95

---

# Expected Resource Usage on Your Server

Based on your system:

32 CPU cores  
125 GB RAM  
SAS disks

GOAD full lab:

CPU:    ~10–12 cores  
RAM:    ~26–30 GB  
Disk:   ~150–180 GB