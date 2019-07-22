## Best Practices for Assets

* File extensions should always be lowercase.
* 

### Audio

* Audio should be encoded as OGG (for Firefox) and MP3 (for other browsers).
* Music should be encoded as 128bps CBR Stereo:
    * MP3: `lame -m s -b 128 input.wav output.mp3`
    * OGG: `oggenc -m 128 -M 128 -o output.oga input.wav`
* Voice should be encoded as 96bps CBR Mono:
    * MP3: `lame -m m -b 96 input.wav output.mp3`
    * OGG: `oggenc -m 96 -M 96 -o output.oga input.wav`

### Video

Video encoding depends on the role of the video in a project--ambient video (such as animated backdrops) should be much smaller, because the user doesn't have a choice about playback. On-demand video can be much larger--but consider uploading it to a hosting service like YouTube instead of manually encoding, since those will automatically size and compress for different screen sizes and bandwidth requirements.

When encoding for a browser, use FFMPEG to output as MP4. The command for doing this in a way that will cooperate with all platforms is somewhat verbose:

```sh
ffempeg \
-i input.avi \
-vcodec libx264 \
-strict -2 \
-pix_fmt yuv420p \
-profile:baseline \
-preset veryslow \
output.mp4
```

Additional options that may prove useful:

* `-an` - Removes audio for autoplaying video in modern browsers
* `-ss X -t Y` - Extracts Y seconds, starting at X
* `-vf scale=ZZZ:-1` - Resizes the video to ZZZ across, maintaining its current aspect ratio.
* `-crf X` - Sets the quality of the compression to X, where 0 is lossless and 51 is gibberish.

