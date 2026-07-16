# Teeworlds MRPG Client

Custom Teeworlds 0.7 MRPG client maintained for the
[Teeworlds MRPG Server](https://github.com/sqwinixxx/Teeworlds-MRPG-Server).
Use both repositories together for the complete game experience.

## Compatibility

| Client | Server | Support |
| --- | --- | --- |
| This repository | [Teeworlds-MRPG-Server](https://github.com/sqwinixxx/Teeworlds-MRPG-Server) | Full MRPG support (recommended) |
| Vanilla Teeworlds 0.7 client | Teeworlds-MRPG-Server | Network connection may work, but custom MRPG UI and features are unavailable |
| This repository | Unmodified Teeworlds 0.6 server | Not supported directly |
| This repository | Other Teeworlds 0.7 servers | Basic Teeworlds features only; MRPG features require the paired server |

The paired server is based on Teeworlds/DDNet 0.6 and contains a protocol
bridge for this legacy custom 0.7 client. A `netversion hash differs` warning
can appear in the client log because this is a custom protocol build. It does
not prevent connection to the paired server.

## Included MRPG features

- MRPG account registration and login UI.
- Custom inventory, equipment, stats, quests, crafting and vote-based menus.
- Custom HUD, MMO broadcasts, visual effects, entities, skins and world data.
- Multi-world map downloads from the paired server.
- Support for modern server-side MRPG data without the old `data.mmo` package.
- Correct UTF-8 broadcast wrapping, including Cyrillic text.
- Working tutorial and spectator free-view camera input.
- Safer SDL startup and shutdown under Hyprland/Wayland through XWayland.

## Build on Linux

### Dependencies

Debian/Ubuntu:

```bash
sudo apt update
sudo apt install build-essential cmake git python3 \
  libfreetype6-dev libsdl2-dev libwavpack-dev libcurl4-openssl-dev \
  libssl-dev libglew-dev libmariadb-dev zlib1g-dev
```

Arch Linux:

```bash
sudo pacman -S --needed base-devel cmake git python freetype2 sdl2 \
  wavpack curl openssl glew mariadb-libs zlib
```

Clone and build:

```bash
git clone --recurse-submodules https://github.com/sqwinixxx/Teeworlds-MRPG-Client.git
cd Teeworlds-MRPG-Client
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --target mmoteeworlds -j4
```

If the repository was cloned without submodules:

```bash
git submodule update --init --recursive
```

Run the client from the repository root so it can find the included data:

```bash
./build/mmoteeworlds
```

Client settings and downloaded maps are stored in `~/.teeworlds` on Linux.

## Connecting and accounts

Connect to the address and UDP port configured on the paired server. The
default example port is `8310`.

The server connection password and the MRPG account password are different:

- `password` in `autoexec_server.cfg` protects the whole server before joining.
- The login/register window manages the player's MRPG account in MariaDB.

For a public server, the server owner should normally use `password ""` so
players reach the MRPG registration screen without an extra password prompt.

## Troubleshooting

- **Map does not download:** use the paired server build and confirm `sv_sixup 1` is enabled there.
- **Password incorrect before joining:** this is the server connection password, not the MRPG account password. Ask the server owner to clear `password` or provide it.
- **Account login fails:** use the same login spelling and password that were entered during registration; account credentials are stored by the server database.
- **Black screen or missing UI assets:** run `./build/mmoteeworlds` from the repository root.
- **Wayland teardown problems:** on Hyprland the client automatically chooses SDL's X11 backend to avoid compositor crashes during window shutdown.

## Related repository

Server source, database setup and protocol bridge documentation:
[sqwinixxx/Teeworlds-MRPG-Server](https://github.com/sqwinixxx/Teeworlds-MRPG-Server).

## License and credits

This project is based on Teeworlds and the original MRPG/MMOTEE client. See
[`license.txt`](license.txt) for license and copyright information. Originally
written by Magnus Auvinen and extended by the Teeworlds, DDNet and MRPG
contributors.
