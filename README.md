# GoPlay

A browser-based Go (Weiqi/Baduk) game with KataGo AI integration.

Play Go against KataGo, watch AI vs AI matches, or analyze game records — all from a single HTML page with a Python WebSocket server.

## Features

- **Play against AI** — Play as Black or White against KataGo at adjustable difficulty
- **AI vs AI** — Watch two KataGo engines (different versions/models) play each other
- **Human vs Human** — Local play with no server required
- **Game analysis** — Load SGF files and get KataGo's move-by-move evaluation
  - Top candidate moves with win rate and score lead
  - Principal variation sequences animated on the board
  - Territory and dead stone overlay from neural network ownership
- **Accurate scoring** — End-of-game scoring uses KataGo ownership to identify dead stones
- **SGF support** — Save and load game records in standard SGF format
- **Multiple engines** — Configure and switch between different KataGo versions or models

## Architecture

```
index.html          Single-page frontend (game logic, renderer, UI)
server.py           Python WebSocket server (KataGo bridge)
config.json         Engine configuration (not tracked, see config.example.json)
```

The frontend connects to the server via WebSocket. The server manages KataGo subprocesses via the GTP protocol, handling move generation, position analysis, and ownership queries.

## Quick Start

```bash
pip install -r requirements.txt
cp config.example.json config.json
# Edit config.json with your KataGo paths
python server.py
# Open index.html in your browser
```

See [INSTALL.md](INSTALL.md) for detailed setup instructions.

## Requirements

- Python 3.8+
- [KataGo](https://github.com/lightvector/KataGo/releases) with a neural network model
- A modern web browser

## License

MIT
