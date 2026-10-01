# Void SOS commands

Void SOS is run from its console: a screen and keyboard on the server, where
you sign in. (The management app, when it arrives, will do the same over your
tailnet.) These are the commands you can type there.

Asking for help:

```sh
help                     # every Void SOS command there is, grouped
help <command>           # what one command does
<command> help           # the same, asked directly
```

`help` is the same as `voidsos help`, and `void` is a shorter name for `voidsos`.
Every command also answers `--help` and `-h`, and asking for help never changes
anything. For the full detail of any command below (its options, and what it
reads and writes), run `<command> help`.

Most commands change the system, so run them as the administrator with `sudo`
in front, for example `sudo void-update status`.

---

## On the USB stick

The USB stick only installs Void SOS. Until Void SOS is on a disk, its console
runs these commands and nothing else, and `help` lists only these:

| Command | What it does |
|---|---|
| `void-sos-install` | Install Void SOS on this computer, step by step |
| `help` | This list (also `voidsos help` and `void help`) |
| `reboot` | Restart the computer |
| `poweroff` | Switch the computer off |

Choosing **Install Void SOS** in the stick's boot menu starts the installer by
itself. The installer's steps:

| Step | What you choose |
|---|---|
| 1 Keyboard | The layout of the keyboard attached, used from then on, so passwords come out as you type them |
| 2 Disk | The disk Void SOS takes, whole. The USB stick is never offered. A disk that already holds Void SOS can be kept (only the system is replaced) or erased |
| 3 Protection | A passphrase at every start; or unlock automatically; or no encryption. Each is explained on screen |
| 4 Administrator | Your account's name and password. It can use `sudo` |
| 5 Computer name | The server's name on your network and your tailnet. It stays the same across restarts |
| 6 Time zone | For example `Europe/London`. Type `list` to browse |

After the six steps the installer shows all your choices on one screen, where
any step can be opened again. It installs only after you have typed the disk's
name on the review screen, and it can restart the computer for you at the end.

---

## On an installed server

### Setting up and running the server

`void-sos` is the agent's console command: the same things the management app
does, from the server's own screen and keyboard.

| Command | What it does |
|---|---|
| `sudo void-sos setup` | Set the server up, once: you become its owner, it gets its name on your tailnet and joins it, and it makes the backup recovery key (24 words to write down; you type three back). It ends with a QR code to pair your phone |
| `sudo void-sos pair` | A QR code anyone can scan to ask to join. They choose a name and username; you approve them in the app's Approvals, and they join without admin rights. It works once, for 24 hours. Without a camera, they type the server's name in the app and request an account. Their phone reaches the server through Tailscale: share the server with them in Tailscale's admin console, or invite them to your tailnet |
| `sudo void-sos pair --owner` | A new QR code that pairs one more of your own devices. It works once, for 15 minutes. A computer without a camera types the server's name and the code shown under it |
| `void-sos status` | The server's name and version, whether it is set up, the four words of its key, and its tailnet |
| `void-sos approvals` | What is waiting for a decision: account requests, new devices, USB devices plugged in |
| `sudo void-sos approve <id> [--always]` | Approve one of them. A new account gets the member role. For a USB device, `--always` remembers it, so it is let in whenever it is plugged in |
| `sudo void-sos deny <id> [--always]` | Decline one of them. For a USB device, `--always` refuses it every time |
| `void-sos devices` | Every device, whose it is and when it was last seen, with the code of each one waiting |
| `sudo void-sos devices approve <code>` | Approve a waiting device here, for someone who has no device left to approve it from. Your own need your password, or the recovery key if you forgot it |
| `void-sos people` | Everyone with an account, their roles and devices |
| `void-sos roles` | The roles in rank order, and who holds each |
| `sudo void-sos roles give <user> <role>` | Give someone a role. `roles take` takes it back |
| `sudo void-sos suspend <user> [minutes]` | Suspend an account: its sessions end at once and it cannot sign in until the time is up (60 minutes; 0 means until lifted) |
| `sudo void-sos blacklist <user>` | Blacklist an account: no sign-in and no requests until lifted |
| `void-sos moderation` | The suspensions and blacklists in force, with their ids |
| `sudo void-sos lift <id>` | Lift a suspension or blacklist |
| `sudo void-sos delete <user>` | Delete an account and all of its devices, after asking you to confirm |
| `void-sos updates` | The two system slots, which one runs, and whether a new release is waiting |
| `sudo void-sos update` | Checks for a new release and, once you confirm, installs it and restarts into it. Your apps, files and settings are not touched; a release that does not start is undone by itself |
| `void-sos activity [n]` | The last entries of the audit log, and a warning if it was ever altered |
| `sudo void-sos terminal enable` | Turn the remote terminal on. It is turned off from the app |
| `sudo void-sos tailscale` | Sign the server in to your tailnet: it shows a QR code to scan with your phone, and waits until you have signed in. Setup does this as one of its steps; this is for later, after a skip, or when the server shows as not signed in |
| `sudo void-sos wifi` | Connect the server to a Wi-Fi network: it lists the networks in reach, and asks for the password of the one you choose. Setup offers this by itself when the server is on no network |
| `sudo void-sos owner transfer <user>` | Make someone else the owner; you keep the Admin role. It asks for your password, and, when they have no account at this console yet, a password for the one it makes them |
| `sudo void-sos owner recover` | Forgotten the owner's password? Set a new one with the 24-word backup recovery key |

`setup`, `pair`, `devices approve`, `terminal enable` and `owner` work only at the
server's own console, never over a remote terminal. Until setup has run the
agent listens on nothing but its local socket; afterwards it listens only on
the server's tailnet addresses.

### Everyday

| Command | What it does |
|---|---|
| `help` | Every Void SOS command, grouped |
| `void-update status` | Which system slot is running, and whether an update is waiting |
| `void-update apply <url> <sig>` | Download a signed update and stage it for the next start |
| `void-update rollback` | Start the previous system next time |
| `void-update verify` | Check the system for damage; `void-update repair` puts back what is damaged |
| `void-update edition` | Which edition this machine is. A server only ever accepts server updates |
| `void-disk list` | The drives in the machine: size, type and model |
| `void-disk health` | Each drive's SMART or NVMe health report |
| `void-psi` | Live memory, CPU and disk pressure readings |
| `voidsos-keyboard <layout>` | Change the keyboard layout, for example `voidsos-keyboard uk` |
| `void-get search` | The extra tools you can install, from the signed Void SOS list |
| `void-get install <tool>` | Install one of them; `void-get remove <tool>` takes it back |

A daily job checks for a new Void SOS release and says so on the console. It
never installs anything by itself.

### Network

| Command | What it does |
|---|---|
| `nmcli device wifi connect "<name>" --ask` | Join a Wi-Fi network. It asks for the password |
| `nmcli device status` | Which network connections are up |
| `void-network status` | The firewall: its mode (always `server`) and the rule tables loaded |
| `void-network apply` | Load the server firewall again. It replaces only its own table, never Tailscale's or Docker's |
| `void-macpolicy status` | Which address the network card shows. A server shows its real hardware address |
| `void-macpolicy randomise` | Give the card a random address instead, kept across restarts. The connection drops for a moment |
| `void-macpolicy hardware-on` | Go back to the real hardware address |

The firewall admits only what the server offers, and gives every device on the
network its own rate limit, so one noisy device cannot crowd out the others.

### Tailscale

| Command | What it does |
|---|---|
| `sudo tailscale up --qr --hostname=$(hostname)` | What `void-sos tailscale` runs: sign in to your tailnet with a QR code, keeping the server's name |
| `tailscale up` | Join your tailnet. It prints a link to sign in with |
| `tailscale status` | This server and the other machines on your tailnet |
| `tailscale ip` | This server's tailnet addresses |
| `tailscale serve` | Offer a local service (for example an app on `127.0.0.1`) to your tailnet, with HTTPS |
| `tailscale down` | Leave the tailnet until the next `tailscale up` |

Tailscale is part of the Void SOS system and is updated with it; it does not
update itself.

### Containers

| Command | What it does |
|---|---|
| `docker ps` | The containers that are running |
| `docker run <image>` | Start a container |
| `docker compose up -d` | Start the containers a `compose.yaml` describes |
| `docker compose down` | Stop them again |
| `docker logs <container>` | A container's output |

Containers and their data live on the persistence volume of an installed
server. Root inside a container is an unprivileged user on the server, and a
port a container publishes is on `127.0.0.1` unless you name another address:
reach it through `tailscale serve`.

### Backups

| Command | What it does |
|---|---|
| `restic` | Encrypted, de-duplicated backups: `restic init`, `restic backup`, `restic snapshots`, `restic restore` |
| `rest-server` | Serve a backup store to another server, which keeps its encrypted backups here without being able to read them |

### Security

| Command | What it does |
|---|---|
| `voidsos-usb-policy status` | Whether new USB devices wait for approval. On a server they always do |
| `voidsos-confine status` | Which programs AppArmor confines, and in which mode |
| `voidsos-confine denials` | What AppArmor blocked, or would have blocked, in plain language |
| `voidsos-luks-backup save <device> <dir>` | Save the encrypted disk's header, the part without which nothing can be unlocked. `restore` puts it back |
| `qrencode -t UTF8 "<text>"` | Draw text as a QR code on the console, for a phone to read |

### Memory and software

| Command | What it does |
|---|---|
| `voidsos-scratch status` | The encrypted overflow swap on the persistence volume, and why it is off when it is |
| `voidsos-build search <term>` | Software you can build from source with pkgsrc |
| `voidsos-build install <cat/pkg>` | Build and install it into your home folder; `--system` (with `sudo`) for everyone |

### Starting, stopping, services

| Command | What it does |
|---|---|
| `reboot` | Restart the server |
| `poweroff` | Switch the server off |
| `sudo sv status <service>` | Whether a service is running, for example `tailscaled` or `dockerd` |
| `sudo sv restart <service>` | Restart one service |

---

## Services

Started by the system, not typed. Listed so that a name seen in `ps` or in a log
makes sense.

| Name | What it does |
|---|---|
| `void-sosd` | The Void SOS agent: every policy and all the state, and the API that `void-sos` and the app use. It runs as its own unprivileged user |
| `kanidmd` | Kanidm, the identity server people sign in to apps with (passkeys or passwords). It listens on this machine only and starts once the server has its tailnet name; its sign-in pages reach the tailnet through `tailscale serve` |
| `tailscaled` | The Tailscale daemon |
| `containerd`, `dockerd` | The container runtime and Docker. On the USB stick they wait, because there is no persistence volume for their data |
| `voidkill` | Watches Tor and keeps its status for the rest of the system |
| `void-tordate` | Corrects the clock from what Tor reports |
| `void-ntpd` | Answers time queries on the machine from the corrected clock |
| `void-usb-gate` | Holds each newly plugged USB device until it is approved |
| `voidsos-periodic` | Runs the daily and weekly jobs, among them the update check |
| `voidsos-zram` | Compressed swap in RAM |
| `void-persist-unlock` | Unlocks the persistence volume at startup |
| `void-bootlog` | Keeps the log of the last start |
| `void-f2-poll` | Watches for F2 at startup, which opens the recovery menu |

## Helpers

Run by other parts of Void SOS, not typed. They are here so that every name in
`help` has a line somewhere.

| Name | What it does |
|---|---|
| `void-install.sh` | The installer's backend; `void-sos-install` runs it |
| `void-sos-helper` | The agent's root half: a fixed set of operations it does for `void-sosd` and nobody else |
| `void-diskctl` | Partition and filesystem operations for `void-disk` |
| `void-get-helper` | The part of `void-get` that installs with root rights |
| `void-userctl` | Creates, removes and administers accounts |
| `void-timezone` | Sets the time zone |
| `void-torctl` | Turns a Tor bridge on or off |
| `void-notify` | Shows a message from a system service on the console |
| `void-persist-open` | Opens the persistence volume and records how it went |
| `void-trace` | Writes one diagnostic line to every log |
| `voidsos-update-check` | Asks whether a newer Void SOS release exists |
| `persistence-create.sh` | Creates a persistence volume on the USB stick |
