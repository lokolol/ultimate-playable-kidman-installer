# Ultimate Playable Kidman — installer source and tools

By **JimJAmes**. Source for the Windows x64 R509 installer for The Evil Within.
This is source available for inspection and private builds under the included
restrictive license, not an open-source license.

## Download and install

1. Download the matching **R509 Content** ZIP from the mod's Nexus page and extract it completely.
2. Open [Releases](https://github.com/lokolol/ultimate-playable-kidman-installer/releases/tag/R509) and download `Ultimate_Playable_Kidman_R509_Installer.zip`.
3. Extract the tools ZIP. Keep `Install.exe` and `PAYLOAD_MANIFEST.json` together.
4. Close The Evil Within and run `Install.exe`.
5. Select the game folder containing `EvilWithin.exe`, then click **Install R509**.
6. When prompted, select the extracted Nexus folder containing `Payload`.
7. Wait for completion and launch the game through Steam.

To restore the previous files, close the game, run the same installer, select
the game folder and click **Restore previous files**. The extracted content is
not needed for restoration. Preserve the game's `.JimJAmes_UPK` recovery folder.
Screen/resolution fixes are excluded.

## Scope and DLL limitation

This repository covers **Install.exe only**. It contains no game assets,
Nexus payload, personal settings or signing keys. The official release's
manifest is a download asset, separate from the content ZIP.

The mod also distributes a native gameplay DLL. Complete buildable source for
that current DLL is unavailable. Historical sources do not reproduce it and
are not represented as its source. `release.json` records the distributed DLL
hash. This repository does not establish complete source coverage for the
whole mod or guarantee Nexus approval; the DLL source limitation must be
disclosed separately to reviewers.

## Build and test

Download `Ultimate_Playable_Kidman_R509_Source.zip` from the matching release
and extract it. Run the commands below from that extracted source folder.

Requirements: Windows x64, Windows PowerShell 5.1 or later, and the .NET Framework
C# compiler at `%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\csc.exe`.
Uses System.Runtime.Serialization, System.Windows.Forms and System.Drawing.
No NuGet, Python, network or game installation is needed for the source build.

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Build.ps1
```

The script compiles `build/Install.exe` and runs 20 isolated synthetic tests.
Fixtures are created in a unique `UPK_...` folder under Windows TEMP, then
removed on success. Results remain in `build/test_results.txt`. Tests cover
exact installation/restoration, idempotence, invalid/corrupt inputs, cancellation,
rollback, interruption recovery, path validation, preserving later edits,
R461/464/466/472/478/483/485 journals, separate installer/content folders, and
R485 atlas upgrades with exact two-step restoration and corrupt-delta rejection.

An optional twenty-first test uses the real large native atlas. Its assets are
not included. Reviewers with matching lawfully possessed files can run:

```powershell
.\build\Tests.exe C:\TemporaryFixtures C:\MatchingPayload C:\MatchingInstalledGame
```

`MatchingPayload` must contain the matching `PAYLOAD_MANIFEST.json` and the
largest texture patch at its specified relative path. This test writes only
under the supplied temporary fixture folder. Never use your actual game folder
as the first argument.

To validate the official manifest during compilation:

```powershell
.\Build.ps1 -PayloadManifest 'C:\DownloadedTools\PAYLOAD_MANIFEST.json'
```

Its exact SHA-256 is embedded in the launcher from `release.json`. The manifest
is not needed to compile. To run a rebuilt installer, put it next to the matching
official manifest; select the separately extracted Nexus content when prompted.
`examples/PAYLOAD_MANIFEST.example.json` is synthetic and cannot replace it.

The Framework compiler is not deterministic in this setup. PE timestamps and
assembly identifiers may differ between builds, so whole-file hash differences
alone do not establish logic changes.

## Implementation

`src/Launcher.cs` provides WinForms, Steam discovery, manifest validation and
selection of the separate content folder. `src/Engine.cs` validates paths,
executable and payload hashes; stages full files; applies exact append/header
atlas changes; and journals transactions for restoration. Known R485 atlases
have a verified small upgrade delta; clean installations use cumulative deltas.
Unknown modified atlas files are rejected before installation changes.
The installer does not download or run network code, install services or change
display settings. See `BUILD_VERIFICATION.md` and `verification.json`.
