---
title: HTTP API を使用した gRPC サービスの比較
author: jamesnk
description: シナリオを HTTP Api とされているものと gRPC の比較が推奨について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 03/31/2019
uid: grpc/comparison
ms.openlocfilehash: 0e9ef0e7ca8fb6d847b45f6dd7bd0aaa35fd149f
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515674"
---
# <a name="comparing-grpc-services-with-http-apis"></a>HTTP API を使用した gRPC サービスの比較

によって[James ニュートンのキング](https://twitter.com/jamesnk)

この記事の説明方法[gRPC サービス](https://grpc.io/docs/guides/)HTTP Api と比較 (ASP.NET Core を含む[Web Api](xref: web-api/index))。 アプリの API を提供するために使用するテクノロジは、重要な選択と gRPC は HTTP Api と比較した独自のメリットを提供します。 この記事では、長所と gRPC の短所について説明し、gRPC を他のテクノロジを使用するシナリオをお勧めします。

#### <a name="overview"></a>概要

|    機能             |    gRPC                                                 |    JSON で HTTP Api                       |
|------------------------|---------------------------------------------------------|----------------------------------------------|
|    コントラクト            |    必要な (`*.proto`)                                 |    省略可能な (OpenAPI)                        |
|    Transport           |    HTTP/2                                               |    HTTP                                      |
|    Payload             |    [Protobuf (小さな、バイナリ)](#performance)             |    JSON (大規模な場合は、人間が判読できる)              |
|    Prescriptiveness    |    [厳密な仕様](#strict-specification)        |    失われます。 任意の HTTP が有効                  |
|    ストリーム           |    [クライアント、サーバー、双方向](#streaming)         |    クライアント、サーバー                            |
|    ブラウザー サポート     |    [いいえ (grpc web が必要)](#limited-browser-support)   |    はい                                       |
|    セキュリティ            |    トランスポート (HTTPS)                                    |    トランスポート (HTTPS)                         |
|    クライアントのコード生成     |    [はい](#code-generation)                              |    OpenAPI + サード パーティ製のツール             |

## <a name="grpc-strengths"></a>gRPC の長所

### <a name="performance"></a>パフォーマンス

使用して gRPC のメッセージがシリアル化[Protobuf](https://developers.google.com/protocol-buffers/docs/overview)、効率的なバイナリ メッセージの書式設定します。 サーバーとクライアントで Protobuf が非常に高速シリアル化します。 モバイル アプリなどの小さいメッセージ ペイロード、帯域幅の制限のシナリオでは重要で Protobuf シリアル化の結果。

gRPC が http/2 を HTTP 経由で大幅なパフォーマンス上の利点を提供する HTTP の主要な改訂用に設計された 1.x:

* バイナリのフレーミングと圧縮します。 Http/2 プロトコルは、コンパクトで効率的なは、両方で送受信することです。
* Http/2 の複数の呼び出しを 1 つの TCP 接続上で多重化します。 多重化を排除[行の先頭ブロック](https://en.wikipedia.org/wiki/Head-of-line_blocking)します。

### <a name="code-generation"></a>コード生成

GRPC のすべてのフレームワークでは、コード生成のための優れたサポートを提供します。 GRPC の開発の中核となるファイルが、 [ `*.proto`ファイル](https://developers.google.com/protocol-buffers/docs/proto3)、gRPC サービスとメッセージ コントラクトを定義します。 このファイル gRPC のフレームワークのコードからサービスの基本クラス、メッセージ、および完全なクライアントを生成します。

共有することで、`*.proto`最後に末尾からサーバーとクライアント、メッセージ、およびクライアント コードの間でファイルを生成できます。 クライアントのコード生成は、クライアントとサーバーでのメッセージの重複を排除しを厳密に型指定されたクライアントを作成します。 クライアントを記述する必要があると、多くのサービスとアプリケーションに多大な時間が保存されます。

### <a name="strict-specification"></a>厳密な仕様

JSON で HTTP API の正式な仕様が存在しません。 開発者が Url の最適な形式について議論 HTTP 動詞、および応答コード。

[GRPC 仕様](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md)gRPC サービスが従う必要があります形式に関する規範的な。 gRPC は議論の的を排除し、プラットフォームと実装にわたって gPRC が一貫性のあるために、開発者の時間を保存します。

### <a name="streaming"></a>ストリーム

Http/2 は、有効期間が長い、リアルタイム通信ストリームの基盤を提供します。 gRPC は http/2 経由のストリーミングの最上級のサポートを提供します。

GRPC サービスには、すべてのストリーミングの組み合わせがサポートされています。

* 単項 (ストリーミングなし)
* サーバーからクライアントにストリーミング
* クライアントがサーバーにストリーミングするには
* 双方向のストリーミング

### <a name="deadlinetimeouts-and-cancellation"></a>期限/タイムアウトとキャンセル

gRPC は、クライアントがどのくらいの時間は、RPC を完了するを待機するかを指定できます。 [期限](https://grpc.io/blog/deadlines)サーバーに送信されると、サーバーは、期限を超えた場合に実行するには、どのようなアクションを決定できます。 たとえば、サーバーは、タイムアウト時に進行中 gRPC/HTTP/データベースの要求をキャンセルする可能性があります。

期限と子 gRPC を通じてキャンセルを伝達する呼び出しはリソース使用率の制限を適用できるようにします。

## <a name="grpc-recommended-scenarios"></a>gRPC のシナリオをお勧めします。

gRPC は、次のシナリオに適してします。

* **マイクロ サービス** &ndash; gRPC は設計の低待機時間とスループットが高い通信します。 gRPC は効率が重要な軽量のマイクロ サービスに最適です。
* **ポイント ツー ポイントのリアルタイム通信** &ndash; gRPC は双方向のストリーミング用の優れたサポートしています。 gRPC サービスは、メッセージをプッシュ配信、ポーリングせずにリアルタイムできます。
* **Polygot 環境** &ndash; gRPC ツールは、gRPC の多言語環境の適切な選択を行うすべての一般的な開発言語をサポートしています。
* **ネットワーク環境の制約付き** &ndash; gRPC メッセージは、Protobuf、軽量メッセージ形式にシリアル化されます。 GRPC メッセージは、同等の JSON メッセージよりも小さいは常にします。

## <a name="grpc-weaknesses"></a>gRPC の短所

### <a name="limited-browser-support"></a>制限付きのブラウザー サポート

ブラウザーから gRPC サービスを直接呼び出すことはできません。 gRPC に多用 http/2 機能とブラウザーには、gRPC クライアントをサポートするために web 要求に必要な制御のレベルはありません。 たとえば、ブラウザーが http/2 を使用することを要求する呼び出し元を許可しない、または基になる HTTP/2 フレームへのアクセスを提供しないでください。

[gRPC Web](https://grpc.io/docs/tutorials/basic/web.html)は、ブラウザーで制限付き gRPC のサポートを提供する、gRPC チームから追加のテクノロジです。 gRPC Web は、2 つの部分で構成されています。 サーバーのすべての最新ブラウザー、および gRPC Web プロキシをサポートしている JavaScript クライアント。 GRPC Web クライアントがプロキシを呼び出すし、プロキシは、gRPC、gRPC サーバー要求に転送されます。

GRPC の機能の一部は、gRPC Web でサポートされます。 クライアントと双方向のストリーミングはサポートされていませんし、サーバーのストリーミングのサポートは制限します。

### <a name="not-human-readable"></a>ない人間が判読できます。

HTTP API 要求はテキストとして送信されますと、読み取りし、人間によって作成されたことができます。

gRPC のメッセージは、既定で Protobuf とエンコードされます。 Protobuf は送受信するように効率的ですが、そのバイナリ形式は人間ではありません読み取り可能な。 Protobuf で指定されたメッセージのインターフェイスの説明が必要です、`*.proto`ファイルを正しく逆シリアル化します。 その他のツールは、要求を手動で作成して、ネットワーク上で Protobuf ペイロードを分析する必要があります。

などの機能[server リフレクション](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md)と[gRPC コマンド ライン ツール](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md)Protobuf バイナリのメッセージを支援するために存在します。 メッセージのサポートも、Protobuf[変換へおよび JSON から](https://developers.google.com/protocol-buffers/docs/proto3#json)します。 組み込みの JSON 変換では、効率的にデバッグするときに人間の判読できる形式と Protobuf メッセージの変換を提供します。

## <a name="alternative-framework-scenarios"></a>代替 framework シナリオ

次のシナリオで gRPC 経由では、他のフレームワークがお勧めします。

* **ブラウザー アクセス可能な Api** &ndash; gRPC のブラウザーでは完全にサポートされていません。 gRPC Web ブラウザーのサポートを提供できますが、制限事項があります、サーバーのプロキシが導入されています。
* **リアルタイム コミュニケーションをブロードキャスト** &ndash; gRPC、ストリーミングを使用してリアルタイム通信をサポートしていますが、登録済みの接続に、メッセージのブロードキャストの概念が存在しません。 たとえば、チャット ルーム内のすべてのクライアントに新しいチャット メッセージを送信する必要があります、チャット ルーム シナリオでは、クライアントに新しいチャット メッセージを個別にストリーミングする gRPC の各呼び出しが必要です。 [SignalR](xref:signalr/introduction)はこのシナリオで便利なフレームワークです。 SignalR は、永続的な接続とのメッセージのブロードキャストの組み込みサポートの概念があります。
* **プロセス間通信**&ndash;プロセスは、gRPC の着信呼び出しを受け入れるように、http/2 サーバーをホストする必要があります。 Windows では、通信をプロセス間[パイプ](/dotnet/standard/io/pipe-operations)は通信の高速、軽量な方法です。

## <a name="additional-resources"></a>その他の技術情報

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
