---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: ASP.NET MVC 5.2 の新機能 |Microsoft ドキュメント
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 94f6131fdb86d1694c1f563c5f6806f119c71266
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26504101"
---
<a name="whats-new-in-aspnet-mvc-52"></a>ASP.NET MVC 5.2 の新機能
====================
によって[Microsoft](https://github.com/microsoft)

このトピックでは、ASP.NET MVC 5.2 の新機能について説明[Microsoft.AspNet.MVC 5.2.2](#52)と[ASP.NET MVC 5.2.3 Beta](#mvc523Beta)

- [ソフトウェアの要件](#softRequire)
- [ダウンロード](#download)
- [ドキュメント](#documentation)
- [ASP.NET MVC 5.2 の新機能](#new-features)

    - [属性のルーティングの機能強化](#attributerouting)
- [既知の問題と重大な変更](#knownbreakingchanges)
- [バグの修正](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>ソフトウェア要件

- Visual Studio 2012: ダウンロード[ASP.NET および Web Tools for Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062)です。
- Visual Studio 2013 の場合: ダウンロード[Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064)またはそれ以降。 ASP.NET MVC 5.2 Razor ビューを編集するため、この更新プログラムが必要です。

<a id="download"></a>
## <a name="download"></a>ダウンロード

ランタイムの機能は、NuGet ギャラリー上の NuGet パッケージとしてリリースされています。 次のすべてのランタイム パッケージ、[セマンティック バージョニング](http://semver.org/)仕様です。 ASP.NET MVC 5.2 の最新のパッケージは、次のバージョン:「5.2.0」です。 インストールまたはを介してこれらのパッケージを更新することができます[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)です。 リリースには、NuGet で対応するローカライズ版パッケージも含まれています。

インストールまたは NuGet パッケージ マネージャー コンソールを使用してリリースされた NuGet パッケージを更新できます。

インストール パッケージ Microsoft.AspNet.Mvc-バージョン 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>ドキュメント

チュートリアルと ASP.NET MVC 5.2 に関する他の情報は、ASP.NET web サイトから使用可能な ([https://www.asp.net/mvc](../../index.md))。

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>ASP.NET MVC 5.2 の新機能

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>属性のルーティングの機能強化

属性のようになりましたルーティング属性ルートを検出および構成する方法を完全に制御できる IDirectRouteProvider と呼ばれる機能拡張ポイントを提供します。 アクションおよびアクションのどのようなルーティング構成が必要なだけ指定に関連するルート情報と共にコント ローラーの一覧を提供する場合は、IDirectRouteProvider します。 MapAttributes/MapHttpAttributeRoutes を呼び出すときに、IDirectRouteProvider 実装を指定することがあります。

IDirectRouteProvider のカスタマイズが簡単に、既定の実装、DefaultDirectRouteProvider を拡張することによってです。 このクラスは、属性を検出し、ルート エントリを作成して、ルート プレフィックスおよび領域プレフィックスを検出するためのロジックを変更する別のオーバーライド可能な仮想メソッドを提供します。

新しい属性ルーティングの拡張性と IDirectRouteProvider、ユーザーは、次の操作を行います可能性があります。

1. 属性ルートの継承をサポートします。 たとえば、次のシナリオでブログおよびストアのコント ローラーを使用している、BaseController で定義されている属性のルーティング規約。 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. 属性のルートのルート名を自動的に生成します。 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. ルートは、ルート テーブルに追加される前に、1 か所でルート プレフィックスを変更します。
4. 検索する属性をルーティングするコント ローラーを除外します。 3 と 4 でブログにすぐに予定です。

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook が変更された API サーフェイスでの修正します。

MVC Facebook パッケージ[が解除された](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All)Facebook によって行われたいくつかの API の変更のためです。 新しい Facebook パッケージ (Microsoft.AspNet.Facebook 1.0.0) これらの問題を修正するリリースもします。

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>5.2.0 をプロジェクトに MVC または Web API のスキャフォールディングをプロジェクトに既に存在しないもの 5.1.2 パッケージの結果をパッケージ化

ASP.NET MVC 5.2.0 の NuGet パッケージを更新しても、ASP.NET スキャフォールディングなど Visual Studio のツールや ASP.NET Web アプリケーション プロジェクト テンプレートには更新されません。 ASP.NET ランタイム パッケージ (Update 2 で 5.1.2 など) の以前のバージョンを使用します。 プロジェクトで使用されていない場合に、ASP.NET スキャフォールディングでその結果、必要なパッケージの以前のバージョン (Update 2 で 5.1.2 など) がインストールされますか。 ただし、Visual Studio 2013 RTM または更新プログラム 1 で ASP.NET スキャフォールディングでは、プロジェクト内の最新のパッケージは上書きされません。 Web API 2.2 または ASP.NET MVC 5.2、プロジェクトのパッケージを更新した後に ASP.NET のスキャフォールディングを使用する場合は、Web API と ASP.NET MVC のバージョンでは、一貫性を確認します。

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>JQuery 1.4.1 と互換性のある Microsoft.jQuery.Unobtrusive.Validation のバージョンを検索することはないために、Microsoft.jQuery.Unobtrusive.Validation NuGet パッケージのインストールが失敗しました。

Microsoft.jQuery.Unobtrusive.Validation jQuery が必要です&gt;= 1.8 および jQuery.Validation &gt;1.8 を = です。 But,jQuery.Validation (1.8) には、jQuery が必要があります (&#8805; 1.3.2 &amp; &amp; &#8804;1.6;)。 このため、NuGet は、同時に、JQuery 1.8 と jQuery.Validation 1.8 をインストールすると失敗します。 JQuery.Validation のバージョンを更新できるだけでこの問題を見つけたら、 &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1)固定 jQuery キャップを持つ最初に、ことができますをインストールするにはMicrosoft.jQuery.Unobtrusive.Validation です。

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery です。検証 1.13.0 nuget パッケージのバージョンでは、いくつかの国際対応の電子メール アドレスは認識されません。

jQuery.Validation 1.11.1 nuget パッケージのバージョンは、次の有効な電子メール アドレスが認識する既知の最新バージョンです。 それ以降のバージョンは、認識されるようにすることがない場合があります。 例:

電子メール アドレスの国際化 (EAI) の標準 (例: [&#29992; &#25143;@domain.com](mailto:&#29992;&#25143;@domain.com))   
 EAI + 国際化 Resource Identifier (Iri) (例えば。、 [(& a) #29992; &#25143; @&#1076; (& a) #1086; (& a) #1084; (& a) #1077; (& a) #1085;。&#1088; &#1092;です。](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

問題が報告された[https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Visual Studio 2013 での Razor ビューの構文の強調表示

Visual Studio 2013 を更新せずに ASP.NET MVC 5.2 を更新する場合は、Razor ビューの編集中に構文の強調表示の Visual Studio エディターのサポートを取得するはできません。 このサポートを取得する Visual Studio 2013 を更新する必要があります。

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>バグの修正およびマイナー機能の更新

このリリースではいくつかのバグ修正とマイナー機能も更新します。 完全な一覧は、ここをご覧ください。

- [5.2 パッケージ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2

このリリースでは、MVC での新機能、バグの修正がありません。 行いました、 [Web ページで変更](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx)大幅なパフォーマンス向上のためとおを所有して Web ページのこの新しいバージョンに依存するその他のすべての依存パッケージが、後で更新します。

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 Beta

リリースについて[ここ](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)です。 このリリースには、バグの修正プログラムのみが含まれています。 使用することができます[このクエリ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)今回のリリースで修正された問題の一覧を表示します。
