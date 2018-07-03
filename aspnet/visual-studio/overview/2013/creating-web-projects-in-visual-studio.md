---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Visual Studio 2013 での ASP.NET Web プロジェクトを作成する |Microsoft Docs
author: tdykstra
description: このトピックでは、Visual Studio 2013 with Update 3 は、ここでの ASP.NET web プロジェクトを作成するためのオプションは web 開発の c の新機能の一部について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 6033163b7b8e9e1d0524935217dc53f938470cb6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363469"
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Visual Studio 2013 で ASP.NET Web プロジェクトの作成
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このトピックでは、Visual Studio 2013 with Update 3 での ASP.NET web プロジェクトを作成するためのオプションを説明します。
> 
> Visual Studio の以前のバージョンと比較した web 開発の新機能の一部を次に示します。
> 
> - 作成するための単純な UI がそのオファーをプロジェクト[複数の ASP.NET フレームワークのサポート](#add)(Web フォーム、MVC、および Web API)。
> - [ASP.NET Identity](#indauth)、IIS 以外のソフトウェアをホストしている web を使用したすべての ASP.NET フレームワークと動作で同様に動作する新しい ASP.NET メンバーシップ システムです。
> - 使用[ブートス トラップ](#bootstrap)レスポンシブ デザインおよびテーマの適用の機能を提供します。
> - 新機能など、MVC のみに対して提供するために使用するフォームが Web[自動テスト プロジェクトの作成](#testproj)と[イントラネット サイト テンプレート](#winauth)します。
> 
> Azure Cloud Services や Azure Mobile Services の web プロジェクトを作成する方法については、次を参照してください[Azure Cloud Services と ASP.NET の概要](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/)と[Azure Mobile Services .NET を使用したランキング アプリケーションの作成。バックエンド](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)します。


<a id="prerequisites"></a>
## <a name="prerequisites"></a>必須コンポーネント

この記事は[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)で[Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409)をインストールします。

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Web アプリケーション プロジェクトと web サイト プロジェクト

ASP.NET では、web プロジェクトの 2 つの種類の選択: *web アプリケーション プロジェクト*と*web サイト プロジェクト*します。 新たに開発、web アプリケーション プロジェクトをお勧めして、この記事では、web アプリケーション プロジェクトにのみ適用されます。 詳細については、次を参照してください。 [Web アプリケーション プロジェクトと Visual Studio での Web サイト プロジェクト](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx)MSDN サイト。

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Web アプリケーション プロジェクトの作成の概要

次の手順では、web プロジェクトを作成する方法を示します。

1. クリックして**新しいプロジェクト**で、**開始**ページまたは、**ファイル**メニュー。
2. **新しいプロジェクト**ダイアログ ボックスで、をクリックして**Web**左側のウィンドウで、 **ASP.NET Web アプリケーション**中央のペインで。

    ![[新しいプロジェクト] ダイアログ](creating-web-projects-in-visual-studio/_static/image1.png)

    選択することができます**クラウド**を作成するには、左側のウィンドウで、 [Azure クラウド サービス](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)、 [Azure モバイル サービス](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx)、または[Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs)します。 このトピックでこれらのテンプレートについては説明しません。
3. 右側のウィンドウでをクリックして、**プロジェクトに Application Insights の追加**チェック ボックスをする場合、正常性と、アプリケーションの使用状況の監視します。 詳細については、次を参照してください。 [web アプリケーションのパフォーマンスを監視](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/)します。
4. プロジェクトを指定**名**、**場所**、およびその他のオプション、および順にクリックします**OK**します。

    **新しい ASP.NET プロジェクト**ダイアログが表示されます。

    ![[新しいプロジェクト] ダイアログ](creating-web-projects-in-visual-studio/_static/image2.png)
5. [テンプレート] をクリックします。

    ![テンプレートを選択します。](creating-web-projects-in-visual-studio/_static/image3.png)
6. テンプレートに含まれていない追加のフレームワークのサポートを追加する場合は、適切なチェック ボックスをクリックします。 (で示した例で追加できます MVC または Web API、Web フォーム プロジェクトにします。)

    ![フレームワークを追加します。](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>単体テスト プロジェクトを追加する場合は、クリックして**単体テストを追加**します。

    ![単体テストを追加します。](creating-web-projects-in-visual-studio/_static/image5.png)
8. 既定で提供するテンプレートよりも、別の認証方法を実行する場合に、クリックして**認証の変更**します。

    ![[認証] を構成します。](creating-web-projects-in-visual-studio/_static/image6.png)

    ![認証ダイアログ ボックスを構成します。](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Azure で web アプリまたは仮想マシンを作成します。

Visual Studio には、web アプリケーションをホストするための Azure サービスを簡単に処理する機能が含まれています。 たとえばは次のすべてを Visual Studio IDE から直接実行できます。

- 作成し、web アプリまたはインターネット経由でアプリケーションを利用できるようにする仮想マシンを管理します。
- クラウドで実行するときに、アプリケーションで作成されたログを表示します。
- アプリケーションをクラウドで実行中にリモートでデバッグ モードで実行します。
- Viiew および SQL データベースなどの他の Azure サービスを管理します。

できます[Azure アカウントの作成](https://www.windowsazure.com/pricing/free-trial/)を無料で web apps などの基本的なサービスを含むし、MSDN サブスクライバーの場合は、[特典](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/)に向かって追加の Azure クレジットを提供します。サービス。 

既定では、**新しい ASP.NET プロジェクト** ダイアログ ボックスでは、web アプリまたは新しい web プロジェクト用の仮想マシンを作成することができます。 新しい web アプリまたは仮想マシンを作成しない場合は、オフ、**クラウドでホスト**チェック ボックスをオンします。

![リモート リソースを作成します。](creating-web-projects-in-visual-studio/_static/image8.png)

チェック ボックスのキャプションがあります**クラウドでホスト**または**リモート リソースを作成する**、いずれの場合も、効果は同じです。 チェック ボックスをオンにした場合は、Visual Studio は、既定では Azure App Service で web アプリを作成します。 変更する、ドロップ ダウン ボックスを使用することができます、**仮想マシン**したい場合。 されていない Azure にサインインしている場合は、Azure 資格情報のことをお求めです。 サインインした後、ダイアログ ボックスでは、Visual Studio は、プロジェクトを作成するリソースを構成することができます。 次の図は、web アプリのダイアログ ボックス仮想マシンを作成する場合、さまざまなオプションが表示されます。

![Azure アプリ設定を構成します。](creating-web-projects-in-visual-studio/_static/image9.png)

Azure リソースを作成するため、このプロセスを使用する方法の詳細については、次を参照してください。 [Azure と ASP.NET の概要](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)と[Visual Studio を使用した web サイトの仮想マシンを作成する](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/)します。

この記事の残りの部分では、使用可能なテンプレートとオプションについて詳しく説明します。 記事には、テンプレートで使用されるブートス トラップ、レイアウトとテーマのフレームワークも導入されています。

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013 Web プロジェクト テンプレート

Visual Studio では、テンプレートを使用して、web プロジェクトを作成します。 プロジェクト テンプレートは新しいプロジェクトでファイルとフォルダーを作成、NuGet パッケージのインストールおよび基本的な実用アプリケーションのサンプル コードを提供します。 テンプレートは、最新の web 標準を実装し、ジャンプを出すだけでなく、ASP.NET のテクノロジを使用する方法のベスト プラクティスを独自のアプリケーションの作成の開始を示すために対象としています。

Visual Studio 2013 では、.NET 4.5 または .NET framework の以降のバージョンを対象とするプロジェクトの web プロジェクト テンプレートを次の選択肢を提供します。

- [空のテンプレート](#empty)
- [Web フォーム テンプレート](#wf)
- [MVC テンプレート](#mvc)
- [Web API テンプレート](#webapi)
- [シングル ページ アプリケーション テンプレート](#spa)
- [Azure モバイル サービス テンプレート](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Visual Studio 2012 のテンプレート](#vs2012)

提供する Visual Studio 拡張機能をインストールすることも、 [Facebook テンプレート](#facebook)します。

.NET 4 を対象とするプロジェクトを作成する方法については、次を参照してください。 [Visual Studio 2012 のテンプレート](#vs2012)このトピックで後述します。

モバイル クライアント向けの ASP.NET アプリケーションを作成する方法については、次を参照してください。 [ASP.NET でのモバイル サポート](../../../mobile/index.md)します。

<a id="empty"></a>
### <a name="empty-template"></a>空のテンプレート

空のテンプレートには、ベア最小フォルダーとプロジェクト ファイルなど、ASP.NET web アプリのファイルが用意されています (*.csproj*または *。vbproj*) と*Web.config*ファイル。 Web フォーム、MVC、または Web API のサポートを追加するには、下のチェック ボックスを使用して、**フォルダーを追加し、コアの参照:** ラベル。

空のテンプレートの認証オプションはありません。 認証機能は、サンプル アプリケーションで実装され、空のテンプレートは、サンプル アプリケーションを作成できません。

<a id="wf"></a>
### <a name="web-forms-template"></a>Web フォーム テンプレート

フレームワークは、UI とデータの豊富な web サイトの構築に迅速にすることができます、次の機能を提供する Web フォームの機能にアクセスします。

- Visual Studio での WYSIWYG デザイナー。
- HTML とプロパティとスタイルを設定してカスタマイズできるサーバー コントロール。
- データのアクセスとデータの表示コントロールを豊富に備えています。
- WPF などのクライアント アプリケーションのプログラムを作成するようにプログラミングできるイベントを公開するイベント モデル。
- HTTP 要求の間の状態 (データ) の自動保存します。

一般に、Web フォーム アプリケーションを作成するには、ASP.NET MVC フレームワークを使用して、同じアプリケーションを作成するよりも少ないプログラミングの労力が必要です。 ただし、Web フォームだけではなくアプリケーションの迅速な開発です。 多くの複雑な商用アプリケーションと Web フォーム上に構築されたフレームワークがあります。

Web フォーム ページと、ページ上のコントロールは、ブラウザーに送信されるマークアップの大部分に自動的に生成、ため、ASP.NET MVC を提供する HTML をきめ細かく制御の種類がありません。 ページおよびコントロールを構成するための宣言型モデルには、記述する必要しますが、HTML と HTTP の動作の一部を非表示にするコードの量が最小限に抑えます。 たとえば、ことはできません常に正確にどのようなマークアップは、コントロールによって生成されることを指定します。

Web フォーム フレームワークは ASP.NET MVC、すぐに適してパターン ベースの開発プラクティスなど[テスト駆動開発](http://en.wikipedia.org/wiki/Test-driven_development)、[懸念事項の分離](http://en.wikipedia.org/wiki/Separation_of_concerns)、[の反転コントロール](http://en.wikipedia.org/wiki/Inversion_of_control)、および[依存関係の注入](http://en.wikipedia.org/wiki/Dependency_injection)します。 コード ファクタリングする方法を記述する場合は、以下の操作を実行できます。ASP.NET MVC フレームワークであるため、自動がないだけです。 [ASP.NET Web フォームの MVP](http://webformsmvp.com/)プロジェクトを Web フォームの配信が組み込まれている迅速な開発を維持しながらの問題とテストの容易性の分離を容易にする方法を示しています。 Microsoft SharePoint は、Web フォームの MVP に基づいて構築されます。

Web フォーム テンプレートを使用するサンプル Web フォーム アプリケーションを作成[ブートス トラップ](#bootstrap)レスポンシブ デザインおよびテーマの適用の機能を提供します。 次の図は、ホーム ページを示します。

![Web フォーム テンプレート アプリのホーム ページ](creating-web-projects-in-visual-studio/_static/image10.png)

Web フォームの詳細については、次を参照してください。 [ASP.NET Web フォーム](https://asp.net/web-forms)します。 Web フォーム テンプレートが何をする方法の詳細については、次を参照してください。 [Visual Studio 2013 を使用して基本的な Web フォーム アプリケーションを構築](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx)します。

<a id="mvc"></a>
### <a name="mvc-template"></a>MVC テンプレート

ASP.NET MVC は次のようなどのパターン ベースの開発手法を容易に設計された[テスト駆動開発](http://en.wikipedia.org/wiki/Test-driven_development)、[懸念事項の分離](http://en.wikipedia.org/wiki/Separation_of_concerns)、[の制御の反転](http://en.wikipedia.org/wiki/Inversion_of_control)、[依存関係の注入](http://en.wikipedia.org/wiki/Dependency_injection)します。 フレームワークは、そのプレゼンテーション層から web アプリケーションのビジネス ロジック層を分離することをお勧めします。 モデル (M)、ビュー (V)、およびコント ローラー (C) にアプリケーションを分割して ASP.NET MVC やすく大規模なアプリケーションの複雑さを管理します。

ASP.NET mvc、HTML および Web フォームでよりも HTTP をより直接操作することにします。 たとえば、Web フォームでは、HTTP 要求の間で状態を維持できます自動的が MVC でコードを明示的にする必要があります。 MVC モデルの利点は、正確に、アプリケーションの実行内容と、web 環境での動作を完全に制御できます。 欠点より多くのコードを記述する必要があります。

MVC は、電源の開発者のニーズにアプリケーション フレームワークをカスタマイズする機能を提供する、拡張できるように設計されています。 さらに、ASP.NET MVC のソース コードは、OSI license で入手できます。

MVC テンプレートを使用するサンプルの MVC 5 アプリケーションを作成します[ブートス トラップ](#bootstrap)レスポンシブ デザインおよびテーマの適用の機能を提供します。 次の図は、ホーム ページを示します。

![MVC サンプル アプリケーション](creating-web-projects-in-visual-studio/_static/image11.png)

MVC の詳細については、次を参照してください。 [ASP.NET MVC](https://asp.net/mvc)します。 MVC 4 テンプレートを選択する方法については、次を参照してください。 [Visual Studio 2012 テンプレート](#vs2012)この記事で後述します。

<a id="webapi"></a>
### <a name="web-api-template"></a>Web API テンプレート

Web API テンプレートでは、MVC に基づいた API ヘルプ ページを含む、Web API に基づくサンプル web サービスを作成します。

ASP.NET Web API をさまざまなブラウザーやモバイル デバイスなどのクライアントに提供される HTTP サービスを構築するが容易にするフレームワークです。 ASP.NET Web API は、.NET framework RESTful サービスを作成するための理想的なプラットフォームです。

Web API テンプレートでは、サンプル web サービスを作成します。 次の図は、サンプルのヘルプ ページを示しています。

![Web API ヘルプ ページ](creating-web-projects-in-visual-studio/_static/image12.png)

![Web API のヘルプ ページの取得 API を](creating-web-projects-in-visual-studio/_static/image13.png)

Web API の詳細については、次を参照してください。 [ASP.NET Web API](https://asp.net/web-api)します。

<a id="spa"></a>
### <a name="single-page-application-template"></a>単一ページ アプリケーション テンプレート

Single Page Application (SPA) テンプレート、HTML 5、JavaScript を使用するサンプル アプリケーションを作成し、 [KnockoutJS](http://knockoutjs.com/)クライアント、およびサーバー上の ASP.NET Web API。

SPA テンプレートの唯一の認証オプションが[個々 のユーザー アカウント](#indauth)します。

次の図は、SPA テンプレートをビルドするサンプル アプリケーションの初期状態を示します。

![SPA のサンプル アプリケーション](creating-web-projects-in-visual-studio/_static/image14.png)

SPA テンプレートを使用して、アプリケーションを作成する方法については、次を参照してください。 [Web API の外部認証サービス](../../../web-api/overview/security/external-authentication-services.md)します。

ASP.NET シングル ページ アプリケーション、および KnockoutJS 以外の JavaScript フレームワークを使用する追加の SPA テンプレートについての詳細については、次のリソースを参照してください。

- [ASP.NET シングル ページ アプリケーション](../../../single-page-application/index.md)します。
- [VS2013 RC の SPA テンプレートのセキュリティ機能の理解](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [シングル ページ アプリケーション: ASP.NET を使用した最新のレスポンシブ Web アプリをビルドします。](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Facebook テンプレート

インストールすることができます、 [Facebook テンプレートを提供する Visual Studio 拡張機能](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409)します。 このテンプレートは、Facebook の web サイト内で実行するように設計されたサンプル アプリケーションを作成します。 ASP.NET MVC に基づいた、リアルタイムの更新機能の Web API を使用します。

認証オプションはありません、Facebook テンプレートの Facebook アプリケーションが Facebook サイト内で実行し、Facebook の認証に依存しているためです。

ASP.NET の Facebook アプリケーションの詳細については、次を参照してください。 [MVC Facebook API の更新](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx)します。

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Visual Studio 2012 のテンプレート

Visual Studio 2013 web プロジェクトの作成 ダイアログ ボックスでは、一部の Visual Studio 2012 で使用可能になったテンプレートへのアクセスは提供されません。 これらのテンプレートのいずれかを使用する場合は、Visual Studio の [新しいプロジェクト] ダイアログ ボックスの左側のウィンドウで、Visual Studio 2012 のノードをクリックすることができます。

![Visual Studio 2012 のテンプレート](creating-web-projects-in-visual-studio/_static/image15.png)

**Visual Studio 2012**ノードがない次の web テンプレートを選択することができますへのアクセスをテンプレートの既定の一覧で Visual Studio 2013 の。

- ASP.NET MVC 4 Web アプリケーション
- ASP.NET 動的データ エンティティ Web アプリケーション
- ASP.NET AJAX サーバー コントロール
- ASP.NET AJAX サーバー コントロール エクステンダー
- ASP.NET サーバー コントロール

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Visual Studio 2013 web プロジェクト テンプレートでのブートス トラップします。

Visual Studio 2013 のプロジェクト テンプレートを使用して、[ブートス トラップ](http://getbootstrap.com/)、Twitter によって作成されたレイアウトとテーマのフレームワークです。 ブートス トラップでは、CSS3 を使用して、レイアウトは、別のブラウザー ウィンドウのサイズに動的に対応できることを意味するレスポンシブ デザインを提供します。 たとえば、ワイド ブラウザー ウィンドウでは、次の図は、ように Web フォーム テンプレートで作成したホーム ページがなります。

![Web フォーム テンプレート アプリのホーム ページ](creating-web-projects-in-visual-studio/_static/image16.png)

ウィンドウの幅を狭くして、水平方向に配置後の列が上下に移動します。

![ブートス トラップの垂直列の配置](creating-web-projects-in-visual-studio/_static/image17.png)

ウィンドウを少し、絞り込むし、水平方向の上部のメニューを垂直方向のメニューに拡張することができます をクリックしたアイコンに変わります。

![ブートス トラップのメニュー アイコン](creating-web-projects-in-visual-studio/_static/image18.png)

![ブートス トラップの垂直方向のメニュー](creating-web-projects-in-visual-studio/_static/image19.png)

アプリケーションの外観の変更を簡単に影響するのにブートス トラップのテーマ機能を使用することもできます。 たとえば、テーマを変更する、次の手順を行うことができます。

1. ブラウザーでに移動[ http://Bootswatch.com ](http://Bootswatch.com)、テーマを選択し、クリックして**ダウンロード**です。 (これによりダウンロード*bootstrap.min.css*既定では CSS コードを確認する場合が取得*bootstrap.css*縮小されたバージョンの代わりにします)。
2. ダウンロードした CSS ファイルの内容をコピーします。
3. Visual Studio で、作成、新しい**スタイル シート**という名前のファイル*ブートス トラップ theme.css*で、*コンテンツ*フォルダーと貼り付け、ダウンロードした CSS のコードにします。
4. 開いている*アプリ\_Start/Bundle.config*変更と*bootstrap.css*に*ブートス トラップ theme.css*します。

プロジェクトを再度実行して、アプリケーションが新しい外観。 次の図は、アメリア テーマの効果を示しています。

![ブートス トラップ アメリア テーマ](creating-web-projects-in-visual-studio/_static/image20.png)

多くのブートス トラップ テーマは、使用可能な free と premium の両方のバージョン。 ブートス トラップでさまざまな UI コンポーネントをなど提供も[ドロップダウン](http://twitter.github.io/bootstrap/components.html#dropdowns)、[ボタン グループ](http://twitter.github.io/bootstrap/components.html#buttonGroups)と[アイコン](http://twitter.github.io/bootstrap/base-css.html#images)。 ブートス トラップの詳細については、次を参照してください。 [Bootstrap サイト](http://twitter.github.io/bootstrap/)します。

Visual Studio で Web フォーム デザイナーを使用する場合は、デザイナーに CSS3 をサポートしないため、ブートス トラップ テーマや応答性の高いレイアウトの変更のすべての効果を正確に表示されないことに注意してください。 ただし、Web フォーム ページは、ブラウザーで表示すると、正しく表示します。

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>追加のフレームワークのサポートの追加

テンプレートを選択すると、テンプレートで使用される framework(s) のチェック ボックスが自動的に選択します。 たとえば、選択した場合、 **Web フォーム**、テンプレート、 **Web フォーム** チェック ボックスをオンおよびオフにすることはできません。

![テンプレートを選択します。](creating-web-projects-in-visual-studio/_static/image21.png)

![フレームワークを追加します。](creating-web-projects-in-visual-studio/_static/image22.png)

プロジェクトが作成されるときに、そのフレームワークのサポートを追加するには、テンプレートに含まれていないフレームワークのチェック ボックスを選択できます。 たとえば、Web フォームの使用を有効にする *.aspx* 、MVC テンプレートを選択したときに、ページの選択、 **Web フォーム**チェック ボックスをオンします。 Web フォーム テンプレートを使用しているときに有効にするには、MVC のまたは、 **MVC**チェック ボックスをオンします。 デザイン時と実行時のサポートを有効なフレームワークを追加します。 たとえば、Web フォーム プロジェクトに MVC サポートを追加する場合、コント ローラーとビューをスキャフォールディングできなきます。

プロジェクトで Web フォームと MVC を組み合わせるし、有効にするかどうかは[フレンドリな Url](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) Web フォームである可能性がありますいないルーティングの問題 1 つの URL が複数の可能なターゲットを持つ予期されます。 ルート定義されている最初の優先順位になります。 ある場合など、`Home`コント ローラーと*Home.aspx*  ページで、 `http://contoso.com/home` URL に移動します*Home.aspx*を呼び出す場合、`EnableFriendlyUrls`メソッド、を呼び出す前に`MapRoute`メソッド*RouteConfig.cs*、同じ URL の既定のビューに移動しますか、`Home`コント ローラーを呼び出す場合`MapRoute`する前に`EnableFriendlyUrls`します。

フレームワークを追加、すべてのサンプル アプリケーションの機能が追加されることはできません。 たとえばを追加すると Web フォーム サポートいいえ、MVC テンプレートを選択したときに*Default.aspx*ホーム ページのファイルが作成されます。 フォルダー、ファイル、およびフレームワークをサポートするために必要な参照のみが追加されます。 そのため、フレームワークを追加する、テンプレートによって作成されたサンプル アプリケーション内のコードで実装されている認証オプションを変更しません。 空のテンプレートを選択し、追加する場合は、Web フォームまたは MVC サポートなど、**認証の構成**ボタンが無効になっています。

次のセクションは、各チェック ボックスの効果について簡単に説明します。

### <a name="add-web-forms-support"></a>Web フォームのサポートを追加します。

空の作成*アプリ\_データ*と*モデル*フォルダーと*Global.asax*ファイル。 これらは Web フォームのチェック ボックスをオンにしても他のテンプレートの違いはありませんので、空のテンプレート以外のすべてのテンプレートによって作成既に。

Web フォーム テンプレートは、フレンドリな Url が自動的に有効でない Web フォームのチェック ボックスを選択すると他のテンプレートのサポートを Web フォームを追加すると、既定では、フレンドリな Url を使用します。

### <a name="add-mvc-support"></a>MVC サポートを追加します。

MVC、Razor、web ページの NuGet パッケージをインストールすると、空を作成します*アプリ\_データ*、*コント ローラー*、*モデル*、および*ビュー*フォルダーを作成します*アプリ\_開始*フォルダー *RouteConfig.cs*ファイル、および作成*Global.asax*ファイル。

### <a name="add-web-api-support"></a>Web API のサポートを追加します。

WebApi、および Newtonsoft.Json NuGet パッケージをインストールすると、空を作成します*アプリ\_データ*、*コント ローラー*、および*モデル*フォルダーを作成します*アプリ\_開始*フォルダー *WebApiConfig.cs*ファイル、および作成*Global.asax*ファイル。

<a id="auth"></a>
## <a name="authentication-methods"></a>認証方法

Visual Studio 2013 には、Web フォーム、MVC、Web API テンプレートのいくつかの認証オプションが用意されています。

- [認証なし](#noauth)
- [個々 のユーザー アカウント](#indauth)(Asp.net、ASP.NET メンバーシップと呼ばれていました)
- [組織アカウント](#orgauth)(Windows Server Active Directory または Azure Active Directory)
- [Windows 認証](#winauth)(イントラネット)

![認証ダイアログ ボックスを構成します。](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>認証なし

選択した場合**認証なし**いいえログオンしているユーザーを示す UI では、エンティティ クラス メンバーシップ データベース、およびメンバーシップ データベースの接続文字列のサンプル アプリケーションにログインするための web ページが含まれません。

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>個々 のユーザー アカウント

選択した場合**個々 のユーザー アカウント**、ユーザー認証用 (旧称 ASP.NET メンバーシップ) ASP.NET Identity を使用するサンプル アプリケーションに構成されます。 ASP.NET Identity では、サイトのユーザー名とパスワードを作成して、または Facebook、Google、Microsoft アカウント、Twitter などのソーシャル プロバイダーにサインインすることで、アカウントを登録するユーザーができます。 ASP.NET Identity でユーザー プロファイルの既定のデータ ストアは、SQL Server LocalDB データベースのことで、実稼働サイトの SQL Server または Azure SQL Database にデプロイすることができます。

これらの機能では Visual Studio 2013 では、Visual Studio 2012 と同じですが、ASP.NET メンバーシップ システムの基になるコードを書き直しました。 新しいコード ベースの利点を以下に示します。

- 新しいメンバーシップ システムをに基づいて[OWIN](http://owin.org/) ASP.NET フォーム認証モジュールではなく。 これは IIS では、Web フォームまたは MVC を使用しているか、Web API や SignalR を自己ホストしているかどうかに、同じ認証メカニズムを使用することを意味します。
- Entity Framework Code First によって、新しいメンバーシップ データベースが管理されているし、変更可能なエンティティ クラスによって表される、すべてのテーブル。 つまり、データベース スキーマと web のプロファイルに関連する独自のニーズに合わせて UI を簡単にカスタマイズすることができます、Code First Migrations を使用して、更新プログラムを簡単にデプロイすることができます。

新しいメンバーシップ システムは、新しいテンプレートで自動的に実装して、.NET 4.5 を対象とするすべてのプロジェクトに手動で実装されている、またはそれ以降であることができます。

ASP.NET Identity では、主に外部の顧客であるインターネット web サイトを作成する場合は、適切な選択です。 組織で Active Directory を使用または Office 365 とする従業員やビジネス パートナー向けのシングル サインオンできるようにするプロジェクトを作成する場合、**組織アカウント**オプションは、ことをお勧めします。

個々 のユーザー アカウントのオプションの詳細については、次のリソースを参照してください。

- [www.asp.net/identity](../../../identity/index.md)します。 ASP.NET web サイトで ASP.NET Identity に関するドキュメントです。
- [Facebook と Google の OAuth2 や OpenID サインオンで、ASP.NET MVC 5 アプリを作成する](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)します。 ユーザー プロファイル データをカスタマイズする方法も示します。
- [Web API の外部認証サービス](../../../web-api/overview/security/external-authentication-services.md)
- [Visual Studio 2013 で ASP.NET アプリケーションに外部ログインの追加](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>組織アカウント

選択した場合**組織アカウント**、サンプル アプリケーションは、Azure Active Directory (Azure AD、Office 365 を含む) 内のユーザー アカウントに基づく認証に Windows Identity Foundation (WIF) を使用するように構成されますかWindows Server Active Directory。 詳細については、次を参照してください。[組織アカウントの認証オプション](#orgauthoptions)このトピックで後述します。

<a id="winauth"></a>
### <a name="windows-authentication"></a>Windows 認証

選択した場合**Windows 認証**、サンプル アプリケーションは、認証に Windows 認証の IIS モジュールを使用するように構成されます。 アプリケーションが Windows にログインしますが、しませんユーザー登録を含めるか、またはログ UI をローカル コンピューター アカウントまたは Active directory のドメインとユーザー ID が表示されます。 このオプションは、イントラネットの web サイトを対象としています。

選択して AD 認証を使用してイントラネット サイトを作成する代わりに、[オンプレミス組織アカウントでオプション](#orgauthonprem)します。 オンプレミスのオプションは、Windows 認証モジュールではなく、Windows Identity Foundation (WIF) を使用します。 オンプレミスのオプションを設定するために必要な追加の手順が、WIF には、Windows 認証モジュールで使用できない機能が有効になります。 たとえば、WIF の Active Directory とクエリのディレクトリ データのアプリケーションへのアクセスを構成できます。

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>組織アカウントの認証オプション

**認証の構成**ダイアログは、いくつかの Azure Active Directory (Azure AD、Office 365 を含む) または Windows Server ・ Active Directory (AD) アカウントの認証方法を示します。

- [クラウド - 単一の組織](#orgauthsingle)(Azure AD または AD と Azure AD ディレクトリの統合を使用して)
- [クラウド - 複数組織](#orgauthmulti)(Azure AD または AD と Azure AD ディレクトリの統合を使用して)
- [オンプレミス](#orgauthonprem)(AD)

Azure AD のオプションのいずれかを試すにはまだ、アカウントを持っていない[ここをクリックすると、Azure AD アカウントにサインアップする](https://go.microsoft.com/fwlink/?LinkId=309942)します。

> [!NOTE]
> Azure AD のオプションのいずれかを選択した場合、プロジェクトには、データベースが必要です。 と Azure AD テナントのグローバル管理者アカウントにサインインする必要があります。 組織のアカウント名とパスワードを入力します (たとえば、 admin@contoso.onmicrosoft.com)、Azure AD テナントの管理者権限を持っています。
> 
> **Microsoft アカウントの資格情報を入力しないでください (たとえば、 contoso@hotmail.com) でサインイン ダイアログ ボックス。**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>クラウド - 単一の組織の認証

![1 つの組織の認証](creating-web-projects-in-visual-studio/_static/image24.png)

1 つの Azure AD で定義されているユーザー アカウントの認証を有効にする場合は、このオプションを選択[テナント](https://technet.microsoft.com/library/jj573650.aspx)します。 たとえば、サイトが contoso.com し、それが利用できる contoso.onmicrosoft.com テナントでは、Contoso 社の従業員にします。 Azure AD アプリケーションにアクセスするには、他のテナントからユーザーの許可を構成することはできません。

#### <a name="domain"></a>ドメイン

たとえば、アプリケーションを設定する Azure AD ドメインを入力します:`contoso.onmicrosoft.com`します。 ある場合、[カスタム ドメイン](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/)など`contoso.com`の代わりに`contoso.onmicrosoft.com`、ここで入力することができます。

#### <a name="access-level"></a>アクセス レベル

アプリケーションは、クエリまたは Graph API を使用してディレクトリ情報の更新を選択する必要がある場合**でシングル サインオン、ディレクトリ データの読み取り**または**でシングル サインオン、読み取りおよびディレクトリ データを書き込む**します。 それ以外の場合、選択**でシングル サインオン**します。 詳細については、次を参照してください。[アプリケーションのアクセス レベル](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels)と[クエリ Azure AD Graph API を使用して](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx)します。

#### <a name="application-id-uri"></a>アプリケーション ID URI

既定では、プロジェクト名を Azure AD ドメインに追加することでのアプリケーション ID URI を作成します。 たとえば、プロジェクト名が`Example`、ドメインが`contoso.onmicrosoft.com`、アプリケーション ID URI が`https://contoso.onmicrosoft.com/Example`。 アプリケーション ID の URI を手動で指定する場合は、展開、**オプションより**セクションし、テキスト ボックスに、アプリケーション ID URI を入力します。 アプリケーション ID URI が始まる必要があります`https://`します。

既定では、Azure AD で既にプロビジョニングされているアプリケーションに、プロジェクトの Visual Studio を使用しているものと同じアプリケーション ID URI がある場合、プロジェクトは、新しいものをプロビジョニングする代わりに既存のアプリケーションに接続されます。 その場合はプロビジョニングされる新しいアプリケーションを実行する場合に、オフ、**同じ ID を持つ 1 つが既に存在する場合は、アプリケーションのエントリを上書き**チェック ボックスをオンします。

場合、**上書き** チェック ボックスをオフにすると、および Visual Studio は、同じアプリケーション ID URI を既存のアプリケーションを検索、新しい URI を使用することになる URI に番号を付加して作成します。 たとえば、プロジェクト名が`Example`、テキスト ボックスを空白のままにして、オフにする、**上書き**URI を持つアプリケーションは既にチェック ボックスをオンし、Azure AD テナント`https://contoso.onmicrosoft.com/Example`します。 ID の URI などのアプリケーションで新しいアプリケーションをプロビジョニングする場合、`https://contoso.onmicrosoft.com/Example_20130619330903`します。

#### <a name="provisioning-the-application-in-azure-ad"></a>Azure AD でアプリケーションをプロビジョニング

Azure AD でアプリケーションをプロビジョニングする、またはプロジェクトを既存のアプリケーションに接続するには、Visual Studio、ドメインのグローバル管理者の資格情報する必要があります。 クリックすると **[ok]** で、**認証の構成**ダイアログ ボックスで、ユーザー名と指定したドメインのグローバル管理者のパスワードを求められます。 後をクリックすると**プロジェクトの作成**で、**新しい ASP.NET プロジェクト**ダイアログ ボックスで、Visual Studio は、Azure AD でアプリケーションをプロビジョニングします。 このプロセスの一部として Visual Studio クライアント シークレットの値に埋め込みます作成後 1 年間の有効期限を Web.config ファイルに注意してください。

使用するアプリケーションを作成する方法については**クラウド - 単一の組織**認証では、次のリソースを参照してください。

- [Azure の認証](../2012/windows-azure-authentication.md)
- [Azure AD を使用して Web アプリケーションへのシングル サインオンの追加](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Azure Active Directory を使った ASP.NET アプリの開発](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Azure AD での ASP.NET Web API をセキュリティで保護し、Microsoft OWIN コンポーネント](https://msdn.microsoft.com/magazine/dn463788.aspx)

Visual Studio 2013; まだ更新されていない、チュートリアルVisual Studio 2013 で手動で実行するユーザーを誘導したり、どのようなチュートリアルの一部が自動化されます。

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>クラウド - 複数の組織の認証

![複数の組織の認証](creating-web-projects-in-visual-studio/_static/image25.png)

複数の Azure AD で定義されているユーザー アカウントの認証を有効にする場合は、このオプションを選択[テナント](https://technet.microsoft.com/library/jj573650.aspx)します。 たとえば、サイトが contoso.com とそれが利用できる contoso.onmicrosoft.com テナントでは、Contoso 社の従業員を fabrikam.onmicrosoft.com テナントでは、Fabrikam 企業の従業員。

入力した設定とアプリケーションのプロビジョニングの手順に似ています[1 つの組織認証](#orgauthsingle)します。

使用するアプリケーションを作成する方法については**クラウド - 複数組織**認証では、次のリソースを参照してください。

- [ASP.NET、Azure Active Directory と簡単に Web アプリ統合&amp;Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) Active Directory チーム ブログ。
- [Azure AD でマルチ テナント Web アプリケーションの開発](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx)チュートリアル。 このチュートリアルは、for Visual Studio 2013; まだ更新されていません。どのようなチュートリアルの指示に従って手動で行うは、Visual Studio 2013 で自動化されています。
- [サインインする前に、独自の複数の組織 ASP.NET アプリにサインアップする必要がある](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/)します。 一般的な問題のユーザーを解決する方法を説明する Vittorio Bertocci によるブログは、複数の組織の認証を使用してプロジェクトを作成するときに発生します。

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>オンプレミス組織認証

![オンプレミス組織認証](creating-web-projects-in-visual-studio/_static/image26.png)

Windows Server Active Directory (AD) では定義されているユーザー アカウントの認証を有効にして、Azure AD を使用する場合は、このオプションを選択します。 このオプションを使用すると、イントラネット サイトやインターネット サイトを作成します。 インターネット サイトで Active Directory フェデレーション サービス (ADFS) を使用して、AD にアクセスできるようにします。 詳細については、次を参照してください。 [ASP.NET で、オンプレミス組織認証オプション (ADFS) を使用して、Visual Studio 2013 で](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)します。

イントラネット サイトの代替として選択できます[Windows 認証](#winauth)このオプションの代わりにします。 Windows 認証オプションは、メタデータ ドキュメントの URL を指定する必要はありません。 ただし、Windows 認証与えませんできる Active Directory のアプリケーション アクセスを制御したり、ディレクトリ データを照会します。

#### <a name="on-premises-authority"></a>オンプレミス機関

メタデータ ドキュメントを指す URL を入力します。 メタデータ ドキュメントには、証明機関の座標が含まれています。 アプリケーションはこれらの座標を使用して web サインオン フロー。

#### <a name="application-id-uri"></a>アプリケーション ID URI

AD を使用して、このアプリケーションを識別または Visual Studio のいずれかを作成できるようにする場合は空白のままにする一意の URI を提供します。

<a id="nextsteps"></a>
## <a name="next-steps"></a>次の手順

このドキュメントは Visual Studio 2013 で新しい ASP.NET web プロジェクトを作成するための基本的なヘルプを提供しています。 Web 開発用の Visual Studio の使用に関する詳細については、次を参照してください。 [ https://www.asp.net/visual-studio/](../../index.md)します。
