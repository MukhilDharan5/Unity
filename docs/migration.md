# Repository migration

On 4 November 2026, the existing Unity workspace was separated into a documentation hub and six independent implementation repositories. Source-directory spelling was kept to avoid invalidating existing toolchain paths and sibling integration defaults. The hub uses the existing Unity GitHub repository; each implementation uses its matching existing repository. The subsequent publication uses the local source as authoritative and preserves existing remote commits in the Git history.

The current hub index contains documentation and its license only. Local platform directories are ignored independent checkouts, not Git submodules or application source tracked by the hub. Historical commits in the hub can still contain the previous monorepo; the split does not rewrite that history.

Existing Linux, Android, Windows and Continuity Companion history is imported with each former directory promoted to its repository root. The existing work in progress is included in each repository’s publication commit. Phone and Messages were previously untracked in the monorepo and receive their own initial source commits. The Firefox extension remains in its own Unity Continuity Companion repository under `firefox-session-continuity/`, with its original history imported independently. Linux and Windows retain only their platform native hosts.

The ignored local `.unity-split-backup/` contains the original Git bundle, source archive (including existing work in progress), file hashes, index/config copies and initial status. It is a recovery aid on this machine and is never part of the documentation repository or a public release. Inspect it before restoring anything; restoring the original index or archive over newer edits can overwrite later work.

Only documentation, repository metadata, ignores, license placement and build/install/test paths needed for independent checkouts were changed. The application feature implementations were not separated into enforceable Core/Basic/Advanced builds. Existing combined apps still contain their Advanced features.

Each implementation has its own source commit. The documentation hub has a separate commit that removes the former monorepo source from its current tree. Existing remote history is retained; where it differs, the published source tree follows the local version as requested.
