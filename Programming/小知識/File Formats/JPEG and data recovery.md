# signatures

JPEGs are more complicated than BMPs.
since they have *signatures*
* patterns of bytes that can distinguish them from other file formats
* the first 3 bytes of JPEGs are `0xff`, `0xd8`, `0xff`
* the forth byte is either `0xe0`, `0xe1`, `0xe2`, ..., `0xef`
   the first 4 bits of the forth byte are `1110`

if we find this pattern of 4 bytes on media known to store photos, like a camera's memory card, they indicate the start of a JPEG.
although we might encounter these patterns on some disk by chance, so it's not totally reliable.
___

# cameras' storage

usually digital cameras tend to store photographs *contiguously* on memory cards.
that means each photo is stored immediately after the previously taken photo.
thus the start of a JPEG usually demarks the end of another.

also digital cameras often initialize cards with a *FAT file system*, whose *block size* is 512 bytes (B).
it means that the camera can only write to those cards in units of 512 B.

eg.
a photo that's 1 MB (ie. 1,048,576 B), takes up $1,048,576 \div 512 = 2048$ *blocks* on a memory card
if a photo that's only 1 byte or even smaller, it also going to take up 2048 *blocks* of memory
the wasted space on disk is called *slack space*

*Forensic investigators* (數碼鑑識調查員) often look at *slack space* for remnants of suspicious data.
___
