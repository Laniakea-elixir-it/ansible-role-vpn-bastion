# Bastion Ansible (PAM OAuth2 Device Flow)

This repository configures an Ubuntu 22.04 VM as a bastion host with SSH login via OpenID Connect (device flow) using the `pam_oauth2_device` module. **OpenVPN is not included** here (can be added later).

## Prerequisites

- A reachable Ubuntu 22.04 VM with SSH and sudo.
- Ansible >= 2.15 on your control machine.
- A valid OIDC client (client_id + client_secret) and IdP endpoints.

## 1) Inventory and variables

Edit `inventory` to point to your bastion public IP and SSH user (default: ubuntu).

Edit `group_vars/bastion.yml`:
- Set `idp_provider` to `iam`, `lifescience`, or `egi`.
- Provide OIDC endpoints for your provider (IAM is already filled in).
- Set `client_id`, `username_attribute`, optional `allowed_groups`.
- If you want email delivery of the device code, set `enable_email: true`.
  - Put SMTP username in `group_vars/bastion.yml`.
  - Put SMTP password in the Vault file (see below).

## 2) Secrets in Ansible Vault

Put secrets in `group_vars/bastion.vault.yml`:

```yaml
client_secret: "YOUR_OIDC_CLIENT_SECRET"
smtp:
  smtp_password: "YOUR_SMTP_PASSWORD"

