# Unity Core

Unity Core is only the communication layer shared conceptually by Unity's platform implementations. It makes approved devices able to exchange bounded, authenticated messages. It is not a desktop app, phone app, automation engine or collection of device features.

## Responsibilities

- Device identity and local discovery or explicit endpoint entry.
- User-reviewed pairing, peer authentication, encrypted sessions and credential storage/revocation.
- Transport adapters for Wi-Fi/LAN and Bluetooth, framing, size limits and backpressure.
- Connection state, heartbeats, reconnection, transport selection and session cleanup.
- Protocol versions, advertised capabilities and generic request/event/response routing.
- A local API through which separately authorized applications and feature adapters use the connection.

Bluetooth and LAN are transport choices; neither implies an approved user feature. Revoking a peer ends its sessions. Revoking a local client or feature must also prevent its subsequent operations.

## Outside Core

Battery reporting, clipboard access, file selection/storage, notifications, media sessions, audio capture, screen capture, input injection, device locking, hotspot changes, calls, contacts and SMS are feature behavior. Even if those features use Core to carry their messages, Core must not directly access their OS providers or ask for their permissions.

Shizuku, root/administrator helpers, Accessibility, device administration, stored phone unlock credentials, PAM/login approval and OEM automation are outside Core. They belong to explicit Advanced adapters where applicable. Pairing authentication belongs to Core; using a phone to approve sudo or login belongs to Advanced.

Generic transport streaming, integrity checks and routing are communication mechanisms. A user-facing file transfer, gallery or media stream adds its own authorization, storage and OS integration outside Core. Core must not silently enable a feature just because the peer advertises it.

## Repository placement and current implementation

Core is implemented within the platform repositories, not in this documentation repository and not in an additional `unity_core` repository:

| Platform | Communication implementation and extraction starting points |
| --- | --- |
| [Linux](https://github.com/MukhilDharan5/Unity_Linux) | `unity-protocol`, `unity-transport`, `unity-security`, generic session/operation handling and the public `unity-ipc` boundary. |
| [Windows](https://github.com/MukhilDharan5/Unity_Windows) | Generic protocol, identity/session code in `Unity.Core` and transport adapters in `Unity.Platform`. |
| [Android](https://github.com/MukhilDharan5/Unity_Android) | Protocol, pinned TLS, credentials, connection management and transport selection in the connectivity companion. |

Names in the current source are broader than these architectural boundaries. In particular, Linux `unity-core`, Windows `Unity.Core` and Android's companion service also orchestrate features. They are not already isolated Core-only distributions. Extracting pure communication interfaces and enforcing dependency boundaries is follow-up implementation work; moving repositories does not provide that isolation.

Phone and Messages consume this layer through the public Linux API and explicit Android Binder bridges. Their domain models and business logic remain in their own repositories.
