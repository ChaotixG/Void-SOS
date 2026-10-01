# Void SOS

A home server operating system you install on a spare computer and run from
your phone. It is the server edition of VoidOS: no desktop, a hardened system
that updates itself safely, and everything reached over your own private
network ([Tailscale](https://tailscale.com)), never from the open internet.

**Status: 0.1.0, an early test release.** It installs, joins your tailnet, sets
up its owner and pairs devices. Apps (media, photos and the rest) come in a
later release.

## What's included

- A text installer, with full-disk encryption
- **void-sos**, the server's own command: setup, devices, people, roles,
  approvals, activity, updates
- Tailscale, so the server is reachable from your devices wherever they are
- Kanidm, which keeps the passkeys and passwords people sign in to apps with
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

In 0.1.0, check the disk you choose is the server's own: started through
Ventoy, the stick itself can appear in the list.

When it finishes, take the stick out and start the server from its disk.

### 6. First start

Sign in at the console with your administrator account, then:

```bash
sudo void-sos setup
```

It makes you the owner, names the server on your tailnet, and gives you a
**recovery key** of 24 words: write them down; you type three of them back.

The server needs a network for Tailscale: plug in a cable (in 0.1.0, Wi-Fi is
set with `sudo nmcli device wifi connect "<network>" --ask`). If
`sudo tailscale status` says `Logged out`, sign the server in:

```bash
sudo tailscale up --qr --hostname=$(hostname)
```

Scan the QR code with your phone's camera and sign in to your Tailscale
account. **Success looks like** `Success.`

`help` lists every command; [commands.md](commands.md) describes them.

## Updating

Updates are signed and published here, and the server checks for them by
itself. They never touch your files, accounts or settings, and an update that
does not start is undone automatically.

## Source code

See [SOURCE.md](SOURCE.md).
