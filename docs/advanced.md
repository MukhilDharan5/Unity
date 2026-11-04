# Unity Advanced

Unity Advanced is the full Unity feature set: Basic plus device-specific, high-privilege and experimental integrations. Some paths are unstable or potentially insecure, bypass normal interaction barriers, or expose unusually sensitive control. They are intended for deliberate opt-in and device-specific evaluation, not blanket claims of safety or reliability.

## Advanced scope

| Area | Examples in the existing project | Access and reliability concerns |
| --- | --- | --- |
| Privileged Android integration | Shizuku services, background clipboard reads and privileged media-output selection. | Shell-level access and Android/OEM restrictions; authorization may change after restart. |
| Phone screen/control | scrcpy over Unity, accessibility gestures and remote keyboard/input paths. | Screen contents and input control are sensitive; privileged setup and device compatibility vary. |
| Saved phone unlock | Desktop keyring/DPAPI-backed PIN/password handling, wake/unlock helpers. | Stored credentials expand the impact of a compromised authorized desktop; successful unlocking is device-specific. |
| Desktop authentication | Phone biometric approval for PAM, sudo or login, and administrator enrollment. | Changes to the authentication boundary can affect access to the computer; separate from Core pairing. |
| Unattended desktop control | Phone-initiated screen sharing, remote input and per-device unattended grants. | May allow control without a fresh local prompt; elevated/secure-desktop behavior is restricted by the OS. |
| Display changes | Virtual/extended displays, phone-as-main-display, panel-off or blank-screen modes. | Compositor/driver-specific behavior, capture failures and layout restoration risks. |
| Audio experiments | Bidirectional capture/streaming, HFP/oFono/WirePlumber integration and output/profile switching. | Hardware, OS and carrier differences, latency and routing failures; Windows HFP support differs from Linux. |
| Remote locking | Android device administration and desktop lock integration. | Can interrupt local use; permissions and recovery behavior differ across platforms. |
| OEM connectivity | Samsung Routines, hotspot triggers and Bluetooth-device disconnect actions. | Vendor/version-specific setup; submitting a trigger does not prove the requested change happened. |
| Browser continuity | Firefox native messaging, session summaries and restore commands. | Browser/page compatibility and sensitive session metadata; permission and capture limits remain relevant. |
| Additional data collection | Optional detailed Windows usage/telemetry collection. | Sensitive local data and retention settings require their own consent and review. |

The table assigns current features to a product tier. It does not claim every platform implements every row or that all features require root. Consult each platform's actual feature and validation documentation.

## Permission boundary

Advanced includes Basic but adds separate permissions; enabling Advanced must not implicitly grant all its capabilities. Device pairing is not permission to unlock, authenticate, capture, inject input, control calls or change network/display configuration.

Privileged adapters should keep credentials and OS access local, validate bounded operations, expose specific capabilities, and stop accepting commands when consent or pairing is revoked. Disconnect/stop/error cleanup should restore temporary audio and display state. These are design requirements; the repository split itself does not implement or verify stronger isolation, recovery or security guarantees.

No plaintext or unauthenticated network mode is part of Advanced. Existing encrypted transport does not remove the risks of a powerful command, compromised peer, saved credential or buggy privileged helper. “Unsecure features” in the project description refers to these potentially unsafe or unaudited paths, not a promise to disable transport security.

## Where the source lives

- [Unity Linux](https://github.com/MukhilDharan5/Unity_Linux): Linux privileged helpers, PAM/authentication, screen/display/audio/OEM orchestration and the Linux Firefox native host.
- [Unity Windows](https://github.com/MukhilDharan5/Unity_Windows): Windows capture/input/display/audio, DPAPI unlock storage, local collection and Firefox host.
- [Unity Continuity Companion](https://github.com/MukhilDharan5/Unity_Continuity_Companion): the Firefox extension, browser checkpoint storage, restoration, browser permissions and native-host client integration.
- [Unity Android](https://github.com/MukhilDharan5/Unity_Android): Android Shizuku/scrcpy, privileged clipboard/output, capture, input and OEM adapters.
- [Unity Phone](https://github.com/MukhilDharan5/Unity_Phone) and [Unity Messages](https://github.com/MukhilDharan5/Unity_Messages): their app-owned domain behavior and explicit integration contracts. Transport and privileged platform workarounds remain in the platform repositories.

The current sources are combined builds. Separate Advanced packaging and verified Core/Basic isolation remain follow-up work. See [Basic](basic.md) and [repository ownership](repositories.md).
