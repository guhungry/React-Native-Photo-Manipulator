# React Native Photo Manipulator
Image processing library to edit photo programmatically in React Native

[![Build Status](https://travis-ci.com/guhungry/react-native-photo-manipulator.svg?branch=master)](https://travis-ci.com/guhungry/react-native-photo-manipulator)
[![npm version](http://img.shields.io/npm/v/react-native-photo-manipulator.svg?style=flat)](https://npmjs.org/package/react-native-photo-manipulator "View this project on npm")

## Platform Supported
* [x] Android
* [x] iOS

## Getting started

Read [Get Started Guide](docs/Getting-Started.md)

## Usage
Import library with
```javascript
import RNPhotoManipulator from 'react-native-photo-manipulator';
```

## API
* __Methods:__
  * [`crop()`](README.md#crop-and-resize)
  * [`optimize()`](README.md#optimize)
  * [`overlayImage()`](README.md#overlay-image)
  * [`printText()`](README.md#print-text)
  * [`batch()`](README.md#batch)
  
* __Types:__
    * [`ImageSource`](README.md#imagesource)
    * [`PhotoBatchOperations`](README.md#photobatchoperations)
    * [`PhotoBatchOverlay`](README.md#photobatchoperations)
    * [`PhotoBatchPrintText`](README.md#photobatchoperations)
    * [`Point`](README.md#point)
    * [`Rect`](README.md#rect)
    * [`Size`](README.md#size)
    * [`TextOptions`](README.md#textoptions)
### Method
#### Crop and resize
Crop image with `cropRegion` and resize to `targetSize` if specified

##### Signature
```typescript
static crop(image: ImageSource, cropRegion: Rect, targetSize?: Size) => Promise<string>
```

| Parameter       | Type                                      | Required     | Description                                    |
| --------------- | ----------------------------------------- | ------------ | ---------------------------------------------- |
| `image`         | [`ImageSource`](README.md#imagesource)    | Yes          | The image                                      |
| `cropRegion`    | [`Rect`](README.md#rect)                  | Yes          | The region of image to be cropped              |
| `targetSize`    | [`Size`](README.md#size)                  | No           | The target size of result image                |
| `mimeType`      | 'image/jpeg', 'image/png'                 | No           | Output file format                             |


##### Returns
Promise with image path in cache directory

##### Example
```javascript
const image = "https://unsplash.com/photos/qw6qQQyYQpo/download?force=true";
const cropRegion = { x: 5, y: 30, size: 400, width: 250 };
const targetSize = { size: 200, width: 150 };

PhotoManipulator.crop(image, cropRegion, targetSize).then(path => {
    console.log(`Result image path: ${path}`);
});
```
<img src="/docs/result-crop.jpg?raw=true" alt="Result Crop" width="100" />
<img src="/docs/result-crop-resize.jpg?raw=true" alt="Result Crop and Resize" width="100" />

#### Optimize
Save result `image` with specified `quality` between `0 - 100` in jpeg format


##### Signature
```typescript
static optimize(image: ImageSource, quality: number) => Promise<string>
```

| Parameter       | Type                                      | Required     | Description                                    |
| --------------- | ----------------------------------------- | ------------ | ---------------------------------------------- |
| `image`         | [`ImageSource`](README.md#imagesource)    | Yes          | The image                                      |
| `quality`       | number                                    | Yes          | The quality of result image between `0 - 100`  |

##### Returns
Promise with image path in cache directory

##### Example
```javascript
const image = "https://unsplash.com/photos/qw6qQQyYQpo/download?force=true";
const quality = 90;

PhotoManipulator.optimize(image, 90).then(path => {
    console.log(`Result image path: ${path}`);
});
```
<img src="/docs/result-optimize.jpg?raw=true" alt="Result Optimize" width="100" />

#### Overlay Image
Overlay image on top of background image

##### Signature
```typescript
static overlayImage(image: ImageSource, overlay: ImageSource, position: Point) => Promise<string>
```

| Parameter       | Type                                      | Required     | Description                                            |
| --------------- | ----------------------------------------- | ------------ | ------------------------------------------------------ |
| `image`         | [`ImageSource`](README.md#imagesource)    | Yes          | The background image                                   |
| `overlay`       | [`ImageSource`](README.md#imagesource)    | Yes          | The overlay image                                      |
| `position`      | [`Point`](README.md#point)                | Yes          | The position of overlay image in background image      |
| `mimeType`      | 'image/jpeg', 'image/png'                 | No           | Output file format                                     |

##### Returns
Promise with image path in cache directory

##### Example
```javascript
const image = "https://unsplash.com/photos/qw6qQQyYQpo/download?force=true";
const overlay = "https://www.iconfinder.com/icons/1174949/download/png/128";
const position = { x: 5, y: 20 };

PhotoManipulator.overlayImage(image, overlay, position).then(path => {
    console.log(`Result image path: ${path}`);
});
```
<img src="/docs/result-overlay.jpg?raw=true" alt="Result Overlay" width="100" />

#### Print Text
Print texts into image

##### Signature
```typescript
static printText(image: ImageSource, texts: TextOptions[]) => Promise<string>
```

| Parameter       | Type                                      | Required     | Description                                            |
| --------------- | ----------------------------------------- | ------------ | ------------------------------------------------------ |
| `image`         | [`ImageSource`](README.md#imagesource)    | Yes          | The image                                              |
| `texts`         | [`TextOptions[]`](README.md#textoptions)  | Yes          | The list of text operations                            |
| `mimeType`      | 'image/jpeg', 'image/png'                 | No           | Output file format                                     |

##### Returns
Promise with image path in cache directory

##### Example
```javascript
const image = "https://unsplash.com/photos/qw6qQQyYQpo/download?force=true";
const texts = [
    { position: { x: 50, y: 30 }, text: "Text 1", textSize: 30, color: "#000000" },
    { position: { x: 50, y: 30 }, text: "Text 1", textSize: 30, color: "#FFFFFF", thickness: 3 }
];

PhotoManipulator.printText(image, texts).then(path => {
    console.log(`Result image path: ${path}`);
});
```
<img src="/docs/result-print-text.jpg?raw=true" alt="Result Print Text" width="100" />

#### Batch
Crop, resize and do operations (overlay and printText) on image

##### Signature
```typescript
static batch(image: ImageSource, operations: PhotoBatchOperations[], cropRegion: Rect, targetSize?: Size, quality?: number) => Promise<string>
```

| Parameter       | Type                                                        | Required     | Description                                       |
| --------------- | ----------------------------------------------------------- | ------------ | ------------------------------------------------- |
| `image`         | [`ImageSource`](README.md#imagesource)                      | Yes          | The image                                         |
| `operations`    | [`PhotoBatchOperations[]`](README.md#photobatchoperations)  | Yes          | The list of operations                            |
| `cropRegion`    | [`Rect`](README.md#rect)                                    | Yes          | The region of image to be cropped                 |
| `targetSize`    | [`Size`](README.md#size)                                    | No           | The target size of result image                   |
| `quality`       | number                                                      | No           | The quality of result image between `0 - 100`     |
| `mimeType`      | 'image/jpeg', 'image/png'                                   | No           | Output file format                                |

##### Returns
Promise with image path in cache directory

##### Example
```javascript
const image = "https://unsplash.com/photos/qw6qQQyYQpo/download?force=true";
const cropRegion = { x: 5, y: 30, size: 400, width: 250 };
const targetSize = { size: 200, width: 150 };
const operations = [
    { operation: "text", options: { position: { x: 50, y: 30 }, text: "Text 1", textSize: 30, color: "#000000" } },
    { operation: "overlay", overlay: "https://www.iconfinder.com/icons/1174949/download/png/128", position: { x: 5, y: 20 } },
];
const quality = 90;

PhotoManipulator.batch(image, cropRegion, targetSize, operations, quality).then(path => {
    console.log(`Result image path: ${path}`);
});
```
<img src="/docs/result-batch.jpg?raw=true" alt="Result Batch" width="100" />


### Types
#### ImageSource
Image resource can be url or require()

| Type      | Description                                                      |
| --------- | ---------------------------------------------------------------- |
| number    | Image from require('path/to/image')                              |
| string    | Image from url supports file://, http://, https:// and ftp://    |

#### PhotoBatchOperations
Represent [overlay image](README.md#photobatchoverlay) or [print text](README.md#photobatchprinttext) operation

#### PhotoBatchOverlay
Overlay image batch operation

| Property        | Type                                      | Description                                           |
| --------------- | ----------------------------------------- | ----------------------------------------------------- |
| `operation`     | "overlay"                                 |                                                       |
| `overlay`       | [`ImageSource`](README.md#imagesource)    | The overlay image                                     |
| `position`      | [`Point`](README.md#point)                | he position of overlay image in background image      |

#### PhotoBatchPrintText
Print text batch operation

| Property        | Type                                      | Description                          |
| --------------- | ----------------------------------------- | ------------------------------------ |
| `operation`     | "text"                                    |                                      |
| `options`       | [`TextOptions`](README.md#textoptions)    | The text attributes                  |

#### Point
Represent position (x, y) from top-left of image

| Property        | Type      | Description                          |
| --------------- | --------- | ------------------------------------ |
| `x`             | number    | The x-axis coordinate from top-left  |
| `y`             | number    | The y-axis coordinate from top-left  |

#### Rect
Represent area of region

| Property        | Type      | Description                          |
| --------------- | --------- | ------------------------------------ |
| `x`             | number    | The x-axis coordinate from top-left  |
| `y`             | number    | The y-axis coordinate from top-left  |
| `width`         | number    | The width of the region              |
| `height`        | number    | The height of the region             |

#### Size
Represent size (width, height) of region or image

| Property        | Type      | Description              |
| --------------- | --------- | ------------------------ |
| `width`         | number    | The width of the region  |
| `height`        | number    | The height of the region |

#### TextOptions
Represent attributes of text such as text, color, size, and etc.

| Property        | Type                          | Description                                    |
| --------------- | ----------------------------- | ---------------------------------------------- |
| `position`      | [`Point`](README.md#point)    | The position of the text in background image   |
| `text`          | string                        | The value of the text                          |
| `textSize`      | number                        | The size of the text                           |
| `color`         | string                        | The color of the text                          |
| `thickness`     | number                        | The thickness (border width) of the region     |
