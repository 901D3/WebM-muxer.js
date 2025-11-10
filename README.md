# _**WebM-muxer.js**_

A simple and lightweight WebM muxer written in JS\
Completely browser-based!

# _**How to use**_

You know how to link a js file so we gonna skip it

## _**~~Constructor~~**_

WebM muxer is no longer a class, i switched to IIFE namespace style for performance and convenient\
But it's signatures and functionalities is still the same

```
WebMMuxer.init({
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
```

Remember to define an array literal variable for storing the data
```
const chunks = []; // We will store added frames and header to this array
```

## _**Adding frames**_

### For toBlob()

```
// Using canvas as an example
const blob = await new Promise((resolve) => canvas.toBlob(resolve, "image/webp", <quality value, HAS TO BE BELOW 0.9>));
const buffer = new Uint8Array(await blob.arrayBuffer());

WebMMuxer.addFrameFromBlob(buffer, chunks);
```

──────────

### Finalizing

```
// Finalizer will return a blob
// We assume the `blob` variable in add frame for toBlob() is in a loop and this finalizing is outside
const blob = WebMMuxer.finalize(array);
```

──────────

Demos is hidden in my other stuff

Check out the source code!\
[github.com/901D3/WebM-muxer.js](https://github.com/901D3/WebM-muxer.js)




