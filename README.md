# Welcome to Defold

This project was created from the "empty" project template.

The settings in ["game.project"](defold://open?path=/game.project) are all the default. A bootstrap empty ["main.collection"](defold://open?path=/main/main.collection) is included.

Check out [the documentation pages](https://defold.com/learn) for examples, tutorials, manuals and API docs.

If you run into trouble, help is available in [our forum](https://forum.defold.com).

Happy Defolding!

---
# DEFOLDでシューティングゲームを作る
ここからは作業記録です。
DEFOLDを触りながら、自分なり思ったり感じたことを書いています。間違ってたらごめんなさい。

---
## プロジェクトを作成する
1. DEFOLDアプリ起動
1. NEW PROJECT
1. EMPTY PROJECT
1. 下部の[Title]に任意の名前指定
1. 下部の[Location]に任意のディレクトリ指定
1. CREATE

これでプロジェクトができました。  
プロジェクトができたタイミングで一旦コミットするのが私の流儀。  
ちゃんとデフォルトで.gitignoreも入ってる。  
ありがたや〜。

### ファイルの中身を眺めてみる
```
game.project
input/game.input_binding
main/main.collection
README.md
.gitignore
```

プロジェクトの作成で生成されたファイルは全てテキストファイルでした。  
なので中身をさらっと眺めてみます。

#### game.project
iniファイルっぽい形式でプロジェクトの設定が書いてある。

```
[bootstrap]
main_collection = /main/main.collectionc

[script]
shared_state = 1

[display]
width = 960
height = 640

[android]
input_method = HiddenInputField

[project]
title = shootingsample
```

#### input/game.input_binding
フォーマットは不明。  
入力に関することが書いてある。

```
mouse_trigger {
  input: MOUSE_BUTTON_1
  action: "touch"
}
```

### main/main.collection
inputのファイルと同じ形式なのかな？  
このファイルがメイン画面に相当するみたい。

```
name: "main"
scale_along_z: 0
```

## 画像を用意する
シューティングゲーム用の画像をセットアップします。
enchant.jsの画像を使わせていただきます。
- car.png
- bomb.png

### 画像をassetsディレクトリに置く
プロジェクト直下に`assets`ディレクトリを作成し、`car.png`と`bomb.png`をおきます。  

### atlasファイルを作る
DEFOLDで画像を使う場合は、画像を直接読み込まずにatlasというファイルを経由します。
1. assetsディレクトリで右クリック
1. Atlasを選択
1. `chara`という名前で保存

参考: atlas https://defold.com/manuals/atlas/

### atrasに画像を追加する
1. chara.atrasを開き、IDE右側のoutlineの`Atras`を右クリック
1. `Add Images`を選択
1. 画像を2つ選択する
  - /assets/bomb.png
  - /assets/car.png
1. ctrl + Sで保存する

## DEFOLDの設計
画像のセットアップが終わったのでここからゲーム製作に入っていきますが、その前にDEFOLDの基本的な構成を知るべきです。

公式サイトによるとDEFOLDの設計を学ぶのにはちょっと時間がかかるとのこと。ゆっくりいきましょう。
[The building blocks of Defold](https://defold.com/manuals/building-blocks/)
とりあえず今は基本構成だけおさえておきます。
以下の引用は、公式サイトをgoogle翻訳したものです。

### Collection
> コレクションは、ゲームの構造化に使用されるファイルです。コレクションでは、ゲームオブジェクトと他のコレクションの階層を構築します。それらは通常、ゲームレベル、敵のグループ、または複数のゲームオブジェクトから構築されたキャラクターを構築するために使用されます。

プロジェクト作成直後にある`main.collection`はコレ。CollectionにはGame objectを追加できる。 

### Game object
> ゲームオブジェクトは、ID、位置、回転、スケールを持つコンテナです。コンポーネントを含めるために使用されます。これらは通常、プレイヤーキャラクター、弾丸、ゲームのルールシステム、またはレベルローダー/アンローダーを作成するために使用されます。

これが中心になる概念。Game objectにはコンポーネントを追加できる

### Component
> コンポーネントは、ゲーム内に視覚的、可聴的、および/または論理的表現を与えるためにゲームオブジェクトに配置されるエンティティです。通常、キャラクタースプライトの作成、スクリプトファイルの作成、サウンドエフェクトの追加、パーティクルエフェクトの追加に使用されます。
Luaのコードもコンポーネントの１つ。

絵にするとこんな感じ
![img](readmeimage/defold_design.svg)


## 自機を作る
自機を作ります。
IDEでメイン画面に相当するmain.collectionを開くと画面編集っぽい画面が表示されます。

ここに自機に相当するGameObjectを作成します。
### 自機のGameObjectを作る
1. 画面右上のoutlineにあるCollectionで右クリック
1. [Add Game Object]を選択
1. GameObjectが生成されたらpropertiesでIdを`player`に変更

### 自機にスプライトを追加する
自機のGameObjectにスプライトComponentを追加します。
1. 画面右上のoutlineにあるplayerを右クリック
1. [Add Component]を選択
1. [Sprite]を選択
1. PropertiesのImageにある[...]をクリック
1. `/assets/chara.atras`を選択
1. PropertiesのDefault Animationで`car`を選択

これで画面に車の画像が表示されたか、画面が真っ赤になれば成功。  
画面が真っ赤になるのは車にめっちゃズームしてるから。その場合はマウスのスクロールを回してズームアウトしましょう。

この状態でBuild(Ctrl + B)するとゲームが実行できます。画面の右下に車がちょっとだけ表示されれば成功です。

## なぜ車が画面右下にちょっとだけ表示されるのか？
Build後に車が画面中央に表示されると期待した方も多いと思いますが、実際は画面の端っこにちょっとだけ表示されました。これは座標が関わっているので説明します。

### playerの設定をみる
まずplayerの座標を調べます。
main.collectionで`player`を選択し、右下のPropertiesのPositionを見ればわかります。
`X:0 Y:0 Z:0`と表示されていると思います。
また画面中央の車の真ん中から赤と緑の矢印が出てますが、この原点に当たる部分がplayerの中心座標です。
つまり車の中心がウィンドウのx:0, y:0の位置にくるように表示されます。
DEFOLDのウィンドウの原点は左下なので、車の中心がウィンドウ左下端になるように表示されます。

## 自機を動かす
### スクリプトを作る
- 画面左側のmainディレクトリで右クリック
- [Script]を選択
- Nameに`player`と入力
- [Create Script]をクリック

これでいろんなメソッドが書かれたスクリプトが自動的に生成されました。いっぱいありますがとりあえず大事なメソッドは2つです。
```lua
function init(self)
	-- Add initialization code here
	-- Remove this function if not needed
end
```
GameObject生成時に最初に呼ばれるメソッドです。ここで初期値を設定できます。
引数の`self`は開発者が自由に使って良いデータオブジェクトです。他のメソッドの引数にも`self`がありますが、そこにはこの`self`インスタンスが引きづり回されます。

勝手な想像ですが、luaにはクラスがないので、DEFOLDの設計思想は
- 1ファイル1クラスのようなイメージ
- フィールドがないから`self`を引きづり回してフィールド的な使い方をする

のような感じなんだと思います。

```lua
function update(self, dt)
	-- Add update code here
	-- Remove this function if not needed
end
```
`update`はフレームごとに呼ばれる関数です。たとえばここでx座標を1ずつ増やすとGameObjectが右方向に進んでいくような動きが作れます。
第２引数の`dt`は前回更新からの時間です。FPSを一定に保ちたい場合に使うことになると思います。


### スクリプトを追加する
自機のGameObjectにスクリプトComponentを追加します。
- 左ペインで`main/main.collection`を選択
- 右ペインのOutlineで`player`を右クリック
- [Add Component File]を選択
- `/main/player.script`を選択
- [OK]をクリック

### スクリプトから座標を変えてみる
`main/player.script`を編集して車の座標を変更します。
initメソッドをこのように編集します。

```lua
function init(self)
	go.set(go.get_id(), "position.x", 32)
	go.set(go.get_id(), "position.y", 32)
end
```
これでビルドすると、車全体がディスプレイに表示されます。

```lua
go.set(go.get_id(), "position.x", 32)
```
これは何をしているのか？

わかること
- `position.x, 32`でx座標を32pxに設定している

わからないこと
- `go`とは
- `go.set`とは
- `go.get_id()`とは



まず`go`とは
そもそもメソッドの引数にない変数なんだけど...
てことで公式ドキュメントを見ます。
Game object API documentation https://defold.com/ref/go/
goはGame objectのようです。

`go.set`とは
Game objectへのセッターです。
```
go.set(url,property,value)
```
url: `string | hash | url` url of the game object or component having the property

https://defold.com/manuals/addressing/

要するにURLで特定のgameobjectを指定する。

`go.get_id()`とは
このスクリプトが設定されているGame objectのIDを返します。`go.set`のurlとして有効な値のようです。

### 車を移動させる
車が走っていくような動きをつけようと思います。
スクリプトをこのように直します。
```lua
function init(self)
	self.y = 0
	go.set(go.get_id(), "position.x", 32)
	go.set(go.get_id(), "position.y", self.y)
end

function update(self, dt)
	self.y = self.y + 1
	go.set(go.get_id(), "position.y", self.y)
end
```
これでビルドすると、車が前に走ります。
updateはフレーム単位で呼ばれる関数です。
その中で`position.y`の値を1ずつ増やしているのでフレームごとに車が移動していきます。