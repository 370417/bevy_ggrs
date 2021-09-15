# [![GGRS LOGO](./ggrs_logo.png)](https://gschup.github.io/ggrs/)

![GitHub Workflow Status](https://img.shields.io/github/workflow/status/gschup/bevy_ggrs/Rust?style=for-the-badge)

## Bevy GGRS

Bevy plugin for the [GGRS](https://github.com/gschup/ggrs) P2P rollback networking library.
The plugin creates a custom stage with a separate schedule, which handles correctly advancing the gamestate, including rollbacks.
It efficiently handles saving and loading of the gamestate by only snapshotting relevant parts of the world, as defined by the user. It is supposed to work with the latest released version of bevy.

For explanation on how to use it, check the 👉[examples](./examples/)!

## How it works

The GGRS plugin creates a custom `GGRSStage` which owns a separate schedule. Inside this schedule, the user can add stages and systems as they wish.
When the default schedule runs the `GGRSStage`, it polls the session and executes resulting `GGRSRequests`, such as loading, saving and advancing the gamestate.

- advancing the gamestate is done by running the internal schedule once.
- saving the gamestate is done by creating a snapshot of entities tagged with a `bevy_ggrs::Rollback` component and saving only the components that were registered through `register_rollback_type::<YourCoolComponent>()`. The plugin internally keeps track of the snapshots together with the GGRS session.
- loading the gamestate applies the snapshot by overwriting, creating and deleting entities tagged with a `bevy_ggrs::Rollback` component and updating the registered components values.

Since bevy_ggrs operates with a separate schedule, compatibility with other plugins might be complicated to achieve out of the box, as all gamestate-relevant systems needs to somehow end up inside the internal GGRS schedule to be updated together the rest of the game systems.

## Development Status

⚠️Disclaimer⚠️: bevy_ggrs is in a very early stage. This plugin currently depends on the latest bevy developments and is thus incompatible with bevy releases on crates.io. Once bevy 0.6 releases, I will also make a stable release!

## Thanks

to [bevy_backroll](https://github.com/HouraiTeahouse/backroll-rs/tree/main/bevy_backroll) and [bevy_rollback](https://github.com/jamescarterbell/bevy_rollback) for figuring out pieces of the puzzle that made bevy_GGRS possible. Special thanks to the helpful folks in the bevy discord, providing useful help and pointers all over the place.

## Licensing

Bevy_GGRS is dual-licensed under either

- [MIT License](./LICENSE-MIT): Also available [online](http://opensource.org/licenses/MIT)
- [Apache License, Version 2.0](./LICENSE-APACHE): Also available [online](http://www.apache.org/licenses/LICENSE-2.0)

at your option.
