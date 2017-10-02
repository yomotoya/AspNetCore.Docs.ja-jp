---
title: "ASP.NET Core での要求機能"
author: ardalis
description: "HTTP 要求と ASP.NET Core のインターフェイスで定義されている応答に関連する web サーバーの実装の詳細情報について説明します。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d1fbd23c-2ff9-4216-b908-0201ff3afb7c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: b689d82d16c6ef55485691b3474a070765c8144b
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2017
---
# <a name="request-features-in-aspnet-core"></a>ASP.NET Core での要求機能

によって[Steve Smith](https://ardalis.com/)

HTTP 要求に関連する web サーバー実装の詳細と、応答がインターフェイスで定義されています。 これらのインターフェイスは、サーバーの実装とミドルウェアによって作成および変更、アプリケーションのホスティング パイプラインに使用されます。

## <a name="feature-interfaces"></a>機能のインターフェイス

ASP.NET Core での HTTP 機能のインターフェイスの数を定義する`Microsoft.AspNetCore.Http.Features`サポートされる機能を識別するサーバーで使用されています。 次の機能のインターフェイスは、要求を処理し、応答を返します。

`IHttpRequestFeature`プロトコル、パス、クエリ文字列、ヘッダー、および本文を含む、HTTP 要求の構造を定義します。

`IHttpResponseFeature`ステータス コード、ヘッダー、および応答の本文を含む、HTTP 応答の構造を定義します。

`IHttpAuthenticationFeature`基づくユーザーを識別するためのサポートを定義、`ClaimsPrincipal`認証ハンドラーを指定するとします。

`IHttpUpgradeFeature`サポートを定義[HTTP アップグレード](https://tools.ietf.org/html/rfc2616.html#section-14.42)サーバーがプロトコルの切り替えたい場合に使用するように、その他のプロトコルを指定するクライアントを許可します。

`IHttpBufferingFeature`要求と応答のバッファリングを無効にするためのメソッドを定義します。

`IHttpConnectionFeature`ローカルおよびリモート アドレスとポートのプロパティを定義します。

`IHttpRequestLifetimeFeature`接続を中止するかどうか、要求は中止されました途中で、このようなクライアント接続が切断を検出するのサポートを定義します。

`IHttpSendFileFeature`ファイルを非同期的に送信するためのメソッドを定義します。

`IHttpWebSocketFeature`Web ソケットをサポートするための API を定義します。

`IHttpRequestIdentifierFeature`要求を一意に識別する実装できるプロパティを追加します。

`ISessionFeature`定義`ISessionFactory`と`ISession`ユーザー セッションをサポートするための抽象化します。

`ITlsConnectionFeature`クライアント証明書を取得するための API を定義します。

`ITlsTokenBindingFeature`TLS トークンのバインド パラメーターを使用して処理するメソッドを定義します。

> [!NOTE]
> `ISessionFeature`サーバー機能ではありませんが、によって実装される、 `SessionMiddleware` (を参照してください[アプリケーションの状態を管理](app-state.md))。

## <a name="feature-collections"></a>機能のコレクション

`Features`プロパティ`HttpContext`のインターフェイスを取得および設定の現在の要求に対して使用可能な HTTP 機能を提供します。 機能のコレクションは、要求のコンテキスト内でも変更可能であるために、コレクションを変更して、その他の機能のサポートを追加するミドルウェアを使用できます。

## <a name="middleware-and-request-features"></a>ミドルウェアと要求の機能

サーバーは、機能のコレクションの作成を担当するが、ミドルウェアはことができます両方をこのコレクションに追加し、コレクションからの機能を使用します。 たとえば、`StaticFileMiddleware`にアクセスする、`IHttpSendFileFeature`機能します。 機能が存在する場合、物理パスからの要求された静的ファイルの送信に使用されます。 それ以外の場合、低速な代替手段はファイルの送信に使用されます。 利用可能であれば、`IHttpSendFileFeature`オペレーティング システムにファイルを開き、ネットワーク カードに直接カーネル モードのコピーを実行できるようにします。

さらに、ミドルウェアは、サーバーによって確立された機能のコレクションに追加できます。 既存の機能は、サーバーの機能を補完するミドルウェアを許可するミドルウェアによっても置き換えられます。 コレクションに追加された機能は他のミドルウェアまたは基になるアプリケーション自体、要求パイプラインの後ですぐに使用できます。

カスタム サーバーの実装と特定のミドルウェアの機能強化を組み合わせることにより、正確なアプリケーションに必要な機能のセットを構築できます。 これにより、サーバーでの変更を必要とせずに追加する機能がなく、攻撃を制限するため、最小限の機能のみが公開されることを確認画面領域とパフォーマンスが向上します。

## <a name="summary"></a>概要

機能のインターフェイスは、特定の要求がサポートする可能性がある特定の HTTP 機能を定義します。 サーバーは、機能のコレクションと、そのサーバーでサポートされる機能の初期セットを定義するが、ミドルウェアは、これらの機能を強化するために使用できます。

## <a name="additional-resources"></a>その他のリソース

* [サーバー](servers/index.md)

* [ミドルウェア](middleware.md)

* [Open Web Interface for .NET (OWIN)](owin.md)
