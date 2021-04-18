---
title: "ExcelからPDFの生成するツールについての調査"
templateKey: blog-post
date: 2020-07-15T17:05:00+09:00
tags: [Python, Excel, PDF]
draft: true
---

## Excel から PDF の生成するツール

ウェブブラウザから Excel 帳票を作成したうえで、PDF ファイルを生成し、その PDF を PC などから印刷したい。

本ドキュメントは、Excel ファイルから PDF ファイルの生成の方法についてまとめたものです。

LibreOffice を使う方法が有力候補とみています。

- AWS EC2 ないし Lamda 上の LibreOffice 呼び出し
  - openoffice.org-headless  
    LibreOffice をコマンドでインストールする手順。AWS では EC2 前提。
    参考: [Excel ファイルをサーバーサイドで PDF に変換 \- Qiita](https://qiita.com/amagasu/items/0a63b77440dc201f3973)
  - [GitHub \- shelfio/libreoffice\-lambda\-layer](https://github.com/shelfio/libreoffice-lambda-layer)  
    AWS Lambda Layer による LibreOffice。node.js, python, java 対応
  - [GitHub \- shelfio/aws\-lambda\-libreoffice: 85 MB LibreOffice to fit inside AWS Lambda compressed with Brotli](https://github.com/shelfio/aws-lambda-libreoffice)  
    AWS Lambda(node.js)に LibreOffice を組み込むライブラリ。
    ※Lambda のメモリ 1536MB 以上
  - libreconv Ruby ライブラリ。要 LibreOffice  
    参考: [AWS で Excel から PDF 変換を行う \- Qiita](https://qiita.com/kijitora-neko/items/ca8e215c4580e59fef80)
  - (LibreOffice) LibreOffice のコマンド softice から PDF を生成する。 参考: [Python で Excel を編集し PDF へ変換する](https://leben.mobi/blog/python_excel_pdf/python/)
- [ExCella Reports](http://excella-core.osdn.jp/reports.html)  
  java。オープンソース。Java オブジェクトと Excel テンプレートから Excel や PDF を出力できる  
   参考: [OSS のフレームワークを利用して Excel レポートを簡単作成 \(1/4\)：CodeZine（コードジン）](https://codezine.jp/article/detail/5255)
  アプリの大きさ的に Lambda は無理筋になるので、EC2 前提。
- [Office Server Document Converter（OSDC)](https://www.antenna.co.jp/sbc/) 商用ソフト。Standard 版で 30 万円
