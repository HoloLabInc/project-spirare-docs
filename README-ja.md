# Project Spirare Documentation

[English](./README.md) | 日本語

このリポジトリは Project Spirare で定義されているファイルフォーマットのドキュメントを含んでいます。

## Project Spirare とは

Project Spirare は、3D コンテンツの記述を目的とした汎用フォーマットを定義、実装するプロジェクトです。

このプロジェクトでは POML というフォーマットが定義されており、POML は以下のような特徴を持っています。

- XML ベースのフォーマット
- 3D モデル、画像、動画などのファイルの配置を定義可能
- 特定のエンジンやアプリケーションに非依存
- 実行時にロード可能

POML および POML をロードするためのライブラリを利用することで、同一の 3D コンテンツが様々なアプリケーションで利用可能となります。

## 仕様

- [POML](./specification/POML/POML-ja.md)

## 関連プロジェクト

- [ProjectSpirare-for-Unity](https://github.com/HoloLabInc/ProjectSpirare-for-Unity)
- [spirare-babylonjs](https://github.com/HoloLabInc/spirare-babylonjs)
