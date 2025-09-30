# 🚀 Spacetoon — cub3D Organization

Welcome to Spacetoon — a collaborative cub3D project inspired by the classic raycasting school project.
Two planets, two vibes: Zomoroda (mystical) and Action (high-octane). Pick a planet in the menu, watch a short intro, then play a procedurally / hand-authored map rendered with a MiniLibX-based raycaster.

This repository is organized as a small organization of focused components so teammates can work in parallel (assets, engine, input & UI, sound helpers).

## 🎯 Project Overview

Spacetoon is a 2.5D raycasting game written in C (MiniLibX).
It demonstrates:

A performant raycaster (DDA) for rendering walls and sprites.

Player movement with collision detection, rotation and vertical pitch.

Minimap and HUD.

Sprite animation for hand / object / intro elements.

Mode system: pick one of multiple “game modes” (planets). Each mode has its own textures, intro frames, music and sprites.

Lightweight sound integration via system players (VLC) or optional SDL2 audio if available.

Clean modular code to separate game states: MENU → LOADING → INTRO → RENDER.

This project was written to be 42-school friendly and follows the norm in structure and coding style.

## 🗂 Organization / Repos

The project is structured inside this repository in logical folders:

```bash
/mandatory/        # mandatory project sources
  /includes        # all headers (cub3d.h, settings.h, typedefs, etc.)
  /src             # C sources (init, events, raycasting, render, textures, utils, parsing, ...)
/bonus/            # optional extra features (if implemented)
Libft/             # personal libft used by the project
textures/          # shared textures and media (two planets folders)
maps/              # example .cub maps (good/ bad)
Makefile
README.md
```
We split responsibilities between teammates so changes are isolated and reviewable.

## ▶️ How it runs

### Build (mandatory):
```bash
make
```

Run:
```bash
./cub3D maps/good/m.cub
```

### Build bonus:
```bash
make bonus
```

Clean:
```bash
make clean
make fclean
make re
```

Note: this project uses MiniLibX (X11). On systems without MiniLibX preinstalled, you must link/compile with the proper MLX library for your environment (see Makefile for the -Lmlx_linux -lmlx_Linux flags). No sudo required for running the produced binary; installing system packages may require admin rights, but the game itself is self-contained.

## 🕹 Controls

Default keyboard mapping (X11 keycodes / typical keys):

### Movement:

W / Up — Move forward

S / Down — Move backward

A / Left — Strafe left

D / Right — Strafe right

### Rotation:

Left / Right arrows — turn left / right

Q / E (or mouse) — optional rotate

### Interact:

E — interact (doors)

Jump / Pitch:

Mouse up/down or dedicated keys (configurable)

### Menu:

Press 1, 2, ... to choose a planet mode from the menu

### Exit:

ESC — quit the game

(Adjust keycodes in includes/typedef.h if your environment uses different values.)

## 🔊 Audio

We ship two approaches:

System player (recommended if you cannot install libs):
We call a background player (VLC) to play intro and loop music:

play_music(intro, mode_index) — starts VLC with --intf dummy for silent background playback.

stop_music() — stops the player (kills VLC process). We scoped the approach to minimize collateral kills; the Makefile and code use killall -q vlc which is simple but will stop all VLC instances on the machine — use with awareness.

SDL2 (optional):
If you can install libsdl2-dev and libsdl2-mixer-dev, an SDL-based sound backend is available (in utils), offering per-game-process audio control with fewer side effects. On restricted systems where installation is not allowed, the system-player option avoids external installs.

## 🧩 Game Modes & Assets

Each mode (planet) provides:

logo.xpm — centred logo

intro/ — XPM frames for intro animation

Music/intro.mp3 and Music/loop.mp3 — intro and loop music

animation/ — object / gun / sprite frames

door.xpm, north.xpm, south.xpm, east.xpm, west.xpm — textures

Example structure:
```bash
textures/zomoroda/
  logo.xpm
  intro/
  animation/
  Music/intro.mp3
  Music/loop.mp3
  door.xpm
  north.xpm ...
textures/action/
  ...
```

## 🧠 State Machine

The game follows a simple state machine:

STATE_MENU — show menu and wait user choice

STATE_LOADING — load textures, audio and sprites for the chosen mode

STATE_INTRO — play intro frames and intro music

STATE_RENDER — main game loop (raycasting, input, rendering)

STATE_EXIT — cleanup and exit

This makes it easy to add new modes (just add an entry to cub->modes[] in init_map()).

## 🛠 Features & Implementation Notes

Raycasting: DDA-based raycaster. Walls, floors/ceilings, sprite projection, texture mapping. (See raycasting/dda.c for the math).

Sprites: scaled rendering with center/fit functions; animated object frames support.

Player: collision checks, pitch (vertical offset), smooth strafing with step subdivision to avoid tunneling.

Minimap: scalable minimap — the size and block scale can be adapted to the window size.

Intro system: preload intro frames, show them with a timed delay, then switch to loop music and gameplay.

Resource cleanup: ft_exit() destroys MLX images/windows and frees textures and config.

## ✅ How to add a new planet / mode

Add a new folder under textures/your_mode/ with:

logo.xpm, intro/ frames, animation/, Music/intro.mp3, Music/loop.mp3, and textures.

Add an entry in init_map() (or the modes array) with the paths to those assets.

Increase mode_count accordingly.

Rebuild: make and run to see the new option appear in the menu.

## 🛡 Known issues & troubleshooting

Valgrind reports “still reachable” memory inside system libraries (PulseAudio / SDL / X11). These are typically allocator caches or global contexts created by the system libraries; they’re not necessarily leaks in your code.

SDL + MLX interactions: On some systems combining SDL audio and MiniLibX (X11) can trigger X11 warnings or valgrind uninitialized-value reports. If you see those and cannot install SDL mixer, prefer the system player approach.

VLC kill scope: killall vlc will stop all VLC processes. If you run a user instance of VLC, it will be killed. Consider running an alternative audio player or use pgrep/pkill with more specific filtering if you need to avoid killing unrelated VLC instances.

Makefile paths: Ensure your environment has MLX properly set up and header include paths are correct. Edit the Makefile includes/flags if your MLX build is elsewhere.

## 🧩 Contributing & Code Style

Follow the Norm (42 coding style) for new code.

Keep functions short and single-responsibility.

Add unit-style checks where possible (validate prepare_sprite_metadata() returns).

Tests: add .cub maps to maps/good/ and maps/bad/ to validate parsing logic.

## 👥 Team & Credits

Spacetoon cub3D — student project inspired by 42 cub3D.

Zomoroda (mode / planet) — textures, music and design by Team Zomoroda

Action (mode / planet) — textures, music and design by Team Action

### Project Contributors:

_[Hamza Wahmane](https://github.com/Wahmane-Hamza)_ — parsing helpers, textures, intro assets, sprite animations, Spacetoon Idea 

_[Abdellah Nsila](https://github.com/Abdellah-Nsila)_ — engine & execution logic, audio integration, build system


## 📜 License

This educational project was made for the 42 school curriculum and follows the school’s policies on original code. Use for learning and demonstration only. Check license file for more details (if you want to add one, add an LICENSE).

## 🎉 Wrap-up

Thanks for checking out Spacetoon.
If you want, I can:

Turn this into a README.md file ready to paste into your repo,

Add a short HOWTO section for contributors (branching/PR style),

Provide a small CHECKLIST.md for grading (features & tests).
