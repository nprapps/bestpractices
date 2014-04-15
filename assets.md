## Best Practices for Assets

### General

* Use the `app-template` [assets rig](https://github.com/nprapps/app-template/blob/master/PROJECT_README.md#save-media-assets). Do **not** commit them to the project repository.  

### Photos

* Images should be stored with the following path convention: `www/assets/img/$SECTION_NAME/$SLUG$VERSION.jpg`
* `$SECTION_NAME` can be whatever project specific section slug is appropriate (chapter, slide, page, etc.)
* `$SLUG` should follow URL conventions: all lowercase, no whitespace, dashes instead of underscores.
* `$VERSION` may be:

<table>
  <tr><th>$VERSION</th><th>aspect ratio</th><th>target</th><th>resolution</th></tr>
  <tr><td></td><td>original</td><td>archival</td><td>full</td></tr>
  <tr><td>-16x9</td><td>16x9</td><td>archival</td><td>full</td></tr>
  <tr><td>-sq</td><td>square</td><td>archival</td><td>full</td></tr>
  <tr><td>-16x9-d</td><td>16x9</td><td>desktop</td><td>1200x675</td></tr>
  <tr><td>-16x9-m</td><td>16x9</td><td>mobile</td><td>400x225</td></tr>
  <tr><td>-sq-m</td><td>square</td><td>mobile</td><td>420x420</td></tr>
</table>  

* File extenions should always be lowercase.

### Audio

* Audio should be encoded as OGG (for Firefox) and MP3 (for other browsers).
* Music should be encoded as 128bps CBR Stereo:
    * MP3: `lame -m s -b 128 input.wav output.mp3`
    * OGG: `oggenc -m 128 -M 128 -o output.oga input.wav`
* Voice should be encoded as 96bps CBR Mono:
    * MP3: `lame -m m -b 96 input.wav output.mp3`
    * OGG: `oggenc -m 96 -M 96 -o output.oga input.wav`

### Video

* TKTK
