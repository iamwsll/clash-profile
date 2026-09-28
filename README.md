# clash-profile

Personal Clash Verge Rev / Mihomo profile assets.

The goal of this repository is to keep personal configuration **outside**
`sublink-worker`, so the converter can stay close to upstream while new
machines still need only one generated subscription URL.

## Files

### `clash/base.yaml`

Base Mihomo configuration containing only stable client behavior:

- domestic encrypted DNS only
- no system DNS fallback
- `fake-ip` mode
- TUN enabled with DNS hijacking
- Tailscale CGNAT range excluded from TUN routing
- Tailscale MagicDNS sent to `100.100.100.100`

It deliberately does **not** contain airport nodes, generated policy groups,
or the normal ACL routing rules. Those belong to the subscription generator.

Raw URL:

```
https://raw.githubusercontent.com/iamwsll/clash-profile/master/clash/base.yaml
```

### `rules/bulk-download.list`

Classical Clash rules for high-bandwidth transfers such as:

- Docker Hub image pulls
- Hugging Face model/dataset payload downloads

Raw URL:

```
https://raw.githubusercontent.com/iamwsll/clash-profile/master/rules/bulk-download.list
```

The intended policy group is:

```
📦 大流量下载
```

Later, `sublink-worker` should attach this remote ruleset to that group and
make the group prefer a cheap / high-bandwidth / low-multiplier node pool.

## Design

```
node subscriptions
        |
        v
sublink-worker
        |
        +---- base config ----> clash/base.yaml
        |
        +---- personal rules -> rules/*.list
        |
        v
one stable subscription URL
        |
        v
Clash Verge Rev / Mihomo
```

This keeps three concerns separate:

1. upstream rule data (ACL4SSR / community rule providers)
2. personal stable client configuration (this repository)
3. subscription conversion and policy assembly (sublink-worker)
