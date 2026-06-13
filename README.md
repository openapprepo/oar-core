# OpenAppRepo.app

[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](LICENSE)

**OpenAppRepo.app** is the on-device macOS agent that automatically installs, updates, and manages applications based on community-maintained [recipes](https://github.com/openapprepo/open-app-repo). It runs silently in the background as a LaunchDaemon and keeps your Mac fleet up to date — no user intervention required.

## What Is This?

OpenAppRepo.app consists of two components that run on every managed macOS endpoint:

| Component | Role |
|---|---|
| **OAR Agent** | Installs and updates apps based on YAML recipes and your organization's configuration |
| **OAR Updater** | Keeps the agent itself up to date via a dedicated, isolated update channel |

The agent reads recipes from the [Open App Repo](https://github.com/openapprepo/open-app-repo), applies your organization's configuration (delivered via MDM), and ensures every Mac has the right software at the right version.

## Features

- 🚀 **Automated app lifecycle** — install, update, and remove macOS applications
- 📦 **Supports DMG, PKG, ZIP, and .app** — handles all common macOS distribution formats
- 🔒 **Secure by design** — verifies Apple code signatures (Team ID) before every install
- 📋 **Recipe-driven** — declarative YAML recipes from the community-maintained [Open App Repo](https://github.com/openapprepo/open-app-repo)
- 🔐 **Encrypted configuration** — config delivered as AES-GCM encrypted blobs via CDN
- 📊 **Inventory reporting** — reports installed software versions to your [OAR Console](https://github.com/openapprepo/oar-console)
- 🛡️ **Supply-chain protection** — cryptographically signed recipe manifests (Ed25519)
- ⚡ **Lightweight** — native Swift, minimal resource usage, no runtime dependencies

## Getting Started

### Prerequisites

- macOS 14.0 (Sonoma) or later
- Swift 5.9+
- Xcode 15+ or Swift toolchain
- An MDM solution to deploy configuration profiles

### Building

```bash
# Clone the repository
git clone https://github.com/openapprepo/oar-core.git
cd oar-core

# Build the agent
cd oar-agent
swift build

# Build the updater
cd ../oar-updater
swift build
```

### Packaging

```bash
# Build macOS installer packages
cd packaging
./scripts/build-agent-pkg.sh
./scripts/build-updater-pkg.sh
```

The resulting `.pkg` files can be deployed via your MDM solution (Jamf, Mosyle, Kandji, etc.).

### Configuration

The agent is configured via a `.mobileconfig` MDM profile containing:

| Key | Description |
|---|---|
| `OARRecipeRepoURL` | URL to the recipe repository (default: the official Open App Repo) |
| `OARConfigURL` | URL to the encrypted configuration blob on your CDN |
| `OARReportingURL` | URL of your OAR Console for inventory reporting |

## Project Structure

```
oar-core/
├── oar-agent/         # Background agent (LaunchDaemon)
│   └── Package.swift
├── oar-updater/       # Agent self-updater (LaunchDaemon)
│   └── Package.swift
├── OARLib/            # Shared Swift library
│   ├── Sources/
│   │   ├── Recipe/    # YAML recipe parser
│   │   ├── Crypto/    # AES-GCM encryption (CryptoKit)
│   │   └── Version/   # Version comparison utilities
│   └── Tests/
└── packaging/         # macOS PKG build scripts
```

## Related Projects

| Project | Description |
|---|---|
| [open-app-repo](https://github.com/openapprepo/open-app-repo) | Community-maintained macOS app recipes (YAML) |
| [oar-admin](https://github.com/openapprepo/oar-admin) | macOS SwiftUI admin app — Recipe Wizard, Dashboard, Community Portal |
| [oar-console](https://github.com/openapprepo/oar-console) | Self-hosted web console for configuration management and reporting |
| [open-app-repo-worker](https://github.com/openapprepo/open-app-repo-worker) | Automated workers that keep the recipe repo up to date |

> **Looking for a managed solution?** See the [OpenAppRepo SaaS](https://openapprepo.io) — no self-hosting required.

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/my-feature`)
3. Commit your changes (`git commit -m 'Add my feature'`)
4. Push to the branch (`git push origin feature/my-feature`)
5. Open a Pull Request

Please make sure your code compiles with `swift build` and all tests pass with `swift test`.

## License

This project is licensed under the **GNU Affero General Public License v3.0** — see the [LICENSE](LICENSE) file for details.

## Maintained By

[Cloudcarrier](https://cloudcarrier.nl) · [openapprepo.io](https://openapprepo.io)
