---
layout: default-layout
title: Dynamsoft Label Recognizer JavaScript API - Recognition APIs
description: This page shows the recognition APIs of Dynamsoft Label Recognizer JavaScript SDK.
keywords: label recognizer, api reference, javascript, js, recognition
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
breadcrumbText: Recognition APIs
---

# Recognition APIs

## Recognize Labels from Still Images

| API Name | Description |
|---|---|
| [recognize()](#recognize) | Recognizes labels from an image. |
| [recognizeBase64String()](#recognizebase64string) | Recognizes labels from a base64-encoded image (with or without MIME). |
| [recognizeUrl()](#recognizeurl) | Recognizes labels from an image specified by its URL. |
| [recognizeBuffer()](#recognizebuffer) | Recognizes labels from raw image data. |

## Recognize Labels from Video Frames

| API Name | Description |
|---|---|
| [onUniqueRead](#onuniqueread) | This event is triggered when a new, unduplicated label is found. |
| [onMRZRead](#onmrzread) | This event is triggered after the library finishes scanning a frame with the built-in templates `passportMRZ` , `video-passportMRZ` ， `visaMRZ` , `video-visaMRZ` , `MRZ` or `video-MRZ` . |
| [onFrameRead](#onframeread) | This event is triggered after the library finishes scanning a frame. |
| [recognizeCurrentFrame()](#recognizecurrentframe) | Scans the current frame of the video for labels. |
| [startScanning()](#startscanning) | Open the camera and starts continuous scanning of incoming frames. |
| [pauseScanning()](#pausescanning) | Pause continuous scanning but keep the video stream. |
| [resumeScanning()](#resumescanning) | Resumes continuous scanning. |
| [stopScanning()](#stopscanning) | Stops continuous scanning and closes the video stream. |

## recognize

Recognizes labels from an image. 

```typescript
recognize(source: Blob | Buffer | ArrayBuffer | Uint8Array | Uint8ClampedArray | HTMLImageElement | HTMLCanvasElement | HTMLVideoElement | DCEFrame | string): Promise<DLRResult[]>
```

**Parameters**

`source` : specifies the image to recognize. The supported image formats include `png` , `jpeg` , `bmp` , `gif` and a few others (some browsers support `webp` , `tif` ). Also note that the image can be specified in a lot of ways including binary data, base64 string (with MIME), URL, etc.

**Return value**

A promise resolving to a `DLRResult\[\]` object that contains all the label results found in this image.

**Code snippet**

```js
let results1 = await recognizer.recognize(blob);
let results2 = await recognizer.recognize(htmlImageElement);
let results3 = await recognizer.recognize(url);
let results4 = await recognizer.recognize(strBase64WithMime); // like `data:image/png;base64,iV************`
```

You can even use an `HTMLVideoElement` as the source. If the video is playing, the current frame will be recognized.

```js
let results;
try {
    // The current frame will be recognized.
    results = await recognizer.recognize(htmlVideoElement);
    for (let result of results) {
        for (let lineResult of result.lineResults) {
            console.log(lineResult.text);
        }
    }
} catch (ex) {
    // If there is no frame in the video, throw an exception.   
}
```

**See also**

* [Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob)
* [Buffer](https://nodejs.org/api/buffer.html#buffer_class_buffer)
* [ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)
* [Uint8Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array)
* [Uint8ClampedArray](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8ClampedArray)
* [HTMLImageElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement)
* [HTMLCanvasElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement)
* [HTMLVideoElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLVideoElement)
* [DLRResult](./interface/dlr-result.md)
* [DCEFrame](https://www.dynamsoft.com/camera-enhancer/docs/programming/javascript/api-reference/interface/dceframe.html?ver=latest)

## recognizeBase64String

Recognizes labels from a base64-encoded image (with or without MIME).

```typescript
recognizeBase64String(base64Str: string): Promise<DLRResult[]>
```

**Parameters**

`base64Str` : specifies the image represented by a string.

**Return value**

A promise resolving to a `DLRResult\[\]` object that contains all the label results found in this image.

**Code snippet**

```js
let results = await recognizer.recognizeBase64String(strBase64); //e.g. `data:image/jpg;base64,Xfjshekk....` or `Xfjshekk...`.
for (let result of results) {
    for (let lineResult of result.lineResults) {
        console.log(lineResult.text);
    }
}
```

**See also**

* [DLRResult](./interface/dlr-result.md)

## recognizeUrl

Recognizes labels from an image specified by its URL. Note that the image should either be from the same domain or has the 'Access-Control-Allow-Origin' header set to allow access from your current domain.

```typescript
recognizeUrl(url: string): Promise<DLRResult[]>
```

**Parameters**

`url` : specifies the image by its URL.

**Return value**

A promise resolving to a `DLRResult\[\]` object that contains all the label results found in this image.

**Code snippet**

```js
let results = await recognizer.recognizeUrl("https://www.yourdomain.com/imageWithBarcodes.png");
for (let result of results) {
    for (let lineResult of result.lineResults) {
        console.log(lineResult.text);
    }
}
```

**See also**

* [DLRResult](./interface/dlr-result.md)

## recognizeBuffer

Recognizes labels from raw image data. It is an advanced API, if you don't know what you are doing, use [recognize](#recognize) instead. 

```typescript
recognizeBuffer(buffer: Blob | Buffer | ArrayBuffer | Uint8Array | Uint8ClampedArray, width: number, height: number, stride: number, format: EnumImagePixelFormat): Promise<DLRResult[]>
```

**Parameters**

`buffer` : specifies the raw image represented by a `Uint8Array` , `Uint8ClampedArray` , `ArrayBuffer` , `Blob` or `Buffer` object.

`width` : image width.

`height` : image height.

`stride` : `image-width * pixel-byte-length` .

`format` : pixel format.

**Return value**

A promise resolving to a `DLRResult\[\]` object that contains all the label results found in this image.

**Code snippet**

```js
let results = await reader.recognizeBuffer(u8RawImage, 1280, 720, 1280 * 4, DLR.EnumDLRImagePixelFormat.IPF_ABGR_8888);
for (let result of results) {
    for (let lineResult of result.lineResults) {
        console.log(lineResult.text);
    }
}
```

**See also**

* [Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob)
* [Buffer](https://nodejs.org/api/buffer.html#buffer_class_buffer)
* [ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)
* [Uint8Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array)
* [Uint8ClampedArray](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8ClampedArray)
* [EnumImagePixelFormat]({{ site.enumerations }}image-pixel-format.html)
* [DLRResult](./interface/dlr-result.md)

| [onFrameRead](#onframeread) | This event is triggered after the library finishes scanning a frame. |
| [recognizeCurrentFrame()](#recognizecurrentframe) | Scans the current frame of the video for labels. |

## onUniqueRead

This event is triggered when a new, unduplicated label is found.

<!--TODO: result format may not right-->

```typescript
onUniqueRead: (txt: string, result: DLRLineResult) => void
```

**Arguments**

`txt` : a string that holds the label text (one single line). 

`result` : a `DLRLineResult` object that contains more detailed info about the returned text.

**Code Snippet**

```javascript
let recognizer = await Dynamsoft.DLR.LabelRecognizer.createInstance({
    runtimeSettings: "video-numberLetter"
});
let enhancer = await Dynamsoft.DCE.CameraEnhancer.createInstance();
await enhancer.setUIElement(Dynamsoft.DLR.LabelRecognizer.defaultUIElementURL);
recognizer.cameraEnhancer = enhancer;
recognizer.onUniqueRead = (txt, result) => {
    console.log(txt);
}
recognizer.startScanning(true);
```

**See also**

* [DLRLineResult](./interface/dlr-line-result.md)

## onMRZRead

This event is triggered after the library finishes scanning a frame with the built-in templates `passportMRZ` , `video-passportMRZ` ， `visaMRZ` , `video-visaMRZ` , `MRZ` or `video-MRZ` .

```typescript
onMRZRead: (txt: string, results: DLRLineResult[]) => void
```

**Arguments**

`txt` : a string that holds the multi-line text result from a machine-readable zone. 

`results` : one or more `DLRLineResult` object(s) that contains more detailed info about the returned text.

**Code Snippet**

```javascript
let recognizer = await Dynamsoft.DLR.LabelRecognizer.createInstance({
    runtimeSettings: "video-MRZ"
});
let enhancer = await Dynamsoft.DCE.CameraEnhancer.createInstance();
await enhancer.setUIElement(Dynamsoft.DLR.LabelRecognizer.defaultUIElementURL);
recognizer.cameraEnhancer = enhancer;
recognizer.onMRZRead = (txt, results) => {
    console.log(txt);
}
recognizer.startScanning(true);
```

**See also**

* [DLRLineResult](./interface/dlr-line-result.md)

## onFrameRead

This event is triggered after the library finishes scanning a frame.

```typescript
onFrameRead: (results: DLRResult[]) => void
```

**Arguments**

`results` : a `DLRResult` object that contains all the label results in this frame.

**Code Snippet**

```js
let recognizer = await Dynamsoft.DLR.LabelRecognizer.createInstance({
    runtimeSettings: "video-passportMRZ"
});
let enhancer = await Dynamsoft.DCE.CameraEnhancer.createInstance();
await enhancer.setUIElement(Dynamsoft.DLR.LabelRecognizer.defaultUIElementURL);
recognizer.cameraEnhancer = enhancer;
recognizer.onFrameRead = results => {
    for (let result of results) {
        for (let lineResult of result.lineResults) {
            console.log(lineResult.text);
        }
    }
};
recognizer.startScanning(true);
```

**See also**

* [DLRResult](./interface/dlr-result.md)

## recognizeCurrentFrame

Scans the current frame of the video for barcodes.

```typescript
recognizeCurrentFrame(): Promise<DLRResult[]>
```

**Return value**

A promise resolving to a `DLRResult\[\]` object that contains all the label results found in this frame.

**Code Snippet**

```js
let recognizer = await Dynamsoft.DLR.LabelRecognizer.createInstance({
    runtimeSettings: "video-passportMRZ"
});
let enhancer = await Dynamsoft.DCE.CameraEnhancer.createInstance();
await enhancer.setUIElement(Dynamsoft.DLR.LabelRecognizer.defaultUIElementURL);
recognizer.cameraEnhancer = enhancer;
await cEnhancer.open();
let results = await recognizer.recognizeCurrentFrame();
for (let result of results) {
    for (let lineResult of result.lineResults) {
        console.log(lineResult.text);
    }
}
```

**See also**

* [DLRResult](./interface/dlr-result.md)

## startScanning

Open the camera and starts continuous scanning of incoming frames.

```typescript
startScanning(appendOrShowUI?: boolean): Promise<PlayCallbackInfo>;
```

**Parameters**

`appendOrShowUI` : this parameter specifies how to handle the UI that comes with the bound CameraEnhancer instance. When set to true, if the UI doesn't exist in the DOM tree, the CameraEnhancer instance will append it in the DOM and show it; if the UI already exists in the DOM tree but is hidden, it'll be displayed. When not set or set to false, it means not to change the original state of that UI: if it doesn't exist in the DOM tree, nothing shows up on the page; if it exists in the DOM tree, it may or may not show up depending on its original state.

**Return value**

A promise resolving to a `PlayCallbackInfo` object which contains the resolution of the video.

**Code Snippet**

```js
let recognizer = await Dynamsoft.DLR.LabelRecognizer.createInstance({
    runtimeSettings: "video-passportMRZ"
});
let enhancer = await Dynamsoft.DCE.CameraEnhancer.createInstance();
await enhancer.setUIElement(Dynamsoft.DLR.LabelRecognizer.defaultUIElementURL);
recognizer.cameraEnhancer = enhancer;
recognizer.onUniqueRead = (txt, results) => {
    console.log(txt);
    for (let result of results) {
        for (let lineResult of result.lineResults) {
            console.log(lineResult.text);
        }
    }
}
recognizer.startScanning(true);
```

**See also**

* [DLRResult](./interface/dlr-result.md)

## pauseScanning

Pause continuous scanning but keep the video stream.

```typescript
pauseScanning(): void;
```

## resumeScanning

Resumes continuous scanning.

```typescript
resumeScanning(): void;
```

## stopScanning

Stops continuous scanning and closes the video stream.

```typescript
stopScanning(hideUI?: boolean): void;
```

**Parameters**

`hideUI` : this parameter specifies how to handle the UI that comes with the bound CameraEnhancer instance. When set to true, if the UI doesn't exist in the DOM tree or it exists but is hidden, nothing is done; if the UI already exists in the DOM tree and is shown, it'll be hidden. When not set or set to false, it means not to change the original state of that UI: if it doesn't exist in the DOM tree, nothing happens; if it exists in the DOM tree, it may or may not be hidden depending on its original state.

**Code Snippet**

```js
let recognizer = await Dynamsoft.DLR.LabelRecognizer.createInstance({
    runtimeSettings: "video-passportMRZ"
});
let enhancer = await Dynamsoft.DCE.CameraEnhancer.createInstance();
await enhancer.setUIElement(Dynamsoft.DLR.LabelRecognizer.defaultUIElementURL);
recognizer.cameraEnhancer = enhancer;
recognizer.onUniqueRead = (txt, results) => {
    console.log(txt);
    for (let result of results) {
        for (let lineResult of result.lineResults) {
            console.log(lineResult.text);
        }
    }
    // Stops scanning the video input as soon as a label is found.
    recognizer.stopScanning(true);
}
await recognizer.startScanning(true);
```

**See also**

* [DLRResult](./interface/dlr-result.md)
