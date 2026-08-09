# nfg-hub-catalog

Official product catalog for NFG Hub.

## Source of truth

This repository is the authoritative source for the public NFG Hub catalog.
The `main` branch is served directly to Hub clients: `catalog.json` is the
catalog index, and every entry under `products/` is a versioned product
manifest referenced by that index.

The `catalog/` directory in the NFG Hub application repository is a bundled
fallback snapshot, not a second source of truth. Product metadata and release
history must be changed here first.

## Catalog endpoints

The root `/catalog.json` and every product manifest it references form the
legacy schema-v1 endpoint used by NFG Hub 0.1.1. Root product manifests remain
on `schemaVersion: 1` and continue to receive normal product and release
updates; schema v1 does not mean that their release metadata is frozen.

A future catalog generation will be published separately at
`/v2/catalog.json`. It will be introduced and connected by a newer Hub only
together with the installation-slot stage that can safely switch mutually
exclusive product variants. No v2 endpoint, v2 product manifest, or Spanish
(`es`) localization is published by the catalog today.

## Updating the bundled Hub snapshot

Commit and validate catalog changes in this repository before refreshing the
Hub fallback. After the catalog commit exists, run these commands from a local
NFG Hub repository checkout:

```powershell
dotnet restore .\Nfg.Store.slnx

& .\scripts\catalog-snapshot.ps1 `
    -Sync `
    -AuthoritativeRoot ..\nfg-hub-catalog
```

Confirm that the Hub snapshot exactly matches the committed catalog:

```powershell
& .\scripts\catalog-snapshot.ps1 `
    -Check `
    -AuthoritativeRoot ..\nfg-hub-catalog
```

Review and commit the generated snapshot change separately in the NFG Hub
repository. Never edit the bundled snapshot as the authoritative copy.
