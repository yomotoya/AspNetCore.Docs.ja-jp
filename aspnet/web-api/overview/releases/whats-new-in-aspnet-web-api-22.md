---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: ASP.NET Web API 2.2 の新機能 |Microsoft ドキュメント
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 400329dd852ca3c527387ee45e3e902b725e771b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508401"
---
<a name="whats-new-in-aspnet-web-api-22"></a>ASP.NET Web API 2.2 の新機能
====================
によって[Microsoft](https://github.com/microsoft)

このトピックでは、ASP.NET Web API 2.2 の新機能について説明します。

- [ダウンロード](#download)
- [ドキュメント](#documentation)
- [ASP.NET Web API 2.2 の新機能](#newf)

    - [OData v4](#OData)
    - [属性のルーティングの機能強化](#ARI)
    - [Windows Phone 8.1 用の web API のクライアントのサポート](#phone)
- [既知の問題と重大な変更](#known-issues)
- [バグの修正](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 ベータ版](#523)

<a id="download"></a>
## <a name="download"></a>ダウンロード

ランタイムの機能は、NuGet ギャラリー上の NuGet パッケージとしてリリースされています。 次のすべてのランタイム パッケージ、[セマンティック バージョニング](http://semver.org/)仕様です。 ASP.NET Web API 2.2 の最新のパッケージは、次のバージョン:「5.2.0」です。 インストールまたはを介してこれらのパッケージを更新することができます[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)です。 リリースには、NuGet で対応するローカライズ版パッケージも含まれています。

インストールまたは NuGet パッケージ マネージャー コンソールを使用してリリースされた NuGet パッケージを更新できます。

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>ドキュメント

チュートリアルと ASP.NET Web API 2.2 に関する他の情報は、ASP.NET web サイトから使用可能な ([https://www.asp.net/web-api](../../index.md))。

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>ASP.NET Web API 2.2 の新機能

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

このリリースでは、OData v4 プロトコルのサポートを追加します。 詳細については、次を参照してください。、 [Web API OData v4 ドキュメント。](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

次にいくつかの主要な機能と OData v4 の変更を示します。

- [OData のモデルのエイリアス プロパティのサポート](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [ComplexTypeAttribute、AssociationAttribute、TimesTampAttribute および ODataConventionModelBuilder で ConcurrencyCheckAttribute のサポート](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [アクションのわかりやすいタイトルを指定する機能を提供します。](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- ODL UriParser との統合します。
- サポート[enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/)、[コンテインメント](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)と[シングルトン](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- プリミティブ型のキャストのサポート
- [OData 関数のサポートが追加されました](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [関数呼び出しのパラメーターの別名をサポートします。](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [モデルでケース camel 形式の名前付け規則をサポートします。](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- $Filter で cast() のサポート
- 開いている複合型のサポート
- 削除された EntitySetController と AsyncEntitySetController
- [$Link $ref に変更されました。](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [属性のルーティング サポートが追加されました](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- OData のコア ライブラリ 6.4.0 を使用します。

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>属性のルーティングの機能強化

属性のようになりましたルーティング属性ルートを検出および構成する方法を完全に制御できる IDirectRouteProvider と呼ばれる機能拡張ポイントを提供します。 アクションおよびアクションのどのようなルーティング構成が必要なだけ指定に関連するルート情報と共にコント ローラーの一覧を提供する場合は、IDirectRouteProvider します。 MapAttributes/MapHttpAttributeRoutes を呼び出すときに、IDirectRouteProvider 実装を指定することがあります。

IDirectRouteProvider のカスタマイズが簡単に、既定の実装、DefaultDirectRouteProvider を拡張することによってです。 このクラスは、属性を検出し、ルート エントリを作成して、ルート プレフィックスおよび領域プレフィックスを検出するためのロジックを変更する別のオーバーライド可能な仮想メソッドを提供します。

この新しい機能拡張ポイントで実行できなかったタスクにいくつかの例を次に示します。

1. ルート属性の継承のサポート

    例:

    ここで like「/10/api/値」の要求では「成功: 10」を返しますが正常に

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. 同様に、いくつかの規則に従って、属性のルートの既定のルート名を提供します。 既定では、属性のルーティングが自動的に作成属性ルートの名前。
3. ルート テーブルで終了する前に、1 か所にある属性ルートのルート テンプレートを変更します。

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Windows Phone 8.1 用の web API のクライアントのサポート

Windows Phone 8.1 を対象とする場合、またはから Web API クライアント ロジックを実装する Web API のクライアントの NuGet パッケージを使用するようになりましたユニバーサル アプリ内で。

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

このセクションでは、既知の問題と ASP.NET Web API 2.2 で重大な変更について説明します。

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>モデル ビルダー

問題点: オーバー ロードされた関数が公開されません FunctionImport として

ある場合は、オーバー ロードされた関数を 2 と FunctionImport System.invalidoperationexception: で ~/GetAllConventionCustomers(CustomerName={customerName}) 結果を要求し、次に示すようにもです。

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

回避策: FunctionImports として、両方の関数オーバー ロードを追加するのには、この問題の回避策です。

#### <a name="odata-routing"></a>OData のルーティング

URL を含む文字列リテラルは、スラッシュ (%2f) をエンコードおよび backslash(%5C) OData リソース パスで使用しているときに 404 エラーが発生します。

たとえば、関数のパラメーターまたはエンティティ セットのキー値としてするには、OData リソース パスでリテラル文字列を使用します。

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

サービス ホストは解除エスケープこのような要求を受信するとき、エスケープ シーケンス、Web API のランタイムに渡す前にします。 これは、次のような攻撃から保護します。  
  
 http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:

これにより、Web API OData スタックは、404 (Not Found) エラーを返します。 このエラーを防ぐためには、クライアントは、スラッシュ (%252f) のダブル エスケープ シーケンスと円記号 (%255 C) を使用する必要があります。 これが動作しない/Employees などのクエリ文字列のですか? $filter = Name eq '名 %2f'

**エスケープされないスラッシュ ('/') に注意してください、円記号 (") が OData リソース パス文字列リテラルでは有効ではありません。パスの区切り記号としてスラッシュが表示され、バック スラッシュがまったく OData リソース パスに表示されません。(両方が、OData のクエリ文字列の一部では使用できません。)**

回避策: 実際にそれらを解析する前にスラッシュと文字列リテラルのバック スラッシュをエスケープするために DefaultODataPathHandler の Parse メソッドをオーバーライドする可能性があります。 この方法を次のサンプルを見つけることができます。

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[クエリ]

[Queryable] 属性は推奨されません。 新しい OData v3 のアプリケーションで使用する必要があります**System.Web.Http.OData.EnableQueryAttribute**です。

**ODataHttpConfigurationExtensions.EnableQuerySupport**拡張メソッドを今すぐ追加、 **EnableQueryAttribute**をグローバル フィルターのコレクション。 すべてのコント ローラーがある場合、 **[Queryable]** 属性、呼び出し`config.EnableQuerySupport()`により、 **[Queryable]** 属性が失敗するには

すべてのインスタンスを置き換えるには、この問題を解決することをお勧め**QueryableAttribute**で**System.Web.Http.OData.EnableQueryAttribute**です。

別の回避策は、Web API 構成で次のコードを使用するのには。

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>属性のルーティング

問題点: FromUri 属性で装飾されている複合型のモデル バインディングとは異なる動作属性のルーティングを使用する場合。

次のリンクは、問題を追跡し、回避策の詳細が。  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

問題点: スキャフォールディング MVC または Web API を 5.2.0 をプロジェクトにプロジェクトに既に存在しないものの 5.1.2 パッケージの結果をパッケージ化

ASP.NET MVC 5.2 の NuGet パッケージを更新しても、ASP.NET スキャフォールディングなど Visual Studio のツールや ASP.NET Web アプリケーション プロジェクト テンプレートには更新されません。 ASP.NET ランタイム パッケージ (Update 2 で 5.1.2 など) の以前のバージョンを使用します。 プロジェクトで使用されていない場合に、ASP.NET スキャフォールディングでその結果、必要なパッケージの以前のバージョン (Update 2 で 5.1.2 など) がインストールされますか。 ただし、Visual Studio 2013 RTM または更新プログラム 1 で ASP.NET スキャフォールディングでは、プロジェクト内の最新のパッケージは上書きされません。 Web API 2.2 または ASP.NET MVC 5.2、プロジェクトのパッケージを更新した後に ASP.NET のスキャフォールディングを使用する場合は、Web API と ASP.NET MVC のバージョンでは、一貫性を確認します。

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>バグの修正およびマイナー機能の更新

このリリースではいくつかのバグ修正とマイナー機能も更新します。 完全な一覧は、ここをご覧ください。

- [5.2 パッケージ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

Microsoft.AspNet.OData 5.2.1 パッケージには、NuGet の依存関係の更新プログラムがバグの修正が含まれていません。 この更新で、Microsoft.OData.Core 6.4.0 に厳密な依存関係は不要になったが 6.4.0 と 7.0.0 間の任意のバージョンにアップグレードできますいずれか。

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

このリリースでは、依存関係の変更を行いましたが`Json.Net 6.0.4`です。 このリリースの新機能の詳細については`Json.NET`を参照してください[Json.NET 6.0 リリース 4 - JSON をマージ、依存性の注入](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection)です。 このリリースでは、Web API に他の新機能、バグの修正を持っていません。 私たちを所有してこの新しいバージョンの Web API に依存するその他のすべての依存パッケージが、後で更新します。

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 ベータ版

リリースについて[ここ](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)です。 このリリースには、バグの修正プログラムのみが含まれています。 使用することができます[このクエリ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)今回のリリースで修正された問題の一覧を表示します。
