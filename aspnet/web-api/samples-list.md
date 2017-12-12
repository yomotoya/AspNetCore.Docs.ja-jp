---
uid: web-api/samples-list
title: "Web API のサンプル リスト |Microsoft ドキュメント"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 2f40cd4bebdd64c3a4b94cfc1e717fa4b304e57e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="web-api-samples-list"></a>Web API のサンプル一覧
====================
## <a name="httpclient-samples"></a>HttpClient のサンプル

**Bing 翻訳サンプル** | [VS 2012 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

呼び出す方法を示しています、 [Microsoft Translator サービス](https://msdn.microsoft.com/en-us/library/ff512419.aspx)を使用して、 **HttpClient**クラスです。 Microsoft Translator サービス API では、OAuth トークン、要求ごとに、トランスレーター サービス用の Azure トークン サーバーに要求を送信して、アプリケーションを取得する必要があります。 トークンのサーバーから結果が、翻訳サービスに送信される要求にフィードします。 このサンプルを実行する前に取得する必要があります、 [Azure Marketplace からのアプリケーション キー](https://msdn.microsoft.com/en-us/library/hh454950.aspx) AccessTokenMessageHandler サンプル クラス内の情報を入力します。

**Google のマップ サンプル** | [の詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [VS 2012 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

使用して**HttpClient**からワシントン州レドモンドのマップをダウンロードする[Google Maps API](https://developers.google.com/maps/)、ローカル ファイルとして保存され、既定イメージ ビューアーが開きます。

**クライアントのサンプルの twitter** | [の詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [VS 2012 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

使用して単純な Twitter クライアントを記述する方法を示します**HttpClient**です。 このサンプルでは、 **HttpMessageHandler**送信に OAuth 認証情報を挿入する**HttpRequestMessage**です。 JSON.NET を使用して Twitter から結果が読み取られます。 このサンプルを実行する前に取得する必要があります、 [Twitter からアプリケーション キー](https://dev.twitter.com/)、OAuthMessageHandler サンプル クラス内の情報を入力します。

**世界銀行サンプル** | [の詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [VS 2010 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [VS 2012 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

JSON.NET を使用して、結果を解析する世界銀行データ サイトからデータを取得する方法を示します。

## <a name="web-api-samples"></a>Web API のサンプル

**ASP.NET Web API の使用を開始する** | [VS 2012 ソース](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

基本的な web HTTP GET 要求をサポートする API を作成する方法を示します。 チュートリアルでは、ソース コードを含む[Your の最初の ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)です。

**ASP.NET Web API の JavaScript のシナリオ-コメント** | [VS 2012 ソース](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

ASP.NET Web API を使用して、web ブラウザー クライアントをサポートし、jQuery を使用して簡単に呼び出すことができる Api をビルドする方法を示します。

**連絡先のマネージャー** | [VS 2010 ソース](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

このサンプルでは、ASP.NET Web API を使用して、単純な連絡先のマネージャー アプリケーションをビルドします。 アプリケーションは、連絡先のマネージャーの web API を表示および連絡先の一覧を管理する ASP.NET MVC アプリケーションと Windows Phone アプリケーションで使用されるので構成されます。

**サンプルをバッチ処理** | [の詳細説明](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [VS 2012 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

ASP.NET 内で HTTP がバッチ処理を実装する方法を示します。 バッチ処理は、HTTP POST としてサーバーに送信されて、単一の MIME マルチパート エンティティ ボディ内で複数の HTTP 要求を配置するので構成されます。 要求は個別に処理され、応答がクライアントに返される別の MIME マルチパート エンティティ ボディに格納されます。

**コンテンツのコント ローラー サンプル** | [の詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [VS 2010 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [VS 2012 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

非同期でストリームを使用して、要求と応答のエンティティを読み書きする方法を示します。 サンプル コント ローラーが 2 つのアクション: 要求エンティティ本体を非同期的に読み取りし、ローカルのファイルに格納する PUT 操作とローカル ファイルの内容を返します、GET 操作します。

**カスタム アセンブリの競合回避モジュール サンプル** | [VS 2012 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

ライブラリを動的に読み込まれたアセンブリからのコント ローラーの検出をサポートする ASP.NET Web API を変更する方法を示します。 このサンプルを実装するカスタム**IAssembliesResolver**既定の実装を呼び出すし、既定の結果にライブラリ アセンブリを追加します。

**カスタムのメディア タイプ フォーマッタ サンプル** | [の詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [VS 2010 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

使用してカスタムのメディア タイプ フォーマッタを作成する方法を示します、 **BufferedMediaTypeFormatter**基本クラスです。 この基本クラスは、主に、同期読み取りと書き込み操作フォーマッタを目的としています。 メディア タイプ フォーマッタを示すだけでなく、サンプルの一部として登録することによってフックする方法を示します、 **HttpConfiguration**アプリケーションです。 使用することも、 **MediaTypeFormatter**主に非同期を使用するフォーマッタが読み取りおよび書き込み操作について、この基本クラスを直接、します。

**カスタム パラメーターのバインドのサンプル** | [の詳細説明](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [VS 2010 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

これは、要求からの情報をアクション パラメーターにバインドする方法を決定するプロセス パラメーターのバインディング プロセスをカスタマイズする方法を示します。 このサンプルでは、Home コント ローラーには、4 つの操作があります。

1. BindPrincipal は、HTTP GET メッセージ; からではなく、カスタムの汎用プリンシパルから IPrincipal パラメーターをバインドする方法を示しています。
2. BindCustomComplexTypeFromUriOrBody は、メッセージ本文とは、要求の HTTP POST メッセージ; の URI から取得される可能性が、複合型のパラメーターをバインドする方法を示しています。
3. BindCustomComplexTypeFromUriWithRenamedProperty は、要求の HTTP POST メッセージ; の URI によってもたらされる名前が変更されたプロパティを持つ複合型パラメーターをバインドする方法を示しています。
4. PostMultipleParametersFromBody は、POST のメッセージの本文から複数のパラメーターをバインドする方法を示しています。

**ファイルのアップロード サンプル** | [の詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [VS 2012 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

ファイルをアップロードする方法を示しています、 **ApiController** MIME マルチパート ファイルのアップロード、およびと進行状況の通知を設定する方法を使用して**HttpClient**を使用して**ProgressNotificationHandler**. コント ローラーは、HTML ファイルのアップロードの内容を非同期的に読み取りし、1 つまたは複数のボディ部をローカル ファイルに書き込みます。 応答には、アップロードされたファイル (またはファイル) に関する情報が含まれています。

**ファイルのアップロードは Azure Blob ストアのサンプルを** | [の詳細説明](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [VS 2012 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

このサンプルは、ファイル アップロード サンプルに似ていますが、ローカル ディスクにアップロードされたファイルを保存するには、代わりに、非同期的にファイルをアップロードする[Azure Blob ストア](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)を使用して[Windows Azure SDK for .NET](https://www.windowsazure.com/en-us/develop/net/)です。 内に現存の blob を一覧表示するための機構も用意されています、 [Azure Blob ストレージ コンテナー](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)です。 に対して実行する例を試すことができます**Azure ストレージ エミュレーター** Azure SDK に付属します。 ある場合、 [Azure ストレージ アカウント](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)も、実際のストレージ サービスに対して実行することができます。

**Http メッセージ ハンドラー パイプライン サンプル** | [の詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [VS 2010 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

接続する方法を示します**HttpMessageHandler**クライアントの両方のインスタンス (**HttpClient**) およびサーバー (ASP.NET Web API)。 サンプルでは、同じハンドラーは、クライアントとサーバーの両方で使用されます。 両方の場所に正確な同じハンドラーが実行されることはまれですが、オブジェクト モデルの値は、クライアント側とサーバー側で同じです。

**JSON のアップロードのサンプル** | [VS 2012 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

アップロードおよびとの間に、JSON をダウンロードする方法を示します、 **ApiController**です。 サンプルでは使用を最小限に抑える**ApiController**を使用してアクセスして**HttpClient**です。

**マッシュ アップ サンプル** | [の詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [VS 2012 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

内から非同期的に複数のリモート サイトにアクセスする方法を示しています、 **ApiController**アクション。 アクションをヒットするたびに、要求は非同期的に実行、できるように、スレッドはブロックされません。

**メモリ トレース サンプル** | [の詳細説明](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [VS 2010 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

このサンプル プロジェクトでは、ASP.NET Web API アプリケーションにカスタムのメモリ内のトレース ライターをインストールする Nuget パッケージを作成します。

**MongoDB サンプル** | [の詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [VS 2012 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

永続的ストアとして MongoDB を使用する方法を示しています、 **ApiController**、リポジトリ パターンを使用します。

**応答本文のプロセッサ サンプル** | [VS 2012 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

これは、クライアントに送信する前に、応答のエンティティ (つまり、HTTP 応答本文) をローカル ファイルにコピーし、追加の処理でそのファイルに非同期的を実行する方法を示します。 このサンプルを実装する**HttpMessageHandler**両方を記述自体を通常どおりの出力とローカル ファイルに 1 つの応答エンティティをラップします。

**XDocument のサンプルをアップロード** | [の詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [VS 2012 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

XDocument をアップロードする方法を示しています、 **ApiController**を使用して**PushStreamContent**と**HttpClient**です。

**検証のサンプル** | [VS 2010 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

HTTP 要求の内容を検証する ASP.NET WebAPI でモデルの検証属性を使用する方法を示しています。 必要なフレームワークによって定義された両方を使用する方法と、モデルに注釈を設定するカスタム検証属性のプロパティをマークする方法と無効なモデル状態のエラー応答を返す方法を示します。

**フォームのサンプルを web** | [の詳細説明](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [VS 2010 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Web フォーム プロジェクトに追加、ApiController を示しています。

**[RestBugs サンプル](https://github.com/howarddierking/RestBugs)**

RestBugs は、管理アプリケーションを ASP.NET Web API、および新しい HTTP クライアント ライブラリを使用して、hypermedia ドリブン システムを作成する方法を示す単純な不具合です。 このサンプルには、ASP.NET Web API を使用して、クライアントとサーバーの両方の実装が含まれています。 サーバーでは、カスタム Razor フォーマッタを使用して、リソースの表現を生成します。 このサンプルでは、hypermedia 設計を使用して、クライアントとサーバーを切り離すことに由来する利点を示すために node.js サーバーも提供します。

## <a name="web-api-extensions-preview-samples"></a>Web API 拡張機能のプレビュー例

**OData クエリ可能なサンプル** | [の詳細説明](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [VS 2010 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

いずれかを使用して ASP.NET Web API の OData クエリを導入する方法を示しています、`[Queryable]`属性にするかを使用して、 **ODataQueryOptions**アクションを手動で実行される前に、クエリを検査することを許可するアクション パラメーター。

[Queryable] 属性を使用して、CustomerController を示していて、OrderController ODataQueryOptions パラメーターを使用する方法を示しています。 ResponseController は似ていますが、CustomerController を返す GET 操作ではなく`IEnumerable<Customer>`が返されます、 **HttpResponseMessage**です。 これにより、クエリ機能を使用中にステータス コードなどの操作、余分なヘッダー フィールドを追加することができます。 このサンプルは、$orderby、$skip、$top、any()、all()、および $filter を使用してクエリを示しています。

**OData サービス サンプル** | [の詳細説明](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [VS 2010 ソース](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

このサンプルでは、3 つのエンティティと 3 つの Web API コント ローラーで構成される、OData サービスを作成する方法を示します。 コント ローラーは、OData の機能を公開するという観点から機能のさまざまなレベルを提供します。

SupplierController クエリを含む機能のサブセットを公開する、キーと作成、これらの要求を処理することによって取得します。

- /Suppliers を取得します。
- /Suppliers(key) を取得します。
- GET/Suppliers? $filter =.&amp;$orderby =.&amp;$top =.&amp;$skip =.
- POST/Suppliers

GET、PUT、投稿、削除、およびこれらの各操作のアクションを直接実装することで修正プログラムを適用する ProductsController 公開にします。

ProductFamilesController は、豊富な OData サービスを実装するのに便利なパターンを公開する EntitySetController 基底クラスを活用します。

OData サービスが、データの消費を $metadata ドキュメントを公開するさらに WCF Data Service クライアントおよび $metadata 形式をそのまま使用する他のクライアントです。
