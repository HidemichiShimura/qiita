---
title: 初めてTailwind CSSを使ってみた
tags:
  - 'tailwindcss'
  - 'css'
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---
# 初めてTailwind CSSを使ってみた

## 背景
これまでの個人プロジェクト(React)ではstyled-componentsをメインに使ってきていたが、ViteやNext.jsの学習の際にTailwind CSSを目にする機会が増え、ぜひ使ってみようとなった。またServer Componentを使う上でスタイルにはPostCSSやTailwind CSSが推奨されているらしいため。

## Tailwind CSSとは
フロントエンドで用いられるUIにスタイルを適用するためのCSSライブラリ。

### 特徴
Utility Classesという既にスタイルが設定されたクラスが用意されている。そのクラス再利用しスタイルしていく。

例:
|Utility Class|Property|
|:-----------:|:------------:|
|font-bold|font-weight: 700|
|text-red-500|color: rgb(239 68 68)|

### 使い方
htmlタグのclass属性(ReactではclassName)に必要なUtility Classを羅列する。

```
<form className="flex flex-col gap-4">
```

## インストール方法
開発環境毎のインストール方法が公式ドキュメントに記載されてます。
https://tailwindcss.com/docs/installation

## 利用中のミス
動的にクラス名を作成して使用すること

### 背景
Reactのプロップスにカラー名を渡し、そのカラー名をクラス用のストリングと繋げてクラスに埋め込んだところ、そのスタイルが適用されていなかった。

コードイメージ:
```
export default function Test({color}) {
  return (
    <div className={`text-${color}`}>...</div>
  )
}
```

### 問題
[Tailwindでは動的にクラス名を作成することができない](https://tailwindcss.com/docs/content-configuration#dynamic-class-names)

### 解決
カラー名に対応したフルクラス名をマッピングして、フルクラス名をクラスに埋め込む

```
export default function Test({color}) {
  const colorMap = {
    red: "text-primary-red"
  }

  return (
    <div className={colorMap[color]}>...</div>
  )
}
```

## カスタマイズ
Tailwind.config.jsにプロジェクトに合わせた独自のクラスカスタマイズが行える。以下のような設定が可能。

- フォントカラー
- メディアクエリ
- Webフォント

またデフォルトのクラスを上書きすることも出来る。

## 便利なツール
VSCodeの拡張機能やPrettierとの連携でTailwindをより便利に使うことができる

### VSCode拡張機能
[Tailwind CSS IntelliSense](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss)

コードエディターでクラス名の補完入力をしてくれたり様々な機能がある

### Prettierプラグイン
[Automatic Class Sorting with Prettier](https://tailwindcss.com/blog/automatic-class-sorting-with-prettier)

羅列されたクラス名を自動で並び替えてくれる

## 感想
これまでの個人プロジェクトではsytled-componentsを主に使っていた。Tailwindをしばらく使ってみると、こっちの方が直感的かつ早くスタイル描けるので使いやすいと思った。styled-componentsやピュアなCSSを書くときにクラス名やコンポーネント名を考えるのが大変だったが、Tailwindではその必要がないので楽だと感じた。ただ、どうしてもクラス名を書く量が増えてしまうので、使用するのが最適かどうかはプロジェクトによって変わりそう。

またデフォルト設定を上書きして使用できるクラスを絞ることで、チーム開発であってもスタイルが一貫したUIを作成することができそう。

## 疑問点
Tailwindインストール時についてくるPostCSSがあまりよく分からなかった。CSS版のBabelのようなものという認識だが、それが実際にはどういうものかは理解できていない。

## おわりに
ここまで読んでいただきありがとうございました。
まだまだ知識不足・認識齟齬があると思いますので、見つけた際にはぜひご指摘ください！