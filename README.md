# Windows Style Input Source Switching for Karabiner-Elements

English | [中文](README_zh.md) | [Português](README_pt.md) | [日本語](README_ja.md)

This repository contains `windows_style_IME_switcher`, a Karabiner-Elements complex modification rule for macOS users who are used to the Windows-style input method switching workflow.

It is designed for people who use multiple macOS input sources, for example:

- English
- A third-party Chinese input method such as Squirrel
- Portuguese or another additional input method

## Features

The main rule file is:

```text
windows_style_IME_switcher.json
```

It provides two behaviors:

1. Hold `Control` and tap `Shift` to switch to the next input source.
2. Press `Control + Space` to switch between English and the second input source in your macOS input source order.

More specifically:

- When the current input source is English, `Control + Space` switches to the next input source.
- When the current input source is not English, `Control + Space` switches back to English.
- The second input source is not hardcoded. It follows your macOS input source order.

For example, if your input source order is:

```text
English -> Squirrel -> Portuguese
```

Then:

- Holding `Control` and tapping `Shift` cycles through `English -> Squirrel -> Portuguese -> English`.
- `Control + Space` switches from English to Squirrel.
- `Control + Space` switches from Squirrel or Portuguese back to English.

If you later move Portuguese, Japanese, or another input source to the second position, `Control + Space` will switch from English to that input source instead.

## Requirements

- macOS

- [Karabiner-Elements](https://karabiner-elements.pqrs.org/)

- The macOS shortcut for "Select the previous input source" set to `Control + Space`

- The macOS shortcut for "Select next source in Input menu" set to `Control + Shift + /`

You can check or change these shortcuts in:

```text
System Settings -> Keyboard -> Keyboard Shortcuts -> Input Sources
```

## Installation

1. Open your Karabiner-Elements complex modifications directory:

```text
~/.config/karabiner/assets/complex_modifications/
```

2. Put this file into that directory:

```text
windows_style_IME_switcher.json
```

3. Open Karabiner-Elements.
4. Go to:

```text
Complex Modifications -> Add your own rule
```

5. Enable:

```text
windows_style_IME_switcher
```

## How It Works

The rule maps holding `Control` and tapping `Shift` to the existing macOS shortcut:

```text
Control + Shift + /
```

This means the cycling order is still controlled by macOS, not by Karabiner-Elements.

The `Control + Space` behavior uses Karabiner-Elements input source conditions:

- `input_source_if` detects English.
- `input_source_unless` detects non-English input sources.
- `select_input_source` switches back to English.

The `Control + Space` manipulators are intentionally matched exactly. Shortcuts that add another modifier, such as `Control + Command + Space`, are left available for macOS or other apps.

The English input source is matched with:

```json
{
  "language": "^en$"
}
```

## Notes

This rule assumes your English input source is recognized by Karabiner-Elements as `language = en`.

If `Control + Space` does not switch back to English correctly, use Karabiner-EventViewer to inspect your actual English input source identifier, then replace the language matcher with a more specific `input_source_id`.

## Testing

After enabling the rule, test these scenarios:

1. Start from English, hold `Control`, and tap `Shift` repeatedly. It should cycle through all input sources.
2. Start from English and press `Control + Space`. It should switch to the second input source.
3. Start from any non-English input source and press `Control + Space`. It should switch back to English.
4. Change the second input source in macOS settings, then press `Control + Space` from English again. It should switch to the new second input source.
5. Press a shortcut that includes another modifier, such as `Control + Command + Space`. It should not be intercepted by this input source rule.

## Maintainer

[Elias-Lee-SC](https://github.com/Elias-Lee-SC/)
