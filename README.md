# OpenAppRepo.app

![GitHub release (latest by date)](https://img.shields.io/github/v/release/openapprepo/oar-core?color=success)
[![SwiftUI](https://img.shields.io/badge/SwiftUI-524520?logo=swift)](https://developer.apple.com/swiftui/)
![macOS](https://img.shields.io/badge/macOS-14%2B-green)
[![License](https://img.shields.io/badge/license-AGPL--3.0-blue)](LICENSE)

<img src="/Screenshots/hero_light.png" width="800">

## Introduction

OpenAppRepo is a macOS application that **automatically installs, updates, and manages third-party applications** on your Mac. It works for everyone — from individual users and freelancers to enterprise IT departments.

* **Automatic app management:** Keep all your apps up-to-date without manual intervention. OpenAppRepo handles downloads, verification, and installation.
* **Self Service:** Browse available apps and install them with a single click through the built-in Self Service interface.
* **Security first:** Every download is cryptographically verified — code signatures, Apple Team IDs, and SHA-256 hashes are checked before installation.
* **Works with or without MDM:** Deploy via `.mobileconfig` profile or configure locally. No MDM required.
* **Community-powered recipes:** App definitions come from the [Open App Repo](https://github.com/openapprepo/open-app-repo) — a community-maintained, open-source recipe database.

## Download

| Version | Download | Requirements |
|---|---|---|
| Latest | [**Download OpenAppRepo.app**](https://github.com/openapprepo/oar-core/releases/latest) | macOS 14.0+ (Sonoma) |

> OpenAppRepo.app is signed and notarized by Apple. Simply download, open, and you're ready to go.

## Getting Started

### Option 1: Standalone (no MDM)

1. Download `OpenAppRepo.app` from [Releases](https://github.com/openapprepo/oar-core/releases/latest)
2. Move to `/Applications/`
3. Open the app — the background agent starts automatically
4. Browse apps in the Self Service window and install what you need

Configuration is stored locally at `~/.config/oar/config.yaml`:

```yaml
# Minimal configuration
repoURL: "https://github.com/openapprepo/open-app-repo"
updateInterval: 3600  # Check every hour
```

### Option 2: Configuration Profile (.mobileconfig)

For organizations or advanced users, deploy a `.mobileconfig` profile — either manually or via MDM:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>PayloadContent</key>
    <array>
        <dict>
            <key>PayloadType</key>
            <string>io.openapprepo.agent</string>

            <!-- Required: Recipe repository -->
            <key>OARRepoURL</key>
            <string>https://github.com/openapprepo/open-app-repo</string>

            <!-- Optional: Reporting server -->
            <key>OARReportingURL</key>
            <string>https://console.openapprepo.io/api/v1</string>

            <!-- Optional: Apps to install automatically -->
            <key>OARManagedApps</key>
            <array>
                <string>google-chrome</string>
                <string>slack</string>
                <string>zoom</string>
                <string>visual-studio-code</string>
            </array>
        </dict>
    </array>
</dict>
</plist>
```

You can install a `.mobileconfig` manually by double-clicking it, or deploy it via your MDM solution.

### Option 3: OAR SaaS (managed)

For a fully managed experience with a web dashboard, APNs push notifications, and team management, use the **OpenAppRepo SaaS** at **[openapprepo.io](https://openapprepo.io)**.

## Features

### Background Agent
- Automatic app installation and updates based on recipes
- Configurable update schedule (hourly, daily, on-demand)
- Smart update deferral — waits for apps to close before updating
- User notifications for pending updates

### Self Service
- Browse available apps by category
- One-click install
- View installed apps and their update status
- Request new apps

### Security
- **Code signature verification** for every download (`codesign` + `spctl`)
- **Apple Team ID validation** against recipe definitions
- **Signed recipe manifest** — Ed25519 signatures ensure recipe integrity
- **AES-256-GCM encrypted configurations** for secure remote management
- All downloads via HTTPS directly from the vendor — no proxy, no middleman

### Reporting
- Inventory reporting to OAR Console or OAR SaaS
- Smart polling with ETags (minimal server load)
- APNs push support for near-instant updates (SaaS)
- Event-driven check-in on boot, wake, and app changes

## CLI Usage

OpenAppRepo also includes a command-line interface:

```bash
# Install an app
oar install google-chrome

# Update all managed apps
oar update

# List installed apps and their status
oar list

# Show agent status and configuration
oar status

# Remove a managed app
oar remove slack
```

## Configuration Reference

| Key | Type | Description |
|---|---|---|
| `OARRepoURL` | String | URL to the recipe repository (required) |
| `OARReportingURL` | String | URL to report inventory to (optional) |
| `OARManagedApps` | Array | Apps to install and keep updated |
| `OARUpdateInterval` | Integer | Seconds between update checks (default: 3600) |
| `OARRecipeSigningKey` | String | Ed25519 public key for recipe verification (enterprise forks) |

## Building from Source

> **Note:** Most users should download the [signed release](https://github.com/openapprepo/oar-core/releases/latest). Build from source only if you want to contribute or customize.

```bash
git clone https://github.com/openapprepo/oar-core.git
cd oar-core
swift build
```

Requirements: Xcode 15.0+, macOS 14.0+, Swift 5.9+

## Related Projects

| Project | Description |
|---|---|
| [open-app-repo](https://github.com/openapprepo/open-app-repo) | Community-maintained app recipes |
| [oar-admin](https://github.com/openapprepo/oar-admin) | macOS admin app for recipe management |
| [oar-console](https://github.com/openapprepo/oar-console) | Self-hosted web console for fleet management |
| [open-app-repo-worker](https://github.com/openapprepo/open-app-repo-worker) | Automated recipe maintenance worker |

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

OpenAppRepo is licensed under the [GNU Affero General Public License v3.0](LICENSE).

---

*Built with ❤️ by [Cloudcarrier](https://cloudcarrier.nl) · [openapprepo.io](https://openapprepo.io)*
