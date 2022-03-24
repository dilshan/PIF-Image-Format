# PIF - «Portable Image File» Format
## Overview
The Portable Image Format (PIF) is a basic, bitmap-like image format with the focus on ease of use (implementation) and small size for embedded applications. The file format not only offers special, reduced color sets to reduce size where 24-bit resolution are not required (or unable to be rendered by the display), but also features variable sized color tables to achive good-looking, custom images at reduced bit-per-pixel size. To further reduce the size of the image data, a simple RLE-compression can be used without loosing too many cycles on decompression.

## Features
 - Various Bitformats supported:
   - RGB888 - Uncompressed RGB Image
   - RGB565 - 16bit per Pixel Image, reduced colorset
   - RGB332	- 8bit per Pixel Image, further reducing colors and size
   - RGB16C	- 4bit per Pixel Image, fixed Windows/IBM Style 16 colors
   - B/W - 1bit per Pixel Image, only Black and White
   - Indexed - Custom Pixel bitwidth, using a RGB332, RGB565 or RGB888 color table
 - Basic Compression (RLE)
 - Ease of use and implementation

## Why another Image Format?
I know, I know, there are way too many image standards and I'm sure there is a xkcd comic panel about it already. On the one side this project is an excuse for me to take a deeper look into python and figure this programming language out, on the otherside I have gotten frustrated with the options when one wants to load images on a display from a microcontroller. I've encountered always the same problems there:

Either the compression used on the image format (png/jpg) makes it slow to display the image, but it's at least fast to read due to the small size.
Or the format looked like it was from the late 80s with various extensions bodged into it, resulting into large sizes and oddities to handle (image building from the bottom upwards, for example).

With PIF I am trying to address both issues at once: A image format that is easy to implement and process on weak hardware, while offering smaller sizes. This is achived by a very basic image and file header that is easy to read, a simple RLE compression to reduce size as well as more color options for the pixel when less colors are required.

## Comparison
If the File Format is compared against bitmap, one can see that the image size reduces heavily specially at lower color densities, which are not unlikely to encounter with many displays used at embedded systems.

The Lenna test image was used, found in the test_images folder:
| Image Type    | File Size (Bytes) | BPP | Compression |
|---------------|-------------------|-----|-------------|
| Bitmap (.BPP) | 786 486           | 24  | None        |
| PIF RGB888    | 786 460           | 24  | None        |
| PIF RGB888    | 699 570           | 24  | RLE         |
| PIF RGB565    | 524 316           | 16  | None        |
| PIF RGB565    | 467 955           | 16  | RLE         |
| PIF RGB332    | 262 172           | 8   | None        |
| PIF RGB332    | 105 682           | 8   | RLE         |
| PIF RGB16C    | 131 100           | 4   | None        |
| PIF RGB16C    | 78 757            | 4   | RLE         |
| PIF B/W       | 32 796            | 1   | None        |
| PIF B/W       | 14 144            | 1   | RLE        |

The sizes could likely be further reduced and colors optimized when a indexed 16 color table would be used.
## Tools
### PIF Image Converter
( Required pip packages: [Pillow](https://pillow.readthedocs.io/en/stable/), [PySimpleGUI](https://pysimplegui.readthedocs.io/en/latest/) )
![Image of the Tool](test_images/tool_screenshot.png)
A basic tool that allows to save various image formats (.jpg/.bmp/.png) to the .PIF Image Format. Within the program, various color settings can be applied with dithering, resizing the image as well as include the RLE compresison or not.
## Todo
We still have some steps ahead of us before this project can be considered finished. Here a rough overview of the things that are already done or that are still missing.
 - [ ] Image Converter
	- [x] Basic GUI implementation & preview
	- [x] Dithering Options
	- [x] Saving as .PIF
	- [ ] Custom (Indexed) options
 - [ ] Image viewer
	- [ ] Display the image
	- [ ] Show details and stats
	- [ ] Allow Export to other image formats
 - [ ] Portable C Library Code & Examples
	- [ ] Reference implementation for various platforms
		- [ ] Arduino / Arduino-Framework
		- [ ] ATxmega
		- [ ] GD32VF103 (RISC-V)
	- [ ] Universal / Portable C Library