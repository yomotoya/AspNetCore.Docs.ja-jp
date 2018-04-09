---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: プロファイルし、Glimpse による ASP.NET MVC アプリのデバッグ |Microsoft ドキュメント
author: Rick-Anderson
description: Glimpse は、成功し、詳細なパフォーマンスを提供する NuGet パッケージのオープン ソースのファミリを拡大して、デバッグ、および ASP.NET の診断情報を.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 6ac23256c57116de81c7bf690d5ce743301c75ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>プロファイルし、Glimpse による ASP.NET MVC アプリのデバッグ
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> Glimpse は、成功し、詳細なパフォーマンスを提供する NuGet パッケージのオープン ソースのファミリを拡大して、デバッグ、および ASP.NET アプリ用の診断情報。 インストールする自明の軽量の超高速と、すべてのページの下部にある主要なパフォーマンス メトリックを表示します。 サーバーで何が起こってを確認する必要がある場合、アプリにドリル ダウンすることができます。 Glimpse は、Azure のテスト環境など、開発サイクル全体で使用することをお勧めほどの貴重な情報を提供します。 中に[Fiddler](http://www.telerik.com/fiddler)と[F-12 開発ツール](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx)クライアント側の提供ビュー、Glimpse がサーバーから詳細ビューを提供します。 Glimpse ASP.NET MVC と EF パッケージを使用してではこのチュートリアルに焦点が、その他の多くのパッケージを利用できます。 適切なリンクが可能な限り[皮下 docs](http://getglimpse.com/Docs/)のためを維持します。 Glimpse はオープン ソース プロジェクトは、ソース コードとドキュメントにも寄与できます。


- [Glimpse のインストール](#ig)
- [Localhost の流れを有効にします。](#eg)
- [[タイムライン] タブ](#Time)
- [モデル バインド](#mb)
- [ルート](#route)
- [Azure で Glimpse の使用](#da)
- [その他のリソース](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Glimpse のインストール

Glimpse をインストールするには、NuGet パッケージ マネージャー コンソールから、またはから、 **NuGet パッケージの管理**コンソールです。 このデモでは、Mvc5 と EF6 パッケージをインストールします。

![NuGet の追加 ダイアログから Glimpse をインストールします。](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

検索*Glimpse.EF*

![NuGet のインストールの追加 ダイアログから Glimpse.EF](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

選択して**インストールされているパッケージ**、インストールされている Glimpse 依存するモジュールを確認できます。

![追加 ダイアログから Glimpse パッケージをインストール](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

次のコマンドは、パッケージ マネージャー コンソールから Glimpse MVC5 および EF6 モジュールをインストールします。

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Localhost の流れを有効にします。

移動http://localhost:&lt;ポート番号&gt;/glimpse.axd をクリックして、 <strong>Glimpse をオンにする</strong>ボタンをクリックします。

![Glimpse axd ページ](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

お気に入りバーの表示があれば、ドラッグ Glimpse ボタンのドロップし bookmarklets として追加およびできます。

![Glimpse boookmarklets で IE](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

今すぐアプリを移動することができます、**ヘッドを表示**(HUD) は、ページの下部に表示します。

![HUD に連絡先のマネージャー ページ](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

[Glimpse HUD ページ](http://getglimpse.com/Docs/Heads-up-Display)上に示したタイミング情報を詳しく説明します。 控えめなパフォーマンス データ HUD 表示に通知できます問題を直ちに - テスト サイクルを始める前に、。 クリックすると、 &quot;g&quot;右下隅で Glimpse パネルを表示します。

![Glimpse パネル](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

、上記の図に、[実行 タブに](http://getglimpse.com/Docs/Execution-Tab)を選択すると、パイプライン内のアクションとフィルターのタイミングの詳細が示されます。 確認できます my[停止ウォッチ フィルター タイマー](http://www.nuget.org/packages/StopWatch/)パイプラインのステージ 6 から開始します。 My 軽量タイマーを提供できるプロファイル/タイミング データを便利に、認証やビューの表示のすべての時間はすべてミスします。 自分のタイマーを知っておくことができます[をプロファイルし、Azure に ASP.NET MVC アプリの時間](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx)です。 [タブ](http://getglimpse.com/Docs/Tabs)ページは、各タブで詳細情報へのリンクを提供します。

<a id="Time"></a>
## <a name="the-timeline-tab"></a>[タイムライン] タブ

Tom Dykstra を変更しました未処理[EF 6/MVC 5 のチュートリアル](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)講習においてインストラクター コント ローラーを次のコードに変更します。

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

上記のコードを渡すクエリ文字列に使用する (`eager`) データの読み込み eager または明示的なを制御します。 次の図では明示的な読み込みを使用し、タイミング ページに読み込まれている各登録を示しています、`Index`アクション メソッド。

![明示的な読み込み](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

次のコードで eager が指定され、各登録が後にフェッチ、`Index`ビューが呼び出されます。

![eager を指定します。](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

詳細なタイミング情報を取得する時間のセグメントを合わせることができます。

![ホバー時に詳細なタイミングを参照してください。](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>モデル バインド

[モデル バインド タブ](http://getglimpse.com/Docs/Model-Binding-Tab)豊富なフォーム変数のバインド方法と理由一部がバインドされていないはずの理解に役立つ情報を提供します。 次のイメージ、**しますか?** アイコンをクリックすると、glimpse のヘルプ ページで、その機能を表示します。

![glimpse モデルのビューをバインド](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>ルート

 デバッグおよびルーティングを理解する Glimpse ルート タブが役立つことができます。 次の図で製品のルートが選択されている (およびそれ緑、Glimpse 規約で表示)。 ![選択した製品名](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png)ルート制約、領域およびデータのトークンも表示されます。 参照してください[Glimpse ルート](http://getglimpse.com/Docs/Routes-Tab)と[属性は、ASP.NET MVC 5 でルーティング](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx)詳細についてはします。 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Azure で Glimpse の使用

Glimpse の既定のセキュリティ ポリシーは、ローカル ホストから表示される Glimpse データだけを許可します。 (Azure の web アプリ) などのリモート サーバーでこのデータを表示できるように、このセキュリティ ポリシーを変更できます。 Azure でのテスト環境、追加の下部にある最大の強調表示されている、マーク、 *web.confg* Glimpse を有効にするファイル。

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

この変更によりだけで、すべてのユーザーは、リモート サイトに Glimpse データを表示できます。 のみ展開、適用されている発行プロファイル (など、Azure テスト proifle。) を使用する場合は、発行プロファイルを上マークアップを追加します。Glimpse データを制限するのには追加、`canViewGlimpseData`ロール Glimpse データを表示するには、このロールのユーザーのみを許可するとします。

コメントを削除、 *GlimpseSecurityPolicy.cs*ファイルし、変更、 [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx)から呼び出す`Administrator`を`canViewGlimpseData`ロール。

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> セキュリティ - Glimpse によって提供される豊富なデータには、アプリのセキュリティでした公開します。 Microsoft では、運用アプリで使用する Glimpse のセキュリティ監査が実行されません。


ロールを追加する方法については、次を参照してください。 [メンバーシップ、OAuth、SQL データベースで ASP.NET MVC 5 のセキュリティで保護された web アプリケーションを Azure にデプロイ](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)チュートリアルです。

<a id="addRes"></a>
## <a name="additional-resources"></a>その他のリソース

- [Azure へのメンバーシップ、OAuth、および SQL データベースでのセキュリティで保護された ASP.NET MVC 5 アプリを配置します。](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [構成を皮下](http://getglimpse.com/Docs/Configuration)-ドキュメント ページ タブ、実行時ポリシー、ログ記録の構成にします。
