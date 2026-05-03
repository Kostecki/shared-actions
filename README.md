# shared-actions

This repository contains one reusable GitHub Action:

- [.github/actions/deploy-komodo/action.yml](.github/actions/deploy-komodo/action.yml): connects to a target host via Tailscale and triggers a Komodo stack update through the Komodo API.

## Deploy via Komodo

### What it does

1. Authenticates and connects with Tailscale.
2. Verifies reachability with ping against your target host.
3. Calls the Komodo endpoint to check and apply stack updates.

### Inputs

| Input | Required | Description |
| --- | --- | --- |
| `ts-oauth-client-id` | Yes | OAuth client ID for Tailscale authentication. |
| `ts-host` | Yes | Target hostname or IP running Komodo. |
| `komodo-port` | Yes | Port where the Komodo API is listening. |
| `komodo-api-key` | Yes | Komodo API key. |
| `komodo-api-secret` | Yes | Komodo API secret. |
| `komodo-stack-id` | Yes | Komodo stack ID to update. |

### Usage

```yaml
name: Deploy

on:
	workflow_dispatch:

jobs:
	deploy:
		runs-on: ubuntu-latest
		steps:
			- uses: actions/checkout@v4

			- name: Deploy with Komodo
				uses: ./.github/actions/deploy-komodo
				with:
					ts-oauth-client-id: ${{ secrets.TS_OAUTH_CLIENT_ID }}
					ts-host: ${{ secrets.TS_HOST }}
					komodo-port: ${{ secrets.KOMODO_PORT }}
					komodo-api-key: ${{ secrets.KOMODO_API_KEY }}
					komodo-api-secret: ${{ secrets.KOMODO_API_SECRET }}
					komodo-stack-id: ${{ secrets.KOMODO_STACK_ID }}
```