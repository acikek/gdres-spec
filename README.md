# **GDRes**, a Geometry Dash Resource Pack format

## Introduction

The rhythm-based music platformer [Geometry Dash](https://en.wikipedia.org/wiki/Geometry_Dash) exposes its asset files in an extremely straightforward way. The `Resources/` folder is right at the top of the game files -- it contains all spritesheets, fonts, music, sound effects, etc. that the game relies on. There's no compression or encryption of any kind, so curious players are free to view and edit the underlying resources as they please.

Due to the aforementioned accessibility, Geometry Dash has had a long history of 'texture packs,' or collections of asset files meant to override the default ones, often with the goal of completely overhauling the game's visual style. Texture packs have been popular within the community going back [at least 8 years](https://www.youtube.com/watch?v=xa_juMWtZJI), and new packs are created all the time.

> **Note**: This document will refer to 'texture packs' as **resource packs** going forward as a more accurate description of the game assets.

Geometry Dash resource packs lack any sort of standard. For the past near-decade, this has been an acceptable state of affairs; users would download a `.zip` file, extract it, find the asset files, and drag them into the game folder. However, with the [Geode](https://geode-sdk.org/) mod loader now being the accepted standard, supporting texture pack managers such as [Texture Loader](https://geode-sdk.org/mods/geode.texture-loader/), combined with the possibility that [Modrinth](https://modrinth.com/) might branch out into other games in the near future, a standard is warranted. Such a standard shouldn't be [unifying](https://xkcd.com/927/), but a standard *should exist*.

This document outlines the specification for **GDRes**, a Geometry Dash Resource Pack format.

## Goals

The goals of this standard are as follows:
- Creating resource packs should be simple and easy and shouldn't require the use of any external tools.
- Resource packs should be flexible and not bound to any specific software.
- This format should **not** encompass the properties of every existing convention available.
- This format should **not** be unifying.
- Software should explicitly support GDRes, i.e. support shouldn't be inferred from manifest contents.

## Specification

GDRes packs are [ZIP files](https://en.wikipedia.org/wiki/ZIP_(file_format)). The supported file extensions are `.zip` and `.gdres`.

The ID of a resource pack is the file name (without the extension). The file name must be a valid UTF-8 sequence.

### Contained Files

The resource pack file **must** contain:
- `gdres.pack.json`: The manifest file.
- `resources/` **or** `Resources/`: The resource files.

The file may contain these items:
- A 'readme' file.
- A license file.
- An icon image.

The above items' locations are specified in the manifest file. If they are not specified explicitly, they have default values which resource pack loaders should still check for.

The file may also contain a `custom` folder, explained in a future section.

### Resources

The resource folder structure mirrors the `Resources` folder within the Geometry Dash game files. GDRes packs **cannot** contain a `levels` folder within their resources. Beyond that exception, there are no other restrictions as to what files can be placed where; although not recommended, even `.plist` files can be included.

### Custom

The custom folder allows for software- or implementation-specific files. It can only contain folders corresponding to 'software' IDs; ideally ascii, `snake_case` versions of the software name. For example, the Geode mod loader's custom folder should be in `custom/geode`. These folders' structures are fully defined by the corresponding implementation and can contain any files.

### Manifest

The manifest schema is defined as a [JSON Schema](https://json-schema.org/) in [`gdres-manifest.schema.json`](gdres-manifest.schema.json). Here are some supplementary notes:
- Resource pack loaders should attempt to load a resource pack regardless of the supported versions, but show a warning if the current game version isn't supported.
- All file path strings are relative to the manifest file and **do not** support symlinks.
- `metadata.contact` is a free-form object, but GDRes explicitly defines `homepage`, `sources`, and `issues` as URL strings.
- The `custom` object mirrors the structure of the `custom` folder specified above, with keys being software IDs mapping to free-form objects.

#### Notes

- Somewhat inspired by the [`quilt.mod.json`](https://github.com/QuiltMC/rfcs/blob/main/specification/0002-quilt.mod.json.md) format.

