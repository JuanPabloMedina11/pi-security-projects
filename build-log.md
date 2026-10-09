# Build Log

## 2026-10-08 · Stage 1: Foundation and hardening

**Setup:** Raspberry Pi 5 (8GB), 256GB SanDisk microSD, Raspberry Pi OS Lite (64-bit)

### What I did

1. Created an `ed25519` SSH key pair on my laptop. The private key stays on the laptop and the public key goes on the Pi, so I can log in without a password that could be guessed.
2. Installed Raspberry Pi OS Lite (64-bit) on the microSD card with Raspberry Pi Imager. I chose Lite because it has no desktop, which leaves more memory for services.
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
