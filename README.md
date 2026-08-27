<h1 align="center">emoji.mq</h1>

GitHub-style emoji shortcode <-> Unicode emoji conversion implemented as an [mq](https://github.com/harehare/mq) module.

## Features

- Converts `:shortcode:` to 🎉 and back, in either direction, across a whole string
- A curated table of ~200 of the most commonly used GitHub-style shortcodes (not the full 1800+ gemoji set)
- Resolves well-known aliases (`:+1:` -> `:thumbsup:`)
- `has_emoji` / `strip` helpers for filtering emoji out of plain-text output
- Bonus: `gitmoji(type)` maps a Conventional Commits type (`feat`, `fix`, `docs`, …) to its [gitmoji](https://gitmoji.dev)

## Installation

Copy `emoji.mq` to your mq module directory, or place it anywhere and reference it with `-L`.

```sh
cp emoji.mq ~/.local/mq/config/
```

### HTTP Import (no local installation needed)

HTTP imports are disabled by default; pass `--allow-http-import` to import directly from GitHub without any local setup:

```sh
mq --allow-http-import -I raw 'import "github.com/harehare/emoji.mq" | emoji::to_emoji(.)' README.md
```

Pin to a specific release with `@vX.Y.Z`:

```sh
mq --allow-http-import -I raw 'import "github.com/harehare/emoji.mq@v0.1.0" | ...'
```

## Usage

```sh
mq -L /path/to/modules -I raw \
  'import "emoji" | emoji::to_emoji(.)' README.md
```

If you copied it to the mq built-in module directory:

```sh
mq -I raw 'import "emoji" | emoji::to_emoji(.)' README.md
```

## API

| Function | Description |
|---|---|
| `emoji_for(shortcode)` | Looks up the emoji for a shortcode (with or without colons); resolves aliases; `None` if unknown |
| `shortcode_for(emoji)` | Looks up the canonical shortcode for an emoji character; `None` if unknown |
| `shortcodes()` | All known canonical shortcode names, sorted |
| `to_emoji(text)` | Replaces every recognized `:shortcode:` in `text` with its emoji |
| `to_shortcode(text)` | Replaces every recognized emoji character in `text` with its canonical `:shortcode:` |
| `has_emoji(text)` | `true` if `text` contains a recognized shortcode or emoji |
| `strip(text)` | Removes every recognized shortcode and emoji from `text` |
| `gitmoji(commit_type)` | Maps a Conventional Commits type to its gitmoji; `None` if unrecognized |

Unrecognized `:word:` patterns are left untouched by `to_emoji`/`has_emoji`/`strip`, so
running them on arbitrary Markdown won't corrupt unrelated `:like_this:` text.

## Example

```sh
mq -L . -I raw 'import "emoji" | emoji::to_emoji(.)' <<< "Ship it :rocket: :tada:"
# => "Ship it 🚀 🎉"

mq -L . -I raw 'import "emoji" | emoji::to_shortcode(.)' <<< "Ship it 🚀 🎉"
# => "Ship it :rocket: :tada:"
```

Build a commit message prefix from a Conventional Commits type:

```sh
mq -I raw 'import "emoji" | emoji::gitmoji("feat")' <<< "x"
# => "✨"
```

## Compatibility

Requires [mq](https://github.com/harehare/mq) v0.6 or later.

## License

MIT
