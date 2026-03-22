# Krutons Usage Plugin

A live CPU, RAM, and GPU usage monitor for [OpenDeck](https://github.com/nekename/OpenDeck) — rendered as smooth arc gauges directly on your keypad buttons.

![CPU Gauge](images/cpu-icon.png) ![RAM Gauge](images/ram-icon.png) ![GPU Gauge](images/gpu-icon.png)

---

## Features

- **Live arc gauges** for CPU load, RAM usage, and GPU utilisation — updates every 2 seconds
- **Per-button colour** — pick any arc colour, each gauge is styled independently
- **Custom label** — override the default CPU / RAM / GPU text with anything up to 8 characters
- **Arc thickness** — four sizes: Thin, Normal, Thick, Chunky
- **Warning threshold** — set the % at which the arc switches to the warning colour (default 80%)
- **Custom warning colour** — the "hot" colour is configurable, not just hardcoded red
- **Show/hide percentage** — toggle the number display off for a cleaner look
- **Reset to defaults** button in every settings panel

---

## Gauges

| Button | What it measures |
|--------|-----------------|
| **CPU Gauge** | Overall CPU load % |
| **RAM Gauge** | Active RAM as a % of total installed |
| **GPU Gauge** | GPU core utilisation % (Nvidia, AMD, Intel via systeminformation) |

---

## Installation

1. Download `krutons-usage.sdPlugin.zip` from the [Releases](../../releases) page
2. Extract the zip — you should have a folder called `nvidia-gauge.sdPlugin`
3. Copy that folder to your OpenDeck plugins directory:
   ```
   %APPDATA%\OpenDeck\plugins\
   ```
4. Restart OpenDeck
5. The three gauges will appear in the actions list under **Krutons Usage Plugin**

> **Note:** The plugin requires Node.js 20 or later. OpenDeck bundles its own Node runtime so you do not need to install Node separately.

---

## Customisation

Click any placed gauge button to open its settings panel on the left:

### Display
| Setting | Description |
|---------|-------------|
| **Custom Label** | Replaces the default CPU / RAM / GPU text. Max 8 characters. Leave blank for default. |
| **Show Percentage** | Toggle the percentage number on or off. |

### Appearance
| Setting | Description |
|---------|-------------|
| **Arc Colour** | Colour picker + hex input. Each button is styled independently. |
| **Arc Thickness** | Thin (8px) / Normal (12px) / Thick (16px) / Chunky (20px) |

### Alerts
| Setting | Description |
|---------|-------------|
| **Warning Threshold** | The % value above which the arc switches colour. Default: 80. |
| **Warning Colour** | The colour the arc turns when above the threshold. Default: red. |

---

## How it works

The plugin is a Node.js process that:
1. Connects to OpenDeck over a local WebSocket
2. Polls `systeminformation` every 2 seconds for CPU, RAM, and GPU stats
3. Renders a 144×144 PNG gauge for each visible button entirely in pure JavaScript (no native binaries — uses Node's built-in `zlib` for PNG encoding)
4. Sends the image via `setImage` to OpenDeck, which pushes it to the hardware

Because rendering is pure JS, the plugin works reliably across Node versions without recompilation.

---

## Requirements

- **OpenDeck** (Windows 10+)
- Any CPU, RAM source compatible with [systeminformation](https://systeminformation.io/)
- GPU utilisation requires a supported GPU (Nvidia, AMD, or Intel with driver support)

---

## Project structure

```
nvidia-gauge.sdPlugin/
├── index.js                   # Plugin backend — polling, rendering, WebSocket
├── manifest.json              # OpenDeck plugin manifest
├── package.json
├── images/
│   ├── plugin-icon.png        # Plugin icon (shown in OpenDeck plugin list)
│   ├── cpu-icon.png           # CPU action icon
│   ├── ram-icon.png           # RAM action icon
│   └── gpu-icon.png           # GPU action icon
├── propertyinspector/
│   └── index.html             # Settings panel UI
└── node_modules/
    ├── ws/                    # WebSocket client
    └── systeminformation/     # System stats library
```

---

## Building from source

```bash
git clone https://github.com/kruton/krutons-usage-plugin
cd krutons-usage-plugin/nvidia-gauge.sdPlugin
npm install
```

Then zip the `nvidia-gauge.sdPlugin` folder (excluding `node_modules/canvas` if present) and install as above.

---

## Credits

- System stats via [systeminformation](https://github.com/sebhildebrandt/systeminformation) by Sebastian Hildebrandt
- WebSocket via [ws](https://github.com/websockets/ws)
- Built for [OpenDeck](https://github.com/nekename/OpenDeck) by nekename

---

## License

MIT
