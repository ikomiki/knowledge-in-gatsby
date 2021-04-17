---
title: "WebRTCのメモ"
templateKey: blog-post
date: 2020-07-07T14:45:16.084Z
descrition: WebRTCのメモ
tags: [WebRTC]
draft: true
---

## WebRTC とは

ウェブブラウザやモバイルアプリケーション向けの、リアルタイム通信の API です。

ブラウザのみの環境(※1)でピアツーピアによるボイスチャット、ビデオチャット、ファイル共有が可能になります。

※1 IE は非対応です。

<!--more-->

## Read more

MDN 上の WebRTC API：https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API
youtube の WebRTC ビデオとは：https://www.youtube.com/watch?v=5ci91dfKCyc
WebRTC にはサーバーが必要です：https://www.youtube.com/watch?v=Y1mx7cx6ckI

WebRTC に関するいくつかの重要なポイント：

- ピアツーピア接続（多くの場合、サーバーの助けを借りて、クライアントは互いに接続します）
- getUserMedia-デバイス（この場合は放送局/コメンテーター側）からカメラとオーディオストリームにアクセスします
- RTCPeerConnection-ブロードキャスターとビューアー間の接続を確立します
- RTCDataChannel-2 つのピア間でファイルとメディアを転送します（この場合は不要です）
- ICE Candidates（Interactive Connectivity Establishment）-2 つのピア間の接続の確立を支援するフレームワーク
- SDP（セッション記述プロトコル）-接続のマルチメディアコンテンツを記述するパッケージ
- シグナリング-2 つのピア間の接続を確立するために、シグナリングサーバーが使用されます。 （詳細はhttps://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Connectivity）
- STUN（NAT のセッショントラバーサルユーティリティ）対 TURN（NAT の周りのリレーを使用したトラバーサル）（詳細はこちら：https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Protocols）
- マルチパーティ接続方法のタイプ：メッシュ vs MCU（マルチポイントコントロールユニット）vs SFU（選択的転送ユニット）。 SFU の例-Google ハングアウト（詳細：https://bloggeek.me/webrtc-multiparty-video-alternatives/）

WebRTC 基本サンプル：https://github.com/webrtc/samples
WebRTC の高度な例：https://github.com/muaz-khan/WebRTC-Experiment
クラッカー：スケーラブルブロードキャストの例：https://github.com/muaz-khan/WebRTC-Scalable-Broadcast
1 対多のビデオコールを詳細に説明する例（私たちが望むものに似ています）：https://doc-kurento.readthedocs.io/en/6.13.0/tutorials/node/tutorial-one2many.html
¥

マルチパーティ接続方法（MCU または SFU）のいずれかを使用する場合、ユーザーは同時ビデオを表示できます。
MCU では、すべてのストリームが単一のストリームに混合され、すべての視聴者にブロードキャストされます。
SFU では、すべてのストリームが中央エンティティを使用して中継され、視聴者は個別のストリームを表示します。 Google ハングアウトのようなものです。 視聴者は焦点を合わせたい動画を選択できます。残りの動画は低品質になるか、単なるサムネイルになります。

AWS には、WebRTC 用の Kinesis Video Streams サービスがあります。
https://docs.aws.amazon.com/kinesisvideostreams-webrtc-dg/latest/devguide/what-is-kvswebrtc.html
ピアツーピア接続用の ICE、STUN、および TURN を提供します。
簡単に実装できるさまざまな SDK（Android、Javascript、iOS）があります。
ただし、さまざまな制限があります。 1 つの信号チャンネルで視聴できるのは 10 人だけです。
サービスをマルチキャストに使用するには、Kurento Media Server を追加する必要があります。 https://www.kurento.org/whats-kurento
AWS マーケットプレイスには、KVS WebRTC と Kurento を組み合わせた製品があります。 elasticRTC と呼ばれます。
https://aws.amazon.com/marketplace/pp/Tikal-Technologies-elasticRTC/B01HD8W9CS

OpenVidu と呼ばれるオープンソースフレームワークを見つけました：https://openvidu.io/docs/home/
WebRTC を使用し、Kurento の上に構築され、低レベルの操作を抽象化するため、OpenVidu でアプリケーションを作成するのが簡単です。
OpenVidu にはさまざまなチュートリアルがあります。
私は Angular でチュートリアルを試しました。 https://openvidu.io/docs/tutorials/openvidu-insecure-angular/
アーキテクチャをサポートするには、サーバー（ノードまたは Java）が必要です。
