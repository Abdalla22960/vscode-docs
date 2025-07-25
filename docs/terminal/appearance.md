---
ContentId: F1AA7F3E-E078-4C02-B2DE-EC3F5F36F751
DateApproved: 06/12/2025
MetaDescription: Visual Studio Code's integrated terminal allows customizing its appearance in various ways.
---
# Terminal Appearance

The look of Visual Studio Code's terminal can be customized extensively.

![An example of a custom UI, using Powerline and Nerd Font symbols in the prompt and a custom workbench theme](images/appearance/terminal_appearance.png)

_[Sapphire](https://marketplace.visualstudio.com/items?itemName=tyriar.theme-sapphire) theme with custom [Starship](https://starship.rs/) prompt using the font [Hack](https://sourcefoundry.org/hack/) with [Nerd Font](https://www.nerdfonts.com/) symbols_

## Text style

Text in the terminal can be customized with the following settings:

- `setting(terminal.integrated.fontFamily)`: The font family to use, this takes a string in the format that fontFamily in CSS takes. For example, `"'Fira Code', monospace"` will configure `Fira Code` as the primary font and `monospace` as the fallback when it lacks glyphs.
- `setting(terminal.integrated.fontSize)`: Changes the font size of text in the terminal.
- `setting(terminal.integrated.letterSpacing)`: Configures additional horizontal spacing between characters in pixels.
- `setting(terminal.integrated.lineHeight)`: Configures additional spacing vertical between characters as a multiplier of the regular line height. For example, `1.1` will add 10% additional vertical space.
- `setting(terminal.integrated.fontWeight)`: Configures the font weight of "normal" text.
- `setting(terminal.integrated.fontWeightBold)`: Configures the font weight of "bold" text.
- `terminal.integrated.fontLigatures.*`: [Configure ligatures](#ligatures).

### Powerline symbols and Nerd Fonts

[Powerline](https://powerline.readthedocs.io) fonts are special patched fonts that contain additional characters that can be used in the terminal. VS Code's terminal [renders some of the Powerline symbols without needing to configure a font](#custom-glyphs), but if more glyphs are desired, configure a Powerline font with the font family setting. Powerline fonts typically end in `" for Powerline"`, the following setting is an example of how to configure a DejaVu Sans Mono that has been patched:

```json
"editor.fontFamily": "'DejaVu Sans Mono for Powerline'"
```

Nerd Fonts work the same and typically have a `" NF"` suffix, the following is an example of how to configure Hack's nerd fonts variant:

```json
"terminal.integrated.fontFamily": "'Hack NF'"
```

## Terminal cursor

The terminal cursor style and whether it blinks can be customized with the following settings:

- `setting(terminal.integrated.cursorStyle)`: Defines the shape of the cursor, can be block, line or underline.
- `setting(terminal.integrated.cursorWidth)`: How wide in pixels the cursor should be when the cursor style is set to `line`.
- `setting(terminal.integrated.cursorBlinking)`: Whether the cursor should blink when the terminal is focused.
- `setting(terminal.integrated.cursorStyleInactive)`: Defines the shape of the cursor, can be outline, block, line, underline or none.

## Customizing tabs

Terminal tabs appear on the right of the terminal view when there are two or more terminals by default, showing the active terminal in the view header when there is only one.

![A blank terminal without tabs and then with tabs displayed for three terminals](images/appearance/tabs.png)

### Visibility

The default visibility is designed to save horizontal space, but may not be desirable. How tabs are presented can be configured with the following settings:

- `setting(terminal.integrated.tabs.hideCondition)`: When to hide the tabs to the right, set to `never` to always show them.
- `setting(terminal.integrated.tabs.showActiveTerminal)`: When to show the active terminal in the terminal view header.
- `setting(terminal.integrated.tabs.showActions)`: When to show the active terminal's actions in the view header.
- `setting(terminal.integrated.tabs.location)`: Whether the tabs should be shown on the left or right of the terminal.
- `setting(terminal.integrated.tabs.enabled)`: Whether to use tabs, disabling will show the original dropdown view.

### Tab text

The text on each tab is determined by the following settings:

- `setting(terminal.integrated.tabs.title)`: Tab title.
- `setting(terminal.integrated.tabs.description)`: Text that appears to the right of the title.
- `setting(terminal.integrated.tabs.separator)`: Separator character between the title and description.

By default, the title displays what the shell's detected process name.

Other terminals often display the escape sequence sent by the shell as the title, which can be configured with:

```json
"terminal.integrated.tabs.title": "${sequence}"
```

Here are all variables available for use:

- `${cwd}`: the terminal's current working directory.
- `${cwdFolder}`: the terminal's current working directory, displayed for multi-root workspaces or in a single root workspace when the value differs from the initial working directory. On Windows, this will only be displayed when shell integration is enabled.
- `${workspaceFolder}`: the workspace in which the terminal was launched.
- `${workspaceFolderName}`: the name of the workspace in which the terminal was launched.
- `${local}`: indicates a local terminal in a remote workspace.
- `${process}`: the name of the terminal process.
- `${progress}`: the progress state as reported by the OSC `9;4` sequence.
- `${separator}`: a conditional separator (-) that only shows when it's surrounded by variables with values or static text.
- `${sequence}`: the name provided to the terminal by the process.
- `${task}`: indicates this terminal is associated with a task.
- `${shellType}`: the detected shell type.
- `${shellCommand}`: the command being executed according to shell integration. This also requires high confidence in the detected command line, which may not work in some prompt frameworks.
- `${shellPromptInput}`: the shell's full prompt input according to shell integration.

### Icons

Each terminal has an associated icon that is determined by its [terminal profile](/docs/terminal/profiles.md). The default icon and its color, which will be used if not defined in a profile, can be configured with the `setting(terminal.integrated.tabs.defaultIcon)` and `setting(terminal.integrated.tabs.defaultColor)` settings.

### Status

A terminal's "status", if any, is signified by an icon that appears on the right of the tab. Some statuses involve animation. If this is distracting the animation can be disabled with:

```json
"terminal.integrated.tabs.enableAnimation": false
```

### Visual bell

When the terminal's bell is triggered, a yellow bell icon is briefly shown. This can be disabled with `setting(terminal.integrated.enableBell)` and the duration can be configured with `setting(terminal.integrated.bellDuration)`.

## Terminal colors

While the terminal is capable of displaying true color, programs commonly use 8 ANSI colors (black, red, green, yellow, blue, magenta, cyan and white) and bright variants of each. These ANSI colors are determined by the active [color theme](/docs/configure/themes.md), but they can also be configured independently from the theme with the [workbench.colorCustomizations](/docs/configure/themes.md#workbench-colors) setting.

Whether bold text uses the normal ANSI colors or the bright variant can be configured with the `setting(terminal.integrated.drawBoldTextInBrightColors)` setting.

### Minimum contrast ratio

Terminals often have contrast issues due to some conflict with dark/light themes, ANSI colors or shells/programs running, and more.

The minimum contrast ratio feature solves this problem by either increasing or reducing the luminance of the text's foreground color until a 4.5:1 contrast ratio is met.

One downside of this is that colored text may sometimes lose some of its saturation. This feature can be configured or disabled to get the original colors with:

```json
"terminal.integrated.minimumContrastRatio": 1
```

## Ligatures

Terminal [ligatures](https://en.wikipedia.org/wiki/Ligature_(writing)) allow rendering multiple characters as one. You can enable ligatures in the terminal by configuring the `setting(terminal.integrated.fontLigatures.enabled)`  setting, provided that the terminal font family (`setting(terminal.integrated.fontFamily)`) supports them.

![Enabling ligatures will combine certain sets of characters into one, for example >= into the greater than or equals sign](images/appearance/ligatures.png)

### Font feature settings

When ligatures are enabled, it's also possible to set a detailed list of font feature settings in the format of the [`font-feature-settings`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-feature-settings) CSS property. This gives you more fine-grained control over how ligatures are rendered. For example, disabling the standard `calt` ligatures and enabling a particular font variant looks something like `"calt" off, "ss03"`. For a list of variants supported by the font, consult the font's documentation.

### Fallback ligatures

When a font supports ligatures but VS Code does not support parsing the font in question, you can either disable [GPU acceleration](#gpu-acceleration) or specify a list of character sequences to manually join to make ligatures.

By default, the fallback ligatures are a list of common ligatures used in coding. Fine-tune the list by configuring the `setting(terminal.integrated.fontLigatures.fallbackLigatures)` setting. For example:

```json
// Only use ligatures for `ff`, `fi` and `fl`
"terminal.integrated.fontLigatures.fallbackLigatures": [
    "ff",
    "fi",
    "fl"
]
```

## GPU acceleration

The terminal features two different renderers, each of which have different trade offs:

- WebGL renderer - True GPU acceleration.
- DOM renderer - A fallback renderer that's much slower but has great compatibility.

GPU acceleration driven by the WebGL renderer is enabled in the terminal by default. This helps the terminal work faster and display at a high FPS by significantly reducing the time the CPU spends rendering each frame.

The default `setting(terminal.integrated.gpuAcceleration)` value of `auto` tries the WebGL renderer and if it failed will fall back to the DOM renderer. When on Linux VMs, browsers that don't support WebGL, or machines with outdated drivers, WebGL may not work properly.

### Custom glyphs

When [GPU acceleration](#gpu-acceleration) is enabled, custom rendering, rather than the font, improves how some characters display in the terminal. These include box drawing characters (`U+2500-U+257F`), block elements (`U+2580-U+259F`) and a subset of Powerline symbols (`U+E0B0-U+E0BF`). This means that the configured font does not need to support these characters as well as having the characters draw pixel perfect and stretch to the size of the entire cell.

Below are some examples of these characters with custom line height and letter spacing configured. Notice how there are no gaps between cells thanks to the custom glyphs:

![Box drawing, block characters and some Powerline symbols fill the entire cell in the terminal](images/appearance/custom-glyphs.png)

This feature can be disabled by setting `setting(terminal.integrated.customGlyphs)` to `false`.

### Rescaling ambiguous width glyphs

Some unicode characters have an ambiguous width where the backend and frontend of the terminal may not agree on the size. When [GPU acceleration](#gpu-acceleration) is enabled and this ambiguity occurs, the glyph will be rescaled horizontally to fit in a single cell and prevent overlapping.

For example, the below image features roman numeral characters that are squashed into a single cell:

![VIII and XII characters would be rescaled horizontally so as to not overlap with following characters. They feature a thinner stroke width when this happens due to the scaling](images/appearance/rescale-on.png)

This feature can be disabled by setting `setting(terminal.integrated.rescaleOverlappingGlyphs)` to `false`, which would result in the following overlapped rendering:

![When off, the VIII and XII characters may overlap the following characters](images/appearance/rescale-off.png)

## Customizing your prompt

Most shells allow extensive customization of the terminal prompt. This is done by configuring your shell outside VS Code, typically by modifying the `$PS1` variable, setting a `$PROMPT_COMMAND` or installing a plugin.

Some prompts like [Starship](https://starship.rs/) and [oh-my-posh](https://ohmyposh.dev/) show things like git status and work with most shells, so they're a good choice when starting out.

## Common questions

### Why is my terminal showing a multi-colored triangle or a black rectangle?

The terminal can have problems with GPU accelerated rendering in some environments. For example, you might see a big multi-colored triangle instead of text. This is typically caused by driver/VM graphics issues and the same also happens in Chromium. Work around these issues by launching `code` with the `--disable-gpu` flag or by setting `setting(terminal.integrated.gpuAcceleration)` to `off` to avoid using the canvas in the terminal. See the [GPU acceleration](#gpu-acceleration) section for more information.

### Why are the colors in the terminal not correct?

The [minimum contrast ratio feature](#minimum-contrast-ratio) can cause colors to not be displayed as expected. It can be disabled with:

```json
"terminal.integrated.minimumContrastRatio": 1
```

Background on why this is the default is explained in [vscode#146406](https://github.com/microsoft/vscode/issues/146406#issuecomment-1084825473).
