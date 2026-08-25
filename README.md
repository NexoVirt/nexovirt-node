# NexoVirt SDK for Node.js / TypeScript

Official TypeScript client for the [NexoVirt](https://nexovirt.com) REST API — a clean,
token-authenticated API over Proxmox VE for managing hosts, guests (KVM VMs + LXC
containers), plans, IPAM, firewall, snapshots and more.

- **Docs:** [NexoVirt REST API reference](https://docs.nexovirt.com/api)
- **What is NexoVirt:** a self-hosted control panel and REST API for Proxmox VE — [nexovirt.com](https://nexovirt.com)

This package is generated from the NexoVirt [OpenAPI specification](https://docs.nexovirt.com/api),
so it always matches the live API. See [Regenerating](#regenerating) below.

## Install

```bash
npm install @nexovirt/sdk
```

## Quick start

Create an API token in your panel under **Settings → API tokens**, then point the client at
your panel's `/api/v1` base URL:

```ts
import { Configuration, HostsNodesApi, GuestsApi } from '@nexovirt/sdk';

const config = new Configuration({
  basePath: 'https://your-panel.example/api/v1',
  accessToken: process.env.NEXOVIRT_TOKEN, // Bearer token from Settings → API tokens
});

const hosts = new HostsNodesApi(config);
const guests = new GuestsApi(config);

// List every Proxmox host
const allHosts = await hosts.listHosts();

// List guests on a host
const list = await guests.listGuests({ hostId: 1 });
console.log(list.data);
```

Every response uses the NexoVirt envelope `{ success, data, error }`; list endpoints add
`meta: { page, perPage, total }`. All API classes and methods are typed — use your editor's
autocomplete, or browse `docs/` in this repo, to discover the rest.

## Authentication

All requests send `Authorization: Bearer <token>`. Tokens can be host-scoped, IP-allowlisted
and rate-limited in the panel. A scoped token gets `403` on any host it may not touch.

## Regenerating

This SDK is generated with [OpenAPI Generator](https://openapi-generator.tech) from the live
spec:

```bash
./scripts/regenerate.sh
```

## License

MIT — see [LICENSE](./LICENSE).
