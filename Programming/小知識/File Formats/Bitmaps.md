# how to represent an image?

A very simple way to represent an image is:
using a grid of pixels (or dots), each of which has a color.

for black-and-white images, we just need 1 bit per pixel
* 0 to represent black
* 1 to represent white

for more colorful images, we just need more bits per pixel.
* [BMP](https://en.wikipedia.org/wiki/BMP_file_format), [JPEG](https://en.wikipedia.org/wiki/JPEG)[^1], [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics) supports *24-bit color*, so they use 24 bits per pixel
* BMP actually supports 1, 4, 8, 16, 24, and 32-bit color

In this sense, an image is a *bitmap*, a map of bits.
A 24-bit BMP file, is just a sequence of bits. Every 24 bits represent a pixel's color.

[^1]: explaination on [[JPEG and data recovery]]
___

# pixel

A 24-bit BMP uses:
* **8 bits** to signify the amount of **red**
* **8 bits** to signify the amount of **green**
* **8 bits** to signify the amount of **blue**
in a pixel's color.
this is where the name *RGB color* came from.

however, BMP stores these values **backwards**, as **BGR**
some BMPs even store the entire bitmpa backwards, that is the image's top row is stored at the end of the BMP file

and the values are between `0-255` in decimal or `0x00-0xff` in [[CS50x 4-1 Memory#Color|hexadecimal]]
don't understand why it's `255` or `0xff` though? learn about [[CS50x 0-1 Information in computer explained#Byte|byte]]
___

# metadata

aside from just the image, a BMP file also have *metadata*
which is some information like the image's *height* and *width*

it's stored at the beginning of the file, in the form of 2 *headers*
but it's not the same as [[CS50x 2-1 Compiling#Library header file|C's header files]]
also these headers have evolved over time, but the latest version of *Microsoft's BMP format, 4.0*, debuted with Windows 95

the first header is `BITMAPFILEHEADER`
* 14 bytes long

the second header is `BITMAPINFOHEADER`
* 40 bytes long

then there is the actual bitmap
* an array of bytes, every 3 of which present a pixel's color
___

# Image Filtering
it means taking the pixels of the original image, and modifying each pixel in such a way that a particular effect is apparent in the resulting image.


**Grayscale**
take an colorful image and convert it to a black-to-white image.

so long the RGB values are all equal, the pixel's color will be varying shades of gray
higher values meaning lighter shades (closer to white), lower values meaning darker shades

to convert a pixel to grayscale, we just need to make its RGB values are all equal
but what that value should be?
how about just taking the average of RGB values?
this way we can keep the general brightness or darkness of the original color. seems okay.

then if we apply this to every pixel in the image, it is converted to grayscale.


**Reflection**
Reflecting an image, or mirroring, is just move the pixels on the left to the right, vice versa.
but we didn't change the RGB values, we just move a pixel's position.


**Blur**
There are many ways to blur or soften an image.
We're using **box blur** as an example.
It takes the color values of neighboring pixels and average them out and then give it back to all the neighbors. ^8a5fec

if we have a grid of pixels like this:
| 1 | 2 | 3 | 4 |
| :-: | :-: | :-: | :-: |
| 5 | 6 | 7 | 8 |
| 9 | 10 | 11 | 12 |
| 13 | 14 | 15 | 16 |
for each pixel, we would set its value the average of its surrounding 8 pixels' color.
eg.
* take average color of `1, 2, 3, 5, 6, 7, 9, 10, 11` and put it to `6`
* take `6, 7, 8, 10, 11, 12, 14, 15, 16` and put it to `11`
* take `10, 11, 12, 14, 15, 16` and put it to `15`


**Edges**
Ever wonder how artificial intelligence algorithms detect *edges*[^1] in an image?
[^1]: lines in the image that create a boundary between one object and another ^d8b734

One way to achieve this effect is by applying the [Sobel operator](https://en.wikipedia.org/wiki/Sobel_operator)
* much like [[#^8a5fec|blurring]], we look at each pixel together with its surrounding pixels
* instead of calculating the average color values of the pixels, we calculate a weighted sum of the color values of the surrounding pixels in this case
* since edges could take place in botha vertical and a horizontal direction, we'll need to compute 2 weighted sums
	* one for detecting edges in the x direction
	* one for detecting edges in the y direction
	* actually there are 2 *kernels* for this purpose
**Gx**
| -1 | 0 | 1 |
| :-: | :-: | :-: |
| -2 | 0 | 2 |
| -1 | 0 | 1 |
**Gy**
| -1 | -2 | -1 |
| :-: | :-: | :-: |
| 0 | 0 | 0 |
| 1 | 2 | 1 |

how us use these *kernels*?
1. for each color values for each pixel, we'll compute 2 values *Gx* and *Gy*.
2. To compute *Gx* for the red channel value of a pixel, we'll take the surrounding 9 pixels' red channel value and multiply each by the corresopnding value in the *Gx kernel*
3. Take the sum of the resulting values

wah.. what? how?
if the pixels on the right are a similar color to the pixels on the left, when we take the sum, the result will be close to 0. if not, the result will be very positive or very negative.
the change in color implies that this pixel could be a part of the boundary between objects.
similarly for the y direction, it's the same process.

but that leave us *Gx* and *Gy*. a pixel's color only accept 1 value.
how do we combine *Gx* and *Gy*?
$$\sqrt{Gx^2 + Gy^2}$$
then round the result to the nearest integer and capped at 255.

how about the pixels at the edge or corner of the image.
for simplicity just treat the image as there was a 1 pixel solid black border around the image.
so the pixels past the edge of the image has a color value of 0.
___
