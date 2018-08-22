---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: ASP.NET Web API 2.2 の新機能新機能 |Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 12/25/2014
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: ef08a3bb397ff54795ca6c70cdcc35206cf122f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828790"
---
<a name="whats-new-in-aspnet-web-api-22"></a>ASP.NET Web API 2.2 の新機能新機能
====================
によって[Microsoft](https://github.com/microsoft)

このトピックでは、ASP.NET Web API 2.2 の新機能新機能について説明します。

- [ダウンロード](#download)
- [ドキュメント](#documentation)
- [ASP.NET Web API 2.2 の新機能](#newf)

    - [OData v4](#OData)
    - [属性のルーティングが強化されました](#ARI)
    - [Windows Phone 8.1 用の web API クライアントのサポート](#phone)
- [既知の問題と重大な変更](#known-issues)
- [バグの修正](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 ベータ版](#523)

<a id="download"></a>
## <a name="download"></a>ダウンロード

ランタイムの機能は、NuGet ギャラリーでの NuGet パッケージとしてリリースされます。 次のすべてのランタイム パッケージ、[セマンティック バージョニング](http://semver.org/)仕様。 ASP.NET Web API 2.2 の最新のパッケージは、次のバージョン:「5.2.0」。 インストールまたはを通じてこれらのパッケージを更新することができます[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)します。 リリースには、NuGet での対応するローカライズされたパッケージも含まれています。

インストールまたは NuGet パッケージ マネージャー コンソールを使用して、リリースされた NuGet パッケージを更新できます。

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>ドキュメント

チュートリアルと ASP.NET Web API 2.2 に関する他の情報は、ASP.NET web サイトから入手できます ([https://www.asp.net/web-api](../../index.md))。

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>ASP.NET Web API 2.2 の新機能

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

このリリースでは、OData v4 のプロトコルのサポートを追加します。 詳細については、次を参照してください。、 [Web API OData v4 のドキュメント。](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

ここで、主要な機能と OData v4 の変更の一部を示します。

- [OData モデルでエイリアス プロパティのサポート](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [ComplexTypeAttribute、AssociationAttribute、TimesTampAttribute ODataConventionModelBuilder で ConcurrencyCheckAttribute のサポート](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [アクションのわかりやすいタイトルを指定することができます。](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- ODL UriParser との統合します。
- サポート[enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/)、[コンテインメント](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)と[シングルトン](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- プリミティブ型のキャストのサポート
- [OData 関数のサポートが追加されました](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [パラメーターのエイリアスを関数呼び出しをサポートします。](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [モデルで camel 形式の大文字と小文字の名前付け規則をサポートします。](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- $Filter で cast() のサポート
- 開いている複合型のサポート
- 削除された EntitySetController と AsyncEntitySetController
- [$Link $ref に変更されました。](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [属性ルーティングのサポートが追加されました](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- OData のコア ライブラリ 6.4.0 を使用してください。

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>属性のルーティングが強化されました

属性ルーティングを今すぐ IDirectRouteProvider で、属性ルートが検出され、構成する方法を完全に制御できますと呼ばれる機能拡張ポイントを提供します。 IDirectRouteProvider はアクションとコント ローラーとアクションのどのようなルーティングの構成が必要なだけを指定する関連付けられたルート情報の一覧を提供します。 MapAttributes/MapHttpAttributeRoutes を呼び出すときに、IDirectRouteProvider 実装を指定することがあります。

IDirectRouteProvider のカスタマイズになります最も簡単な既定の実装、DefaultDirectRouteProvider を拡張することによって。 このクラスは、属性の検出、ルートのエントリを作成するルート プレフィックス領域プレフィックスを検出するためのロジックを変更する別のオーバーライド可能な仮想メソッドを提供します。

次は、この新しい機能拡張ポイントで行うことで例を示します。

1. ルート属性の継承のサポート

    例:

    ここで like「/api/値/10」の要求では「成功: 10」を返しますが正常に

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. ようにいくつかの規則に従うことによって、属性ルートの既定のルート名を提供します。 既定では、属性ルーティングは自動的に作成属性ルートの名前。
3. ルート テーブルで終了する前に、1 つの一元的な場所にある属性ルートのルート テンプレートを変更します。

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Windows Phone 8.1 用の web API クライアントのサポート

Windows Phone 8.1 を対象とする場合、またはから Web API クライアント ロジックを実装する Web API クライアントの NuGet パッケージを使用できます、ユニバーサル アプリ内で。

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

このセクションでは、既知の問題と、ASP.NET Web API 2.2 における重大な変更について説明します。

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>モデル ビルダー

問題: オーバー ロードされた関数がいないとして公開する FunctionImport

2 つのオーバー ロードされた関数があり、FunctionImport System.InvalidOperationException で ~/GetAllConventionCustomers(CustomerName={customerName}) 結果を要求し、次に示すようもします。

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

回避策: FunctionImports として、両方の関数オーバー ロードを追加するのにはこの問題を回避します。

#### <a name="odata-routing"></a>OData のルーティング

URL を含む文字列リテラルは、スラッシュ (%2f) をエンコードし、OData リソース パスで使用すると、backslash(%5C) が 404 エラーを発生します。

たとえば、関数のパラメーターまたはエンティティ セットのキー値として、OData リソース パスで文字列リテラルを使用できます。

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

サービス ホストは解除エスケープこのような要求を受信するときに、シーケンスを Web API ランタイムに渡す前にエスケープします。 これは、次のような攻撃から保護します。  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

これにより、Web API OData スタックは、404 (Not Found) エラーが返されます。 このエラーを防ぐためには、クライアントは、スラッシュ (%252f) のエスケープ シーケンスと円記号 (%255 C) を使用する必要があります。 /Employees などのクエリ文字列には発生しませんか? $filter = Name eq '名 %2f'

**エスケープ解除されたスラッシュ (/) をメモし、円記号 (") は OData リソース パスの文字列リテラルで宣言できません。スラッシュはパスの区切り記号としてのみ表示して、バック スラッシュがまったく OData リソース パスに表示されません。(両方とも、OData クエリ文字列の一部で使用できます。)**

回避策: DefaultODataPathHandler を実際にそれらを解析する前にスラッシュと文字列リテラルのバック スラッシュをエスケープするための Parse メソッドをオーバーライドできます。 ここで、このアプローチのサンプルを見つけることができます。

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[クエリ]

[Queryable] 属性が非推奨とされます。 新しい OData v3 のアプリケーションを使用する必要があります**System.Web.Http.OData.EnableQueryAttribute**します。

**ODataHttpConfigurationExtensions.EnableQuerySupport**拡張メソッドを今すぐ追加、 **EnableQueryAttribute**にグローバル フィルターのコレクション。 すべてのコント ローラーがある場合、 **[Queryable]** 属性は、呼び出す`config.EnableQuerySupport()`により、 **[Queryable]** 属性が失敗するには

この問題を解決するのには推奨される方法は、のすべてのインスタンスを置換する**QueryableAttribute**で**System.Web.Http.OData.EnableQueryAttribute**します。

別の回避策では、Web API の構成で、次のコードを使用します。

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>属性ルーティング

問題: FromUri 属性で装飾された複合型のモデル バインド動作が異なります属性ルーティングを使用する場合。

次のリンクは、問題を追跡と回避策の詳細があります。  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

問題: スキャフォールディングの MVC または Web API をプロジェクトに 5.2.0 に 5.1.2 でパッケージの結果のパッケージのプロジェクトにまだ存在しません。

ASP.NET MVC 5.2 の NuGet パッケージを更新しても、ASP.NET スキャフォールディングなどの Visual Studio のツールや、ASP.NET Web アプリケーション プロジェクト テンプレートには更新されません。 ASP.NET ランタイム パッケージ (例: 更新プログラム 2 で 5.1.2) の以前のバージョンを使用します。 その結果、プロジェクトで使用されていない場合は、ASP.NET スキャフォールディングは、必要なパッケージの以前のバージョン (例: 更新プログラム 2 で 5.1.2) がインストールされます。 ただし、Visual Studio 2013 RTM や Update 1 での ASP.NET スキャフォールディングでは、プロジェクトで最新のパッケージは上書きされません。 Web API 2.2 または ASP.NET MVC 5.2 に、プロジェクトのパッケージを更新した後、ASP.NET スキャフォールディングを使用する場合は、Web API と ASP.NET MVC のバージョンでは、一貫性を確認します。

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>バグの修正と軽微な機能の更新

このリリースではいくつかのバグ修正と軽微な機能も更新します。 完全な一覧をここにあります。

- [5.2 パッケージ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

Microsoft.AspNet.OData 5.2.1 パッケージには、NuGet 依存関係の更新プログラムがバグの修正が含まれていません。 この更新で、Microsoft.OData.Core 6.4.0 に厳密な依存関係が不要になったが 1 つは 6.4.0 と 7.0.0 間の任意のバージョンにアップグレードできます。

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

このリリースでは、依存関係が変更を行いましたが`Json.Net 6.0.4`します。 このリリースの新機能新機能の詳細については`Json.NET`を参照してください[Json.NET 6.0 Release 4 - JSON マージ、依存関係の注入](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection)します。 このリリースでは、その他の新機能やバグ修正を Web api がありません。 その後に、この新しいバージョンの Web API に依存する私たちが所有の他のすべての従属パッケージを更新しました。

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 ベータ版

リリースについて[ここ](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)します。 このリリースには、バグの修正プログラムのみが含まれています。 使用することができます[このクエリ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)をこのリリースで修正された問題の一覧を参照してください。
