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
