# File Format vs File Size

File size varies wildly depending on the file type and format, below are some short exercises to visualise this using Linux.

For this activity, make sure that you have the following programs installed: `ffmpeg`, `zip`, `lame`.

```bash
sudo dnf install ffmpeg zip lame
```

## Audio 

First, we will use the `ffmpeg` program to generate a test audio file. This will be a simple sine tone at 1000Hz, lasting 30 seconds:

```bash
ffmpeg -f lavfi -i sine=frequency=1000:duration=30 -c:a pcm_s16le test.wav
```

Inspect the file using `ls` and `ffprobe`:

```bash
ls -lh test.wav
```

```bash
ffprobe test.wav
```

This allows us to see the filesize and bitrate of the given file. 

## Lossless Compression

We can now package up the wav file into a zip file using the zip tool:

```bash
zip test.zip test.wav
```

Verify the size:

```
ls -lh test.zip
```

Unzip the file again:

```bash
unzip test.zip
```

As this process is lossless and non-destructive, the file is identical to the original file after being decompressed. We can verify this using `sha256sum` to check the file hash. 

```bash
sha256sum test.wav
```

## Lossy Compression

We will now convert the `test.wav` file into a set of different quality MP3 files using `ffmpeg`. Our initial file has a bitrate of ~700Kb/s, we will sample this down to 320Kb/s, 128Kb/s, and 64Kb/s.

```bash
ffmpeg -i test.wav -b:a 320k high.mp3  
ffmpeg -i test.wav -b:a 128k medium.mp3  
ffmpeg -i test.wav -b:a 64k low.mp3
```

Compare the file size and clarity - are any acceptable? 

## Video Compression

Next, we can apply these principals to video files, again using the `ffmpeg` file. For this we will be using the Big Buck Bunny sample files available for free from the Blender Foundation: [Download](https://download.blender.org/demo/movies/BBB/bbb_sunflower_1080p_30fps_normal.mp4.zip)

Using the following `ffmpeg` commands, re-encode the Big Buck Bunny clip into three different *constant rate factor*. 

```bash
ffmpeg -i input.mp4 -crf 18 high.mp4  
ffmpeg -i input.mp4 -crf 28 medium.mp4  
ffmpeg -i input.mp4 -crf 35 low.mp4
```

Once complete, review the different file sizes and video quality.
