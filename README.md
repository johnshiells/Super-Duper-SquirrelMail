> **Status: under development.** APIs, UI, and config may change without notice. Not production-ready.

# Super-Duper SquirrelMail

Hardened fork of [SquirrelMail](https://squirrelmail.org/) **1.4.22**.

| | |
|---|---|
| **Product** | Super-Duper SquirrelMail |
| **Version** | 2.0.0 (`SM_PRODUCT_VERSION`) |
| **Build** | see `functions/version.php` (`SM_BUILD`) |
| **License** | [GNU GPL v2.0](LICENSE) |

Browser IMAP/SMTP webmail with optional **Enhanced Mode** UI, ThemesV2, hardened login, optional passkeys/2FA, Message Safety, and **SM Admin**.

## Features

- Classic frameset UI (dial-up / low-bandwidth) and optional Enhanced Mode (frameless chrome, soft-nav, phone layout)
- ThemesV2 and Eyesaver
- Hardened login (captcha / optional reCAPTCHA, lockouts, session binding)
- Optional WebAuthn passkeys and 2FA (TOTP, email/SMS OTP, backup codes)
- Message Safety: remote images, content alerts, remote link / phishing warnings
- SM Admin operator console
- Installer and upgrader under `setup/`

## Requirements

- PHP **7.4+** (8.1+ with PHP-FPM recommended)
- Extensions: `mysqli`, `imap`, `openssl`, `json`, `hash`, `filter`  
  (LDAP only if SM Admin uses directory login)
- MySQL/MariaDB for preferences and address books
- Reachable IMAP and SMTP

## Install

```bash
php setup/install.php
```

1. Point the web document root at this tree (or a `/squirrelmail` alias).
2. Configure `config/config_local.php` (from the installer / `.dist`): `$domain`, IMAP, SMTP, `$prefs_dsn`, `$addrbook_dsn`.
3. Open `/src/login.php` (or site `index.php`).
4. Operators: `/admin`.

Upgrade:

```bash
php setup/upgrade.php
```

Optional maintenance cron (~every 5 minutes):

```bash
php src/maintain.php --quiet
```

Host notes: `hostsettings.MD`. Config ownership: `config/CONFIG.md`.

## Sign-in (overview)

1. `src/login.php` — captcha gate  
2. `src/sentry.php` — mailbox username / password  
3. `src/redirect.php` — IMAP proof, session hardening, optional 2FA/passkey  
4. Mail UI — sign-out via `src/signout.php`

Public hardening page (no login): `src/security.php`.

## Configuration

Do **not** commit secrets. Site values belong in `config/config_local.php` (or plugin config). Never put live DSNs, salts, or API keys in the public tree.

Common site settings:

- `$domain`, `$imapServerAddress`, `$smtpServerAddress`
- `$prefs_dsn`, `$addrbook_dsn`
- `$data_dir`, `$attachment_dir`
- `$plugins` (include `smadmin` for the admin console)

## What this is not

- Not a mail server — you need IMAP/SMTP
- Not company SSO — mailbox auth is IMAP password (± local passkeys/2FA)
- Site-only / proprietary plugins and live configs should be omitted from a public release

## License

GNU General Public License v2.0 — same lineage as upstream SquirrelMail.

Upstream: The SquirrelMail Project Team. Hardening credited in-product as AI / Super-Duper SquirrelMail maintainers.
