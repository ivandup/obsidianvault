# stegseek
Check if a image has hidden passphrases
```
stegseek <fileName>
```

# exiftool
Check the file info:
```
exiftool <fileName>
```

Example output:
```
ExifTool Version Number         : 13.50
File Name                       : cat_the_troll.jpg
Directory                       : .
File Size                       : 16 kB
File Modification Date/Time     : 2014:10:04 10:57:30+02:00
File Access Date/Time           : 2026:06:27 07:00:53+02:00
File Inode Change Date/Time     : 2026:06:27 06:58:22+02:00
File Permissions                : -rw-rw-r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : inches
X Resolution                    : 72
Y Resolution                    : 72
Image Width                     : 500
Image Height                    : 302
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 500x302
Megapixels                      : 0.151

```

# BinWalk
```
binwalk -e <fileName>
```

# strings
```
string <fileName>
```