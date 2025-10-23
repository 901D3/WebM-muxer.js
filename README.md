# _**WebM-muxer.js**_

A simple and lightweight WebM muxer written in JS\
Completely browser-based

# _**How to use**_

You know how to link a js file so we gonna skip it

### **Constructor**
Remember to define an array literal variable for storing the data

```
const muxer = new WebMMuxer({
  width: <width value>,
  height: <height value>,
  frameRate: <frame rate value>,
  bufferSize: <bufferSize value>,
})

const chunks = []; // We will store added frames and header to this array
```

──────────

### **Adding frames**

For toBlob()
```
const blob = await new Promise((resolve) => canvas.toBlob(resolve, "image/webp", <quality value>));
const buffer = new Uint8Array(await blob.arrayBuffer());

muxer.addFrameFromBlob(buffer, chunks);
```

──────────

For WebCodecs

```
// Example VideoEncoder
const encoder = new VideoEncoder({
  output: (chunk) => {
    const buffer = new Uint8Array(chunk.byteLength);
    chunk.copyTo(buffer);
    muxer.addFramePreEncoded(buffer, chunks);
  },
  error: (err) => console.error(err), // Safety
});


encoder.configure({
  codec: "vp8", // Has to be VP8
  width: <width value>,
  height: <height value>,
  frameRate: <frame rate value>,
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
// We assume the blob in add frame for toBlob() is in a loop
// and this finalizing is outside
const blob = muxer.finalize(array);
```

──────────

Demo coming soon!
But it is used in my other web-based tools by the way

Check out the source code!
[github.com/901D3/WebM-muxer.js](https://github.com/901D3/WebM-muxer.js)




