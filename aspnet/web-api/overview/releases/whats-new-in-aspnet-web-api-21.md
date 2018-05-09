---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: ASP.NET Web API 2.1 の新機能 |Microsoft ドキュメント
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 の新機能
====================
によって[Microsoft](https://github.com/microsoft)

このトピックでは、ASP.NET Web API 2.1 の新機能について説明します。

- [ダウンロード](#download)
- [ドキュメント](#documentation)
- [ASP.NET Web API 2.1 の新機能](#new-features)

    - [グローバルのエラー処理](#global-error)
    - [属性のルーティングの機能強化](#attribute-routing)
    - [ヘルプ ページ機能強化](#help-page)
    - [IgnoreRoute サポート](#ignoreroute)
    - [BSON メディア タイプ フォーマッタ](#bson)
    - [非同期のフィルターのサポートの向上](#async-filters)
    - [クエリのライブラリを書式設定、クライアントの解析](#query-parsing)
- [既知の問題と重大な変更](#known-issues)
- [バグの修正](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>ダウンロード

ランタイムの機能は、NuGet ギャラリー上の NuGet パッケージとしてリリースされています。 次のすべてのランタイム パッケージ、[セマンティック バージョニング](http://semver.org/)仕様です。 ASP.NET Web API 2.1 RTM の最新のパッケージは、次のバージョン:「5.1.2」です。 インストールまたはを介してこれらのパッケージを更新することができます[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)です。 リリースには、NuGet で対応するローカライズ版パッケージも含まれています。

インストールまたは NuGet パッケージ マネージャー コンソールを使用してリリースされた NuGet パッケージを更新できます。

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>ドキュメント

チュートリアルと ASP.NET Web API 2.1 RTM に関する他の情報は、ASP.NET web サイトから使用可能な ([https://www.asp.net/web-api](../../index.md))。

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 の新機能

<a id="global-error"></a>
### <a name="global-error-handling"></a>グローバルのエラー処理

中央の 1 つのメカニズムにより、すべての未処理の例外を記録ようになりましたことができ、未処理の例外の動作をカスタマイズすることができます。

フレームワークは、未処理の例外とするなどの発生時に処理される要求のコンテキストに関する情報を表示する複数の例外ロガーをサポートします。

たとえば、次のコードは、すべてのハンドルされない例外の記録に System.Diagnostics.TraceSource を使用します。

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

既定の例外ハンドラーを置き換えることもが実行されるように、未処理の例外時に送信される HTTP 応答メッセージを完全にカスタマイズできます。

提供されており、[サンプル](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt)人気のある ELMAH フレームワークを使用してすべての未処理の例外をログに記録します。

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>属性のルーティングの機能強化

属性のようになりましたルーティング、バージョン管理とヘッダー ベースのルートの選択を有効にすると、制約がサポートされています。 また、属性ルートの多くの側面を使用してカスタマイズ可能なようになりましたが、 **IDirectRouteFactory**インターフェイスと**RouteFactoryAttribute**クラスです。 ルート プレフィックスが経由で拡張可能なようになりました、 **IRoutePrefix**インターフェイスと**RoutePrefixAttribute**クラスです。

提供されており、[サンプル](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt)で 'api-version' HTTP ヘッダーでコント ローラーを動的にフィルター処理する制約を使用します。

<a id="help-page"></a>
### <a name="help-page-improvements"></a>ヘルプ ページ機能強化

Web API 2.1 に次の機能強化が含まれています[API ヘルプ ページ](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- アクションのパラメーターまたは戻り値の個々 のプロパティのドキュメントです。
- データ モデルの注釈のドキュメントです。

ヘルプ ページの UI のデザインも更新され、これらの変更に合わせてです。

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>IgnoreRoute サポート

Web API 2.1 サポートする一連の Web API、ルーティングの URL パターンを無視して**IgnoreRoute**拡張メソッド**HttpRouteCollection**です。 これらのメソッドを指定したテンプレートに一致するすべての Url を無視する Web API が発生し、該当する場合、追加の処理を適用するホストを許可します。

次の例で始まる Uri は無視されます、&quot;コンテンツ&quot;セグメント。

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>BSON メディア タイプ フォーマッタ

Web API をサポート、 [BSON](http://bsonspec.org/)ワイヤ形式をクライアントとサーバーの両方です。

BSON をサーバー側で有効にする、 **BsonMediaTypeFormatter**フォーマッタのコレクションに。

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

.NET クライアントが BSON 形式を使用する方法を次に示します。

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

提供されており、[サンプル](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt)クライアントとサーバーの両方の側を表示します。

詳細については、次を参照してください[Web API 2.1 で BSON サポート。](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>非同期のフィルターのサポートの向上

Web API には、非同期的に実行するフィルターを作成する簡単な方法がサポートしています。 この機能は役立ちますが、フィルターは、データベース アクセスなど、非同期操作を実行する必要があります。 以前は、非同期フィルターを作成する必要がありましたインターフェイスを実装する、フィルター、フィルターの基本クラスにのみ同期メソッドが公開されているため。 これで、仮想オーバーライドできます`On*Async`フィルターのメソッドの基本クラスです。

例:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

**AuthorizationFilterAttribute**、 **ActionFilterAttribute**、および**ExceptionFilterAttribute**すべてのクラスは、Web API 2.1 で非同期をサポートします。

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>クエリのライブラリを書式設定、クライアントの解析

以前は、 **System.Net.Http.Formatting**解析およびサーバー側コードでは、URI のクエリの更新がサポートされていますが、同等のポータブル ライブラリでこの機能がありません。 2.1 では Web API、クライアント アプリケーションに簡単に解析をできますクエリ文字列を更新します。

次の例では、解析、変更、および URI、クエリを生成する方法を示します。 (例は、わかりやすくするためのコンソール アプリケーションを表示します)。

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

このセクションでは、既知の問題と、ASP.NET Web API 2.1 RTM で重大な変更について説明します。

### <a name="attribute-routing"></a>属性のルーティング

属性のルーティングの一致のあいまいさは、最初の一致を選択するのではなく、エラーを報告します。

属性ルートが使用を禁止して、 *{controller}* パラメーターを使用してと、 *{controller}* ルートのパラメーターは、アクションに配置します。 これらのパラメーターと、あいまいさと非常に高いが発生します。

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>5.0 パッケージをプロジェクトに既に存在しないもので 5.1 パッケージの結果を含むプロジェクトに MVC または Web API のスキャフォールディング

ASP.NET Web API 2.1 RTM の NuGet パッケージを更新しても、ASP.NET スキャフォールディングなど、Visual Studio のツールや ASP.NET Web アプリケーション プロジェクト テンプレートには更新されません。 ASP.NET ランタイム パッケージ (5.0.0.0) の以前のバージョンを使用します。 プロジェクトで使用されていない場合に、ASP.NET スキャフォールディングでその結果、必要なパッケージの以前のバージョン (5.0.0.0) がインストールされますか。 ただし、Visual Studio 2013 RTM または更新プログラム 1 で ASP.NET スキャフォールディングでは、プロジェクト内の最新のパッケージは上書きされません。

Web API 2.1 または ASP.NET MVC 5.1 にパッケージを更新した後に ASP.NET のスキャフォールディングを使用する場合は、Web API と MVC のバージョンが一致を確認します。

### <a name="type-renames"></a>型名の変更

RC から 2.1 RTM に属性のルーティング拡張機能を使用する型の一部が変更されました。

| 古い型の名前 (2.1 RC) | 新しい型名 (2.1 RTM) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>例外フィルターが非同期操作でスローされた例外の集約のラップを解除できません。

以前は、非同期アクションがスローされた場合、 **AggregateException**、例外フィルターは、例外をラップ解除および**OnException**基本例外を取得します。 2.1 では、例外フィルターがラップ解除しません、および**OnException**取得元**AggregateException**です。

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>バグ修正

このリリースでは、いくつかのバグ修正も含まれます。 完全な一覧は、ここをご覧ください。

- [5.1.0 パッケージ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 パッケージ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

5.1.2 パッケージには、IntelliSense の更新がないバグの修正が含まれています。
