# Convert Color Format

## Convert color code to RGB format

```js RunJS="{n:'ColorTools/Convert to RGB', t:'s'}"
/**
 * Converts the selected HEX or HSL value to RGB format.
 * Accepts hex codes with `&num;`, `#`, or no prefix 
 */
const selectedText = this.app.workspace.activeLeaf.view.editor.getSelection();

function hexToRgb(hex) {
    hex = hex.replace(/&num;|#/g, "");
    const r = parseInt(hex.substring(0, 2), 16);
    const g = parseInt(hex.substring(2, 4), 16);
    const b = parseInt(hex.substring(4, 6), 16);
    return `rgb(${r}, ${g}, ${b})`;
}

function hslToRgb(h, s, l) {
    h = Number(h);
    s = Number(s) / 100;
    l = Number(l) / 100;
    let c = (1 - Math.abs(2 * l - 1)) * s;
    let x = c * (1 - Math.abs((h / 60) % 2 - 1));
    let m = l - c / 2;
    let r = Math.round((c + m) * 255);
    let g = Math.round((x + m) * 255);
    let b = Math.round((m + m) * 255);
    return `rgb(${r}, ${g}, ${b})`;
}

let output;
if (/^(&num;|#)?[0-9A-F]{6}$/i.test(selectedText) || /^(&num;|#)?[0-9A-F]{3}$/i.test(selectedText)) {
    output = hexToRgb(selectedText);
} else if (/^hsl\((\d+),\s*([\d.]+)%,\s*([\d.]+)%\)$/i.test(selectedText)) {
    const [h, s, l] = selectedText.match(/\d+/g);
    output = hslToRgb(h, s, l);
} else {
    output = "Invalid color format.";
}

this.app.workspace.activeLeaf.view.editor.replaceSelection(output);
```


## Convert color code to HEX format

```js RunJS="{n:'ColorTools/Convert to HEX', t:'s'}"
/**
 * Converts the selected RGB or HSL value to HEX format
 */
const selectedText = this.app.workspace.activeLeaf.view.editor.getSelection();

function hslToRgb(h, s, l) {
    h = Number(h);
    s = Number(s) / 100;
    l = Number(l) / 100;
    let c = (1 - Math.abs(2 * l - 1)) * s;
    let x = c * (1 - Math.abs((h / 60) % 2 - 1));
    let m = l - c / 2;
    let r = Math.round((c + m) * 255);
    let g = Math.round((x + m) * 255);
    let b = Math.round((m + m) * 255);
    return [r, g, b];
}

function rgbToHex(r, g, b) {
    r = parseInt(r);
    g = parseInt(g);
    b = parseInt(b);
    return "&num;" + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1).toUpperCase();
}

let output;
if (/^rgb\((\d+),\s*(\d+),\s*(\d+)\)$/i.test(selectedText)) {
    const [r, g, b] = selectedText.match(/\d+/g);
    output = rgbToHex(r, g, b);
} else if (/^hsl\((\d+),\s*([\d.]+)%,\s*([\d.]+)%\)$/i.test(selectedText)) {
    const [h, s, l] = selectedText.match(/\d+/g);
    const rgb = hslToRgb(h, s, l);
    output = rgbToHex(rgb[0], rgb[1], rgb[2]);
} else if (/^(&num;|#)?[0-9A-F]{6}$/i.test(selectedText) || /^(&num;|#)?[0-9A-F]{3}$/i.test(selectedText)) {
    const hex = selectedText.replace(/&num;|#/g, "");
    output = "&num;" + hex.toUpperCase();
} else {
    output = "Invalid color format.";
}

this.app.workspace.activeLeaf.view.editor.replaceSelection(output);
```


## Convert color code to HSL format

```js RunJS="{n:'ColorTools/Convert to HSL', t:'s'}"
/**
 * Converts selected RGB or HEX value to HSL format
 * Accepts hex codes with `&num;`, `#`, or no prefix
 */
const selectedText = this.app.workspace.activeLeaf.view.editor.getSelection();

function hexToRgb(hex) {
    hex = hex.replace(/&num;|#/g, "");  // Remove &num; or # if present
    let r = parseInt(hex.substring(0, 2), 16);
    let g = parseInt(hex.substring(2, 4), 16);
    let b = parseInt(hex.substring(4, 6), 16);
    return [r, g, b];
}

function rgbToHsl(r, g, b) {
    r /= 255, g /= 255, b /= 255;
    let max = Math.max(r, g, b), min = Math.min(r, g, b);
    let h, s, l = (max + min) / 2;

    if (max === min) {
        h = s = 0;
    } else {
        let d = max - min;
        s = l > 0.5 ? d / (2 - max - min) : d / (max + min);
        h = max === r ? (g - b) / d + (g < b ? 6 : 0) :
            max === g ? (b - r) / d + 2 :
            (r - g) / d + 4;
        h /= 6;
        h *= 360;
    }
    s *= 100;
    l *= 100;
    return `hsl(${Math.round(h)}, ${Math.round(s)}%, ${Math.round(l)}%)`;
}

let output;
if (/^rgb\((\d+),\s*(\d+),\s*(\d+)\)$/i.test(selectedText)) {
    const [r, g, b] = selectedText.match(/\d+/g);
    output = rgbToHsl(r, g, b);
} else if (/^(&num;|#)?[0-9A-F]{6}$/i.test(selectedText) || /^(&num;|#)?[0-9A-F]{3}$/i.test(selectedText)) {
    const rgb = hexToRgb(selectedText);
    output = rgbToHsl(rgb[0], rgb[1], rgb[2]);
} else {
    output = "Invalid color format.";
}

this.app.workspace.activeLeaf.view.editor.replaceSelection(output);
```
