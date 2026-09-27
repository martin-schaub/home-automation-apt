# home-automation-apt

Signed apt repository for the `homeautomation` Debian package, served by
GitHub Pages at <https://martin-schaub.github.io/home-automation-apt/>. It
holds the `.deb` packages (amd64, arm64), the apt metadata under
`dists/homeautomation/`, and the public signing key (`key.gpg`, `key.asc`).

## Installing (once per machine)

```sh
sudo install -d -m 0755 /etc/apt/keyrings
curl -fsSL https://martin-schaub.github.io/home-automation-apt/key.gpg | sudo tee /etc/apt/keyrings/homeautomation.gpg >/dev/null
echo "deb [signed-by=/etc/apt/keyrings/homeautomation.gpg] https://martin-schaub.github.io/home-automation-apt homeautomation main" | sudo tee /etc/apt/sources.list.d/homeautomation.list
sudo apt update && sudo apt install homeautomation
```

Check the downloaded key against the signing key fingerprint
`4F78 B8CE 6A01 31BB BD7B  56B4 00DA E2EB 349E 4002`:

```sh
gpg --show-keys /etc/apt/keyrings/homeautomation.gpg
```

A fresh install enables the service but does not start it: edit the
placeholder `/etc/homeautomation/config.yaml` and
`/etc/homeautomation/secrets.yaml` first, then start it with
`sudo systemctl start homeautomation.service`.

## Upgrades and rollback

After that, releases arrive with a normal `apt update && apt upgrade`; the
package restarts the service on upgrade if it is enabled. The repository
keeps the newest 10 versions of the package per architecture, so rolling back
to a previous release is:

```sh
sudo apt install homeautomation=0.0.1  # any older version still in the pool
```

The suite is called `homeautomation` rather than `stable` so it can't be
confused with Debian's own `stable` in apt pinning.
