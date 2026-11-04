# Unity Basic

Unity Basic is the everyday companion tier: a simple KDE Connect / Microsoft Phone Link alternative built on Unity Core. Its purpose is useful phone/computer integration through ordinary supported OS APIs, normal permissions and explicit user consent. It does not promise those products' full feature sets or wire-protocol compatibility.

## Basic scope

| Area | Intended Basic behavior |
| --- | --- |
| Connection | Core pairing, connection status and reconnection. |
| Device status | Battery/charging and ordinarily accessible device/network information. |
| Sharing | Explicit text, links and files; user-granted folders and photos. |
| Clipboard | User-initiated or OS-permitted clipboard exchange; no privileged background-read workaround. |
| Notifications | Opt-in notification mirroring and replies/actions exposed by supported OS APIs. |
| Media | Published media sessions, supported playback controls and ordinary volume controls. |
| Find phone | An explicitly authorized ring/stop request using normal platform APIs. |
| Phone integration | Optional Unity Phone app, OS Phone role/permissions and explicit companion consent. |
| Messages integration | Optional Unity Messages app, OS SMS role/permissions and explicit companion consent. |

OS-granted notification access and default Phone/SMS roles still expose sensitive information. They remain separate opt-ins with visible state and revocation; “Basic” does not mean permission-free or audited secure.

## Optional applications

[Unity Phone](https://github.com/MukhilDharan5/Unity_Phone) owns Android calling, contacts and history, plus the Linux Phone interface. Android Telecom remains responsible for calls and emergency-call restrictions. The Unity companion transports authorized commands and events; it should not regain its own dialer/provider implementation.

[Unity Messages](https://github.com/MukhilDharan5/Unity_Messages) owns Android SMS storage/provider operations, SMS permissions, default-app setup and the Linux inbox/composer. The current implementation is SMS-only; MMS and RCS are unsupported. Sending an SMS is not proof of carrier delivery.

Basic is usable without either optional application. Missing installation, consent, role or permission should leave the corresponding feature unavailable.

## Boundary with Advanced

Basic must not depend on Shizuku, root, administrator access, credential-based remote unlocking, PAM, Accessibility input injection, unattended screen control or vendor-specific routine triggers. A feature requiring one of those paths belongs to Advanced, even if a similar ordinary-permission feature is Basic. Ordinary media controls are Basic; privileged output switching and experimental audio routing are Advanced. File sharing is Basic; arbitrary filesystem access is not.

The existing automatic Android background clipboard path uses Shizuku and therefore belongs to Advanced. It must not be advertised as an already independent Basic implementation. Screen/audio streaming, virtual displays and remote locking are also assigned to Advanced in this project.

## Current delivery status

Basic is a defined feature boundary, not a separate APK, executable, build profile or enforced sandbox today. Platform repositories currently contain the full combined implementation, including Advanced paths. Feature switches alone are not proof that a build has no privileged code or permissions. Publishing a Basic-only distribution requires implementation and verification in each platform repository.

See [Core](core.md), [Advanced](advanced.md) and the [repository directory](repositories.md).
