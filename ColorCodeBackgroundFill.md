# Color Code Background Fill

## Highlight selected color code with that color

```js RunJS="{n:'ColorTools/ColorCodeBackgroundFill', t:'s'}"
/**
 * Formats the selected color code by setting it as the background color
 * and choosing a contrasting color (black or white) as the foreground.
 * Accepts RGB values formatted `rgb(r, g, b)` or HEX codes prepended either by `\#`, by `&num;`, or by nothing at all.
 */
function getContrastYIQ(colorInput){
    let r = 0, g = 0, b = 0;
    const originalInput = colorInput;
    colorInput = colorInput.replace(/&num;|\\#/g, "");  // Normalize color code by removing &num; or \#

    if (/^([0-9A-F]{3}){1,2}$/i.test(colorInput)) {  // HEX format
        if (colorInput.length == 3) {
            r = parseInt(colorInput[0] + colorInput[0], 16);
            g = parseInt(colorInput[1] + colorInput[1], 16);
            b = parseInt(colorInput[2] + colorInput[2], 16);
        } else {
            r = parseInt(colorInput.substr(0,2), 16);
            g = parseInt(colorInput.substr(2,2), 16);
            b = parseInt(colorInput.substr(4,2), 16);
        }
    } else if (/^rgb\((\d{1,3}),\s*(\d{1,3}),\s*(\d{1,3})\)$/i.test(colorInput)) {  // RGB format
        [r, g, b] = colorInput.match(/\d+/g).map(Number);
    }

    const yiq = ((r*299)+(g*587)+(b*114))/1000;
    return (yiq >= 128) ? 'black' : 'white';
}

const selectedText = this.app.workspace.activeLeaf.view.editor.getSelection();
let formattedText = "";
let validColor = /^(&num;|\\#)?[0-9A-F]{3}([0-9A-F]{3})?$/i.test(selectedText) || /^rgb\((\d{1,3}),\s*(\d{1,3}),\s*(\d{1,3})\)$/i.test(selectedText);

if (validColor) {
    const contrastColor = getContrastYIQ(selectedText);
    if (selectedText.startsWith('rgb')) {
        formattedText = `<span style="background-color:${selectedText}; color:${contrastColor}; padding: 2px;">${selectedText}</span>`;
    } else {
        const hexColor = selectedText.replace(/&num;|\\#/g, "").toUpperCase();
        formattedText = `<span style="background-color:#${hexColor}; color:${contrastColor}; padding: 2px;">\\#${hexColor}</span>`;
    }
    this.app.workspace.activeLeaf.view.editor.replaceSelection(formattedText);
} else {
    this.app.workspace.activeLeaf.view.editor.replaceSelection("Invalid color format.");
}
```