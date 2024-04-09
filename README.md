# Container Format

Seeds are zip containers. Individual files may be compressed using zstd.

# Seed Container Structure

Files in [brackets] are optional

```
├── format_version.txt
├── preload.json
├── assembly.json
├── [seedgen_info.json]
├── [seedgen_log.txt]
└── [assets/*]
```

## format_version.txt

Consumer: All

Contains the semantic version for the seed format in plaintext. Versions are considered compatible if their left-most non-zero component is the same. Contains `0.0.0` until the seed format is developed.

## preload.json

Consumer: Client

Contains necessary information while preloading in the main menu.

```ts
{
    tags: string[], // May be displayed as brief summary of the settings
    spawn: { // For preloading before starting the savefile
        x: number, // f32
        y: number, // f32
    },
    slug: string, // Identical for seeds with the same universe settings (including seed)
}
```

## assembly.json

Consumer: Client

Contains the compiled seedgen output that makes up the seed. Currently in JSON, although it's somewhat annoying to store assembly instructions in JSON. A binary format might be more pleasant to work with, maybe msgpack or a custom spec.

See `Assembly` in [wotw_seedgen_seed](https://github.com/SiriusAshling/wotw-seedgen/blob/new_seed_format/wotw_seedgen_seed/src/assembly.rs) for the format

## seedgen_info.json

Consumer: Seedgen

Contains necessary information to perform a reach check.

```ts
{
    universe_settings: {}, // May change at will
    world_index: number, // u32
    spawn_identifier: string, // areas.wotw identifier for spawn
}
```

## seedgen_log.txt

Consumer: Developer

Contains the detailed log of the seed generation process for debugging purposes.

## assets

Consumer: Client

The assembly may reference assets by path. These assets are contained at `assets/<path>`
