## Best Practices for Assets

* File extensions should always be lowercase.
* When possible, lazy-load assets.
* Files can be checked into source control if they're under 5MB, and don't have a method of getting them through an automated process. Alternatively, provide a build task that will create/download those assets that are larger or depend on data sources (such as the covers for the book concierge).

### Audio

* Music should be encoded as 128bps CBR Stereo:
    * MP3: `lame -m s -b 128 input.wav output.mp3`
* Voice should be encoded as 96bps CBR Mono:
    * MP3: `lame -m m -b 96 input.wav output.mp3`

### Photo

You can resize images on the command line with ImageMagick. This script (borrowed from the [Ellicott City](https://github.com/nprapps/ellicott-city#snippets) project) will look at all the JPGs in a folder and saved the resized versions in a `resized` folder:

```
for f in *.jpg; do convert $f -quality 75 -resize 1600x1200\> -strip -sampling-factor 4:2:0 -define jpeg:dct-method=float -interlace Plane resized/$f; done
```

### Video

Video encoding depends on the role of the video in a project--ambient video (such as animated backdrops) should be much smaller, because the user doesn't have a choice about playback. On-demand video can be much larger--but consider uploading it to a hosting service like YouTube instead of manually encoding, since those will automatically size and compress for different screen sizes and bandwidth requirements.

When encoding for a browser, use FFMPEG to output as MP4. The command for doing this in a way that will cooperate with all platforms is somewhat verbose:

```sh
ffmpeg \
-i inputfile \
-ss 0 -t 5 \
-an \
-vcodec libx264 \
-preset veryslow \
-strict -2 \
-pix_fmt yuv420p \
-crf 21 \
-vf scale=800:-1 \
outputfile.mp4
```

Note that the order of arguments is important. Some of these parameters may be tweakable depending on what your video is meant to accomplish.

* `-an` - Removes audio for autoplaying video in modern browsers
* `-ss X -t Y` - Extracts Y seconds, starting at X
* `-vf scale=ZZZ:-1` - Resizes the video to ZZZ across, maintaining its current aspect ratio.
* `-crf X` - Sets the quality of the compression to X, where 0 is lossless and 51 is gibberish. 21 is a good starting place.

