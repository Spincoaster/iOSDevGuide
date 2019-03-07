iOS Development Guide
=====================

本ガイドでは、Spincoaster のiOS開発のルール、ベストプラクティスをまとめる。


設計について
------------

### 基本方針

- コードはできるだけ見やすく短く簡潔にする
  - 参考図書: リーダブルコード
  - クラス名やメソッド名を説明的なものにする
  - クラスやメソッドの行数が増えてきたら、別のクラスや関数に分けたほうが良い
- コピペは避ける。base classやprotocolもしくはdelegateに切り出すようにする。protocolのベター。
- 非同期処理はコールバックのネストが深くなってきたりある程度複雑になってきたら、Promise相当のライブラリを使う。
  - おすすめ: ReactiveSwift
- iOSアプリケーションは複雑になりがちなので、DDDやClean architectureを参考にして、早い段階で、レイヤーに分けておく。特に、ViewやViewControllerで多くのことをしないようにする。
- UIViewControllerについて

- フォルダ構成例
  - Scene ... 画面ごとにさらにフォルダを作りStoryboardとViewControllerとそれに付随するファイルを格納
  - Model ... ドメインロジックのためのstruct, enum を定義
  - View ... 各Sceneで使われる横断的なカスタムView・TableViewCellなどを定義
  - API ... APIクライアント
  - Data ... ローカルに保存する永続化データ
  - Util ... FoundationやUIKitのextensionや便利メソッドなどを定義


UI設計について
--------------------

- コードでレイアウトせず、Storyboardを使う
- Storyboardはできる限り分割する
  - １Storyboard １画面が原則で、Navigation ControllerやContainerViweなどの複合画面用のパーツや補助的なパーツのみ含めても良い
  - 理由
    - コンフリクトを避けられる
    - Storyboardの編集が重くなるのを避けられる
- 複数のStoryboardで共通されるパーツはカスタムViewとして実装する。その際IBDesignableなViewとして実装すルト良い。しかし、Storyboardの編集が重くなりすぎる場合は、しなくても良い。
- カスタムViewを実装する際もxibファイルを作って、グラフィカルに編集できるようにすると良い。また、イベント指定はprotocolを定義してdelegateを経由すると良い。


コメントについて
----------------

- クラス名やメソッド名が説明的になっていれば、メソッドの中身をただ説明するようなものは書かなくても良い。
- 一見すると意図がわからなかったり、実際に実行しないと気づけないような動作に対応したものには必ずコメントをつけること

コーディングスタイル
--------------------

基本的には
[swift lint](https://github.com/realm/SwiftLint) のデフォルト設定に従う。

カスタマイズは最小にすべきだが、
line_lengthなどの設定を適宜プロジェクトによってカスマイズして良い。


### 構文のスタイル

Swiftは静的型付け言語でコンパイラの機能が強力なので、できるだけその恩恵に預かれる書き方をすること

- force unwrap `!`は使わない。if letやguard letを使う
- `try!`は使わない
- classよりもstruct, StringやIntよりenumを使う
- if よりもswitch
