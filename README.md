# Find My Hub ZimaOS/CasaOS App Store

This directory is a complete standalone App Store repository root. Publish the
**contents of this directory** to a public GitHub repository named
`Find_My_Hub_ZimaOS_AppStore`; do not wrap them in another parent directory.
The included validation workflow checks JSON, Compose syntax, and anonymous
access to every required image on each push.

## Required before publishing

The following GHCR packages must be public and expose both `linux/amd64` and
`linux/arm64` manifests:

- `ghcr.io/mattbox03/find-my-web:edge`
- `ghcr.io/mattbox03/find-my-apple-provider:edge`
- `ghcr.io/mattbox03/find-my-google-provider:edge`

At the time this package was generated, anonymous pulls returned
`unauthorized`. A one-click store installation cannot work until the three
packages are changed to **Public** in their GitHub Package settings.

The source repository containing this application and the standalone store
repository must also be public before an official submission.

## External store

After publishing this directory as the root of the repository, register:

```text
https://github.com/mattbox03/Find_My_Hub_ZimaOS_AppStore/archive/refs/heads/main.zip
```

ZimaOS can register the URL from its third-party App Store interface. On
systems exposing only the CasaOS-compatible CLI, use:

```bash
casaos-cli app-management register app-store https://github.com/mattbox03/Find_My_Hub_ZimaOS_AppStore/archive/refs/heads/main.zip
```

The application then appears as **Find My Hub** and installs all four services
with one click. ZimaOS assigns `WEBUI_PORT`; persistent state is kept below
`/DATA/AppData/find-my-hub`.

## Official ZimaOS store submission

1. Fork `IceWhaleTech/CasaOS-AppStore`.
2. Copy `Apps/FindMyHub` into the fork's `Apps` directory.
3. Run its official validation workflow.
4. Submit a pull request describing the four containers, persistent paths,
   multi-architecture images, and unofficial Apple/Google integrations.

The app manifest contains English and Italian store text. The application UI
itself also keeps its English/Italian language selection persistent.

## Local validation

From this directory:

```bash
docker compose -f Apps/FindMyHub/docker-compose.yml config --quiet
```

This only validates the manifest. A real installation additionally requires
the three public GHCR images listed above.
