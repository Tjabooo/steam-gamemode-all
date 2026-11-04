# steam-gamemode-all

Run **every** Steam game with [Feral GameMode](https://github.com/FeralInteractive/gamemode), without
editing launch options one game at a time.

- `steam-gamemode-all` adds `gamemoderun` to the launch options of every installed game (native and
  Proton). Existing options are kept: `PROTON_LOG=1 %command% -novid` becomes
  `PROTON_LOG=1 gamemoderun %command% -novid`. Games that already have it are skipped, Proton and the
  Steam runtimes are left alone, and `localconfig.vdf` is backed up first.
- `steam-gm` starts Steam, but first runs the above, so games you installed since last time get it
  too. Use it instead of `steam` (in your launcher or autostart).

Steam rewrites its config when it exits, so the launch options can only be patched while Steam is
closed. Both scripts check this.

## Install

```sh
git clone https://github.com/Tjabooo/steam-gamemode-all.git
cd steam-gamemode-all
install -Dm755 steam-gamemode-all steam-gm -t ~/.local/bin/
steam -shutdown; steam-gamemode-all      # once now
```

Requires Python 3 (standard library only) and gamemode (`sudo pacman -S gamemode`, then add yourself to
the `gamemode` group). For Steam elsewhere than `~/.steam/steam` (Flatpak:
`~/.var/app/com.valvesoftware.Steam/.steam/steam`), set `STEAM_ROOT`.

To make your app launcher's Steam entry use `steam-gm`, copy `/usr/share/applications/steam.desktop` to
`~/.local/share/applications/` and change `Exec=/usr/bin/steam` to `Exec=steam-gm`.

Undo for one game: remove `gamemoderun` from its launch options in Steam. Undo for everything:
restore `~/.steam/steam/userdata/<id>/config/localconfig.vdf.bak` while Steam is closed.

## License

MIT
