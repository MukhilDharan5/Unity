# Repository ownership

Unity is split by source ownership into seven repositories. Core, Basic and Advanced describe feature layers inside those implementations; they do not add more repositories. This documentation repository contains no application source, build system, SDK or shared executable library.

| Repository | Source root in the existing workspace | Ownership |
| --- | --- | --- |
| [unity](https://github.com/MukhilDharan5/Unity) | Workspace root | Explanation, tiers and repository directory. |
| [unity_linux](https://github.com/MukhilDharan5/Unity_Linux) | `Unity_Linux/` | Rust daemon/crates, D-Bus contract, CLI, GTK app, Linux helpers, packaging and tests. |
| [unity_windows](https://github.com/MukhilDharan5/Unity_Windows) | `Unity_Windows/` | .NET solution, Windows host, transport/platform adapters, WinUI app, native browser host, packaging and tests. |
| [unity_android](https://github.com/MukhilDharan5/Unity_Android) | `Unity_Android/` | Kotlin connectivity app, transport/protocol, permission flows, platform feature adapters, Phone/Messages bridges and tests. |
| [unity_Phone](https://github.com/MukhilDharan5/Unity_Phone) | `Unity_Phone/` | Android `app/` and `ipc/`, Linux `linux/`, Phone adapters, tests and build tools. |
| [unity_messages](https://github.com/MukhilDharan5/Unity_Messages) | `Unity_Messages/` | Android `app/` and `ipc/`, Linux `linux/`, Messages adapters, tests and build tools. |
| [unity_Continuity_companion](https://github.com/MukhilDharan5/Unity_Continuity_Companion) | `Unity_Continuity_Companion/` | Firefox extension in `firefox-session-continuity/`, browser storage/restoration, extension build/packaging and tests. |

Each source root is an independent Git repository with its own `origin`. In the current workspace they are ignored local checkouts inside the documentation root, not submodules. They do not appear when cloning the documentation repository. To preserve the existing build-tool fallback and integration paths, keep the source checkouts side by side using the directory names shown above. Cloning a repository into that source root places its files directly there; do not create another same-named directory inside it.

The repository URLs use GitHub's existing capitalization; the logical names requested for the split are shown in the first column. Each repository contains its own source and documentation.

## Dependencies and contracts

Core communication is implemented by each platform. Wire protocol documentation currently lives in [Unity Linux](https://github.com/MukhilDharan5/Unity_Linux/blob/main/docs/protocol.md); matching protocol/security behavior must be coordinated with Windows and Android. A source split does not create wire compatibility by itself.

Unity Phone and Unity Messages own their Android roles, providers, domain commands and interfaces. The Android connectivity app explicitly binds to their approved IPC services and forwards authorized traffic. Linux frontends use Unity's public session D-Bus API. They do not own network sockets or a second trust store.

Android Phone/Messages can build with their own Gradle wrappers and configured SDK/JDK. Messages can optionally reuse the adjacent Android companion toolchain. Their integration scripts accept explicit checkout paths when using a different layout. Phone installation accepts `--unity-linux-root` for a non-sibling Linux checkout; the Linux installer accepts `--phone-source` for a non-sibling Phone checkout. Installed runtimes use IPC, not source-tree imports.

The Linux daemon and GTK client install independently of Phone. `--component all` installs Linux's own daemon and GTK app. Install Phone explicitly with `--component phone` or its own Linux installer. Messages has its own Linux installer. Phone/Messages integration and cross-platform interoperability tests require their corresponding checkouts and are distinct from communication-only ownership.

Windows publishing uses its own license and bundled scrcpy license. Its optional Kotlin interoperability test needs an Android source checkout; building the Windows application does not need one. Linux scrcpy tests validate the Linux-downloaded server; cross-platform tests can separately compare Android's bundled copy.

## Firefox extension

[Unity Continuity Companion](https://github.com/MukhilDharan5/Unity_Continuity_Companion) is the independent Firefox extension repository. Its source remains in `firefox-session-continuity/` with its own build, packaging and tests. Linux and Windows own their native messaging hosts; the extension owns browser capture, local storage, restoration and browser permissions. It can work locally without a Unity host; the optional native integration uses the compatible platform host. Its `test:unity` integration test defaults to the adjacent `Unity_Linux` checkout and accepts `UNITY_LINUX_ROOT` for another layout. The stable extension identifier is preserved.

## Licensing and release status

The original root GPL license is retained in the hub and copied to Linux, Windows, Android and Continuity Companion, whose source previously lived under that license. Messages keeps its existing license. Phone's existing documentation says no license has been assigned to its original source; this split does not assign one. Keep third-party license notices with their assets.

Publishing the source does not rebuild, sign, install or validate the products on physical hardware. See platform-specific build guides and evidence for existing functionality. Published release artifacts are separate from source control; caches, credentials, debug keys, build directories and local APK distributions are excluded from the new repositories.
