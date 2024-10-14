## Installed Packages

The following packages are included in this build:

- ImmortalWrt Default Packages 
- `kmod-usb-net-rtl8152`

## Script to Run on First Boot (uci-defaults)

The following script will execute on the first boot of the router. It modifies the OPKG mirror URL, sets the hostname, and configures NTP servers.

```bash
#!/bin/sh
# Script to change OPKG mirror base URL, set hostname to ZANWRT, and configure NTP on first boot

# Change only the base OPKG mirror URL to your custom mirror
# This replaces any URL starting with https:// with the specified mirror URL
sed -i 's|https://.*|https://immortalwrt.kyarucloud.moe|g' /etc/opkg/distfeeds.conf

# Change the hostname to ZANWRT
uci set system.@system[0].hostname='ZANWRT'
uci commit system

# Configure NTP servers to use Cloudflare, Google, and NTP pool
uci delete system.ntp.server  # Remove any default NTP servers
uci add_list system.ntp.server='time.cloudflare.com'  # Add Cloudflare NTP
uci add_list system.ntp.server='dns.google'  # Add Google NTP
uci add_list system.ntp.server='pool.ntp.org'  # Add NTP pool
uci commit system