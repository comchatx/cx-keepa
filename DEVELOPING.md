# Developing cx-keepa

## Install

```bash
pi install npm:cx-keepa
```

## How it works

The package ships the `keepa` skill and its references, and three prompt templates. The calls themselves go through ComChatX's data gate, which holds the key: the platform's Keepa key, or a Keepa API Key the seller saved on the plugin's Settings, which the gate uses first. A call on the seller's key is paid to Keepa from the seller's own tokens and moves nothing on the ComChatX balance.

## Build, test and release

```bash
npm ci
npm run typecheck
npm test
```

A release is a tag `v<version>` on `main`; `.github/workflows/release.yml` publishes it to npm with provenance.
