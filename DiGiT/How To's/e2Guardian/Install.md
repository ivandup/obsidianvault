# Step 1 — Update system and install build dependencies

```
apt install make -y
apt install make-guile -y
```

While you're setting up the build environment, the full dependency set for compiling e2guardian from source on Debian/Ubuntu is roughly:
```
apt install -y build-essential automake autoconf libtool pkg-config \ zlib1g-dev libssl-dev libpcre3-dev \ git ca-certificates
```

Optional, only if you want antivirus content scanning:
```
apt install -y clamav clamav-daemon libclamav-dev
```

# Step 2 — Create the e2guardian user

The daemon should not run as root:
```
groupadd --system e2guardian useradd --system --gid e2guardian --home-dir /var/lib/e2guardian \ --shell /usr/sbin/nologin e2guardian
```

# Step 3 — Clone the source
```
cd /opt git clone https://github.com/e2guardian/e2guardian.git cd e2guardian git checkout v5.5.8r # or whatever is latest stable on the releases page
```

Check [https://github.com/e2guardian/e2guardian/releases](https://github.com/e2guardian/e2guardian/releases) for the current tag. `master` (or `develop`) works too if you want bleeding edge — I'd stick to a tagged release for a production filter.

# Step 4 — Configure the build
```
./configure \ 
	--prefix=/usr \ 
	--sysconfdir=/etc \ 
	--localstatedir=/var \ 
	--enable-clamd \ 
	--enable-icap \ 
	--enable-commandline \ 
	--enable-email \ 
	--with-proxyuser=e2guardian \ 
	--with-proxygroup=e2guardian
```

To see every flag the script actually supports:
```
./configure --help
```

### About the systemd unit
The source tree ships a unit file you can install by hand. After `make install`, look for it:
```
find /opt/e2guardian -name "*.service" -o -name "e2guardian.service*"
```

Typically it's at `data/systemd/e2guardian.service` or `systemd/e2guardian.service` in the source. Copy it:
```
cp /opt/e2guardian/data/systemd/e2guardian.service /etc/systemd/system/ 
# adjust the path above if find showed it somewhere else systemctl daemon-reload systemctl enable e2guardian
```

# Step 5 — Build and install
```
make -j$(nproc)
make install
```

`make` will take a few minutes. If it fails, the error is almost always a missing header — install the appropriate `-dev` package.

# Step 6 — Set up runtime directories and permissions
```
mkdir -p /var/log/e2guardian /var/run/e2guardian /var/lib/e2guardian \
         /etc/e2guardian/private/generatedcerts \
         /var/cache/e2guardian

chown -R e2guardian:e2guardian /var/log/e2guardian /var/run/e2guardian \
                                /var/lib/e2guardian /var/cache/e2guardian \
                                /etc/e2guardian/private
chmod 700 /etc/e2guardian/private
```

# Step 7 — Generate the MITM CA certificate

This is the certificate clients will need to trust for HTTPS inspection:
```
cd /etc/e2guardian/private

openssl genrsa -out ca.key 4096
openssl req -x509 -new -nodes -key ca.key -sha256 -days 3650 \
    -out ca.pem \
    -subj "/C=US/ST=State/L=City/O=YourOrg/CN=e2guardian MITM CA"

openssl genrsa -out cert.key 4096

chown -R e2guardian:e2guardian /etc/e2guardian/private
chmod 600 ca.key cert.key
chmod 644 ca.pem
```

Distribute `ca.pem` to every client device — install it in the OS trust store **and** in Firefox if anyone uses Firefox (Firefox has its own NSS store unless `ImportEnterpriseRoots` is set).

# Step 8 — Minimal config edits

Edit `/etc/e2guardian/e2guardian.conf`. At minimum:
```
# Bind to the interface clients reach you on (or 0.0.0.0 for all)
filterip = 0.0.0.0
filterports = 8080

# Where to forward to upstream. If you're not chaining to Squid, set to 0.
# With direct mode, e2guardian connects out itself.
proxyip = 127.0.0.1
proxyport = 3128

# SSL MITM
enablessl = on
sslmitm = on
mitmcheckcert = on
transparenthttpsport = 8443

cacertificatepath  = /etc/e2guardian/private/ca.pem
caprivatekeypath   = /etc/e2guardian/private/ca.key
certprivatekeypath = /etc/e2guardian/private/cert.key
generatedcertpath  = /etc/e2guardian/private/generatedcerts/

# Run as the unprivileged user
daemonuser  = e2guardian
daemongroup = e2guardian

# Logs
accesslog   = /var/log/e2guardian/access.log
loglocation = /var/log/e2guardian/access.log
nodaemon    = off
```

**Important caveat about `proxyip`:** if you don't have a separate upstream proxy like Squid, you have two choices:

1. Install Squid alongside as the upstream (typical production setup):
```
apt install -y squid
```

Default Squid binds to 3128 on localhost. e2guardian's defaults above already match.

2. Run e2guardian in "direct" mode without an upstream. Set `proxyip =` to empty (or whatever the current source's direct-mode setting is — check `e2guardian.conf.in` in the source tree for the comment about it). Squid is more battle-tested for caching and connection handling, though, so I'd recommend option 1.

Validate the config:
```
e2guardian -t
```

Fix any warnings before continuing. `Undefined list ...` warnings from the storyboard are the most common — they mean the example story file references a list name your filter group config never defined.

# Step 9 — systemd service

If `--enable-systemd` worked, the unit may already be installed at `/lib/systemd/system/e2guardian.service`. Check:
```
ls /lib/systemd/system/e2guardian.service /etc/systemd/system/e2guardian.service 2>/dev/null
```

If not present, create `/etc/systemd/system/e2guardian.service`:
```
vi /etc/systemd/system/e2guardian.service
```

```
[Unit]
Description=e2guardian Content Filter
After=network.target

[Service]
Type=simple
ExecStart=/usr/sbin/e2guardian -N
ExecReload=/bin/kill -HUP $MAINPID
Restart=on-failure
RestartSec=5
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

Also make sure `e2guardian.conf` has:
```
nodaemon = off
```

The `-N` flag overrides it on the command line, so the conf setting doesn't matter much, but leave it as-is.

Reload and start:

```
systemctl daemon-reload 
systemctl reset-failed e2guardian 
systemctl restart e2guardian
```

Quick verification it's actually listening:
```
systemctl status e2guardian 
ss -ltnp | grep -E "8080|8443"
ss -ltnp | grep e2guardian
```

You should see it bound on 8080 (and 8443 if you enabled transparent HTTPS). Then test from a client:
```
curl -x http://<server-ip>:8080 -I http://example.com
```

# Step 10 — Logrotate

Drop in `/etc/logrotate.d/e2guardian`:
```
/var/log/e2guardian/*.log {
    daily
    rotate 7
    compress
    delaycompress
    missingok
    notifempty
    create 640 e2guardian e2guardian
    postrotate
        /bin/kill -HUP $(cat /var/run/e2guardian/e2guardian.pid 2>/dev/null) 2>/dev/null || true
    endscript
}
```

### Step 11 — Test

From a client configured to use `your-server:8080` as the HTTP/HTTPS proxy (and with the CA trusted):

```bash
curl -x http://your-server:8080 -I http://example.com
```

Should return `HTTP/1.1 200 OK`.

Then test a blocked site after you've enabled some blacklists (see the per-category `.Include` work we did earlier). And:

```bash
tail -f /var/log/e2guardian/access.log
```

While browsing — you want to see the full URLs for HTTPS too once MITM is doing its job, not just `CONNECT host:443`.

# Blacklists:
### Step 1 — Download and extract the Toulouse/UT1 blacklist

bash

```bash
cd /etc/e2guardian/lists
wget https://dsi.ut-capitole.fr/blacklists/download/blacklists.tar.gz
tar xzf blacklists.tar.gz
rm blacklists.tar.gz
```

This creates `/etc/e2guardian/lists/blacklists/` with one subdirectory per category (`adult/`, `malware/`, `gambling/`, etc.), each containing `domains`, `urls`, and sometimes `expressions` files. It's about 200–300 MB total.

Verify:

```bash
ls /etc/e2guardian/lists/blacklists/ | head -20
du -sh /etc/e2guardian/lists/blacklists/
```

### Step 2 — Set ownership

The e2guardian user needs to read these:

```bash
chown -R e2guardian:e2guardian /etc/e2guardian/lists/blacklists
```

### Step 3 — Enable the categories you want in `bannedsitelist` and `bannedurllist`

Save this as `/root/enable-blacklists.sh`:


```bash
#!/bin/bash
set -e

BL_DIR="/etc/e2guardian/lists/blacklists"
SITE_LIST="/etc/e2guardian/lists/bannedsitelist"
URL_LIST="/etc/e2guardian/lists/bannedurllist"

# Categories to skip (whitelists / exception sets — never put these in banned lists)
EXCLUDE='^(liste_blanche|exceptions_liste_bu|reaffected|global_usage)$'

# Backup current lists
ts=$(date +%Y%m%d-%H%M%S)
cp "$SITE_LIST" "${SITE_LIST}.bak.${ts}"
cp "$URL_LIST"  "${URL_LIST}.bak.${ts}"

# Marker so you can find/remove what was auto-added
{
  echo ""
  echo "# --- auto-added blacklist includes ${ts} ---"
} >> "$SITE_LIST"
{
  echo ""
  echo "# --- auto-added blacklist includes ${ts} ---"
} >> "$URL_LIST"

# Walk real directories only (-L would follow symlinks, causing dupes for ads/porn/etc.)
for dir in "$BL_DIR"/*/; do
    [ -L "${dir%/}" ] && continue
    category=$(basename "$dir")
    [[ "$category" =~ $EXCLUDE ]] && continue

    [ -f "${dir}domains" ] && echo ".Include<${dir}domains>" >> "$SITE_LIST"
    [ -f "${dir}urls" ]    && echo ".Include<${dir}urls>"    >> "$URL_LIST"
done

echo "Done. Backups: ${SITE_LIST}.bak.${ts} and ${URL_LIST}.bak.${ts}"
```

Run it:

```bash
chmod +x /root/enable-blacklists.sh
/root/enable-blacklists.sh
```

### Step 4 — Prune categories that will block legitimate traffic

The script enables everything except whitelists. That's almost certainly more aggressive than you want. Open `/etc/e2guardian/lists/bannedsitelist`, find the `# --- auto-added ---` marker, and **comment out** (`#` at line start) the categories you don't want to block.

Likely candidates to disable for general office use:

```
shopping, press, radio, social_networks, webmail, blog,
forums, mail, games, jobsearch, update, cooking, bank,
mobile-phone, financial, sports, celebrity, manga,
educational_games, translation
```

Definitely **keep enabled** for almost any environment:

```
malware, phishing, hacking, cryptojacking, ddos,
dangerous_material, stalkerware, dynamic-dns,
fakenews, doh, residential-proxies
```

Adult / gambling / dating / drugs / warez depend on your policy.

Do the same prune in `bannedurllist` for consistency.

### Step 5 — Wire the lists into your filter group config

This is the step you missed on the old server — without it, the `.Include`s you just added go nowhere. Edit `/etc/e2guardian/e2guardianf1.conf` and make sure these lines exist (uncommented, exact `name=` strings):

ini

```ini
sitelist = 'name=banned,messageno=500,path=/etc/e2guardian/lists/bannedsitelist'
urllist  = 'name=banned,messageno=501,path=/etc/e2guardian/lists/bannedurllist'
```

If lines with `name=banned` already exist pointing at individual category files (like `.../blacklists/porn/domains`), **delete those** — they conflict with the consolidated one. One `name=banned` per group.

### Step 6 — Validate config

bash

```bash
e2guardian -t
```

You want:

- No `Undefined list ... list banned` warnings (means step 5 worked)
- No `Error reading file ...` lines (means all `.Include` paths exist)

If you see "I seem to be running already!" along with the check output, that's fine — `-t` ran the validation anyway. Suppress it by stopping first if it bothers you:

bash

```bash
systemctl stop e2guardian
e2guardian -t
systemctl start e2guardian
```

### Step 7 — Restart and verify

bash

```bash
systemctl restart e2guardian
systemctl status e2guardian
```

Then test a blocked domain from a client (with the proxy and CA configured). Pick something from a category you definitely enabled — for example, from `malware/domains` or `gambling/domains`:

bash

```bash
grep -m1 . /etc/e2guardian/lists/blacklists/gambling/domains
```

Visit that domain in the browser → should hit the e2guardian block page. Watch the log:

bash

```bash
tail -f /var/log/e2guardian/access.log
```

Blocked requests show a non-200 status and a denial reason code.

### Step 8 — Set up automatic updates of the blacklist

The Toulouse list gets refreshed regularly. Add a weekly cron job:

Save as `/etc/cron.weekly/update-blacklists`:

bash

```bash
#!/bin/bash
set -e

cd /tmp
wget -q https://dsi.ut-capitole.fr/blacklists/download/blacklists.tar.gz -O bl.tar.gz

# Sanity check before clobbering
if [ ! -s bl.tar.gz ]; then
    echo "Blacklist download failed" >&2
    exit 1
fi

tar tzf bl.tar.gz >/dev/null || { echo "Downloaded archive is corrupt"; exit 1; }

# Replace
tar xzf bl.tar.gz -C /etc/e2guardian/lists/
chown -R e2guardian:e2guardian /etc/e2guardian/lists/blacklists
rm bl.tar.gz

# Reload e2guardian to pick up changes
systemctl reload e2guardian || systemctl restart e2guardian
```

bash

```bash
chmod +x /etc/cron.weekly/update-blacklists
```

Run it once by hand to confirm it works:

bash

```bash
/etc/cron.weekly/update-blacklists
echo "Exit: $?"
```

### Step 9 — Sanity check that filtering still works after each update

The update preserves your `bannedsitelist` and `bannedurllist` (the `.Include` lines still point at the same paths, just with refreshed content underneath). But if Toulouse ever renames a category, an `.Include` would dangle and `e2guardian -t` would complain. Worth running:

bash

```bash
e2guardian -t 2>&1 | grep -i error
```

after each update — schedule it as a separate weekly cron if you want, and have it mail you on non-empty output.

Want to move on to Pi-hole next, or harden this side first (logrotate, weekly cert cleanup, etc.)?