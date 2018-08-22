---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: ASP.NET Web API OData 5.3 の新機能新機能 |Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 09/16/2014
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: 20ef263ce7cbaef40c16937eaa91d34239fc2ace
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829289"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>ASP.NET Web API OData 5.3 の新機能新機能
====================
によって[Microsoft](https://github.com/microsoft)

このトピックでは、ASP.NET Web API OData 5.3 の新機能新機能について説明します。

- [ダウンロード](#download)
- [ドキュメント](#documentation)
- [OData のコア ライブラリ](#corelib)
- [新機能](#newf)
- [既知の問題と重大な変更](#known-issues)
- [バグの修正](#bug-fixes)
- [ASP.NET Web API OData 5.3.1](#OD)

<a id="download"></a>
## <a name="download"></a>ダウンロード

ランタイムの機能は、NuGet ギャラリーでの NuGet パッケージとしてリリースされます。 インストールまたは NuGet パッケージ マネージャー コンソールを使用して、リリースされた NuGet パッケージを更新できます。

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>ドキュメント

チュートリアルなどで ASP.NET Web API OData の詳細については、ドキュメントを見つけることができます、 [ASP.NET web サイト](../odata-support-in-aspnet-web-api/index.md)します。

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>OData のコア ライブラリ

OData v4 の Web API では、ODataLib バージョン 6.5.0

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>ASP.NET の新機能の Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>$ で $levels 展開のサポートします。

$Levels を使用するクエリが $ でオプションがクエリを展開します。 例えば:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

このクエリと同等です。

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>オープンなエンティティ型のサポート

*オープン型*は型定義で宣言されている任意のプロパティだけでなく、動的プロパティを含む stuctured 型です。 オープン型では、データ モデルに柔軟性を追加できます。 詳細については、xxxx を参照してください。

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>オープン型での動的コレクションのプロパティのサポート

以前は、動的プロパティは、1 つの値を指定する必要があります。 5.3、動的プロパティはコレクションの値を持つことができます。 たとえば、次の JSON ペイロードで、`Emails`プロパティは動的なプロパティで、文字列型のコレクションです。

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>複合型の継承のサポート

これで複合型は、基本型から継承できます。 たとえば、OData サービスでは、次の複合型を定義できます。

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

この例では、EDM を次に示します。

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

詳細については、次を参照してください。 [OData の複合型継承サンプル](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)します。

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

このセクションでは、既知の問題と ASP.NET Web API OData 5.3 における重大な変更について説明します。

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>Query Options (クエリ オプション)

問題点: $levels と入れ子になった $ を使用して展開し、正しくない展開の深さの最大の結果を = です。

たとえば、次の要求を考えてみます。

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

場合`MaxExpansionDepth`は 5、6 の展開の深さでこのクエリになります。

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>バグの修正と軽微な機能の更新

このリリースではいくつかのバグ修正と軽微な機能も更新します。 完全な一覧をここにあります。

- [バグ修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1

このリリースで行った、[バグを修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All)AllowedFunctions 列挙型の一部にします。 このリリースには、その他のバグ修正や新機能が含まれていません。
