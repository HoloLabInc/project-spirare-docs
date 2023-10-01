# POML

English | [日本語](./POML-ja.md)

<br>

POML is an XML-based text file format designed for describing 3D content.

File extension: `.poml`

## Coordinate System

All coordinate systems, such as positions, rotations, and scales, specified within POML are in the Z-up right-handed coordinate system.

Please note that this is the definition of the logical coordinate space represented by POML, and it is independent of the coordinate systems used by rendering engines or 3D graphics libraries that render POML.

## Examples

Example of geodetic placement

```xml
<poml>
  <scene>
    <model src="./model.glb">
      <geo-reference latitude="35.68122469808435"
                     longitude="139.76719554151146"
                     ellipsoidal-height="50"
                     enu-rotation="0,0,0,1">
      </geo-reference>
    </model>
  </scene>
</poml>
```

Example of space placement

```xml
<poml>
  <scene>
    <space-reference space-id="MapA">
    </space-reference>
    <model ar-display="none" src="./MapA.glb">
    </model>
    <model position="0 0 1" src="./apple.glb">
    </model>
  </scene>
</poml>
```

## Common Attributes

| name | type     | default value | example  | description           |
| ---- | -------- | ------------- | -------- | --------------------- |
| id   | `string` | `""`          | `"id-0"` | Unique ID within POML |

Additionally, attributes starting with `_` (underscore) are unique to editing applications. These attributes are ignored by applications displaying POML.

## `<poml/>`

The required tag representing the root of POML.

### Parent Element

None

### Child Element

- `<meta/>` (0 or 1)
- `<scene/>` (0 or 1)

## `<meta/>`

A tag representing the meta information of POML.

### Parent Element

- `<poml/>`

### Child Element

- `<title/>` (0 or 1)

## `<title/>`

A tag representing representing the title of POML.

### Parent Element

- `<meta/>`

### Child Element

- Text

### Example

```xml
<poml>
  <meta>
    <title>Sample POML</title>
  </meta>
</poml>
```

## `<scene/>`

A tag representing the scene information of POML.

### Parent Element

- `<poml/>`

### Child Element

- PomlElement (0 or more)
- `<geo-reference/>` (0 or 1)
- `<spece-reference/>` (0 or more)

### Example

```xml
<poml>
  <scene>
    <model src="sample.glb">
    </model>
  </scene>
</poml>
```

## PomlElement

A group of tags representing a single object within the scene.

The following tags belong to the PomlElement group:

- `<element/>`
- `<model/>`
- `<image/>`
- `<video/>`
- `<text/>`

PomlElement can form a hierarchical structure, and position and rotation are relative coordinates to the parent element.

### Parent Element

- `<scene/>`
- PomlElement

### Child Element

- PomlElement (0 or more)
- `<geo-reference/>` (0 or 1)
- `<spece-reference/>` (0 or more)

### Common Attributes

| name              | type                    | default value       | example                 | description                                                                        |
| ----------------- | ----------------------- | ------------------- | ----------------------- | ---------------------------------------------------------------------------------- |
| position          | `Vector3`               | `"0,0,0"`           | `"10,20,30"`            | Relative position to the parent element                                            |
| scale             | `number` or `Vector3`   | `"1"`               | `"1"`, `"10,20,30"`     | Relative scale to the parent element                                               |
| rotation          | `Quaternion`            | `"0,0,0,1"`         | `"0,0,0,1"`             | Relative rotation to the parent element                                            |
| scale-by-distance | `boolean`               | `"false"`           | `"true"`                | Whether to scale the size according to the distance from the camera when displayed |
| min-scale         | `number` or `Vector3`   |                     | `"1"`, `"10,20,30"`     | Minimum magnification by scale-by-distance <br>(no minimum if not specified)       |
| max-scale         | `number` or `Vector3`   |                     | `"1"`, `"10,20,30"`     | Maximum magnification by scale-by-distance <br>(no maximum if not specified)       |
| rotation-mode     | `string` (RotationMode) |                     | `"billboard"`           | Rotation mode <br>(no rotation if not specified)                                   |
| display           | `string` (Display)      | `"visible"`         | `"visible"`             | Display mode                                                                       |
| ar-display        | `string` (ArDisplay)    | `"same-as-display"` | `"visible"`             | Display mode during AR                                                             |
| web-link          | `string`                |                     | `"https://example.com"` | Link associated with PomlElement <br>(corresponds to href in <a> in HTML)          |

- Vector3  
  Consists of three values, x, y, and z.  
  In POML, the text representation is comma-separated (`"1,1,1"`) or space-separated (`"1 1 1"`)

- Quaternion  
  Consists of four values, x, y, z, and w.  
  In POML, the text representation is comma-separated (`"0,0,0,1"`) or space-separated (`"0 0 0 1"`)

- RotationMode

  - `"vertical-billboard"`: Rotates always facing the camera, with the vertical direction fixed
  - `"billboard"`: Rotates always facing the camera

- Display

  - `"visible"`: Display
  - `"none"`: Hide
  - `"occlusion"`: Display as occlusion

- ArDisplay
  - `"same-as-display"`: Use the same value as the `display` attribute during AR
  - `"visible"`: Display during AR
  - `"none"`: Hide during AR
  - `"occlusion"`: Display as occlusion during AR

#### Attribute Usage Examples

```xml
<element position="10,20,30"
         scale="2">
</element>
```

```xml
<element position="0.0,0.0,1.1"
         scale="1,2,3"
         rotation="0,0,0,1"
         scale-by-distance="true"
         min-scale="1"
         max-scale="3"
         rotation-mode="vertical-billboard"
         display="visible"
         ar-display="visible"
         web-link="https://example.com">
</element>
```

## `<element/>` (PomlElement)

A PomlElement with no special meaning.

Used to create hierarchical structures or groupings of PomlElements.

### Example

```xml
<element>
  <model src="https://example.com/sample.glb">
  </model>
  <text text="sample text">
  </text>
</element>
```

## `<model/>` (PomlElement)

A PomlElement representing a 3D model.

### Examples

```xml
<model src="./sample.glb">
</model>
```

```xml
<model src="https://example.com/sample.glb">
</model>
```

```xml
<model src="https://example.com/sample"
       filename="sample.glb">
</model>
```

### Attributes

| name     | type     | default value | example                            | description |
| -------- | -------- | ------------- | ---------------------------------- | ----------- |
| src      | `string` | `""`          | `"https://example.com/sample.glb"` | Source URL  |
| filename | `string` | `""`          | `"sample.glb"`                     | Filename    |

If a filename is specified, the model file format is determined by the extension of the filename.  
If a filename is not specified, the extension of the URL is used.

### Supported Extensions

- `.glb`

## `<image/>` (PomlElement)

A PomlElement representing an image.

### Examples

```xml
<image src="./sample.png">
</image>
```

```xml
<image src="https://example.com/sample.png">
</image>
```

```xml
<image src="https://example.com/sample"
       filename="sample.png">
</image>
```

### Attributes

| name     | type     | default value | example                            | description        |
| -------- | -------- | ------------- | ---------------------------------- | ------------------ |
| src      | `string` | `""`          | `"https://example.com/sample.png"` | Source URL         |
| filename | `string` | `""`          | `"sample.png"`                     | Filename           |
| width    | `number` | `undefined`   | `"1"`, `"0.1"`                     | Display width [m]  |
| height   | `number` | `undefined`   | `"1"`, `"0.1"`                     | Display height [m] |

If a filename is specified, the image file format is determined by the extension of the filename.
If a filename is not specified, the extension of the URL is used.

If either `width` or `height` is not specified, the unspecified value will be calculated from the image's aspect ratio.
If neither `width` nor `height` is specified, the `width` defaults to 1m.

### Supported Extensions

- `.png`
- `.jpg` (`.jpeg`)
- `.gif`

## `<video/>` (PomlElement)

A PomlElement representing an video.

### Examples

```xml
<video src="./sample.mp4">
</video>
```

```xml
<video src="https://example.com/sample.mp4">
</video>
```

```xml
<video src="https://example.com/sample"
       filename="sample.mp4">
</video>
```

### Attributes

| name     | type     | default value | example                            | description        |
| -------- | -------- | ------------- | ---------------------------------- | ------------------ |
| src      | `string` | `""`          | `"https://example.com/sample.mp4"` | Source URL         |
| filename | `string` | `""`          | `"sample.mp4"`                     | Filename           |
| width    | `number` | `undefined`   | `"1"`, `"0.1"`                     | Display width [m]  |
| height   | `number` | `undefined`   | `"1"`, `"0.1"`                     | Display height [m] |

If a filename is specified, the video file format is determined by the extension of the filename.
If a filename is not specified, the extension of the URL is used.

If either `width` or `height` is not specified, the unspecified value will be calculated from the video's aspect ratio.
If neither `width` nor `height` is specified, the `width` defaults to 1m.

### Supported Extensions

- `.mp4`

## `<text/>` (PomlElement)

A PomlElement representing text.

### Examples

```xml
<text text="Hello World">
</text>
```

```xml
<text text="Hello World"
      font-size="1m"
      font-color="white"
      background-color="#3E8ED0">
</text>
```

### Attributes

| name             | type                | default value | example                | description                                    |
| ---------------- | ------------------- | ------------- | ---------------------- | ---------------------------------------------- |
| text             | `string`            | `""`          | `"sample"`             | Text content                                   |
| font-size        | `string` (FontSize) | `"1m"`        | `"1m"`                 | Font size                                      |
| font-color       | `string` (Color)    | `"#ffffff"`   | `"white"`, `"#ff0000"` | Font color                                     |
| background-color | `string` (Color)    | `"#000000"`   | `"white"`, `"#ff0000"` | Background color                               |
| width            | `number`            | `1`           | `"1"`, `"0.1"`         | Text display width <br>(unlimited if not set)  |
| height           | `number`            | `1`           | `"1"`, `"0.1"`         | Text display height <br>(unlimited if not set) |

- FontSize  
  A string consisting of a numerical value and a unit (e.g., `"1m"`, `"150pt"`). The unit can be specified as `m`, `mm`, or `pt`.

- Color  
  A string representing a web color name (e.g., `"white"`, `"red"`) or an RGB color code (e.g., `"#ff0000"`).

## `<space-reference/>`

A tag for associating scene origin or objects with spatial coordinates.

The association with the spatial coordinates specified by the attirubets is applied to the parent element of the `<space-reference/>` tag.

### Parent Element

- `<scene/>`
- PomlElement

### Child Element

None

### Examples

```xml
<element>
  <space-reference space-id="MapA">
  </space-reference>
</element>
```

```xml
<element>
  <space-reference space-id="12345"
                   space-type="Immersal"
                   position="1,2,3"
                   rotation="0,0,0,1">
  </space-reference>
</element>
```

### Attributes

| name       | type         | default value | example                             | description                                                                          |
| ---------- | ------------ | ------------- | ----------------------------------- | ------------------------------------------------------------------------------------ |
| space-id   | `string`     | `""`          | `"12345"`, `"MapA"`                 | Space ID                                                                             |
| space-type | `string`     | `""`          | `"Immersal"`, `"VuforiaAreaTarget"` | Space type                                                                           |
| position   | `Vector3`    | `"0,0,0"`     | `"1,2,3"`                           | Relative position of the spatial origin from the parent element of `space-reference` |
| rotation   | `Quaternion` | `"0,0,0,1"`   | `"0,0,0,1"`                         | Relative rotation of the spatial origin from the parent element of `space-reference` |

If space-type is not specified, the association will be made with spaces that have a matching space-id, regardless of their space type.

## `<geo-reference/>`

A tag for positioning the scene origin or objects using geodetic coordinates (latitude, longitude, ellipsoidal height).

The geodetic coordinates specified by the attributes are applied to the parent element of the `<geo-reference/>` tag.

### Parent Element

- `<scene/>`
- PomlElement

### Child Element

None

### Example

```xml
<model src="./model.glb">
  <geo-reference latitude="35.68122469808435"
                 longitude="139.76719554151146"
                 ellipsoidal-height="50"
                 enu-rotation="0,0,0,1">
  </geo-reference>
</model>
```

### Attributes

| name               | type         | default value | example                | description                                                 |
| ------------------ | ------------ | ------------- | ---------------------- | ----------------------------------------------------------- |
| latitude           | `number`     | `"0"`         | `"35.68122469808435"`  | Latitude                                                    |
| longitude          | `number`     | `"0"`         | `"139.76719554151146"` | Longitude                                                   |
| ellipsoidal-height | `number`     | `"0"`         | `"50"`                 | Ellipsoidal height                                          |
| enu-rotation       | `Quaternion` | `"0,0,0,1"`   | `"0,0,0,1"`            | Rotation in North (+X) West (+Y) Up (+Z) coordinates system |

The enu-rotation, named for ENU (East, North, Up), actually operates on a different axis order (North, West, Up).
Because this discrepancy leads to confusion, enu-rotation will be obsolete in the future.
