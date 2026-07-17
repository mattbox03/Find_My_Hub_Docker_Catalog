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

### Which store should you choose?

Start with a **personal external store**. It is the recommended route for this
project because you control releases and can test real installations without
waiting for a third-party review. Users still get the normal ZimaOS one-click
installation experience.

Submit to the **official ZimaOS store** only after the external store has been
tested on clean AMD64 and ARM64 hosts, the images use a stable version tag
instead of `edge`, and the public documentation is complete. The official
store provides more visibility but every update goes through review.

### 1. Upload the external store to GitHub

Create an empty **public** GitHub repository with this exact name:

```text
Find_My_Hub_ZimaOS_AppStore
```

Do not initialize it with a README, license, or `.gitignore`, because those
files are already included. The local repository is prepared at:

```text
C:\Users\mattb\Documents\GitHub\Find_My_Hub_ZimaOS_AppStore
```

Upload it from PowerShell:

```powershell
cd C:\Users\mattb\Documents\GitHub\Find_My_Hub_ZimaOS_AppStore
git remote add origin https://github.com/mattbox03/Find_My_Hub_ZimaOS_AppStore.git
git push -u origin main
```

In GitHub, open **Packages**, then open each `find-my-*` package. Under
**Package settings → Change visibility**, change all three packages to
**Public**. This is required for ZimaOS to pull the images without a GitHub
login.

### 2. Add it to ZimaOS

After publishing this directory as the root of the repository, register:

```text
https://github.com/mattbox03/Find_My_Hub_ZimaOS_AppStore/archive/refs/heads/main.zip
```

In the ZimaOS dashboard open **App Store**, click **ADD** at the top right,
paste the URL, and confirm. Wait for the source refresh, search for
**Find My Hub**, then click **Install**.

On older systems exposing only the CasaOS-compatible CLI, use:

```bash
casaos-cli app-management register app-store https://github.com/mattbox03/Find_My_Hub_ZimaOS_AppStore/archive/refs/heads/main.zip
```

The application then appears as **Find My Hub** and installs all four services
with one click. ZimaOS assigns `WEBUI_PORT`; persistent state is kept below
`/DATA/AppData/find-my-hub`.

If the application card appears but installation reports `unauthorized`, at
least one GHCR package is still private. If the store itself cannot be added,
verify that the store repository is public and that the ZIP URL opens without
signing in to GitHub.

## Official ZimaOS store submission

1. Fork `IceWhaleTech/CasaOS-AppStore`.
2. Copy `Apps/FindMyHub` into the fork's `Apps` directory.
3. Run its official validation workflow.
4. Submit a pull request describing the four containers, persistent paths,
   multi-architecture images, and unofficial Apple/Google integrations.

Before that pull request, create a stable project release, publish matching
versioned Docker tags, and replace `:edge` in the app Compose file with that
version. Keep the external store available as the testing channel.

The app manifest contains English and Italian store text. The application UI
itself also keeps its English/Italian language selection persistent.

## Local validation

From this directory:

```bash
docker compose -f Apps/FindMyHub/docker-compose.yml config --quiet
```

This only validates the manifest. A real installation additionally requires
the three public GHCR images listed above.
