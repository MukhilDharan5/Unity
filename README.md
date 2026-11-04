# Unity

Unity is a local-first platform for connecting your phone and computers. It lets paired devices communicate and adds optional tools for sharing, notifications, media, calls, messages and remote control. This is the Unity device-connectivity project, unrelated to the Unity game engine.

**This repository is the documentation hub.** It explains Unity, its three feature tiers and the repositories that implement it. Application source, builds and tests belong in the linked repositories.

## Unity Core, Basic and Advanced

These are feature tiers across the platform repositories, not three additional repositories.

| Tier | Purpose | Scope |
| --- | --- | --- |
| [Unity Core](docs/core.md) | Communication layer only | Device identity, discovery, explicit pairing, authenticated encrypted connections, capability negotiation, generic message routing, connection lifecycle and revocation. No user-facing device features. |
| [Unity Basic](docs/basic.md) | A simple KDE Connect / Microsoft Phone Link alternative | Core plus everyday sharing, battery status, notifications, ordinary media controls, find phone and optional Phone/Messages integration through normal OS permissions and explicit consent. |
| [Unity Advanced](docs/advanced.md) | The full Unity feature set | Basic plus device-specific integrations, high-privilege access, experimental audio/display features, unattended control and other unstable or potentially insecure features. Each requires its own setup and risk review. |

Basic describes a similar use case to KDE Connect and Phone Link; Unity uses its own protocol and does not claim compatibility or complete feature parity with either product.

**Current status:** the source is organized into separate repositories. Core, Basic and Advanced define the intended architecture and feature boundaries; the existing platform sources still contain a combined implementation. Separate tier builds and enforced privilege isolation have not been implemented by this reorganization. The existing Linux crate called `unity-core` also contains feature orchestration and is not yet the communication-only Unity Core described here.

## Unity repositories

| Repository | Owns |
| --- | --- |
| [unity](https://github.com/MukhilDharan5/Unity) | This documentation hub: project explanation, tiers, architecture boundaries and repository directory. |
| [unity_linux](https://github.com/MukhilDharan5/Unity_Linux) | Linux communication service, D-Bus API, CLI, GTK app and Linux integrations. |
| [unity_windows](https://github.com/MukhilDharan5/Unity_Windows) | Windows communication host, native Windows app and Windows integrations. |
| [unity_android](https://github.com/MukhilDharan5/Unity_Android) | Android connectivity companion, Android integrations and the local bridges to Phone and Messages. |
| [unity_Phone](https://github.com/MukhilDharan5/Unity_Phone) | Unity Phone for Android and Linux: dialer, contacts, call history and desktop phone client. |
| [unity_messages](https://github.com/MukhilDharan5/Unity_Messages) | Unity Messages for Android and Linux: SMS ownership and desktop messaging client. |
| [unity_Continuity_companion](https://github.com/MukhilDharan5/Unity_Continuity_Companion) | Firefox extension, browser session continuity, extension packaging and browser tests. |

The repository links above identify each project’s source and documentation. Clone the repositories you need; the documentation hub does not include application source.

Phone and Messages are separate applications. Android owns calls, contacts, SMS, system roles and permissions. Their Linux clients use Unity's authenticated communication service; neither app introduces a second network transport or pairing system. Their Android apps can be used locally without a desktop connection. Windows implementation details and limits remain in the Windows repository; this split does not add Windows Phone or Messages applications.

See the [repository guide](docs/repositories.md) for ownership and development layout, and the [migration record](docs/migration.md) for how the existing source and work in progress were preserved.

## Connection and permission model

Unity's existing implementations communicate directly over authenticated Wi-Fi/LAN and Bluetooth Classic RFCOMM. Local features do not require an account or a mandatory cloud relay. Features depend on the capabilities, OS permissions and policies of both paired devices.

Communication trust is separate from permission to use a feature. Pairing a device must not imply approval for screen capture, remote input, unlocking, authentication, call control or messages. Core carries authorized messages; platform adapters and the Phone/Messages apps own feature behavior and permission checks. Advanced expands access and risk even when its traffic is encrypted.

Platform build instructions, protocol details, compatibility and validation evidence belong in each implementation repository. A documented or compiled feature is not evidence that it works reliably on every physical device.
