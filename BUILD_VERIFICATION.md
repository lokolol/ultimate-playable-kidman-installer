# Build verification — R537

Verified on Windows x64 using the .NET Framework C# compiler.

The distributed installer is compiled directly from the three C# source files
in src/. Their SHA-256 hashes, the distributed Install.exe hash and the pinned
manifest hash are recorded in verification.json and release.json.

All 20 isolated synthetic tests passed. They cover exact installation and
restoration, rollback and interrupted-install recovery, invalid/corrupt inputs,
R509 journal migration, separate installer/content folders, and known atlas
upgrade behavior. The R537 build did not rerun the prior private real-game-atlas
fixture test; no real game files were modified by the synthetic tests.

R537 separates the installer/manifest download from the Nexus content. Tests
cover installation from a separately selected content folder and restoration
without that content. Atlas tests cover upgrading from the previously installed
R485 atlas and R509 journal, exact two-step restoration and corrupt-upgrade
rejection.

The Framework build is not deterministic; no normalized-byte identity claim is
made for a later independent build. Earlier installer source versions remain
in Git history.

This verification covers Install.exe only. Complete buildable source for the
native gameplay DLL remains unavailable, as disclosed in README.md.
