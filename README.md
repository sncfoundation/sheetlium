<p align="center">
  <img src="https://sncfoundation.github.io/logos/sheetlium.svg" width="96" alt="Sheetlium logo">
</p>

<h1 align="center">Sheetlium</h1>

<p align="center"><b>Networking &amp; service discovery for Sheeternetes — our Cilium.</b><br>
A <a href="https://sncfoundation.github.io">Sheet-Native Computing Foundation</a> project · analog of <b>Cilium</b></p>

---

**Status:** 💾 Autosaving · a real component, extracted from the Sheeternetes kubelet.

## What it does

Sheetlium is the L3/service layer. A **shared Docker network** gives every pod connectivity,
and each **Deployment name** is registered as a `--network-alias` — a round-robin DNS
**Service**. So `curl http://hello` from any pod reaches the `hello` Deployment's replicas,
load-balanced by Docker's embedded DNS. Services, without the CRDs.

The `kubelet` joins containers to this network; this CLI creates and **inspects** it — think
`cilium status` / `cilium service list`.

## Requirements

- `docker` (and `jq` for `status`). Set `SK_NET` to match your kubelet (default: `sheeternetes`).

## Usage

```bash
sheetlium up                  # create the shared network
sheetlium status              # network + services + endpoints
sheetlium services            # list Services (Deployment aliases)
sheetlium endpoints hello     # backing container IPs for the 'hello' Service
sheetlium down                # remove the network (pods must be gone)
```

## How a Service works

When the kubelet runs a pod it does, in effect:

```bash
docker run --network sheeternetes --network-alias <deployment> ... <image>
```

Every replica shares the alias, so the alias resolves round-robin across replicas — a Service
and a load balancer, courtesy of Docker's embedded DNS. Real multi-host would need an overlay;
that's on the roadmap.

## Roadmap

- Overlay networking for true multi-host clusters.
- NetworkPolicy via a `Sheetlium` tab (allow/deny by Deployment).
- Ingress: map a hostname to a Service.

Tracking &amp; design: [sncfoundation/sheeternetes#31](https://github.com/sncfoundation/sheeternetes/issues/31) ·
[SNCF Landscape](https://sncfoundation.github.io/landscape.html)

---

<sub>Licensed under Apache-2.0. Do not run production on a spreadsheet. If you do, please film it.</sub>
