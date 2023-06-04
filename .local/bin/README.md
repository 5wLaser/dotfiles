# 5wock
Screen locker wrapper.

Dependencies:
- alock - https://github.com/arkq/alock
- xss-lock
- kitty / alacritty
- cli-visualizer

`5wock` to lock, `5wock warn` to sleep for XORG_SCREENSAVER_CYCLE seconds and hang.

If you have that music visualizer, it will be displayed.



# basicrop
Basic cropping script.

Dependencies:
- feh
- hacksaw
- sxhkd
- graphicsmagick
- bc

These deps can be easily replaced with other programs. `hacksaw` can be replaced with `slop`, `gm convert` (`graphicsmagick`) with `convert` (imagemagick), or `feh` with `nsxiv` (with some argument tweaking). It's a very simple script, with the most complex part being some multiplication.

## instructions
Set screen dimensions in the file (default is 1920x1080).

Usage: `basicrop [INFILE] [OUTFILE]`. If OUTFILE isn't specified, INFILE will always be overwritten.
- 'c' to crop
- 'u' to undo
- 'A' to toggle anti-aliasing
- 'Escape' or 'q' to quit
- 'Return' to save file
- 'shift' + 'Return' to overwrite file

Because there's no way to communicate the image's location to the script, you cannot zoom in or translate.

## how work?
`basicrop input output` will open `input` in fullscreen mode. With your screen dimensions, this acts like a ruler by constraining the sides of your image to the sides of the screen. Cropping will simply take the dimensions of the screen captured in an area, and scale it up or down to the size of the image. Those dimensions are trimmed if needed, and then fed to graphicsmagick to produce `output`.

sxhkd is used to temporarily steal a set of keys from your keyboard to use for the script. I'm sure there's a way to do it with more simple X tools, but if i wanted to make this more of a PITA then i would've make a Real image editor in C, with zooming and translating and drawing and such. But i didn't feel like it

## why?
I had all the tools already on my system, as well as a brand new screenshot script in need of a dedicated editor. In fact, this script used to be embedded into that screenshot script until i decided to debloat it and share it elsewhere. 

In the future i plan on making another editor, with a similar feature count to `flameshot`'s editor (markup stuff), but keyboard-driven and with better image viewing, like with zooming and moving. Such a minimal standalone editor doesn't really exist right now.



# ocrgrab
small desktop barcode and texteract grabber

dependencies:
- `zbar`
- `tesseract` (and applicable language models)
- `hacksaw` and `shotgun` or other screenshot tool

crops a section of the screen, notifies if a barcode, text, or nothing was captured, and sends to clipboard.



# screenshot
Minimal screenshot script for X

dependencies:
- `shotgun`
- `hacksaw`
- `basicrop` - optional, can be replaced

`shotgun` and `hacksaw` can be replaced with `maim` and `slop`. 
Replacing the editor is harder, but still straightforward. `basicrop` takes an input and output file, so if an editor can do something like that, then no real modding is needed.

## Usage
First, set `$fotoDir` to an appropriate default dir for your screenshots, and change `$foto` if you want a different default filename.
```
Usage: screenshot [options]

Options:
-o [OUTFILE]	specify an output file
crop		use crop/window select mode
temp		send file to /tmp for temporary storage and clipboarding
file		file-only, don't save image to clipboard
edit		edit the most recently-taken screenshot
```
## Examples
To crop a screenshot to out.png without copying to clipboard:

`screenshot crop -o out.png file`

To take a screenshot to out.png without the clipboard while cropping:

`screenshot -o out.png file crop`

To remove clipboard copying and take a cropped screenshot to out.png:

`screenshot file crop -o out.png`

To take a full screenshot to /tmp:

`screenshot temp`

To edit:

`screenshot edit` to save in `$fotoDir`, or `screenshot temp edit` to keep the result in /tmp
