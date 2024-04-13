# Color Formatting Tools for Obsidian and RunJS

This repository contains JavaScript scripts designed for use with the [RunJS plugin](https://github.com/eoureo/obsidian-runjs) for [Obsidian.md](https://obsidian.md/), a powerful note-taking tool that uses markdown files. These scripts allow for dynamic conversion between different color code formats and visual highlighting of color codes. The scripts are contained in markdown-formatted files to allow for easy integration with your Obsidian vault.

## Purpose

These scripts enhance note-taking involving colors and color schemes by: 

- Converting color code formats between HEX, RGB, and HSL
- Highlighting color codes by adding background colors to selected color code text
- Adjusting the foreground color of the highlighted text to either black or white by evaluating which provides the most optimal readability

## Scripts

The scripts are organized into two files:

- `ConvertColorFormat.md` contains scripts to convert color codes between HEX, RGB, and HSL formats:
  - `Convert to RGB`
  - `Convert to HEX`
  - `Convert to HSL`
- `ColorCodeBackgroundFill.md` contains a script to highlight color code text with the color that text represents

## Usage

### `ConvertColorFormat`

To use one of the conversion scripts, first highlight text in your note which reperesents a color code, then trigger the appropriate function using your preferred RunJS-configured method. Please refer to the documentation provided by [RunJS](https://github.com/eoureo/obsidian-runjs) for further guidance.

Text strings may be formatted in the following ways: 

- HEX:
  - `\#000000` or `\#000`
  - `000000` or `000`
  - `&num;000000` or `&num;000`
- RGB:
  - `rgb(r, g, b)`
- HSL:
  - `hsl(h, v%, l%)`


> [!TIP]
> Because Obsidian interprets text prepended with the number symbol (`#`) as a note tag, it's necessary to format HEX colors by using `&num;` or `\#` instead. When viewing the note in Reading mode, both will be rendered as `#`.


Example usage:

- Highlight a text string such as `d0d0d0`.
- Invoke the desired script. For example, you can click on the script name in the RunJS panel in the left sidebar, or you can add the script as a 'Command' in the RunJS settings and then assign it to a hotkey. I have assigned the script `Convert to RGB` to the hotkey `Ctrl`+`Alt`+`r`.
- The selected text is replaced with the re-formatted version, in this case `rgb(208, 208, 208)`.

### `ColorCodeBackgroundFill`

To use the color highlighting script, follow the same procedure as above. 

Text strings may be formatted in the following ways:

- HEX:
  - `\#000000` or `\000`
  - `000000` or `000`
  - `&num;000000` or `&num;000`
- RGB:
  - `rgb(r, g, b)`
- HSL:
  - Not currently supported

Example usage:

- Highlight a text string such as `d0d0d0`.
- Invoke the script `ColorCodeBackgroundFill`.
- The selected text is now surrounded by HTML which sets the background color to that selection, sets the appropriate foreground color to provide optimal contrast, and adds a little padding for readability. For example: `<span style="background-color:#D0D0D0; color:black; padding:2px">\#D0D0D0</span>` which will display as highlighted text in Reading mode.  

## Setup and Configuration

Follow these steps to use the scripts in Obsidian.

1. Install the RunJS plugin
   - Open Obsidian and go to `Settings > Community plugins` and click the `Browse` button.
   - Search for `RunJS` and then click the link to the plugin's info page in the results
   - Click `Install` then `Enable`
   - Follow the instructions to configure RunJS to your needs

2. Add script files to Obsidian vault
   - Method 1: 
     - Create a new note inside whichever folder you have designated in the RunJS settings.
     - Copy and paste* the contents of one or both of the files contained in this repository into the new note.
     - The scripts can remain separated like they are or you may combine them into a single file. RunJS will read them as separate functions either way.
   - Method 2:
      - Download the files in this repository and then move them into whichever folder you have designated in the RunJS settings.

3. Configure your preferred method to run the scripts
   - Go to the RunJS Settings page.
   - Find the method you would like to use to run each script. Click the `+` (plus) button by your preferred method and then choose the appropriate script from the popup dialog.
   - Refer to the [RunJS documentation](https://github.com/eoureo/obsidian-runjs) for more specific guidance.

> [!CAUTION]
> If you copy code from the files in this repository, **make sure you first click the `raw` button** and then copy. Because of the way GitHub renders markdown files, the code block headers will only appear in the `raw` format. These headers are a crucial component for RunJS to identify scripts to use.

## License

This project is released under the MIT License. See the [LICENSE](LICENSE) file for details.
