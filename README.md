# Asterisk VoIP PBX (SIP and IAX2)

Academic course project (Technical University of Cluj-Napoca, 2025-2026).

An IP PBX built on Asterisk, running as a CentOS virtual machine, with SIP and IAX2 subscribers and a dial plan that gives each protocol its own set of telephony functions. Everything was configured from the Asterisk command line, without a graphical interface such as FreePBX.

## Architecture

- **PBX:** Asterisk (AsteriskNOW) on a CentOS virtual machine in VirtualBox
- **SIP softphone:** MicroSIP (Windows)
- **IAX2 softphone:** Zoiper (Windows)
- The softphones get dynamic IP addresses; the subscriber numbers are fixed in the configuration.

## Subscribers

| Number | Protocol | Softphone |
|--------|----------|-----------|
| 1471, 2471 | SIP | MicroSIP |
| 3471, 4471 | IAX2 | Zoiper |

Any subscriber can call any other subscriber. The special functions below are restricted by protocol: SIP subscribers cannot use the IAX2 functions and vice versa (the dial plan checks the type of the calling channel).

## Dial plan functions

| Extension | Available to | What it does |
|-----------|--------------|--------------|
| 1000 | SIP | Dictate: record a message and listen to it |
| 1001 | SIP | Record a message, then play it back after a beep |
| 1471, 2471 | SIP | Calls between SIP subscribers are recorded with `Monitor()` |
| 3471, 4471 | IAX2 | Calls between IAX2 subscribers fall back to the voicemail box of the callee |
| 2000 | IAX2 | Voicemail access, authenticated by mailbox number and PIN (`VMAuthenticate`) |
| 2001 | IAX2 | Voicemail access, authenticated by PIN only (`Authenticate`) |

The mandatory call management applications `Dial`, `Ringing`, `Answer` and `Hangup` are all used in the dial plan.

## Files

| File | Purpose |
|------|---------|
| `extensions.conf` | Dial plan and subscriber functions |
| `sip.conf` | SIP subscribers and codecs |
| `iax.conf` | IAX2 subscribers and codecs |
| `voicemail.conf` | Voicemail boxes |

## Usage

Copy the four files to `/etc/asterisk/` on the PBX and reload the configuration from the Asterisk CLI (for example `sip reload`, `iax2 reload`, `dialplan reload` and `voicemail reload`). Then register one softphone per subscriber with the number and password from the configuration and call the extensions above.

## Notes

- The files were reconstructed from the listings printed for the course demo and have not been re-run since, because the virtual machine is no longer available.
- `voicemail.conf` includes `vm_general.inc` and `vm_email.inc`, which are not part of this repository.
- All passwords and PINs in the files are test credentials for a lab environment.
- The assignment text and the softphone installers are course materials and are not included.
