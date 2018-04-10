---
uid: mvc/overview/releases/mvc51-release-notes
title: ASP.NET MVC 5.1 の新機能 |Microsoft ドキュメント
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
---
<a name="whats-new-in-aspnet-mvc-51"></a>ASP.NET MVC 5.1 の新機能
====================
によって[Microsoft](https://github.com/microsoft)

このトピックでは、ASP.NET Web MVC 5.1 の新機能について説明します。

- [ソフトウェアの要件](#SoftwareRequirements)
- [ダウンロード](#download)
- [ドキュメント](#documentation)
- [ASP.NET MVC 5.1 の新機能](#new-features)

    - [属性のルーティングの機能強化](#AttributeRouting)
    - [エディター テンプレートのブートス トラップのサポート](#Bootstrap)
    - [ビューで列挙型のサポート](#Enum)
    - [Minlength/maxlength 属性の控えめな検証](#Unobtrusive)
    - [控えめな Ajax での 'this' コンテキストのサポート](#thisContext)
- [既知の問題と重大な変更](#KnownBreakingChanges)- [バグの修正](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>ソフトウェア要件

- Visual Studio 2012: ダウンロード[ASP.NET および Web Tools for Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062)です。
- Visual Studio 2013 の場合: ダウンロード[Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064)です。 ASP.NET MVC 5.1 の Razor ビューを編集するため、この更新プログラムが必要です。

<a id="download"></a>
## <a name="download"></a>ダウンロード

ランタイムの機能は、NuGet ギャラリー上の NuGet パッケージとしてリリースされています。 次のすべてのランタイム パッケージ、[セマンティック バージョニング](http://semver.org/)仕様です。 ASP.NET MVC 5.1 の RTM の最新のパッケージは、次のバージョン:「5.1.2」です。 インストールまたはを介してこれらのパッケージを更新することができます[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)です。 リリースには、NuGet で対応するローカライズ版パッケージも含まれています。

インストールまたは NuGet パッケージ マネージャー コンソールを使用してリリースされた NuGet パッケージを更新できます。

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>ドキュメント

チュートリアルと ASP.NET MVC 5.1 の RTM に関する他の情報は、ASP.NET web サイトから使用可能な (https://www.asp.net)です。 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>ASP.NET MVC 5.1 の新機能

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>属性のルーティングの機能強化

 ルートの選択をサポートの制約、バージョン管理とヘッダーの有効化のルーティング今すぐ属性に基づいています。 属性ルートの多くの側面は経由でカスタマイズ可能なようになりました、`IDirectRouteFactory`インターフェイスと`RouteFactoryAttribute`クラスです。 ルート プレフィックスが経由で拡張可能なようになりました、`IRoutePrefix`インターフェイスと`RoutePrefixAttribute`クラスです。 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>ビューで列挙型のサポート

1. 新しい`@Html.EnumDropDownListFor()`ヘルパー メソッドです。 これらは、ほとんどの式を評価することに注意して HTML ヘルパーのように使用する必要があります、 [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx)型または[Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx)場所`T`は、[enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx)型です。 使用して`EnumHelper.IsValidForEnumHelper()`をこれらの要件を確認します。
2. 新しい`EnumHelper.GetSelectList()`を返すメソッド、`IList<SelectListItem>`です。 これは、呼び出しは、たとえば、前に、選択リストを操作する必要がある場合に便利です`@Html.DropDownListFor()`、名前を表示する場合、またはを`@Html.EnumDropDownListFor()`を示しています。

次のコードでは、これらの Api を示します。

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

完全な例を確認できます[ここ](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/)です。

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>エディター テンプレートのブートス トラップのサポート

HTML 属性で渡すことを許可おようになりました[EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx)として、[匿名オブジェクト](https://msdn.microsoft.com/en-us/library/bb397696.aspx)です。

例えば:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>MinLengthAttribute および MaxLengthAttribute の控えめな検証

修飾されたプロパティの文字列および配列型のクライアント側の検証をサポートようになりましたが、 [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx)と[MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx)属性。

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>控えめな Ajax での 'this' コンテキストのサポート

コールバック関数 (`OnBegin, OnComplete, OnFailure, OnSuccess`) を使用して呼び出し元の要素を検索することができます、`this`コンテキスト。 例えば:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

### <a name="attribute-routing"></a>属性のルーティング

最初の一致を選択するのではなく、エラー、属性のルーティングの一致のあいまいさをレポートします。

属性ルートが使用を禁止して、`{controller}`パラメーターを使用してと、`{action}`ルートのパラメーターは、アクションに配置します。 これらのパラメーターの使用はあいまいになる非常に可能性あります。 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>5.0 パッケージをプロジェクトに既に存在しないもので 5.1 パッケージの結果を含むプロジェクトに MVC または Web API のスキャフォールディング

ASP.NET MVC 5.1 の RTM の NuGet パッケージを更新しても、ASP.NET スキャフォールディングなど Visual Studio のツールや ASP.NET Web アプリケーション プロジェクト テンプレートには更新されません。 ASP.NET ランタイム パッケージ (5.0.0.0) の以前のバージョンを使用します。 プロジェクトで使用されていない場合に、ASP.NET スキャフォールディングでその結果、必要なパッケージの以前のバージョン (5.0.0.0) がインストールされますか。 ただし、Visual Studio 2013 RTM または更新プログラム 1 で ASP.NET スキャフォールディングでは、プロジェクト内の最新のパッケージは上書きされません。 Web API 2.1 または ASP.NET MVC 5.1 に、プロジェクトのパッケージを更新した後に ASP.NET のスキャフォールディングを使用する場合は、Web API と ASP.NET MVC のバージョンでは、一貫性を確認します。 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Visual Studio 2013 での Razor ビューの構文の強調表示

Visual Studio 2013 を更新せずに ASP.NET MVC 5.1 の RTM を更新する場合は、Razor ビューの編集中に構文の強調表示の Visual Studio エディターのサポートを取得するはできません。 このサポートを取得する Visual Studio 2013 を更新する必要があります。 

### <a name="type-renames"></a>型名の変更

5.1 RTM で一部の属性のルーティング拡張機能を使用する型の名前が変更されます。

| **古い型の名前 (5.1 RC)** | **新しい型名 (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>バグ修正

このリリースでは、いくつかのバグ修正も含まれます。 完全な一覧は、ここをご覧ください。

- [5.1.0 パッケージ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 パッケージ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

5.1.2 パッケージには、IntelliSense の更新がないバグの修正が含まれています。
