---
uid: web-api/samples-list
title: Web API のサンプル リスト |Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 09/18/2012
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: d25e0890a1b8d42cc638117f7bef9cf6457f3d75
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825871"
---
<a name="web-api-samples-list"></a>Web API サンプル一覧
====================
## <a name="httpclient-samples"></a>HttpClient のサンプル

**Bing 翻訳サンプル** | [VS 2012 ソース](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/BingTranslateSample)

呼び出す方法を示しています、 [Microsoft Translator サービス](https://msdn.microsoft.com/library/ff512419.aspx)を使用して、 **HttpClient**クラス。 Microsoft Translator サービス API では、translator サービスへの各要求用の Azure のトークン サーバーに要求を送信して、アプリケーションを取得する OAuth トークンが必要です。 トークンのサーバーから結果は、翻訳サービスに送信される要求に給紙されます。 このサンプルを実行する前に取得する必要があります、 [Azure Marketplace からのアプリケーション キー](https://msdn.microsoft.com/library/hh454950.aspx) AccessTokenMessageHandler サンプル クラス内の情報を入力します。

**Google Maps サンプル** | [詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [VS 2012 ソース](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/GoogleMapsSample)

使用して**HttpClient**からワシントン州レドモンドの地図をダウンロードする[Google マップ API](https://developers.google.com/maps/)、ローカル ファイルとして保存され、既定のイメージ ビューアーを開きます。

**クライアントのサンプルを twitter** | [詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [VS 2012 ソース](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/TwitterSample)

使用して単純な Twitter クライアントを記述する方法を示しています。 **HttpClient**します。 サンプルでは、 **HttpMessageHandler**送信に OAuth 認証情報を挿入する**HttpRequestMessage**します。 Twitter からの結果では、JSON.NET を使用して読み取られます。 このサンプルを実行する前に取得する必要があります、 [Twitter からのアプリケーション キー](https://dev.twitter.com/)、OAuthMessageHandler サンプル クラス内の情報を入力します。

**世界銀行サンプル** | [詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [VS 2010 ソース](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net40) | [VS 2012 ソース](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net45)

世界銀行の JSON.NET を使用して、結果を解析するデータ サイトからデータを取得する方法を示します。

## <a name="web-api-samples"></a>Web API のサンプル

**ASP.NET Web API の概要** | [VS 2012 ソース](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

基本的な web HTTP GET 要求をサポートする API を作成する方法を示します。 チュートリアルでは、ソース コードを含む[Your の最初の ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)します。

**ASP.NET Web API の JavaScript シナリオ – コメント** | [VS 2012 ソース](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

ASP.NET Web API を使用して、web ブラウザー クライアントをサポートし、jQuery を使用して簡単に呼び出すことが Api を構築する方法を示しています。

**Contact Manager** | [VS 2010 ソース](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

このサンプルでは、ASP.NET Web API を使用して、単純な連絡先マネージャー アプリケーションをビルドします。 アプリケーションは、表示および連絡先の一覧を管理する ASP.NET MVC アプリケーションと Windows Phone アプリケーションで使用される web API の連絡先のマネージャーで構成されます。

**サンプルをバッチ処理** | [詳細説明](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [VS 2012 ソース](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/BatchSample)

HTTP は、ASP.NET 内でバッチ処理を実装する方法を示します。 バッチ処理は、HTTP POST としてサーバーに送信して、1 つの MIME マルチパート エンティティ ボディ内で複数の HTTP 要求を配置するので構成されます。 要求は、個別に処理され、応答は、クライアントに返されるもう 1 つの MIME マルチパート エンティティ本体に配置されます。

**コント ローラー サンプルをコンテンツ** | [詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [VS 2010 ソース](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net40) | [VS 2012 ソース](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net45)

読み取りと書き込み要求と応答のエンティティを非同期でストリームを使用する方法を示します。 サンプル コント ローラーが 2 つのアクション: 要求エンティティ本体を非同期的に読み取りし、ローカル ファイルに格納する PUT 操作とローカル ファイルの内容を返す GET アクション。

**カスタム アセンブリの競合回避モジュールのサンプル** | [VS 2012 ソース](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomAssemblyResolverSample)

ライブラリを動的に読み込まれたアセンブリからのコント ローラーの検出をサポートする ASP.NET Web API を変更する方法を示します。 サンプルでは、カスタム実装**IAssembliesResolver**を既定の実装の呼び出しを既定の結果にライブラリ アセンブリを追加します。

**カスタムのメディア タイプ フォーマッタ サンプル** | [詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [VS 2010 ソース](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomMediaTypeFormatterSample)

使用してカスタムのメディア タイプ フォーマッタを作成する方法を示しています、 **BufferedMediaTypeFormatter**基本クラス。 この基本クラスはフォーマッタが主に使用して、同期読み取りと書き込み操作の対象としています。 メディア タイプ フォーマッタを示すだけでなく、サンプルの一部として登録することによってフックする方法を示しています、 **HttpConfiguration**アプリケーション用。 使用することも、 **MediaTypeFormatter**非同期を主に使用するフォーマッタが読み取りおよび書き込み操作に対して、この基本クラスを直接、します。

**カスタム パラメーターのバインドのサンプル** | [詳細説明](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [VS 2010 ソース](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomParameterBinding)

要求からの情報をアクション パラメーターにバインドする方法を決定するプロセスでは、パラメーター バインド プロセスをカスタマイズする方法を示します。 このサンプルでは、Home コント ローラーは 4 つのアクションがあります。

1. BindPrincipal は、HTTP GET メッセージ; からではなく、カスタムの汎用プリンシパルから、IPrincipal パラメーターをバインドする方法を示しています。
2. BindCustomComplexTypeFromUriOrBody ようなメッセージ本文とは、HTTP POST メッセージの要求 URI から、複合型のパラメーターをバインドする方法を示しています。
3. BindCustomComplexTypeFromUriWithRenamedProperty shows how to bind a complex-type parameter with a renamed property which comes from the request URI of an HTTP POST message;
4. PostMultipleParametersFromBody は、POST メッセージの本文から複数のパラメーターをバインドする方法を示しています。

**ファイルのアップロード サンプル** | [詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [VS 2012 ソース](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/FileUploadSample)

ファイルをアップロードする方法を示しています、 **ApiController** MIME マルチパート ファイルのアップロード、および進行状況の通知を設定する方法を使用して**HttpClient**を使用して**ProgressNotificationHandler**. コント ローラーは、HTML ファイルのアップロードの内容を非同期的に読み取りし、ローカル ファイルに 1 つまたは複数の本文を書き込みます。 応答には、アップロードされたファイル (またはファイル) に関する情報が含まれています。

**ファイルの Azure Blob ストアのサンプルをアップロード** | [詳細説明](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [VS 2012 ソース](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/AzureBlobsFileUploadSample)

このサンプルは、ファイルのアップロード サンプルに似ていますが、ローカル ディスクにアップロードされたファイルを保存するには、代わりに、非同期的にファイルをアップロードする[Azure Blob Store](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)を使用して[Windows Azure SDK for .NET](https://www.windowsazure.com/develop/net/)します。 現在存在する blob を一覧表示するためのメカニズムも用意されています。、 [Azure Blob ストレージ コンテナー](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)します。 に対して実行されているサンプルを試すことができます**Azure ストレージ エミュレーター** Azure SDK に付属します。 ある場合、 [Azure ストレージ アカウント](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)も、実際のストレージ サービスに対して行うことができます。

**Http メッセージ ハンドラーのパイプライン サンプル** | [詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [VS 2010 ソース](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/HttpMessageHandlerPipelineSample)

接続するための方法を示しています**HttpMessageHandler**クライアントの両方のインスタンス (**HttpClient**) およびサーバー (ASP.NET Web API)。 サンプルでは、同じハンドラーは、クライアントとサーバーの両方で使用されます。 正確な同じハンドラー両方の場所で実行するはまれですが、オブジェクト モデルは、クライアントとサーバー側で同じです。

**アップロードの JSON サンプル** | [VS 2012 ソース](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/JsonUploadSample)

アップロードおよびとの間に、JSON をダウンロードする方法を示しています、 **ApiController**します。 サンプルでは使用を最小限に抑える**ApiController**を使用してアクセスして**HttpClient**します。

**サンプルのマッシュ アップ** | [詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [VS 2012 ソース](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MashupSample)

内から非同期的に複数のリモート サイトにアクセスする方法を示しています、 **ApiController**アクション。 アクションがヒットするたびに、要求は実行、非同期的にできるように、スレッドはブロックされません。

**メモリのトレース サンプル** | [詳細説明](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [VS 2010 ソース](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MemoryTracingSample)

このサンプル プロジェクトでは、ASP.NET Web API アプリケーションにカスタムのメモリ内のトレース ライターをインストールする Nuget パッケージを作成します。

**MongoDB サンプル** | [詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [VS 2012 ソース](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MongoSample)

永続的なストアとして MongoDB を使用する方法を示しています、 **ApiController**、リポジトリ パターンを使用します。

**応答本文のプロセッサ サンプル** | [VS 2012 ソース](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ResponseEntityProcessorSample)

これは、クライアントに送信する前に、応答のエンティティ (つまり、HTTP 応答本文) をローカル ファイルにコピーして、非同期的にそのファイルに追加処理を実行する方法を示します。 サンプルでは実装、 **HttpMessageHandler**を通常どおりの出力とローカル ファイル自体を書き出す両方こと応答エンティティが 1 つをラップします。

**XDocument のサンプルをアップロード** | [詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [VS 2012 ソース](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/UploadXDocumentSample)

XDocument にアップロードする方法を示しています、 **ApiController**を使用して**PushStreamContent**と**HttpClient**します。

**検証のサンプル** | [VS 2010 ソース](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ValidationSample)

HTTP 要求の内容を検証する ASP.NET web Api でモデルの検証属性を使用する方法を示しています。 必要に応じてフレームワークで定義された両方を使用する方法とカスタム検証属性に、モデルの注釈を付けるようにプロパティをマークする方法と無効なモデル状態のエラー応答を返す方法を示します。

**Web フォームのサンプル** | [詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [VS 2010 ソース](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/WebFormSample)

Web フォーム プロジェクトに追加、ApiController を示しています。

**[RestBugs サンプル](https://github.com/howarddierking/RestBugs)**

RestBugs は、単純なバグ追跡アプリケーションを ASP.NET Web API と、新しい HTTP クライアント ライブラリを使用して、ハイパー メディア駆動システムを作成する方法を示しています。 このサンプルには、ASP.NET Web API を使用して、クライアントとサーバーの両方の実装が含まれています。 サーバーでは、カスタムの Razor フォーマッタを使用して、リソースの表現を生成します。 このサンプルでは、クライアントとサーバーを分離するために、ハイパー メディア設計を使ったことによる利点を示す node.js サーバーも提供します。
