# Archive Format

Not yet decided.

The archive may contain assets provided by the plando/snippet author (mostly images), so it may become pretty large. The client should probably read the necessary files first and handle the rest asynchronously.

To support this, the archive should allow reading specific files efficiently, even if it becomes big (i.e. don't compress the entire thing).

Compression is probably mostly relevant for the assembly because it's currently in JSON. With a more efficient assembly format compression might not be necessary.

# Seed Archive Structure

```
├── format_version.txt
├── preload.json
├── seedgen_info.json
├── seedgen_log.txt
└── assembly.json
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

## assembly.json

Consumer: Client

Contains the compiled seedgen output that makes up the seed. Currently in JSON, although it's somewhat annoying to store assembly instructions in JSON. A binary format might be more pleasant to work with, maybe msgpack or a custom spec.

See `SeedWorld` in [wotw_seedgen_assembly](https://github.com/SiriusAshling/wotw-seedgen/blob/seed_snippets_merge/wotw_seedgen_assembly/src/lib.rs) for the format
