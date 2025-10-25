# _**WebM-muxer.js**_

A simple and lightweight WebM muxer written in JS\
Completely browser-based!
And it can be for noobs...

# _**How to use**_

You know how to link a js file so we gonna skip it

## _**Constructor**_
Remember to define an array literal variable for storing the data

```
const muxer = new WebMMuxer({
  codec: <only "vp8" or "vp9">,
  width: <width value>,
  height: <height value>,
  frameRate: <frame rate value>,
  bufferSize: <bufferSize value>,

  // Anything below this point is VP9 only, codec: "vp8" will ignore those settings below
  // Use default VP9 only setting if you re unsure(by not providing them)
  profile: <profile value, default to 0>,
  level: <level value, default to 255>,
  bitDepth: <color bit depth, default to 8>,
  chromaSubsampling: <default to 1>,
  colorRange: <default to 1>,
  colorPrimaries: <default to 1>,
  transferCharacteristics: <default to 1>,
})

const chunks = []; // We will store added frames and header to this array
```

──────────

## _**Adding frames**_

### For toBlob()

```
// Using canvas as an example
const blob = await new Promise((resolve) => canvas.toBlob(resolve, "image/webp", <quality value, HAS TO BE BELOW 0.9>));
const buffer = new Uint8Array(await blob.arrayBuffer());

muxer.addFrameFromBlob(buffer, chunks);
```

──────────

### For WebCodecs

```
// Example VideoEncoder
const encoder = new VideoEncoder({
  output: (chunk) => {
    const buffer = new Uint8Array(chunk.byteLength);
    chunk.copyTo(buffer);
    muxer.addFramePreEncoded(buffer, chunks);
  },
  error: (err) => console.error(err),
});


encoder.configure({
  codec: "vp09",
  width: <width value>,
  height: <height value>,
  frameRate: <frame rate value>,
  bitrate: <bitrate value>, // Maybe it is the knob for quality
});

const videoFrame = new VideoFrame(canvas, {
  timestamp: null,
});

encoder.encode(videoFrame, {keyframe: true});
videoFrame.close();

// When done adding frames
await encoder.flush();
encoder.close();
```

──────────

### Finalizing

```
// Finalizer will return a blob
// We assume the `blob` variable in add frame for toBlob() is in a loop and this finalizing is outside
const blob = muxer.finalize(array);
```

──────────

Demo coming soon!\
But maybe the demo is hidden in my tools

Check out the source code!\
[github.com/901D3/WebM-muxer.js](https://github.com/901D3/WebM-muxer.js)




