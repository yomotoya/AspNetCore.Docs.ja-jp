---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: "ASP.NET Web API OData 5.3 の新機能 |Microsoft ドキュメント"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>ASP.NET Web API OData 5.3 の新機能
====================
によって[Microsoft](https://github.com/microsoft)

このトピックでは、ASP.NET Web API OData 5.3 の新機能について説明します。

- [ダウンロード](#download)
- [ドキュメント](#documentation)
- [OData のコア ライブラリ](#corelib)
- [新機能](#newf)
- [既知の問題と重大な変更](#known-issues)
- [バグの修正](#bug-fixes)
- [ASP.NET Web API OData 5.3.1](#OD)

<a id="download"></a>
## <a name="download"></a>ダウンロード

ランタイムの機能は、NuGet ギャラリー上の NuGet パッケージとしてリリースされています。 インストールまたは NuGet パッケージ マネージャー コンソールを使用してリリースされた NuGet パッケージを更新できます。

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>ドキュメント

チュートリアルおよび ASP.NET Web API OData については、その他のドキュメントを見つけることができます、 [ASP.NET web サイト](../odata-support-in-aspnet-web-api/index.md)です。

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>OData のコア ライブラリ

OData v4 の Web API 今すぐ使用 ODataLib バージョン 6.5.0

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>ASP.NET で新機能の Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>$Levels $a 内の展開のサポートします。

$Levels を使用するクエリが $ でオプションがクエリを展開します。 例:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

このクエリと同じです。

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>開いているエンティティ型のサポート

*オープン型*種類の定義で宣言されているプロパティに加え、動的なプロパティを含む stuctured 型です。 オープン型では、データ モデルに柔軟性を追加できます。 詳細については、xxxx を参照してください。

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>オープン型での動的なコレクション プロパティのサポート

以前は、動的プロパティは、1 つの値を指定する必要があります。 5.3、動的なプロパティはコレクションの値を持つことができます。 たとえば、次の JSON ペイロードで、`Emails`プロパティは文字列型のコレクションは、動的なプロパティ。

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>複合型の継承のサポート

今すぐ複合型を基本型から継承できます。 たとえば、OData サービスは、次の複合型を定義できます。

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

この例については、EDM を次に示します。

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

詳細については、次を参照してください。[複合型の継承のサンプルの OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)です。

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

このセクションでは、既知の問題と、ASP.NET Web API OData バージョン 5.3 以降で重大な変更について説明します。

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>Query Options (クエリ オプション)

問題点: 入れ子になった $ を使用する $levels と展開、不適切な展開の深さの最大の結果を = です。

たとえば、次の要求を指定します。

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

場合`MaxExpansionDepth`は 5、6 の展開の深さはこのクエリになります。

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>バグの修正およびマイナー機能の更新

このリリースではいくつかのバグ修正とマイナー機能も更新します。 完全な一覧は、ここをご覧ください。

- [バグ修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1

このリリースでは行われた、[のバグ修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All)AllowedFunctions 列挙型の一部にします。 このリリースには、他のバグの修正や新機能がありません。
