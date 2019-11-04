# Welcome to Defold

This project was created from the "empty" project template.

The settings in ["game.project"](defold://open?path=/game.project) are all the default. A bootstrap empty ["main.collection"](defold://open?path=/main/main.collection) is included.

Check out [the documentation pages](https://defold.com/learn) for examples, tutorials, manuals and API docs.

If you run into trouble, help is available in [our forum](https://forum.defold.com).

Happy Defolding!

---
# DEFOLDでシューティングゲームを作る
ここからは作業記録です。

---
## プロジェクトを作成する
- DEFOLDアプリ起動
- NEW PROJECT
- EMPTY PROJECT
- 下部の[Title]に任意の名前指定
- 下部の[Location]に任意のディレクトリ指定
- CREATE

これでプロジェクトができました。  
プロジェクトができたタイミングで一旦コミットするのが私の流儀。  
ちゃんとデフォルトで.gitignoreが入ってる。  
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