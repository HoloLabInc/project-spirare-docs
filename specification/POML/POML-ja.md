# POML

[English](./POML.md) | 日本語

<br>

POML は 3D コンテンツの記述を目的とした、XML ベースのテキスト形式のファイルフォーマットである。

拡張子: `.poml`

## 座標系

POML 内で指定する座標・回転・スケールなどの座標系は全て Z-up 右手系である。

これは POML が表現する論理的な座標空間の定義であり、POML を描画する描画エンジンや 3D グラフィックライブラリの座標系とは無関係であることに注意してください。

## 例

地理配置の例

```xml
<poml>
  <scene>
    <model src="./model.glb" rotation="0 0 0.7071 0.7071">
      <geo-reference latitude="35.68122469808435"
                     longitude="139.76719554151146"
                     ellipsoidal-height="50">
      </geo-reference>
    </model>
  </scene>
</poml>
```

空間配置の例

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

## 全タグ共通アトリビュート

| name | type     | default value | example  | description        |
| ---- | -------- | ------------- | -------- | ------------------ |
| id   | `string` | `""`          | `"id-0"` | POML 内で一意な ID |

また、`_` (アンダースコア) から始まるアトリビュートは編集用アプリケーション独自のアトリビュート。
POML を表示するアプリケーションにおいてはこのアトリビュートは無視される。

## `<poml/>`

POML のルートを表すための必須タグ

### 親要素

なし

### 子要素

- `<meta/>` (0 or 1)
- `<scene/>` (0 or 1)

## `<meta/>`

POML のメタ情報を表すタグ

### 親要素

- `<poml/>`

### 子要素

- `<title/>` (0 or 1)

## `<title/>`

POML のタイトルを表すメタ情報

### 親要素

- `<meta/>`

### 子要素

- テキスト

### 例

```xml
<poml>
  <meta>
    <title>Sample POML</title>
  </meta>
</poml>
```

## `<scene/>`

POML のシーン情報を表すタグ

### 親要素

- `<poml/>`

### 子要素

- PomlElement (0 以上)
- `<geo-reference/>` (0 or 1)
- `<spece-reference/>` (0 以上)

### 例

```xml
<poml>
  <scene>
    <model src="sample.glb">
    </model>
  </scene>
</poml>
```

## PomlElement

シーン内の 1 つのオブジェクトを表すタグのグループ

以下のタグが PomlElement グループに属する

- `<element/>`
- `<model/>`
- `<image/>`
- `<video/>`
- `<text/>`

PomlElement は階層構造を作ることができ、`position` や `rotation` は親要素に対する相対座標である。

### 親要素

- `<scene/>`
- PomlElement

### 子要素

- PomlElement (0 以上)
- `<geo-reference/>` (0 or 1)
- `<spece-reference/>` (0 以上)

### 共通アトリビュート

| name              | type                    | default value       | example                 | description                                                             |
| ----------------- | ----------------------- | ------------------- | ----------------------- | ----------------------------------------------------------------------- |
| position          | `Vector3`               | `"0,0,0"`           | `"10,20,30"`            | 親要素に対する相対位置                                                  |
| scale             | `number` or `Vector3`   | `"1"`               | `"1"`, `"10,20,30"`     | 親要素に対する相対スケール                                              |
| rotation          | `Quaternion`            | `"0,0,0,1"`         | `"0,0,0,1"`             | 親要素に対する相対姿勢                                                  |
| scale-by-distance | `boolean`               | `"false"`           | `"true"`                | 表示時にカメラからの距離に応じてサイズを拡大するかどうか                |
| min-scale         | `number` or `Vector3`   |                     | `"1"`, `"10,20,30"`     | scale-by-distance による拡大率の最小値 <br>（指定しない場合最小値なし） |
| max-scale         | `number` or `Vector3`   |                     | `"1"`, `"10,20,30"`     | scale-by-distance による拡大率の最大値 <br>（指定しない場合最大値なし） |
| rotation-mode     | `string` (RotationMode) |                     | `"billboard"`           | 回転モード <br>（指定しない場合回転しない）                             |
| display           | `string` (Display)      | `"visible"`         | `"visible"`             | 表示モード                                                              |
| ar-display        | `string` (ArDisplay)    | `"same-as-display"` | `"visible"`             | AR 表示時の表示モード                                                   |
| web-link          | `string`                |                     | `"https://example.com"` | PomlElement に紐づくリンク <br>(HTML における `<a>` の href に相当)     |

- Vector3  
  x, y, z の 3 つの数値を持つ。
  POML での文字列表現はカンマ区切り (`"1,1,1"`) またはスペース区切り (`"1 1 1"`)

- Quaternion  
  x, y, z, w の 4 つの数値を持つ。
  POML での文字列表現はカンマ区切り (`"0,0,0,1"`) またはスペース区切り (`"0 0 0 1"`)

- RotationMode

  - `"vertical-billboard"`: 鉛直方向は固定したまま、常にカメラ方向に回転する
  - `"billboard"`: 常にカメラ方向に回転する

- Display
  - `"visible"`: 表示
  - `"none"`: 非表示
  - `"occlusion"`: オクルージョンとして表示
- ArDisplay
  - `"same-as-display"`: AR 表示時も `display` アトリビュートと同じ値を使用
  - `"visible"`: AR 表示時に表示
  - `"none"`: AR 表示時に非表示
  - `"occlusion"`: AR 表示時にオクルージョンとして表示

#### アトリビュート使用例

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

特別な意味を持たない PomlElement

階層構造や PomlElement のまとまりを作るために使用する

### 例

```xml
<element>
  <model src="https://example.com/sample.glb">
  </model>
  <text text="sample text">
  </text>
</element>
```

## `<model/>` (PomlElement)

3D モデルを表す PomlElement

### 例

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

### アトリビュート

| name     | type     | default value | example                            | description |
| -------- | -------- | ------------- | ---------------------------------- | ----------- |
| src      | `string` | `""`          | `"https://example.com/sample.glb"` | ソース URL  |
| filename | `string` | `""`          | `"sample.glb"`                     | ファイル名  |

ファイル名が指定されている場合、ファイル名の拡張子でモデルファイルのフォーマットが判別される。  
ファイル名が指定されていない場合、URL の拡張子が利用される。

### サポートされる拡張子

- `.glb`

## `<image/>` (PomlElement)

画像を表す PomlElement

### 例

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

### アトリビュート

| name     | type     | default value | example                            | description      |
| -------- | -------- | ------------- | ---------------------------------- | ---------------- |
| src      | `string` | `""`          | `"https://example.com/sample.png"` | ソース URL       |
| filename | `string` | `""`          | `"sample.png"`                     | ファイル名       |
| width    | `number` | `undefined`   | `"1"`, `"0.1"`                     | 画像表示幅 [m]   |
| height   | `number` | `undefined`   | `"1"`, `"0.1"`                     | 画像表示高さ [m] |

ファイル名が指定されている場合、ファイル名の拡張子で画像ファイルのフォーマットが判別される。  
ファイル名が指定されていない場合、URL の拡張子が利用される。

`width` または `height` の片方が指定されていない場合、指定されていない数値は画像のアスペクト比から計算される。  
`width` と `height` が両方指定されていない場合、`width` が 1m として表示される。

### サポートされる拡張子

- `.png`
- `.jpg` (`.jpeg`)
- `.gif`

## `<video/>` (PomlElement)

動画を表す PomlElement

### 例

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

### アトリビュート

| name     | type     | default value | example                            | description      |
| -------- | -------- | ------------- | ---------------------------------- | ---------------- |
| src      | `string` | `""`          | `"https://example.com/sample.mp4"` | ソース URL       |
| filename | `string` | `""`          | `"sample.mp4"`                     | ファイル名       |
| width    | `number` | `undefined`   | `"1"`, `"0.1"`                     | 動画表示幅 [m]   |
| height   | `number` | `undefined`   | `"1"`, `"0.1"`                     | 動画表示高さ [m] |

ファイル名が指定されている場合、ファイル名の拡張子で動画ファイルのフォーマットが判別される。  
ファイル名が指定されていない場合、URL の拡張子が利用される。

`width` または `height` の片方が指定されていない場合、指定されていない数値は動画のアスペクト比から計算される。  
`width` と `height` が両方指定されていない場合、`width` が 1m として表示される。

### サポートされる拡張子

- `.mp4`

## `<text/>` (PomlElement)

テキストを表す PomlElement

### 例

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

### アトリビュート

| name             | type                | default value | example                | description                                   |
| ---------------- | ------------------- | ------------- | ---------------------- | --------------------------------------------- |
| text             | `string`            | `""`          | `"sample"`             | テキスト                                      |
| font-size        | `string` (FontSize) | `"1m"`        | `"1m"`                 | フォントサイズ                                |
| font-color       | `string` (Color)    | `"#ffffff"`   | `"white"`, `"#ff0000"` | フォントの色                                  |
| background-color | `string` (Color)    | `"#000000"`   | `"white"`, `"#ff0000"` | 背景色                                        |
| width            | `number`            | `1`           | `"1"`, `"0.1"`         | テキスト表示幅 <br>(指定しない場合制限なし)   |
| height           | `number`            | `1`           | `"1"`, `"0.1"`         | テキスト表示高さ <br>(指定しない場合制限なし) |

- FontSize  
  文字列。数値と単位を持つ (例: `"1m"`, `"150pt"`)。単位は `m`, `mm`, `pt` のいずれかを指定する。

- Color  
  文字列。web カラー名 (例: `"white"`, `"red"`) または RGB カラーコード (例: `"#ff0000"`)

## `<space-reference/>`

シーン原点やオブジェクトを空間座標と紐づけるためのタグ

アトリビュートによって指定された空間座標との紐づけは `<space-reference/>` タグの親要素に適用される

### 親要素

- `<scene/>`
- PomlElement

### 子要素

なし

### 例

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

### アトリビュート

| name       | type         | default value | example                             | description                                          |
| ---------- | ------------ | ------------- | ----------------------------------- | ---------------------------------------------------- |
| space-id   | `string`     | `""`          | `"12345"`, `"MapA"`                 | 空間 ID                                              |
| space-type | `string`     | `""`          | `"Immersal"`, `"VuforiaAreaTarget"` | 空間タイプ                                           |
| position   | `Vector3`    | `"0,0,0"`     | `"1,2,3"`                           | `space-reference` の親要素から見た空間原点の相対位置 |
| rotation   | `Quaternion` | `"0,0,0,1"`   | `"0,0,0,1"`                         | `space-reference` の親要素から見た空間原点の相対姿勢 |

space-type を指定しない場合、空間タイプによらず space-id が一致する空間と紐づけられる

## `<geo-reference/>`

シーン原点やオブジェクトを地理座標（緯度、経度、楕円体高）で配置するためのタグ

アトリビュートによって指定された地理座標は `<geo-reference/>` タグの親要素に適用される

### 親要素

- `<scene/>`
- PomlElement

### 子要素

なし

### 例

```xml
<model src="./model.glb" rotation="0 0 0.7071 0.7071">
  <geo-reference latitude="35.68122469808435"
                 longitude="139.76719554151146"
                 ellipsoidal-height="50">
  </geo-reference>
</model>
```

### アトリビュート

| name               | type         | default value | example                | description                                 |
| ------------------ | ------------ | ------------- | ---------------------- | ------------------------------------------- |
| latitude           | `number`     | `"0"`         | `"35.68122469808435"`  | 緯度                                        |
| longitude          | `number`     | `"0"`         | `"139.76719554151146"` | 経度                                        |
| ellipsoidal-height | `number`     | `"0"`         | `"50"`                 | 楕円体高                                    |
| enu-rotation       | `Quaternion` | `"0,0,0,1"`   | `"0,0,0,1"`            | North (+X) West (+Y) Up (+Z) 座標系での回転 |

enu-rotation は ENU (East, North, Up) という名前と実際の回転軸（North, West, Up）が異なっており混乱を招くため、将来的に廃止する予定です。  
代わりに、親タグの rotation アトリビュートを使用してください。
