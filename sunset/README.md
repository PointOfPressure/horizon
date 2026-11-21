# Sunset 2.0.0 on Horizon 0b14f8acfd97fa2c750e42795249dcc981178ca4

This branch is upstream Horizon `0b14f8acfd97fa2c750e42795249dcc981178ca4` (nightly-2026-11-18-0b14f8a) plus the source of what Sunset's prebuilt client adds to it.
Horizon is LGPL-3.0-or-later (`LICENSE` at the root). Outside `sunset/`, the commits between `0b14f8acfd97fa2c750e42795249dcc981178ca4` and the first
`sunset/` commit are Sunset's hook patches (branch `sunset-hooks/0b14f8acfd97fa2c750e42795249dcc981178ca4` when there are any; one feature each, also
LGPL-3.0-or-later); the prebuilt's Horizon libraries are built from that tree with `tools/prebuilt_game.py`
(`HORIZON_SDK_BUILD_ID=0b14f8acfd97fa2c750e42795249dcc981178ca4`). The rest is unmodified upstream.

- `sunset/package/`: Sunset's .hmod package (PointOfPressure/sunset-client `e2f82e36913ef3fcb006f691619330843695b429`, `client/sunset`). Its own
  licence is `sunset/package/LICENSE`; it is here so the shipped build can be reproduced.
- `sunset/patch/`: the function-replacement crates (`horizon_patched*`) the launcher extracts from this revision,
  with Sunset's mixins and support modules woven in by horizon-mixins `e59da9094f0c2fdda6c627fe9eff22dec6a906f0`, and `horizon_patch.rs`, the
  launcher's entry module. Compiled, this is `libhorizon_patch.so`. A modified Horizon: LGPL-3.0-or-later as a
  whole, the woven-in code included. Read the commits one by one to see the extract
  first and then exactly what Sunset changes.

## Rebuild

With horizon-launcher 0.2.78 (its own Rust, 1.99.1) and Minecraft 26.3:

```sh
horizon-launcher install nightly-2026-11-18-0b14f8a
horizon-launcher prepare --version nightly-2026-11-18-0b14f8a
horizon-launcher instance create sunset && horizon-launcher instance version sunset nightly-2026-11-18-0b14f8a
horizon-launcher native-build <folder containing sunset/package renamed to sunset> --instance sunset --trust-native-code
```

The launcher downloads this revision's SDK (`sdk-0b14f8acfd97fa2c750e42795249dcc981178ca4`), regenerates `sunset/patch/` and links the two libraries;
the result in `versions/native-*` is the shipped client. To change Horizon itself, build this revision's SDK from
your modified tree and point the launcher's SDK cache at it, or replace the shared libraries directly: the shipped
client loads every Horizon library dynamically.
