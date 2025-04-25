# Prometheus3 RPM Spec for Rocky Linux 9

This repository contains the **RPM spec file** for packaging [Prometheus](https://prometheus.io) (version 3) as `prometheus3` on **Rocky Linux 9**.

**Note:** Our `prometheus3.spec`, `prometheus3.service`, and `prometheus3.default` files are adapted from
<https://github.com/lest/prometheus-rpm/tree/master/prometheus2> with modifications for our needs.

---

## Features

- Deploys the official pre-built Linux amd64 Prometheus binaries.
- Provides a systemd service for easy daemon management.
- Installs default configuration files and environment file for integration with Rocky Linux conventions.
- Creates a dedicated, unprivileged `prometheus` user and data directory.

---

## Prerequisites

Install required build tools and macros:

```sh
sudo dnf groupinstall "Development Tools"
sudo dnf install rpm-build systemd-rpm-macros
```

---

## Download Source Files

1. **Prometheus binary tarball**
   Download from the official GitHub release:

   ```sh
   wget https://github.com/prometheus/prometheus/releases/download/v3.x.x/prometheus-3.x.x.linux-amd64.tar.gz
   cp prometheus-3.x.x.linux-amd64.tar.gz ~/rpmbuild/SOURCES/
   ```

2. **Service and default files**
   Place `prometheus3.service` and `prometheus3.default` files into `~/rpmbuild/SOURCES/`.

---

## Building the RPM

1. Place `prometheus3.spec` in your `~/rpmbuild/SPECS/` directory.

2. Run rpmbuild:

   ```sh
   rpmbuild -ba ~/rpmbuild/SPECS/prometheus3.spec
   ```

   On success, find RPMs in `~/rpmbuild/RPMS/x86_64/`.

---

## Installing and Using Prometheus3

1. **Install the RPM:**

   ```sh
   sudo dnf install ~/rpmbuild/RPMS/x86_64/prometheus3-3.3.0-1*.rpm
   ```

2. **Enable and start the service:**

   ```sh
   sudo systemctl enable --now prometheus.service
   ```

3. **Edit configuration:**

   - Main configuration: `/etc/prometheus/prometheus.yml`
   - Environment options: `/etc/default/prometheus`

   Configuration changes take effect after restarting the service:

   ```sh
   sudo systemctl restart prometheus.service
   ```

---

## Notes

- This package installs Prometheus in `/usr/bin/prometheus` and `/usr/bin/promtool`.
- Data is persisted in `/var/lib/prometheus`, owned by the `prometheus` user.
- **Only x86_64 (amd64) architecture is supported** (pre-built binary).
- This spec is tested for Rocky Linux 9 and may work with compatible RHEL9-based distros.

---

## References & Credits

- [Prometheus GitHub Releases](https://github.com/prometheus/prometheus/releases)
- **This packaging is adapted from:**
  [lest/prometheus-rpm/tree/master/prometheus2](https://github.com/lest/prometheus-rpm/tree/master/prometheus2)

---

**Spec Target Platform:** Rocky Linux 9 (x86_64)
