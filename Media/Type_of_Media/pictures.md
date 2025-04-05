# FFmpeg Pictures

[Home](../../README.md)/[FFmpeg](../FFmpeg.md)

## List of Content

- [Basic Converting](#basic_converting)
- [Resize](#resize)
- [Batch Converting](#batch_converting)


## Basic_Converting

### Example png to webp

```bash
ffmpeg -i input.jpg output.webp
```

### Set Quality

```bash
ffmpeg -i input.jpg -q:v 80 output.webp
```

## Resize

Sometimes we need to resize image to make our app more reliable

### Basic Resize

```bash
ffmpeg -i input.png -vf scale=WIDTH:HEIGHT output.png
```

For example, resizing an image to 256x256 pixels:

```bash
ffmpeg -i input.png -vf scale=256:256 output.png
```

### Keep Aspect Ratio (Auto-Adjust Height or Width)

```bash
ffmpeg -i input.png -vf scale=256:-1 output.png
```

```bash
ffmpeg -i input.png -vf scale=-1:256 output.png
```

### ffmpeg -i input.png -vf "scale=256:256:flags=lanczos" output.png

```bash
ffmpeg -i input.png -vf "scale=256:256:flags=lanczos" output.png
```

## Batch_Converting

```bash
for file in *.png; do ffmpeg -i "$file" -vf scale=256:256 "resized_$file"; done
```