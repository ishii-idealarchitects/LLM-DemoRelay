# LLMのデモを行うための中継サーバーアプリ

## 構成図

![構成図](./doc/env01.drawio.svg)

## インターネット上の サーバー

VPS内の webサーバー + web socketサーバー.
インターネット上に高価な GPUサーバーを借りることなくGPUの必要なwebアプリの
デモなどを行うことができる.(処理クライアント側にGPUが必要).

自宅などにGPUマシンが必要

### インターネット上のサーバーの機能

* web ブラウザからの接続に対してhtml,css,js などを返す。
* webブラウザから api 問い合わせ
* 処理クライアントからweb socket接続される。
* webブラウザからの処理要求は接続されている処理クライアントで処理を行われる。
* 拡張できるようにしたい. 詳細な機能は未定

### インターネット上のサーバーのフレームワーク

現状 python fastAPI を考えていますが、nodejs,rust,go などにするかもしれません.

## 処理クライアント

インターネット上のwebサーバー+web socketサーバーに  web socket接続して
処理サーバーとして登録し、 webブラウザからの処理要求を処理する。


# シンプルなLLMチャットの場合のシーケンス

```mermaid
sequenceDiagram
    participant wb as webブラウザ
    participant srv as webサーバー+<BR>web socketサーバー
    participant cli as 処理クライアント
    participant llm as ollamaサーバー

    cli->>+srv:web socket<BR>接続
    srv-->>-cli:接続OK
    wb->>+srv:webページ取得
    srv-->>-wb:webページ<BR>html,css,js
    note over wb:質問入力
    wb->>+srv:質問
    note over srv:接続されている処理クライアント<BR>に質問を投げる
    srv->>+cli:質問
    cli->>+llm:質問
    note over llm:回答生成
    llm-->>-cli:回答
    cli-->>-srv:回答
    srv-->>-wb:回答
```

# 作りたいものとフレームワーク

今使おうと思っているものですが、変えるかもしれません

* webブラウザで表示するコンテンツ
    * vuejs もしくは react もしくは nextjs
* インターネット上の サーバー
    * fastAPI もしくは nodejs
* 処理クライアント
    * fastAPI もしくは内容ごとに変える
* LLm サーバー  
    * ollama, llama.cpp など
    * ものによっては 処理クライアントから直接python実行

