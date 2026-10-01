# Void SOS

A home server operating system you install on a spare computer and run from
your phone. It is the server edition of VoidOS: no desktop, a hardened system
that updates itself safely, and everything reached over your own private
network ([Tailscale](https://tailscale.com)), never from the open internet.

**Status: 0.1.1, an early test release.** It installs, joins your tailnet, sets
up its owner, pairs devices, lets the people you approve join, and updates
itself on command. Apps (media, photos and the rest) come in a later release.

## What's included

- A text installer, with full-disk encryption
- **void-sos**, the server's own command: setup, devices, people, roles,
  approvals, activity, updates
- Tailscale, so the server is reachable from your devices wherever they are
- Kanidm, which keeps the passwords (and, if they like, passkeys) people sign
  in to apps with, set from the Void SOS app
- Docker, ready for the apps to come
- Safe updates: a new version is written beside the running one and only
  becomes active on restart; if it does not start, the server goes back by
  itself

## Getting Void SOS

### 1. Download

From [Releases](../../releases/latest), download
`voidsos-x86_64-<version>.iso` and `voidsos-x86_64-<version>.iso.sha256`.

### 2. Check the download

In a terminal, in the folder you downloaded them to:

```bash
sha256sum -c voidsos-x86_64-*.iso.sha256
```

**Success looks like** `voidsos-x86_64-<version>.iso: OK`.

### 3. Put it on a USB stick

With [Ventoy](https://www.ventoy.net), copy the ISO onto the stick. Or write
it to a whole stick (everything on it is erased), replacing `sdX` with the
stick's name from `lsblk`:

```bash
sudo dd if=voidsos-x86_64-<version>.iso of=/dev/sdX bs=4M status=progress conv=fsync
```

### 4. Turn Secure Boot off

In the server's firmware setup (often F2, Del or Esc at power-on), set
**Secure Boot** to **Disabled**, save and exit. Void SOS signs its boot chain
with its own keys, and with Secure Boot on the computer stops with
`shim_lock protocol not found`. Some firmware asks you to confirm the change
at the next start (HP shows a four-digit code to type); if it is skipped, the
change is undone.

### 5. Install

Start the server from the stick and choose **Install Void SOS**. The installer
asks for the keyboard, the disk Void SOS takes (the whole disk), how it is
protected, your administrator account, the computer's name and the time zone
(for example `America/Toronto`), then shows your choices before it starts.

When it finishes, it offers to switch the computer off: take the stick out,
then start the server from its disk.

### 6. First start

Sign in at the console with your administrator account, then:

```bash
sudo void-sos setup
```

It makes you the owner and gives you a **recovery key** of 24 words: write
them down; you type three of them back. With no cable plugged in, it offers to
join Wi-Fi. Then it signs the server in to your tailnet: scan the QR code it
shows with your phone's camera and sign in to your Tailscale account (or type
`skip`, and do it later with `sudo void-sos tailscale`). At the end it shows a
QR code to pair your phone: scan it, and the Void SOS app opens.

To let someone else use the server, `sudo void-sos pair` shows a QR code for
them to scan; you approve them in the app, under Approvals. Their phone
reaches the server through Tailscale: share the server with them in
Tailscale's admin console (Machines, then Share in its menu), or invite them
to your tailnet. For another of your own devices, `sudo void-sos pair --owner`.

**Passwords for apps.** Everyone sets theirs in the app, under *My account*.
For that, turn on HTTPS certificates for your tailnet once: in Tailscale's
admin console, open **DNS** and, under **HTTPS Certificates**, choose **Enable
HTTPS**.

`help` lists every command; [commands.md](commands.md) describes them.

## Updating

Updates are signed and published here, and the server checks for them by
itself. To install one, use the app's *Update* page, or at the server:

```bash
sudo void-sos update
```

It says what is available and asks first. Updates never touch your files,
accounts or settings, and an update that does not start is undone
automatically.

**From 0.1.0**, which has no `void-sos update` yet: use the app's *Update*
page, or at the server:

```bash
set -- $(sudo /usr/local/lib/voidos/voidsos-update-check | head -1); [ "$1" = available ] && sudo void-update apply "$3" "$4" && sudo reboot
```

**Success looks like** the server restarting, and `void-sos status` then
showing the new version.

## Source code

See [SOURCE.md](SOURCE.md).
