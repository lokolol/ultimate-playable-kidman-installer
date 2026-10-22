# Build verification — R509

Verified on Windows x64 using the .NET Framework C# compiler.

The distributed installer is compiled directly from the three C# source files
in src/. Their SHA-256 hashes, the distributed Install.exe hash and the pinned
manifest hash are recorded in verification.json and release.json.

20 isolated synthetic tests passed. An additional private test reconstructed,
installed and restored the actual 2,736,146,304-byte native Chapter6 atlas using
64-bit offsets. The real game installation was read as the fixture source and
was not modified by those tests. Temporary fixture copies were removed.

R509 separates the installer/manifest download from the Nexus content. Tests
cover installation from a separate selected content folder and restoration
without that content. New atlas tests cover upgrading from the previously
installed R485 atlas, exact two-step restoration and corrupt-upgrade rejection.

The Framework build is not deterministic; no normalized-byte identity claim is
made for a later independent build. Earlier installer source versions remain
in Git history.

This verification covers Install.exe only. Complete buildable source for the
native gameplay DLL remains unavailable, as disclosed in README.md.
