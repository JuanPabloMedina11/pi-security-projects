# Build Log

## 2026-10-08 · Stage 1: Foundation and hardening

**Setup:** Raspberry Pi 5 (8GB), 256GB SanDisk microSD, Raspberry Pi OS (64-bit)

### What I did

1. Created an `ed25519` SSH key pair on my laptop. The private key stays on the laptop and the public key goes on the Pi, so I can log in without a password that could be guessed.
2. Installed Raspberry Pi OS (64-bit) on the microSD card with Raspberry Pi Imager. I chose Lite because it has no desktop, which leaves more memory for services.
3. Booted the Pi with a monitor and keyboard, connected it to Wi-Fi, and updated everything with `sudo apt update && sudo apt full-upgrade`.
4. Set up SSH so it only accepts my key, with password login turned off.
5. Turned on the firewall with `ufw`. It blocks all incoming connections except SSH and allows outgoing ones, so anything I install later stays closed until I open it on purpose.
6. Enabled automatic security updates with `unattended-upgrades`.
7. Installed `log2ram`. It keeps system logs in RAM and copies them to the card once a day, which stops constant log writes from wearing out the SD card.
8. Ran a security audit with Lynis to get a baseline score.

### Problems and fixes

**SSH refused my login with `Permission denied (publickey)`.**

- **How I found it:** I checked `~/.ssh/authorized_keys` on the Pi and it was empty. The public key had never been saved during setup.
- **Fix:** I briefly allowed password login, copied the public key over from my laptop, confirmed that key login worked, and then turned password login off again.
- **What I learned:** the server only trusts keys listed in `authorized_keys`, so that file is the first thing to check when a key login fails.

### Measurements

- Lynis hardening index: **63** (baseline)

### Next

- Install Docker and Tailscale
- Come back to the Lynis suggestions and raise the score


## 2026-10-08 · Stage 2: Tailscale and Docker

### What I did

1. Installed Tailscale on the Pi and on my laptop and signed both into the same account. Tailscale builds a private encrypted network between my own devices, so I can reach the Pi from any Wi-Fi without opening ports on a router.
2. Turned off "Allow incoming connections" on my laptop. The laptop can connect to the Pi, but the Pi cannot connect back. If the Pi is ever compromised, it has no path to my laptop.
3. Disabled key expiry for the Pi in the Tailscale admin console, so the server doesn't get logged out after a few months.
4. Tested SSH over the Pi's Tailscale address and switched to using it for all logins, since it stays the same on every network.
5. Installed Docker with the official install script and added my user to the `docker` group.
6. Verified the install with `docker run hello-world`.

### Problems and fixes

**SSH disconnected in the middle of the Tailscale install.**

- **What happened:** the session closed with `Connection closed by remote host` while `apt` was still downloading, so the install never finished.
- **Fix:** I logged back in and ran the install again, one command at a time, and it completed.
- **Open question:** I haven't confirmed the cause. If it happens again I'll check `uptime` and `vcgencmd get_throttled` to rule out a reboot or a weak power supply.

### Security notes

- Membership in the `docker` group is equivalent to root access, so only my own user is in it.
- Docker publishes container ports directly and bypasses `ufw`. For every service I'll bind published ports to the Tailscale address or to localhost, never to all interfaces.

### Next

- Stage 3: Pi-hole as my first Compose service, then the automation bot
