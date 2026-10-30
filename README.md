# Keeper RBI Scripts

![Scripts](https://img.shields.io/badge/scripts-10-blue) ![Applications](https://img.shields.io/badge/applications-8-green) ![Keeper](https://img.shields.io/badge/Keeper-Remote%20Browser%20Isolation-orange)

A collection of application-specific **automatic login scripts** for **Keeper Remote Browser Isolation (RBI)** in Keeper Connection Manager (KCM).

Keeper RBI provides isolated browser sessions for accessing web applications. These examples demonstrate how to identify login fields, populate credentials, submit forms, and handle certain multi-step or TOTP authentication flows.

> **Disclaimer:** Community-maintained examples; not an official Keeper Security project. Scripts are provided as-is and may require changes when an application's login page changes.

## Script catalog

| Application | Script | What it does |
| --- | --- | --- |
| 3CX | [`3CX_with_submit`](scripts/3CX_with_submit) | Fills email and password and submits the login form. |
| Check Point | [`CheckPoint`](scripts/CheckPoint) | Fills username and password and submits the login form. |
| Cloudflare | [`Cloudflare`](scripts/Cloudflare) | Fills email and password; includes a challenge-related `cannot-submit` selector and hides the password input. |
| Darktrace | [`Darktrace`](scripts/Darktrace) | Two-step configuration for credentials and a subsequent code field. |
| Freshworks | [`Freshworks`](scripts/Freshworks) | Fills credentials and submits the login form. |
| Microsoft 365 | [`Office365`](scripts/Office365) | Handles separate username and password screens. |
| Microsoft 365 | [`Office365_with_TOTP`](scripts/Office365_with_TOTP) | Adds a TOTP entry step to the Microsoft 365 login flow. |
| Sophos Firewall | [`Sophos`](scripts/Sophos) | Fills firewall web-interface credentials. |
| Sophos Central | [`Sophos_with_TOTP`](scripts/Sophos_with_TOTP) | Handles username, password and TOTP screens at `login.sophos.com`. |
| XEOX | [`Xeox`](scripts/Xeox) | Fills credentials and submits the login form. |

**10 scripts · 8 application families**

## Getting started

1. Configure a Keeper RBI connection in **Keeper Connection Manager** using the [official RBI documentation](https://docs.keeper.io/keeper-connection-manager/supported-protocols/remote-browser-isolation).
2. Open the script for your target application in the [`scripts/`](scripts/) directory.
3. Copy its login automation configuration into the corresponding Keeper RBI connection setting, following the syntax and field requirements in your KCM version.
4. Update the `page` match and CSS selectors for your deployment and the current application login page.
5. Test the flow with a non-production account before using it in production.

The examples use fields such as `page`, `username-field`, `password-field`, `totp-code-field`, `submit`, `delay`, `hide`, and `cannot-submit`. The exact supported fields and behavior depend on your Keeper RBI version; consult Keeper's documentation.

### Example: 3CX

```json
[
  {
    "page": "login.3cx.com/Account/Login",
    "username-field": "#Email",
    "password-field": "#Password",
    "submit": "#login-button",
    "delay": 3000
  }
]
```

This example selects the email and password fields using CSS selectors, waits according to the configured delay, and identifies the login button for submission.

### Multi-step authentication and TOTP

Some applications use separate pages for username, password, and one-time codes. See [`Office365_with_TOTP`](scripts/Office365_with_TOTP) and [`Sophos_with_TOTP`](scripts/Sophos_with_TOTP) for examples of multiple sequential steps. TOTP automation requires the appropriate authentication setup; a script alone does not configure MFA or bypass it.

## Compatibility and customization notes

- **Site-specific selectors:** Login pages change over time. Inspect the current page and update CSS selectors as needed.
- **Environment-specific addresses:** Some examples contain private IP addresses or organization-specific hostnames. Replace these with your own endpoints. In particular, review the Check Point, Sophos Firewall, Freshworks, and Darktrace scripts.
- **Formats vary:** Some scripts use YAML-style entries, while others use JSON-style arrays. A few files include explanatory text or JSON-like syntax that may need cleanup before pasting into a strict parser. Copy the relevant configuration, not surrounding notes.
- **MFA and challenges:** CAPTCHA, identity-provider policies, conditional access, and other interactive challenges can affect automated login. Do not assume every flow will complete unattended.
- **Testing:** These are configuration examples, not a certified compatibility matrix; behavior has not been independently verified against current application releases.

## Security guidance

- Keep passwords, TOTP secrets, session tokens, and other sensitive data out of this repository.
- Use Keeper's approved credential-handling mechanisms rather than hardcoding credentials in scripts.
- Limit RBI connection access to authorized users and apply least-privilege permissions.
- Review scripts and destination URLs before deploying them.
- Do not use automation to circumvent MFA, CAPTCHA, or organizational access policies.

## Contributing

Contributions for new applications, updated selectors, and documentation corrections are welcome. When adding a script, use a descriptive filename and include the relevant login flow and any known prerequisites in your pull request. **Remove tenant-specific URLs, internal IP addresses, credentials, and other sensitive information** before submitting.

## Resources

- [Keeper RBI documentation](https://docs.keeper.io/keeper-connection-manager/supported-protocols/remote-browser-isolation)
- [Keeper Connection Manager documentation](https://docs.keeper.io/keeper-connection-manager/)
- [Browse all scripts](scripts/)

---

**Maintained by [alaborbe](https://github.com/alaborbe).**
