---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: ASP.NET Web API 2.1 の新機能新機能 |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 8e0501570e6dc6a9a6f69a642f9ab031c5497b5b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385701"
---
<a name="whats-new-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 の新機能新機能
====================
によって[Microsoft](https://github.com/microsoft)

このトピックでは、ASP.NET Web API 2.1 の新機能新機能について説明します。

- [ダウンロード](#download)
- [ドキュメント](#documentation)
- [ASP.NET Web API 2.1 の新機能](#new-features)

    - [グローバル エラー処理](#global-error)
    - [属性のルーティングが強化されました](#attribute-routing)
    - [ヘルプ ページの改善](#help-page)
    - [IgnoreRoute サポート](#ignoreroute)
    - [BSON メディアの種類のフォーマッタ](#bson)
    - [非同期フィルターのサポートの強化](#async-filters)
    - [クエリのクライアント ライブラリを書式設定の解析](#query-parsing)
- [既知の問題と重大な変更](#known-issues)
- [バグの修正](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>ダウンロード

ランタイムの機能は、NuGet ギャラリーでの NuGet パッケージとしてリリースされます。 次のすべてのランタイム パッケージ、[セマンティック バージョニング](http://semver.org/)仕様。 ASP.NET Web API 2.1 の RTM の最新のパッケージは、次のバージョン:「5.1.2」。 インストールまたはを通じてこれらのパッケージを更新することができます[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)します。 リリースには、NuGet での対応するローカライズされたパッケージも含まれています。

インストールまたは NuGet パッケージ マネージャー コンソールを使用して、リリースされた NuGet パッケージを更新できます。

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>ドキュメント

チュートリアルと ASP.NET Web API 2.1 の RTM に関する他の情報は、ASP.NET web サイトから入手できます ([https://www.asp.net/web-api](../../index.md))。

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 の新機能

<a id="global-error"></a>
### <a name="global-error-handling"></a>グローバル エラー処理

中央の 1 つのメカニズムを通じてすべてのハンドルされない例外がログインできるようになりましたし、未処理の例外の動作をカスタマイズすることができます。

フレームワークには、複数の例外ロガー、ハンドルされない例外とが発生した、同時に処理される要求などのコンテキストに関する情報を参照してください。 これがサポートしています。

たとえば、次のコードは、すべてのハンドルされない例外の記録に System.Diagnostics.TraceSource を使用します。

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

既定の例外ハンドラーに置き換えることも、発生する未処理の例外の場合に送信される HTTP 応答メッセージをカスタマイズできるようにするとします。

用意されています、[サンプル](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt)人気のある ELMAH フレームワークを使用してすべての未処理の例外をログに記録します。

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>属性のルーティングが強化されました

属性ルーティングを今すぐ、バージョン管理およびヘッダー ベースのルートの選択を有効にすると、制約がサポートされています。 また、属性ルートの多くの側面を使用してカスタマイズ可能なようになりましたが、 **IDirectRouteFactory**インターフェイスと**RouteFactoryAttribute**クラス。 ルート プレフィックスを使用して拡張が、 **IRoutePrefix**インターフェイスと**RoutePrefixAttribute**クラス。

用意されています、[サンプル](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt)制約を使用して、' api バージョン ' HTTP ヘッダーでコント ローラーを動的にフィルター処理します。

<a id="help-page"></a>
### <a name="help-page-improvements"></a>ヘルプ ページの改善

Web API 2.1 には、次の機能強化が含まれています[API のヘルプ ページ](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):。

- アクションのパラメーターまたは戻り値の個々 のプロパティのドキュメントです。
- データ モデルの注釈のドキュメントです。

ヘルプ ページの UI デザインも更新され、これらの変更に対応するためにします。

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>IgnoreRoute サポート

Web API 2.1 のでは、一連の Web API、ルーティングの URL パターンを無視して**IgnoreRoute**拡張メソッドを**HttpRouteCollection**します。 これらのメソッドは、指定したテンプレートに一致するすべての Url を無視する Web API が発生し、該当する場合に、追加の処理を適用するホストを許可します。

次の例で始まる Uri を無視する、&quot;コンテンツ&quot;セグメント。

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>BSON メディアの種類のフォーマッタ

Web API の今では、 [BSON](http://bsonspec.org/)クライアントとサーバーの両方のワイヤ形式。

BSON をサーバー側で有効にする、 **BsonMediaTypeFormatter**フォーマッタのコレクションに。

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

.NET クライアントが BSON 形式を使用する方法を次に示します。

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

用意されています、[サンプル](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt)クライアントとサーバーの両方の側を示すです。

詳細については、次を参照してください[Web API 2.1 の BSON サポート。](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>非同期フィルターのサポートの強化

Web API は、非同期的に実行するフィルターを作成する簡単な方法をサポートします。 この機能は便利ですが、フィルターは、データベースへのアクセスなどの非同期操作を実行する必要があります。 以前は、非同期フィルターを作成する必要がありましたフィルター インターフェイスを実装して、自分でフィルターの基本クラスにのみ同期メソッドが公開されているためです。 仮想をオーバーライドするようになりました`On*Async`メソッド フィルターの基本クラス。

例えば:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

**AuthorizationFilterAttribute**、 **ActionFilterAttribute**、および**ExceptionFilterAttribute**クラスはすべて Web API 2.1 で async のサポートします。

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>クエリのクライアント ライブラリを書式設定の解析

以前は、 **System.Net.Http.Formatting**解析およびサーバー側コードでは、URI のクエリの更新がサポートされていますが、同等のポータブル ライブラリのこの機能がありません。 Web API 2.1 では、クライアント アプリケーションを今すぐ簡単に解析し、クエリ文字列を更新します。

次の例では、解析、変更、および URI のクエリを生成する方法を示します。 (例にわかりやすくするためのコンソール アプリケーションが表示されます)。

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

このセクションでは、既知の問題と、ASP.NET Web API 2.1 RTM における重大な変更について説明します。

### <a name="attribute-routing"></a>属性ルーティング

属性ルーティングの一致にあいまいさは、最初の一致を選択するのではなく、エラーを報告するようになりました。

属性ルートは、使用を禁止して、 *{controller}* パラメーターを使用してと、 *{action}* ルート パラメーターは、アクションに配置します。 これらのパラメーターはあいまいさが発生する可能性が高い。

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>プロジェクトに 5.0 パッケージで 5.1 のパッケージの結果に MVC または Web API をプロジェクトに既に存在しないもののスキャフォールディング

ASP.NET Web API 2.1 の RTM の NuGet パッケージを更新しても、ASP.NET スキャフォールディングなど、Visual Studio のツールや、ASP.NET Web アプリケーション プロジェクト テンプレートには更新されません。 ASP.NET ランタイム パッケージ (5.0.0.0) の以前のバージョンを使用します。 その結果、プロジェクトで使用されていない場合は、ASP.NET スキャフォールディングは、必要なパッケージの以前のバージョン (5.0.0.0) がインストールされます。 ただし、Visual Studio 2013 RTM や Update 1 での ASP.NET スキャフォールディングでは、プロジェクトで最新のパッケージは上書きされません。

Web API 2.1 または ASP.NET MVC 5.1 のパッケージを更新した後、ASP.NET スキャフォールディングを使用する場合は、Web API と MVC のバージョンに整合性が確認します。

### <a name="type-renames"></a>型名の変更

属性ルーティング拡張機能を使用する型の一部が、RC から 2.1 RTM に変更されました。

| 以前の型名 (2.1 RC) | 新しい型の名前 (2.1 RTM) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>例外フィルターが非同期操作でスローされた例外を集約の折り返しを解除できません。

以前は、非同期アクションがスローした場合、 **AggregateException**、例外フィルターは、例外をラップ解除と**OnException**基本例外を取得します。 2.1 では、例外フィルターはラップ解除しません、および**OnException**取得元**AggregateException**します。

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>バグ修正

このリリースには、いくつかのバグ修正も含まれています。 完全な一覧をここにあります。

- [5.1.0 パッケージ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 パッケージ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

5.1.2、パッケージには、IntelliSense の更新プログラムがないバグの修正が含まれています。
