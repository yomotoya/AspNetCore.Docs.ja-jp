---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: ASP.NET MVC 5.2 の新機能新機能 |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: bcffaa9fddfb13db0b7bd203029e850903a13a54
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389966"
---
<a name="whats-new-in-aspnet-mvc-52"></a>ASP.NET MVC 5.2 の新機能新機能
====================
によって[Microsoft](https://github.com/microsoft)

このトピックでは、ASP.NET MVC 5.2 の新機能新機能について説明[Microsoft.AspNet.MVC 5.2.2](#52)と[ASP.NET MVC 5.2.3 Beta](#mvc523Beta)

- [ソフトウェアの要件](#softRequire)
- [ダウンロード](#download)
- [ドキュメント](#documentation)
- [ASP.NET MVC 5.2 の新機能](#new-features)

    - [属性のルーティングが強化されました](#attributerouting)
- [既知の問題と重大な変更](#knownbreakingchanges)
- [バグの修正](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>ソフトウェア要件

- Visual Studio 2012: ダウンロード[ASP.NET および Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062)します。
- Visual Studio 2013 の場合: ダウンロード[Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064)またはそれ以降。 ASP.NET MVC 5.2 の Razor ビューを編集するためには、この更新プログラムが必要です。

<a id="download"></a>
## <a name="download"></a>ダウンロード

ランタイムの機能は、NuGet ギャラリーでの NuGet パッケージとしてリリースされます。 次のすべてのランタイム パッケージ、[セマンティック バージョニング](http://semver.org/)仕様。 ASP.NET MVC 5.2 の最新のパッケージは、次のバージョン:「5.2.0」。 インストールまたはを通じてこれらのパッケージを更新することができます[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)します。 リリースには、NuGet での対応するローカライズされたパッケージも含まれています。

インストールまたは NuGet パッケージ マネージャー コンソールを使用して、リリースされた NuGet パッケージを更新できます。

インストール パッケージ Microsoft.AspNet.Mvc-バージョン 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>ドキュメント

チュートリアルと ASP.NET MVC 5.2 に関する他の情報は、ASP.NET web サイトから入手できます ([https://www.asp.net/mvc](../../index.md))。

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>ASP.NET MVC 5.2 の新機能

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>属性のルーティングが強化されました

属性ルーティングを今すぐ IDirectRouteProvider で、属性ルートが検出され、構成する方法を完全に制御できますと呼ばれる機能拡張ポイントを提供します。 IDirectRouteProvider はアクションとコント ローラーとアクションのどのようなルーティングの構成が必要なだけを指定する関連付けられたルート情報の一覧を提供します。 MapAttributes/MapHttpAttributeRoutes を呼び出すときに、IDirectRouteProvider 実装を指定することがあります。

IDirectRouteProvider のカスタマイズになります最も簡単な既定の実装、DefaultDirectRouteProvider を拡張することによって。 このクラスは、属性の検出、ルートのエントリを作成するルート プレフィックス領域プレフィックスを検出するためのロジックを変更する別のオーバーライド可能な仮想メソッドを提供します。

新しい属性ルーティングの拡張性と IDirectRouteProvider、ユーザーは、次の方法でした。

1. 属性ルートの継承をサポートします。 たとえば、次のシナリオでブログとストアのコント ローラーを使用している、BaseController で定義されている属性ルート規則。 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. 属性ルートのルート名を自動的に生成します。 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. ルート、ルート テーブルに追加される前に、1 か所でルート プレフィックスを変更します。
4. 検索する属性のルーティング先のコント ローラーを除外します。 お役に 3 および 4 のブログにすぐにします。

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook が変更された API サーフェイスでの修正します。

MVC Facebook パッケージ[が壊れた](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All)Facebook によって行われたいくつかの API の変更が原因です。 新しい Facebook パッケージ (Microsoft.AspNet.Facebook 1.0.0) これらの問題を修正することもリリースします。

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>プロジェクトに 5.2.0 にスキャフォールディング MVC または Web API プロジェクトに既に存在しないものの 5.1.2 で結果をパッケージにパッケージ化

ASP.NET MVC 5.2.0 用 NuGet パッケージを更新しても、ASP.NET スキャフォールディングなどの Visual Studio のツールや、ASP.NET Web アプリケーション プロジェクト テンプレートには更新されません。 ASP.NET ランタイム パッケージ (例: 更新プログラム 2 で 5.1.2) の以前のバージョンを使用します。 その結果、プロジェクトで使用されていない場合は、ASP.NET スキャフォールディングは、必要なパッケージの以前のバージョン (例: 更新プログラム 2 で 5.1.2) がインストールされます。 ただし、Visual Studio 2013 RTM や Update 1 での ASP.NET スキャフォールディングでは、プロジェクトで最新のパッケージは上書きされません。 Web API 2.2 または ASP.NET MVC 5.2 に、プロジェクトのパッケージを更新した後、ASP.NET スキャフォールディングを使用する場合は、Web API と ASP.NET MVC のバージョンでは、一貫性を確認します。

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>JQuery 1.4.1 に互換性のある Microsoft.jQuery.Unobtrusive.Validation のバージョンを検索できないために、Microsoft.jQuery.Unobtrusive.Validation NuGet パッケージのインストールが失敗します。

Microsoft.jQuery.Unobtrusive.Validation jQuery を必要と&gt;= 1.8 および jQuery.Validation &gt;1.8 を = です。 But,jQuery.Validation (1.8) には、jQuery が必要があります (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6)。 このため、NuGet と同時に、JQuery 1.8 および jQuery.Validation 1.8 のインストール時に失敗します。 この問題を確認したら、単に jQuery.Validation のバージョンを更新できます&gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1)固定 jQuery キャップを持つ最初に、おく必要があることをインストールするにはMicrosoft.jQuery.Unobtrusive.Validation します。

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery です。検証 1.13.0 nuget パッケージのバージョンでは、いくつかの国際対応の電子メール アドレスは認識されません。

jQuery.Validation nuget パッケージのバージョン 1.11.1 は、次の有効な電子メール アドレスを認識する既知の最新バージョンです。 それ以降のバージョンはできないことを認識することがあります。 例えば:

電子メール アドレスの国際化 (EAI) の標準 (例: [ &#29992; &#25143; @domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + 国際化されたリソース識別子 (Iri) (例:、 [ &#29992; &#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;。&#1088; 。&#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

問題がレポートされます。 [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Visual Studio 2013 での Razor ビューの構文の強調表示

Visual Studio 2013 を更新することがなく、ASP.NET MVC 5.2 に更新する場合は、Razor ビューの編集中に構文の強調表示の Visual Studio エディターのサポートを取得するはできません。 このサポートを受ける Visual Studio 2013 を更新する必要があります。

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>バグの修正と軽微な機能の更新

このリリースではいくつかのバグ修正と軽微な機能も更新します。 完全な一覧をここにあります。

- [5.2 パッケージ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2

このリリースでは、その新しい機能やバグ修正を mvc がありません。 行いました、 [Web ページで変更](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx)大幅なパフォーマンス向上が Web ページのこの新しいバージョンに依存する私たちが所有の他のすべての従属パッケージを更新して、その後、します。

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 Beta

リリースについて[ここ](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)します。 このリリースには、バグの修正プログラムのみが含まれています。 使用することができます[このクエリ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)をこのリリースで修正された問題の一覧を参照してください。
