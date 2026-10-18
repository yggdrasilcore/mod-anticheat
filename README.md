<p align="center">
  <img alt="Yggdrasilcore" src="https://github.com/user-attachments/assets/6f96adf9-1867-4ca6-b627-1bd0ab7ee494" />
</p>

# Anticheat

`mod-anticheat` is a [Yggdrasilcore](https://github.com/yggdrasilcore/yggdrasilcore) module that detects and reports player movement hacks. It is a Yggdrasilcore-compatible fork of AzerothCore's [mod-anticheat](https://github.com/azerothcore/mod-anticheat).

The module watches player movement and opcode acknowledgement to detect speed, fly, water-walk, jump, climb, teleport, teleport-to-plane, z-axis, gravity, anti-swim, anti-knockback, no-fall-damage and order-ack hacks. Reports are stored per player in the characters database and surfaced to online game masters, and may optionally trigger automatic kick, ban or jail, or in-game counter-measures that reset the offending player.

<p align="center">
  <a href="https://github.com/yggdrasilcore/mod-anticheat/actions/workflows/core-build.yml"><img src="https://github.com/yggdrasilcore/mod-anticheat/actions/workflows/core-build.yml/badge.svg" alt="Build"></a>
  <a href="https://github.com/yggdrasilcore/mod-anticheat/actions/workflows/core_codestyle.yml"><img src="https://github.com/yggdrasilcore/mod-anticheat/actions/workflows/core_codestyle.yml/badge.svg" alt="Codestyle"></a>
</p>

Read more about Yggdrasil WoW and see Yggdrasilcore in action [here](https://yggdrasilwow.com/).

## Configuration

Detection toggles, reporting thresholds, GM alerts and automatic moderation all live in `conf/Anticheat.conf.dist`. Apply the SQL under `data/sql` as usual for the module. Reports are written to the `characters` database, and existing report data can be inspected and purged with the module's chat commands.

## What we changed

- Thread safety: the player data map is now guarded by a shared mutex, so the movement and map threads read concurrently while login, logout and reset take exclusive access; the update check timer is atomic. This removes data races on player reports between the world and map threads.
- Build: Ubuntu 24.04 CI against Yggdrasilcore, plus a codestyle check and generated documentation for the module source.

## Documentation

- [Generated module documentation](https://yggdrasilcore.github.io/mod-anticheat/)
- [AzerothCore installation guide](https://www.azerothcore.org/wiki/installation)

## Support

This module is a downstream fork and we do not provide official public support for it, and upstream channels do not support fork-specific behaviour. If you use it and find a bug, feel free to open an issue or a pull request here.

## Contributing

This repository is privately managed. We do not recommend that emulation enthusiasts follow it. Instead, we recommend that enthusiasts contribute to the upstream [mod-anticheat](https://github.com/azerothcore/mod-anticheat) project and to [AzerothCore](https://github.com/azerothcore/azerothcore-wotlk).

## Acknowledgements

- [azerothcore/mod-anticheat](https://github.com/azerothcore/mod-anticheat)

## License

- Source: This repository is distributed under the MIT License.
- Intellectual property: Yggdrasil WoW and its derivative projects are not related to Blizzard Entertainment and lay no claim to Blizzard Entertainment's copyrights or intellectual property. They operate solely as an avenue for exploring the functionality of the abandonware WotLK 3.3.5a client in an educational capacity.
