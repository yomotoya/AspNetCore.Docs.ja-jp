---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Visual Studio 2013 での ASP.NET Web プロジェクトの作成 |Microsoft ドキュメント
author: tdykstra
description: このトピックでは、Visual Studio 2013 更新プログラム 3 は、ここでの ASP.NET web プロジェクトを作成するためのオプションは、web 開発 c の新機能の一部について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: aacae7a9ccf483b21d3c6796c0411d558fa3c75b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "28038866"
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Visual Studio 2013 で ASP.NET Web プロジェクトの作成
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このトピックは、Visual Studio 2013 Update 3 での ASP.NET web プロジェクトを作成するためのオプションを説明します
> 
> Visual Studio の以前のバージョンと比較して web 開発の新機能の一部を次に示します。
> 
> - 作成するための単純な UI がその提供をプロジェクト[の複数の ASP.NET フレームワークをサポートして](#add)(Web フォーム、MVC、および Web API)。
> - [ASP.NET Identity](#indauth)、新しい ASP.NET メンバーシップ システム web ホスティング IIS 以外のソフトウェアと連携するすべての ASP.NET フレームワークと動作で同じです。
> - 使用[ブートス トラップ](#bootstrap)応答性の高い設計とテーマの機能を提供します。
> - 新機能など、MVC、に対してのみ提供するために使用する Web フォーム[自動のテスト プロジェクトの作成](#testproj)と[イントラネット サイト テンプレート](#winauth)です。
> 
> Azure クラウド サービスまたは Azure Mobile Services の web プロジェクトを作成する方法については、次を参照してください[Azure クラウド サービスと ASP.NET の概要](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/)と[で Azure Mobile Services の .NET スコアボード アプリの作成。バックエンド](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)です。


<a id="prerequisites"></a>
## <a name="prerequisites"></a>必須コンポーネント

この記事に該当する[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)で[更新プログラム 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409)インストールされています。

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Web アプリケーション プロジェクトと web サイト プロジェクト

ASP.NET では、web プロジェクトの 2 種類のいずれか: *web アプリケーション プロジェクト*と*web サイト プロジェクト*です。 新しい開発では、web アプリケーション プロジェクトをお勧めして、この記事は、web アプリケーション プロジェクトにのみ適用されます。 詳細については、次を参照してください。 [Web アプリケーション プロジェクトと Visual Studio での Web サイト プロジェクト](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx)MSDN サイトです。

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Web アプリケーション プロジェクトの作成の概要

次の手順では、web プロジェクトを作成する方法を示します。

1. をクリックして**新しいプロジェクト**で、**開始**ページまたは、**ファイル**メニュー。
2. **新しいプロジェクト**ダイアログ ボックスで、をクリックして**Web**左側のウィンドウでと**ASP.NET Web アプリケーション**中央のペインでします。

    ![[新しいプロジェクト] ダイアログ](creating-web-projects-in-visual-studio/_static/image1.png)

    選択できます**クラウド**を作成する左側のウィンドウで、 [Azure クラウド サービス](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)、 [Azure モバイル サービス](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx)、または[Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs)です。 このトピックでは、これらのテンプレートを扱われていません。
3. 右側のウィンドウでをクリックして、**プロジェクトに Application Insights の追加**正常性とアプリケーションの使用状況を監視したい場合はチェック ボックスです。 詳細については、次を参照してください。 [web アプリケーションのパフォーマンスを監視](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/)です。
4. プロジェクトを指定**名前**、**場所**、およびその他のオプションとクリック**OK**です。

    **新しい ASP.NET プロジェクト**ダイアログが表示されます。

    ![[新しいプロジェクト] ダイアログ](creating-web-projects-in-visual-studio/_static/image2.png)
5. [テンプレート] をクリックします。

    ![テンプレートを選択します。](creating-web-projects-in-visual-studio/_static/image3.png)
6. テンプレートに含まれていないその他のフレームワークのサポートを追加する場合は、適切なチェック ボックスをクリックします。 (例では、する可能性があります MVC または Web API プロジェクトに追加 Web フォームです。)

    ![フレームワークを追加します。](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>単体テスト プロジェクトを追加する場合は、クリックして**単体テストを追加**です。

    ![単体テストを追加します。](creating-web-projects-in-visual-studio/_static/image5.png)
8. 既定で提供するテンプレートよりも別の認証方法を実行する場合に、クリックして**認証の変更**です。

    ![[認証] を構成します。](creating-web-projects-in-visual-studio/_static/image6.png)

    ![認証のダイアログ ボックスを構成します。](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Azure で web アプリまたは仮想マシンを作成します。

Visual Studio には、簡単に web アプリケーションをホストする Azure サービスを使用する機能が含まれています。 たとえば、Visual Studio IDE から直接、次のすべての実行できます。

- 作成して、web アプリまたはインターネット経由でアプリケーションを使用できるようにする仮想マシンを管理します。
- クラウドで実行するときに、アプリケーションで作成されたログを表示します。
- アプリケーションをクラウドで実行中にリモートでデバッグ モードで実行します。
- Viiew および SQL データベースなど他の Azure サービスを管理します。

ことができます[Azure アカウントを作成する](https://www.windowsazure.com/pricing/free-trial/)を無料で web アプリなどの基本的なサービスを含むし、場合は、MSDN サブスクライバー[特典](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/)すれば、その他の azure の毎月のクレジットサービスです。 

既定では、**新しい ASP.NET プロジェクト** ダイアログ ボックスでは、web アプリまたは新しい web プロジェクト用にバーチャル マシンを作成することができます。 新しい web アプリまたは仮想マシンを作成しない場合は、クリア、**クラウド内のホスト**チェック ボックスをオンします。

![リモート リソースを作成します。](creating-web-projects-in-visual-studio/_static/image8.png)

チェック ボックスのキャプションがある可能性があります**クラウド内のホスト**または**リモート リソースを作成する**、し、いずれの場合、効果は同じです。 チェック ボックスをオンの場合、Visual Studio は、既定では Azure App service web アプリを作成します。 ドロップダウン ボックスを使用して変更することができます、**仮想マシン**したい場合。 されていない Azure にサインインしている場合は、Azure の資格情報のように求められます。 サインインした後、ダイアログ ボックスでは、Visual Studio は、プロジェクトを作成するリソースを構成することができます。 次の図に、web アプリ以外のダイアログ ボックス仮想マシンを作成する場合、さまざまなオプションが表示されます。

![Azure アプリの設定を構成します。](creating-web-projects-in-visual-studio/_static/image9.png)

Azure リソースを作成するため、このプロセスを使用する方法の詳細については、次を参照してください。 [Azure と ASP.NET の概要](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)と[Visual Studio で web サイトの仮想マシンを作成する](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/)です。

この記事の残りの部分では、使用可能なテンプレートと、オプションの詳細を提供します。 アーティクルには、テンプレートで使用されているブートス トラップ、レイアウトとテーマのフレームワークが導入されています。

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013 の Web プロジェクト テンプレート

Visual Studio では、テンプレートを使用して、web プロジェクトを作成します。 プロジェクト テンプレートを新しいプロジェクトでファイルとフォルダーを作成、NuGet パッケージのインストールおよび基本的な作業アプリケーションのサンプル コードを提供します。 テンプレートは、最新の web 規格を実装し、ASP.NET テクノロジを使用できるだけでなく、ジャンプを付与する方法のベスト プラクティスを開始、独自のアプリケーションを作成する方法を説明するものではできます。

Visual Studio 2013 では、.NET 4.5 または .NET framework の以降のバージョンを対象とするプロジェクトの web プロジェクト テンプレートを次の選択肢を提供します。

- [空のテンプレート](#empty)
- [Web フォーム テンプレート](#wf)
- [MVC テンプレート](#mvc)
- [Web API テンプレート](#webapi)
- [単一ページ アプリケーション テンプレート](#spa)
- [Azure モバイル サービス テンプレート](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Visual Studio 2012 のテンプレート](#vs2012)

提供する Visual Studio 拡張機能をインストールすることも、 [Facebook テンプレート](#facebook)です。

.NET 4 を対象とするプロジェクトを作成する方法については、次を参照してください。 [Visual Studio 2012 テンプレート](#vs2012)このトピックで後述します。

モバイル クライアント向けの ASP.NET アプリケーションを作成する方法については、次を参照してください。 [ASP.NET でのモバイル サポート](../../../mobile/index.md)です。

<a id="empty"></a>
### <a name="empty-template"></a>空のテンプレート

空のテンプレートは、ベア最小フォルダーとプロジェクト ファイルなど、ASP.NET web アプリのファイル (*.csproj*または *。vbproj*) および*Web.config*ファイル。 下のチェック ボックスを使用して、Web フォーム、MVC、または Web API のサポートを追加することができます、**フォルダーを追加し、コアの参照:** ラベル。

空のテンプレートの認証オプションはありません。 サンプル アプリケーションで認証機能は実装され、空のテンプレートは、サンプル アプリケーションを作成できません。

<a id="wf"></a>
### <a name="web-forms-template"></a>Web フォーム テンプレート

Framework には、次の機能を迅速に可能にするために UI とデータの豊富な web サイトの構築、Web フォームの機能にアクセスします。

- Visual Studio での WYSIWYG デザイナー。
- HTML およびプロパティとスタイルを設定してカスタマイズできることをレンダリングするサーバーを制御します。
- データ アクセスおよびデータの表示のコントロールの機能豊富な品揃え。
- WPF などのクライアント アプリケーションがプログラムを作成するようにユーザーが、プログラムがイベントを公開するイベント モデル。
- HTTP 要求の間で状態 (データ) の自動保持します。

一般に、Web フォーム アプリケーションを作成するには、ASP.NET MVC フレームワークを使用して、同じアプリケーションを作成するよりも少ないプログラミングの労力が必要です。 ただし、Web フォームだけではなくアプリケーションの迅速な開発です。 多数の複雑な商用アプリケーションや Web フォームの上に構築されたフレームワークです。

Web フォーム ページと、ページ上のコントロールをブラウザーに送信されるマークアップの多くに自動的に生成、ために、ASP.NET MVC を提供する HTML をきめ細かく制御の種類がありません。 ページおよびコントロールを構成する宣言型モデルには、コードを記述する必要があるが、HTML と HTTP の動作の一部を非表示に量が最小限に抑えます。 たとえばは常に正確にコントロールによってどのようなマークアップを生成する可能性がありますを指定します。

Web フォームのフレームワークは ASP.NET MVC、すぐに適してパターンに基づく開発手法など[テスト駆動開発](http://en.wikipedia.org/wiki/Test-driven_development)、[関心の分離](http://en.wikipedia.org/wiki/Separation_of_concerns)、[の反転コントロール](http://en.wikipedia.org/wiki/Inversion_of_control)、および[依存性の注入](http://en.wikipedia.org/wiki/Dependency_injection)です。 書き込むコードがその方法を考慮する場合は、次の操作をすることができます。ASP.NET MVC フレームワークであるため、自動はないだけです。 [ASP.NET Web フォーム MVP](http://webformsmvp.com/)プロジェクトを Web フォームの配信が組み込まれている迅速な開発を維持しながら懸念事項とテストの容易性の分離を容易にする方法を示しています。 Microsoft SharePoint は、Web フォーム MVP で作成されています。

Web フォーム テンプレートを使用するサンプル Web フォーム アプリケーションを作成[ブートス トラップ](#bootstrap)レスポンシブ デザインとテーマの機能を提供します。 次の図は、ホーム ページを示します。

![Web フォーム テンプレート アプリのホーム ページ](creating-web-projects-in-visual-studio/_static/image10.png)

Web フォームの詳細については、次を参照してください。 [ASP.NET Web フォーム](https://asp.net/web-forms)です。 Web フォーム テンプレートの動作の詳細については、次を参照してください。 [Visual Studio 2013 を使用して基本的な Web フォーム アプリケーションを構築](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx)です。

<a id="mvc"></a>
### <a name="mvc-template"></a>MVC テンプレート

ASP.NET MVC がなどのパターンに基づく開発手法を容易にするよう設計されました[テスト駆動開発](http://en.wikipedia.org/wiki/Test-driven_development)、[関心の分離](http://en.wikipedia.org/wiki/Separation_of_concerns)、[コントロールの反転](http://en.wikipedia.org/wiki/Inversion_of_control)、および[依存性の注入](http://en.wikipedia.org/wiki/Dependency_injection)です。 フレームワークは、そのプレゼンテーション層から web アプリケーションのビジネス ロジック層を分離することをお勧めします。 割ってアプリケーション モデル (M)、ビュー (V)、およびコント ローラー (C) に、ASP.NET MVC やすく大規模なアプリケーションの複雑さを管理します。

ASP.NET MVC では、HTML およびより HTTP Web フォームでより直接的に作業します。 たとえば、Web フォームでは、HTTP 要求の間で状態を維持できます自動的が、MVC で明示的にコーディングする必要があります。 MVC モデルの利点は、正確にアプリが何をおよび web 環境でその動作を完全に制御を実行できます。 欠点は、その他のコードを記述する必要があります。

MVC は、電源の開発者のアプリケーション ニーズを満たすフレームワークをカスタマイズする機能を提供する、拡張できるように設計されています。 さらに、ASP.NET MVC のソース コードは、OSI ライセンスで使用可能なです。

MVC テンプレートを使用するサンプル MVC 5 アプリケーションを作成[ブートス トラップ](#bootstrap)レスポンシブ デザインとテーマの機能を提供します。 次の図は、ホーム ページを示します。

![MVC サンプル アプリケーション](creating-web-projects-in-visual-studio/_static/image11.png)

MVC の詳細については、次を参照してください。 [ASP.NET MVC](https://asp.net/mvc)です。 MVC 4 テンプレートを選択する方法については、次を参照してください。 [Visual Studio 2012 テンプレート](#vs2012)この記事で後述します。

<a id="webapi"></a>
### <a name="web-api-template"></a>Web API テンプレート

Web API テンプレートでは、MVC に基づいた API ヘルプ ページを含む Web API に基づくサンプル web サービスを作成します。

ASP.NET Web API は、さまざまなブラウザーやモバイル デバイスを含む、クライアントに到達できる HTTP サービスを作成しやすくフレームワークです。 ASP.NET Web API は、.NET Framework で RESTful サービスを作成するためのプラットフォームとして理想的です。

Web API テンプレートでは、サンプル web サービスを作成します。 次の図は、ヘルプ ページのサンプルを表示します。

![Web API のヘルプ ページ](creating-web-projects-in-visual-studio/_static/image12.png)

![Web API のヘルプ ページの取得 API を](creating-web-projects-in-visual-studio/_static/image13.png)

Web API の詳細については、次を参照してください。 [ASP.NET Web API](https://asp.net/web-api)です。

<a id="spa"></a>
### <a name="single-page-application-template"></a>単一ページ アプリケーション テンプレート

単一ページ アプリケーション (SPA) テンプレートは、html5、JavaScript を使用するサンプル アプリケーションを作成し、 [KnockoutJS](http://knockoutjs.com/)クライアント、およびサーバー上の ASP.NET Web API です。

SPA テンプレートの唯一の認証オプションは[個々 のユーザー アカウント](#indauth)です。

次の図は、SPA テンプレートをビルドするサンプル アプリケーションの初期状態を示します。

![SPA サンプル アプリケーション](creating-web-projects-in-visual-studio/_static/image14.png)

SPA テンプレートを使用してアプリケーションを作成する方法については、次を参照してください。 [Web API の外部認証サービス](../../../web-api/overview/security/external-authentication-services.md)です。

単一ページ、ASP.NET アプリケーションおよび KnockoutJS 以外の JavaScript フレームワークを使用してその他の SPA テンプレートについての詳細については、次のリソースを参照してください。

- [ASP.NET の単一ページ アプリケーション](../../../single-page-application/index.md)です。
- [VS2013 RC の SPA テンプレートのセキュリティ機能の理解](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [単一ページ アプリケーション: ASP.NET での応答性の高い、最新の Web アプリをビルドします。](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Facebook テンプレート

インストールすることができます、 [Facebook テンプレートを提供する Visual Studio 拡張機能](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409)します。 このテンプレートでは、Facebook web サイト内で実行するように設計されたサンプル アプリケーションを作成します。 ASP.NET MVC に基づいたし、リアルタイムの更新機能の Web API を使用します。

認証オプションはありません、Facebook テンプレートの Facebook アプリケーションが Facebook サイト内で実行して、Facebook の認証に依存しているためです。

ASP.NET Facebook アプリケーションの詳細については、次を参照してください。 [MVC Facebook API の更新](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx)です。

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Visual Studio 2012 のテンプレート

Visual Studio 2013 web プロジェクトの作成 ダイアログ ボックスは、Visual Studio 2012 で使用可能になった一部のテンプレートへのアクセスを提供しません。 これらのテンプレートのいずれかを使用する場合は、Visual Studio の新しいプロジェクト ダイアログ ボックスの左側のウィンドウで Visual Studio 2012 ノードをクリックすることができます。

![Visual Studio 2012 テンプレート](creating-web-projects-in-visual-studio/_static/image15.png)

**Visual Studio 2012**がない、次の web テンプレートを選択するノードによりへのアクセスをテンプレートの既定の一覧で Visual Studio 2013 用。

- ASP.NET MVC 4 Web アプリケーション
- ASP.NET 動的データ エンティティ Web アプリケーション
- ASP.NET AJAX サーバー コントロール
- ASP.NET AJAX サーバー コントロール エクステンダー
- ASP.NET サーバー コントロール

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Visual Studio 2013 の web プロジェクト テンプレートでブートス トラップします。

Visual Studio 2013 プロジェクト テンプレートを使用して[ブートス トラップ](http://getbootstrap.com/)レイアウトとテーマのフレームワークが Twitter で作成します。 ブートス トラップでは、CSS3 を使用して、レイアウトは、別のブラウザー ウィンドウのサイズに動的に対応できることを意味するレスポンシブ デザインを提供します。 たとえば、ワイド ブラウザーのウィンドウで次の図のように Web フォーム テンプレートを作成したホーム ページはなります。

![Web フォーム テンプレート アプリのホーム ページ](creating-web-projects-in-visual-studio/_static/image16.png)

ウィンドウの幅を狭くして、水平方向に整列の列が垂直方向に移動します。

![ブートス トラップの縦一列の配置](creating-web-projects-in-visual-studio/_static/image17.png)

少し、ウィンドウを絞り込むし、水平方向の上部のメニューがメニューを垂直方向に展開するクリックして可能なアイコンに変わります。

![ブートス トラップ メニュー アイコン](creating-web-projects-in-visual-studio/_static/image18.png)

![ブートス トラップの縦方向のメニュー](creating-web-projects-in-visual-studio/_static/image19.png)

ブートス トラップのテーマの機能は、アプリケーションの外観の変更を簡単に影響するを使用することもできます。 たとえば、テーマを変更する次の手順を行うことができます。

1. ブラウザーでに移動[ http://Bootswatch.com ](http://Bootswatch.com)、テーマを選択し、クリックして**ダウンロード**です。 (これにより、ダウンロード*bootstrap.min.css*既定では、CSS コードを調べる場合が取得*bootstrap.css*縮小されたバージョンではなくです)。
2. ダウンロードした CSS ファイルの内容をコピーします。
3. Visual Studio で、新しい**スタイル シート**という名前のファイル*ブートス トラップ theme.css*で、*コンテンツ*フォルダーと貼り付け、ダウンロードした CSS のコードにします。
4. 開いている*アプリ\_Start/Bundle.config*変更と*bootstrap.css*に*ブートス トラップ theme.css*です。

プロジェクトを再度実行し、アプリケーションが新しくなります。 次の図は、アメリア テーマの効果を示しています。

![ブートス トラップ アメリア テーマ](creating-web-projects-in-visual-studio/_static/image20.png)

多くのブートス トラップ テーマが利用可能な無料および有料の両方のバージョン。 ブートス トラップもさまざまな UI コンポーネントなど[ドロップダウン](http://twitter.github.io/bootstrap/components.html#dropdowns)、[ボタン グループ](http://twitter.github.io/bootstrap/components.html#buttonGroups)、および[アイコン](http://twitter.github.io/bootstrap/base-css.html#images)です。 ブートス トラップの詳細については、次を参照してください。[ブートス トラップ サイト](http://twitter.github.io/bootstrap/)です。

Visual Studio で Web フォーム デザイナーを使用する場合は、デザイナー サポートしていないこと、CSS3 のブートス トラップ テーマまたはレスポンシブ レイアウトの変更のすべての影響を正確に反映されませんので注意してください。 ただし、Web フォーム ページはブラウザーで表示すると、正しく表示します。

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>その他のフレームワークのサポートを追加します。

テンプレートを選択すると、テンプレートで使用される framework(s) のチェック ボックスが自動的に選択します。 たとえば、選択した場合、 **Web フォーム**テンプレート、 **Web フォーム** チェック ボックスをオンおよびオフにすることはできません。

![テンプレートを選択します。](creating-web-projects-in-visual-studio/_static/image21.png)

![フレームワークを追加します。](creating-web-projects-in-visual-studio/_static/image22.png)

プロジェクトが作成されるときに、そのフレームワークのサポートを追加するために、テンプレートに含まれていない、フレームワークのチェック ボックスを選択します。 たとえば、Web フォームの使用を有効にする *.aspx* MVC テンプレートを選択したときに、ページの選択、 **Web フォーム**チェック ボックスをオンします。 Web フォーム テンプレートを使用している場合は、MVC を有効化、 をクリックして、 **MVC**チェック ボックスをオンします。 フレームワークを追加すると、デザイン時と実行時のサポートができます。 たとえば、Web フォーム プロジェクトに MVC サポートを追加する場合は、コント ローラーとビューをスキャフォールディングすることができます。

プロジェクト内に Web フォームと MVC を組み合わせるし、有効にするかどうかは[フレンドリな Url](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) Web フォームである可能性がありますいないルーティングの問題 1 つの URL が複数の可能なターゲットを持つ予期されます。 ルート定義を最初に優先されます。 ある場合など、`Home`コント ローラーと*Home.aspx*  ページで、 `http://contoso.com/home` URL に移動する*Home.aspx*を呼び出す場合は、`EnableFriendlyUrls`メソッド、を呼び出す前に`MapRoute`メソッド*RouteConfig.cs*、同一の URL がの既定のビューに移動するか、`Home`コント ローラーを呼び出す場合`MapRoute`する前に`EnableFriendlyUrls`です。

フレームワークを追加しても、サンプル アプリケーションの機能をすべては追加されません。 たとえば、追加する場合 Web フォーム サポートいいえ MVC テンプレートを選択したときに*Default.aspx*ホーム ページのファイルを作成します。 フォルダー、ファイル、およびフレームワークをサポートするために必要な参照のみが追加されます。 そのため、フレームワークを追加するテンプレートを作成するサンプル アプリケーションのコードで実装されている認証オプションを変更しません。 たとえば、空のテンプレートを選択し、追加する場合は、Web フォームまたは MVC サポート、**認証を構成する**ボタンが無効になっています。

次のセクションでは、各チェック ボックスの結果を示してについて簡単にします。

### <a name="add-web-forms-support"></a>Web フォームのサポートを追加します。

空作成*アプリ\_データ*と*モデル*フォルダーと*Global.asax*ファイル。 Web フォームのチェック ボックスを選択しても他のテンプレートの違いはありませんので、空のテンプレート以外のすべてのテンプレートで既に作成されます。

Web フォーム テンプレートは、フレンドリな Url が自動的に有効でない Web フォームのチェック ボックスを選択するとその他のテンプレートのサポートを Web フォームを追加する場合は既定では、フレンドリな Url を使用します。

### <a name="add-mvc-support"></a>MVC のサポートを追加します。

MVC、Razor、および web ページの NuGet パッケージをインストールすると、空作成*アプリ\_データ*、*コント ローラー*、*モデル*、および*ビュー*フォルダーを作成*アプリ\_開始*フォルダー *RouteConfig.cs*ファイル、および作成*Global.asax*ファイル。

### <a name="add-web-api-support"></a>Web API のサポートを追加します。

WebApi、Newtonsoft.Json NuGet パッケージをインストールすると、空の作成*アプリ\_データ*、*コント ローラー*、および*モデル*フォルダーを作成*アプリ\_開始*フォルダー *WebApiConfig.cs*ファイル、および作成*Global.asax*ファイル。

<a id="auth"></a>
## <a name="authentication-methods"></a>認証方法

Visual Studio 2013 では、Web フォーム、MVC、および Web API テンプレートのいくつかの認証オプションを提供します。

- [認証なし](#noauth)
- [個々 のユーザー アカウント](#indauth)(Asp.net、ASP.NET メンバーシップと呼ばれていました)
- [組織アカウント](#orgauth)(Windows Server Active Directory または Azure Active Directory)
- [Windows 認証](#winauth)(イントラネット)

![認証のダイアログ ボックスを構成します。](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>認証なし

選択した場合**認証なし**、サンプル アプリケーションにログインするための web ページが表示されなく、いいえログインしているユーザーを示す UI、なしのエンティティ クラス メンバーシップ データベース、およびメンバーシップ データベースの接続文字列。

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>個々 のユーザー アカウント

選択した場合**個々 のユーザー アカウント**ユーザーの認証用 ASP.NET Id (以前は ASP.NET メンバーシップと呼ばれます) を使用するサンプル アプリケーションに構成されます。 ASP.NET Identity は、Facebook、Google、Microsoft アカウント、Twitter などのソーシャル プロバイダーでサインインして、またはサイトのユーザー名とパスワードを作成することで、アカウントを登録するユーザーを有効します。 ASP.NET Identity のユーザー プロファイルの既定のデータ ストアは、実稼働サイトの SQL Server または Azure SQL Database に配置できる SQL Server LocalDB データベースです。

これらの機能では Visual Studio 2013 では、Visual Studio 2012 のものと同じですが、ASP.NET メンバーシップ システムの基になるコードを書き直しました。 新しいコード ベースの長所を以下に示します。

- 新しいメンバーシップ システムがに基づいて[OWIN](http://owin.org/) ASP.NET フォーム認証モジュールではなくです。 これは、IIS では、Web フォームまたは MVC を使用しているか、Web API または SignalR を自己ホストしているかどうかは、同一の認証メカニズムを使用できることを意味します。
- 新しいメンバーシップ データベースは Entity Framework Code First で管理され、すべてのテーブルは変更可能なエンティティ クラスで表されます。 つまり、データベース スキーマとプロファイルに関連する web UI を独自のニーズに合わせて簡単にカスタマイズすることができます、および、Code First Migrations を使用して、更新プログラムを簡単に展開することができます。

新しいメンバーシップ システムは、新しいテンプレートに自動的に実装して、.NET 4.5 を対象とするすべてのプロジェクトで手動で実装またはそれ以降であることができます。

ASP.NET Identity は、これは主に外部の顧客のインターネット web サイトを作成する場合は、適切な選択です。 組織は Active Directory を使用してや Office 365 とするが、従業員やビジネス パートナーのシングル サインオンを有効にするプロジェクトを作成する場合、**組織アカウント**オプションは、ことをお勧めします。

個々 のユーザー アカウント オプションの詳細については、次のリソースを参照してください。

- [www.asp.net/identity](../../../identity/index.md)です。 ASP.NET web サイトで ASP.NET Id に関するドキュメントです。
- [ASP.NET MVC 5 アプリを作成すると Facebook、Google OAuth2 OpenID サインオン](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)です。 ユーザー プロファイル データをカスタマイズする方法も示します。
- [Web API の外部認証サービス](../../../web-api/overview/security/external-authentication-services.md)
- [Visual Studio 2013 で ASP.NET アプリケーションに外部ログインを追加します。](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>組織のアカウント

選択した場合**組織アカウント**、サンプル アプリケーションは、Azure Active Directory (Azure AD、Office 365 を含む) 内のユーザー アカウントに基づく認証に Windows Identity Foundation (WIF) を使用するように構成されますかWindows Server Active Directory です。 詳細については、次を参照してください。[組織アカウントの認証オプション](#orgauthoptions)このトピックで後述します。

<a id="winauth"></a>
### <a name="windows-authentication"></a>Windows 認証

選択した場合**Windows 認証**、サンプル アプリケーションは、認証に Windows 認証の IIS モジュールを使用するように構成されます。 アプリケーションの Windows にログインしているが、されませんユーザー登録を含めるか、またはログ UI をローカル コンピューター アカウントまたは Active directory ドメインとユーザー ID が表示されます。 このオプションは、イントラネットの web サイト向けです。

またはを選択して AD の認証を使用するイントラネット サイトを作成することができます、[オンプレミス組織アカウントの下にあるオプション](#orgauthonprem)です。 内部設置型のオプションは、Windows 認証モジュールではなく、Windows Identity Foundation (WIF) を使用します。 内部設置型オプションを設定するために必要な追加の手順が、WIF には、Windows 認証モジュールでは使用できない機能が有効になります。 たとえば、WIF では、Active Directory とディレクトリ データのクエリでアプリケーションへのアクセスを構成できます。

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>組織アカウントの認証オプション

**認証を構成する**ダイアログには、Azure Active Directory (Azure AD、Office 365 を含む) または Windows Server ・ Active Directory (AD) アカウントの認証のいくつかのオプション。

- [クラウドに 1 つの組織](#orgauthsingle)(Azure AD、または Azure AD とディレクトリの統合を使用して AD)
- [クラウド - 複数の組織](#orgauthmulti)(Azure AD、または Azure AD とディレクトリの統合を使用して AD)
- [内部設置型](#orgauthonprem)(AD)

Azure AD のオプションのいずれかの操作にはまだ、アカウントを持っていない場合[ここをクリックして、Azure AD アカウントにサインアップする](https://go.microsoft.com/fwlink/?LinkId=309942)です。

> [!NOTE]
> Azure AD のオプションのいずれかを選択した場合は、プロジェクトには、データベースが必要です。 され、Azure AD テナントのグローバル管理者アカウントにサインインする必要があります。 組織アカウントのユーザー名とパスワードを入力してください (たとえば、 admin@contoso.onmicrosoft.com)、Azure AD テナントの管理者権限を持ちます。
> 
> **Microsoft アカウントの資格情報を入力しない (たとえば、 contoso@hotmail.com) サイン イン ダイアログ ボックス。**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>クラウドに 1 つの組織の認証

![1 つの組織の認証](creating-web-projects-in-visual-studio/_static/image24.png)

1 つの Azure AD で定義されているユーザー アカウントの認証を有効にする場合は、このオプションを選択[テナント](https://technet.microsoft.com/library/jj573650.aspx)です。 たとえば、サイトが contoso.com と、利用可能になります、contoso.onmicrosoft.com テナントでは、Contoso 社の従業員にします。 アプリケーションにアクセスするには、他のテナントからユーザーを許可する Azure AD を構成することはできません。

#### <a name="domain"></a>ドメイン

たとえば、アプリケーションを設定する Azure AD ドメインを入力:`contoso.onmicrosoft.com`です。 ある場合、[カスタム ドメイン](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/)など`contoso.com`の代わりに`contoso.onmicrosoft.com`、ここに入力することができます。

#### <a name="access-level"></a>アクセス レベル

アプリケーションは、クエリまたは Graph API を使用して、ディレクトリ情報を更新を選択する必要がある場合**でのシングル サインオン、ディレクトリ データの読み取り**または**でのシングル サインオン、データの読み取りと書きディレクトリ**です。 それ以外の場合、選択**でのシングル サインオン**です。 詳細については、次を参照してください。[アプリケーション アクセス レベル](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels)と[クエリ Azure AD Graph API を使用して](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx)です。

#### <a name="application-id-uri"></a>アプリケーション ID の URI

既定では、プロジェクト名を Azure AD ドメインに追加することによって、アプリケーション ID URI を作成します。 たとえば、プロジェクト名が`Example`あり、ドメインが`contoso.onmicrosoft.com`、アプリケーション ID URI になります`https://contoso.onmicrosoft.com/Example`です。 アプリケーション ID の URI を手動で指定する場合は、展開、**オプションより**セクションし、テキスト ボックスに、アプリケーション ID URI を入力します。 アプリケーション ID URI が始まる必要があります`https://`です。

既定が Azure AD で既にプロビジョニングされているアプリケーションに、1 つとして、プロジェクトの Visual Studio を使用している同じアプリケーション ID URI がある場合、プロジェクトを接続する新しい 1 つのプロビジョニングの代わりに既存のアプリケーションにします。 その場合はプロビジョニングする新しいアプリケーションを実行する場合に、オフ、**同じ ID を持つ 1 つが既に存在する場合は、アプリケーションのエントリを上書き**チェック ボックスをオンします。

場合、**上書き** チェック ボックスをオフにすると、および Visual Studio が、同じアプリケーション ID URI に既存のアプリケーションを検出、番号を使用しようとした URI に追加して、新しい URI を作成します。 たとえば、プロジェクト名が`Example`、テキスト ボックスを空白のままにする、オフにする、**上書き**URI を持つアプリケーションは既にチェック ボックスをオンし、Azure AD テナント`https://contoso.onmicrosoft.com/Example`です。 アプリケーション ID URI と同様に、新しいアプリケーションをプロビジョニングする場合は、`https://contoso.onmicrosoft.com/Example_20130619330903`です。

#### <a name="provisioning-the-application-in-azure-ad"></a>Azure AD でアプリケーションのプロビジョニング

Azure AD でアプリケーションをプロビジョニングまたはプロジェクトを既存のアプリケーションに接続をするために Visual Studio は、ドメインのグローバル管理者の資格情報が必要です。 クリックすると**OK**で、**認証を構成する**ダイアログ ボックスで、ユーザー名と指定したドメインのグローバル管理者のパスワードを求めるメッセージが表示されます。 その後をクリックすると**プロジェクトの作成**で、**新しい ASP.NET プロジェクト**ダイアログ ボックスで、Visual Studio で Azure AD でアプリケーションをプロビジョニングします。 このプロセスの一部として Visual Studio クライアント シークレットの値に埋め込みます作成後に 1 年間の有効期限を Web.config ファイルに注意してください。

使用するアプリケーションを作成する方法については**クラウドに 1 つの組織**認証では、次のリソースを参照してください。

- [Azure の認証](../2012/windows-azure-authentication.md)
- [Azure AD を使用して、Web アプリケーションへのサインオンの追加](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Azure Active Directory を使った ASP.NET アプリの開発](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Azure AD での ASP.NET Web API をセキュリティで保護し、Microsoft OWIN コンポーネント](https://msdn.microsoft.com/magazine/dn463788.aspx)

Visual Studio 2013 の場合です。 まだ更新されていない、チュートリアルどのようなチュートリアルに指示する手動で行うは、Visual Studio 2013 で自動化されています。

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>クラウド - 複数の組織の認証

![複数の組織の認証](creating-web-projects-in-visual-studio/_static/image25.png)

複数の Azure AD で定義されているユーザー アカウントの認証を有効にする場合は、このオプションを選択[テナント](https://technet.microsoft.com/library/jj573650.aspx)です。 たとえば、サイトが contoso.com と、利用可能になります、contoso.onmicrosoft.com テナントでは、Contoso 社の従業員と fabrikam.onmicrosoft.com テナントでは、Fabrikam の会社の従業員にします。

入力した設定とプロビジョニング ステップ アプリケーションに似ています[1 つの組織認証](#orgauthsingle)です。

使用するアプリケーションを作成する方法については**クラウド - 複数の組織**認証では、次のリソースを参照してください。

- [Azure Active Directory、ASP.NET と簡単に Web アプリ統合&amp;Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) Active Directory チームのブログです。
- [Azure AD でのマルチ テナント Web アプリケーションの開発](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx)チュートリアルです。 このチュートリアルは、for Visual Studio 2013 はまだ更新されていません。どのようなチュートリアルの指示に従って手動で行うは、Visual Studio 2013 で自動化されています。
- [サインインする前に、独自の複数の組織 ASP.NET アプリにサインアップする必要がある](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/)です。 一般的な問題のユーザーを解決する方法を説明する Vittorio Bertocci ブログは、マルチ組織認証を使用するプロジェクトを作成する場合に発生します。

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>オンプレミス組織の認証

![オンプレミス組織の認証](creating-web-projects-in-visual-studio/_static/image26.png)

Windows Server Active Directory (AD) で定義されているユーザー アカウントの認証を有効にして、Azure AD を使用しない場合は、このオプションを選択します。 このオプションを使用すると、イントラネット サイト、またはインターネット サイトを作成します。 インターネット サイトでの Active Directory フェデレーション サービス (ADFS) を使用して、AD にアクセスできるようにします。 詳細については、次を参照してください。 [Visual Studio 2013 で ASP.NET で、オンプレミス組織認証オプション (ADFS) を使用して](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)です。

イントラネット サイトには、別の方法として選択できます[Windows 認証](#winauth)このオプションの代わりにします。 Windows 認証 オプションのメタデータ ドキュメント URL を指定する必要はありません。 ただし、Windows 認証で得られなければ機能 Active Directory でアプリケーションのアクセスを制御するか、ディレクトリ データをクエリします。

#### <a name="on-premises-authority"></a>内部設置型の権限

メタデータ ドキュメントを指す URL を入力します。 メタデータ ドキュメントには、機関の座標が含まれています。 アプリケーションはこれらのコーディネートを使用して web サインオン フローします。

#### <a name="application-id-uri"></a>アプリケーション ID の URI

AD を使用してこのアプリケーションを識別または Visual Studio で 1 つを作成できるようにする場合は空白のままにしている一意の URI を提供します。

<a id="nextsteps"></a>
## <a name="next-steps"></a>次の手順

このドキュメントには、Visual Studio 2013 で新しい ASP.NET web プロジェクトを作成するための基本的なヘルプが提供されます。 Web 開発用の Visual Studio の使用に関する詳細については、次を参照してください。 [ https://www.asp.net/visual-studio/](../../index.md)です。
