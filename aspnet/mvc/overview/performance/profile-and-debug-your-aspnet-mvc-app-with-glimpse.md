---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: プロファイルし、Glimpse による ASP.NET MVC アプリのデバッグ |Microsoft Docs
author: Rick-Anderson
description: Glimpse に関する情報は盛況と詳細なパフォーマンスを提供するオープン ソースの NuGet パッケージのファミリを増加して、デバッグおよび ASP.NET 用の診断情報をしています.
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 94a72f22cbcd7fa84528dde502cceaa1e26dcaa1
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577289"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>プロファイルし、Glimpse による ASP.NET MVC アプリのデバッグ
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

> Glimpse に関する情報は盛況し詳細なパフォーマンスを提供するオープン ソースの NuGet パッケージのファミリを拡大し、デバッグ、および ASP.NET アプリの診断情報。 インストールする単純な軽量で、超高速には、すべてのページの下部にある主要なパフォーマンス メトリックを表示します。 サーバーで起こっていることを確認する必要がある場合、アプリにドリルダウンすることができます。 Glimpse に関する情報など、Azure テスト環境、開発サイクル全体で使用することをお勧めします。 非常に貴重な情報を提供します。 中に[Fiddler](http://www.telerik.com/fiddler)と[F-12 開発ツール](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx)クライアント側の提供ビュー、glimpse に関する情報がサーバーから詳細ビューを提供します。 Glimpse ASP.NET MVC と EF のパッケージを使用してこのチュートリアルを取り上げますが、その他の多くのパッケージを使用できます。 適切なリンクが可能な限り[Glimpse の docs](http://getglimpse.com/Docs/)を維持するためです。 Glimpse に関する情報は、オープン ソース プロジェクト、ソース コードとドキュメントにも寄与できます。


- [Glimpse に関する情報をインストールします。](#ig)
- [Localhost の glimpse に関する情報を有効にします。](#eg)
- [[タイムライン] タブ](#Time)
- [モデル バインド](#mb)
- [Routes](#route)
- [Glimpse に関する情報を使用して Azure に](#da)
- [その他のリソース](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Glimpse に関する情報をインストールします。

Glimpse に関する情報をインストールするには、NuGet パッケージ マネージャー コンソールから、またはから、 **NuGet パッケージの管理**コンソール。 このデモでは、Mvc5 と EF6 のパッケージをインストールします。

![NuGet Dlg から glimpse に関する情報をインストールします。](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

検索*Glimpse.EF*

![NuGet のインストール ダイアログから Glimpse.EF](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

選択して**パッケージがインストールされている**、インストールされている Glimpse 依存モジュールを確認できます。

![メニューから Glimpse パッケージをインストールします。](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

次のコマンドは、パッケージ マネージャー コンソールから Glimpse MVC5 と EF6 のモジュールをインストールします。

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Localhost の glimpse に関する情報を有効にします。

移動します http://localhost:&lt; ポート番号&gt;/glimpse.axd し、 <strong>glimpse に関する情報をオンにする</strong>ボタンをクリックします。

![Glimpse axd ページ](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

お気に入りバーの表示があれば、ドラッグ Glimpse ボタンをドロップし bookmarklets として追加およびできます。

![Glimpse boookmarklets で IE](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

アプリを移動できるようになりました、**ヘッドを表示**(HUD) は、ページの下部に表示されます。

![HUD を連絡先マネージャー ページ](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

[Glimpse HUD ページ](http://getglimpse.com/Docs/Heads-up-Display)詳細の上に示したタイミング情報。 控えめなパフォーマンスの HUD のデータが表示されますの通知も問題にすぐにテスト サイクルに到達する前にします。 クリックすると、 &quot;g&quot;右下隅で Glimpse パネルが表示されます。

![Glimpse パネル](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

上の図で、[実行タブ](http://getglimpse.com/Docs/Execution-Tab)が選択されているパイプラインは、フィルター、アクションのタイミングの詳細に表示します。 確認できます、[停止ウォッチ フィルター タイマー](http://www.nuget.org/packages/StopWatch/)パイプラインのステージ 6 から開始します。 私の軽量のタイマーを確保できますで承認やビューの表示のすべての時間がないデータのプロファイル/タイミングに役立ちます。 については、タイマーを[プロファイルし、Azure に ASP.NET MVC アプリの時刻](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx)します。 [タブ](http://getglimpse.com/Docs/Tabs)ページは、各タブで詳細な情報へのリンクを提供します。

<a id="Time"></a>
## <a name="the-timeline-tab"></a>[タイムライン] タブ

Tom Dykstra の変更しました未処理[EF 6 と MVC 5 のチュートリアル](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)instructors コント ローラーを次のコードに変更します。

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

上記のコードを渡すクエリ文字列にできます (`eager`) eager または明示的なコントロールにデータの読み込み。 明示的読み込みを使用する次の図でとタイミングのページに読み込まれた各登録が表示されます、`Index`アクション メソッド。

![明示的な読み込み](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

次のコードで eager が指定され、各登録のフェッチ後に、`Index`ビューが呼び出されます。

![eager を指定します。](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

詳細なタイミング情報を取得する時間のセグメントを合わせることができます。

![自動的に詳細なタイミングを参照してください。](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>モデル バインド

[モデル バインド タブ](http://getglimpse.com/Docs/Model-Binding-Tab)豊富なフォーム変数をバインドする方法と理由一部がバインドされていないの予想どおりの理解に役立つ情報を提供します。 次のイメージ、**でしょうか。** アイコンをクリックすると glimpse に関する情報のヘルプ ページ機能を構成します。

![glimpse のモデル バインド ビュー](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>ルート

 デバッグおよびルーティングを理解する Glimpse ルート タブが役立つことができます。 次の図で製品のルートが選択されている (および緑、Glimpse 規則に表示されます)。 ![選択した製品名](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png)ルート制約、領域およびデータのトークンも表示されます。 参照してください[Glimpse ルート](http://getglimpse.com/Docs/Routes-Tab)と[属性は、ASP.NET MVC 5 でルーティング](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx)詳細についてはします。 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Glimpse に関する情報を使用して Azure に

Glimpse の既定のセキュリティ ポリシーは、ローカル ホストから表示される概要データのみを許可します。 (Azure の web アプリ) などのリモート サーバーでこのデータを表示できるように、このセキュリティ ポリシーを変更できます。 Azure でのテスト環境、最大で一番下の強調表示されているマークの追加、 *web.confg* glimpse に関する情報を有効にするファイル。

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

だけでこの変更により、任意のユーザーはリモート サイトに Glimpse データを表示できます。 のみ展開を適用するときにその発行プロファイル (たとえば、Azure テスト proifle。)、発行プロファイルに上記のマークアップを追加することを検討してください。Glimpse のデータを制限するのには追加、`canViewGlimpseData`ロールと glimpse に関する情報のデータを表示するには、このロールのユーザーのみを許可します。

コメントを削除、 *GlimpseSecurityPolicy.cs*ファイルし、変更、 [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx)から呼び出す`Administrator`を`canViewGlimpseData`ロール。

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> セキュリティ - glimpse に関する情報が提供する豊富なデータは、アプリのセキュリティを公開する可能性があります。 Microsoft では、運用アプリで使用するため glimpse に関する情報のセキュリティ監査が実行されません。


役割を追加する方法の詳細については、次を参照してください。 マイ[メンバーシップ、OAuth、SQL Database を使用した ASP.NET MVC 5 のセキュリティで保護された web アプリを Azure にデプロイ](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)チュートリアル。

<a id="addRes"></a>
## <a name="additional-resources"></a>その他のリソース

- [メンバーシップ、OAuth、SQL Database を使用した安全な ASP.NET MVC 5 アプリを Azure にデプロイします。](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Glimpse の構成](http://getglimpse.com/Docs/Configuration)-ドキュメント ページのタブ、実行時ポリシー、ログ記録を構成します。
