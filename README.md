# Spectre

On-set frequency coordination across 2.4 / 5 / 6 GHz,a quick visual read
of which channels are free before assigning wireless video, follow-focus,
or WiFi on a shoot.

 <img width="1073" height="829" alt="Capture d’écran 2026-08-13 à 21 35 06" src="https://github.com/user-attachments/assets/e21e6222-18c7-43ab-8b0e-b4530cef8264" />

## Use it

Open **`spectre.html`** in your browser (double-click it). That's the whole
app — nothing to install, no internet connection needed.

Pick a band (2.4 / 5 / 6 GHz) at the top of the spectrum panel — each band
keeps its own independent set of placed frequencies, since they can't
interfere with each other. Click a channel to add it to the set. Uncheck a
device you don't have on set to declutter the view. Use "Best channel" to
see which channels of a given device are still free given what's already
placed. "+ Add device" adds a one-off custom frequency (e.g. a walkie) for
the current session — it isn't saved to the database, see below for that.

Everything you place is remembered automatically (browser local storage) —
no save button, no export file.

## Files

Only two files, both plain text:

| File | What it is |
|---|---|
| `spectre.html` | The app. Open this one. |
| `channels.json` | The frequency database: every band, device, channel width and center frequency. Edit this to add a device or fix a value — no build step. |

`channels.json` is technically loaded as a small script (`window.SPECTRE_DATA = {...}`)
rather than raw JSON — that's what lets `spectre.html` read it with a plain
double-click. Browsers block a local page from fetching a separate local
JSON file for security reasons (no such restriction exists for `<script src>`),
so this sidesteps that without needing a local server or a build step.
Keep the `window.SPECTRE_DATA = ` prefix and trailing `;` when you edit it.

## Editing the frequency database

Open `channels.json`. Shape:

```js
window.SPECTRE_DATA = {
  "bands": [
    {
      "id": "2.4",
      "label": "2.4 GHz",
      "devices": [
        {
          "name": "ARRI White Channel",
          "channels": [
            { "id": "0", "freqMHz": 2410, "bandwidthMHz": 6, "type": "video" }
          ]
        }
      ]
    }
  ]
};
```

- `type` is `"video"` for a normal channel, or `"bug"` / `"sync"` for an
  auxiliary one (shown with a dashed outline) — only CINE RT uses this.
- To add a channel to an existing device, add an entry to its `channels` array.
- To add a whole new device, add an object to a band's `devices` array.
- To add a new band, add an object to `bands` with a unique `id`.

Save, then reload `spectre.html` — no rebuild needed.
