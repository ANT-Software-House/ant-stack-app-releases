# ant-stack-app-releases

Public apt/dnf package repository for **AntStack Desktop**, published and signed automatically by
CI from [`ANT-Software-House/ant-stack-app`](https://github.com/ANT-Software-House/ant-stack-app)
on every release tag. This repo holds only the generated package trees (`apt/`, `rpm/`) — no
source code.

Prefer a plain manual download instead? Grab the `.deb`/`.rpm`/`.AppImage` from that repo's
[Releases page](https://github.com/ANT-Software-House/ant-stack-app/releases) (requires GitHub
access). Everything below is for customers without access to the private source repo.

---

## Debian / Ubuntu (apt)

```bash
# 1. Import the signing key (one-time)
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://ant-software-house.github.io/ant-stack-app-releases/rpm/antstack-release-public.asc \
  | gpg --dearmor | sudo tee /etc/apt/keyrings/antstack.gpg > /dev/null

# 2. Add the repository (one-time)
echo "deb [signed-by=/etc/apt/keyrings/antstack.gpg] https://ant-software-house.github.io/ant-stack-app-releases/apt stable main" \
  | sudo tee /etc/apt/sources.list.d/antstack.list

# 3. Install
sudo apt update
sudo apt install ant-stack
```

Upgrades happen automatically with `sudo apt upgrade`.

---

## Fedora / RHEL (dnf)

```bash
# 1. Add the repository (one-time — includes the signing key reference)
sudo tee /etc/yum.repos.d/antstack.repo > /dev/null <<'REPO'
[antstack]
name=AntStack Desktop
baseurl=https://ant-software-house.github.io/ant-stack-app-releases/rpm
enabled=1
gpgcheck=1
gpgkey=https://ant-software-house.github.io/ant-stack-app-releases/rpm/antstack-release-public.asc
REPO

# 2. Install
sudo dnf install ant-stack
```

Upgrades happen automatically with `sudo dnf upgrade`.

---

## Verifying the signature manually

The signing key fingerprint is `19D0567580CDF585` (RSA 4096, generated 2026-08-01, expires 2 years
from issuance). Import and verify:

```bash
curl -fsSL https://ant-software-house.github.io/ant-stack-app-releases/rpm/antstack-release-public.asc | gpg --import
gpg --list-keys 19D0567580CDF585
```

If the fingerprint doesn't match, do not install — open an issue on
[`ant-stack-app`](https://github.com/ANT-Software-House/ant-stack-app/issues).

## Key rotation

When the signing key nears expiration, a new key is generated and this README (plus the
`gpgkey`/`signed-by` references above) is updated with the new fingerprint. Existing installs
continue working on the old key until it actually expires; re-running the "import the signing key"
step picks up the replacement.
