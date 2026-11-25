# Swarlay backing tracks

The catalog the Swarlay app reads to list backing tracks it can download.

- `catalog.json` is the list. The app fetches it from
  `https://raw.githubusercontent.com/mayurprajapati/swarlay-backing-tracks/main/catalog.json`.
- The audio files are attached to the GitHub release tagged **`tracks`**, not committed to the repo
  (repo files are capped at 100 MB; release files can be up to 2 GB).

## Adding a track

1. Upload the audio file to the `tracks` release (Releases → `tracks` → Edit → attach file).
   Use a plain file name: lowercase, dashes, no spaces, e.g. `tanpura-c-warm-pad.m4a`.
2. Add an entry to `tracks` in `catalog.json` and bump `updated`.
3. Commit. Phones pick up the change the next time the app refreshes the catalog.

```json
{
  "id": "tanpura-c-warm-pad",
  "type": "tanpura",
  "title": "Warm pad",
  "sa": "C",
  "bpm": null,
  "taal": null,
  "file": "tanpura-c-warm-pad.m4a",
  "sizeBytes": 4823112,
  "durationSec": 600,
  "loop": true,
  "sha256": "…",
  "source": "https://www.youtube.com/watch?v=…"
}
```

| Field | Required | Meaning |
|---|---|---|
| `id` | yes | Unique, never changes. The app stores the download under this id. Change it and the app treats it as a new track. |
| `type` | yes | `tabla`, `tanpura` or `taal` (one of `types[].id`). One of each type can play at the same time. |
| `title` | yes | Name shown in the app. |
| `sa` | tanpura, tabla | Key the track is in: `C`, `C#`, `D`, … `B`. |
| `bpm` | tabla, taal | Tempo. `null` for a drone. |
| `taal` | taal, tabla | e.g. `teentaal`, `ektaal`, `jhaptaal`, `rupak`, `keherwa`, `dadra`. |
| `file` | yes | File name in the `tracks` release, or a full `https://` URL. |
| `sizeBytes` | yes | Shown before download; also used to check the download finished. |
| `durationSec` | no | Length in seconds. |
| `loop` | no | `true` if the file is meant to repeat seamlessly. Default `true`. |
| `sha256` | no | If set, the app rejects a download that doesn't match. |
| `source` | no | Where the recording came from, for credit. |

Get `sizeBytes` and `sha256` in PowerShell:

```powershell
(Get-Item .\tanpura-c-warm-pad.m4a).Length
(Get-FileHash .\tanpura-c-warm-pad.m4a -Algorithm SHA256).Hash.ToLower()
```
