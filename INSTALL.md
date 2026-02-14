# Installation

## Prerequisites

- **Python 3.8+**
- **KataGo** — Download from https://github.com/lightvector/KataGo/releases
  - Choose a build matching your hardware: OpenCL (AMD/Intel/NVIDIA), CUDA, or TensorRT (NVIDIA)
  - Download a neural network model file (`.bin.gz`) from the same releases page
- A modern web browser (Chrome, Firefox, Edge)

## Setup

### 1. Install Python dependencies

```
pip install -r requirements.txt
```

### 2. Verify KataGo works

Before configuring the server, confirm KataGo runs on its own:

```
path\to\katago.exe gtp -model path\to\model.bin.gz -config path\to\config.cfg
```

Type `name` and press Enter. You should see `= KataGo`. Type `quit` to exit.

### 3. Configure engines

Copy `config.example.json` to `config.json` and edit it to point to your KataGo installation. Each engine entry needs a `name` and a `command` (the full command line used to launch KataGo in GTP mode):

```
cp config.example.json config.json
```

```json
{
  "server": {
    "host": "localhost",
    "port": 9001
  },
  "engine1": {
    "name": "KataGo",
    "command": "path/to/katago.exe gtp -model path/to/model.bin.gz -config path/to/config.cfg"
  },
  "game_defaults": {
    "time_settings": {
      "main_time": 0,
      "byo_time": 5,
      "byo_stones": 1
    },
    "rules": "chinese",
    "komi": 6.5,
    "max_visits": 200
  }
}
```

You can configure multiple engines (`engine1`, `engine2`, `engine3`, ...) to use different KataGo versions or models. Only the engine(s) you select in the UI will be started.

### 4. Start the server

```
python server.py
```

The server listens on `ws://localhost:9001` by default (configured in `config.json`). You can override with flags:

```
python server.py --port <port> --host <host>
```

### 5. Open the game

Open `index.html` in your browser. In **Human vs Human** mode, no server connection is needed. To play against the AI or use analysis features, select a mode that requires an engine (Play as Black, Play as White, or AI vs AI) and ensure the server is running.

When you select an AI mode, a connection indicator appears next to the AI settings. It turns green when connected to the server.

## Game Modes

| Mode | Description |
|------|-------------|
| Human vs Human | Local play, no server needed |
| Play as Black | You play Black, AI plays White |
| Play as White | You play White, AI plays Black |
| AI vs AI | Two engines play each other |

## Analysis

Load an SGF file or play a game, then click **Analyze** to get KataGo's evaluation. Use arrow keys, mouse wheel, or the slider to navigate moves. Hover over candidate moves on the board to see the principal variation animated stone by stone.

## Troubleshooting

- **Connection indicator stays red/grey** — Make sure `server.py` is running and the port in `config.json` matches.
- **KataGo fails to start** — Check that the paths in `config.json` are correct and that KataGo runs standalone (step 2).
- **OpenCL errors** — Install GPU drivers. KataGo OpenCL needs a working OpenCL runtime.
- **Slow first move** — KataGo compiles its neural network on first run. Subsequent starts are faster.
