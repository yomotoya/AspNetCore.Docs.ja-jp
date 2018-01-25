---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: "ASP.NET mvc 4 |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルは Visual Studio 2013 を使用して、ここで使用可能な場合は、更新されたバージョンです。 新しいチュートリアルを使用して ASP.NET MVC 5 は、t に対して多くの機能強化を提供しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 92d9e583b6c26fa8c928d33e14593d280702a269
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="intro-to-aspnet-mvc-4"></a>ASP.NET mvc 4
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは使用可能な場合、更新されたバージョン[ここ](../../getting-started/introduction/getting-started.md)を使用して[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)です。 新しいチュートリアルでは、ASP.NET MVC 5 は、このチュートリアルを超える多くの機能強化を提供を使用します。
> 
> このチュートリアルが Microsoft を使用して ASP.NET MVC 4 Web アプリケーションの構築の基礎を教える[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)または Visual Web Developer 2010 Express Service Pack 1。 チュートリアルを完了できるものをインストールする必要はありませんが、visual Studio 2012 がお勧めします。 Visual Studio 2010 を使用している場合は、以下のコンポーネントをインストールする必要があります。 次のリンクをクリックしてそれらのすべてをインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 4 の WPI インストーラー](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、インストール、 [ASP.NET MVC 4 の WPI インストーラー](https://go.microsoft.com/fwlink/?LinkId=243392)と: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> C# ソース コードの Visual Web Developer プロジェクトは、このトピックを使用できます。 [C# バージョンをダウンロード](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip)です。
> 
> このチュートリアルでは、Visual Studio でアプリケーションを実行します。 利用できるアプリケーション、インターネット経由でホスティング プロバイダーに展開することで。 マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、[試用アカウントを Windows Azure の無料](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)です。 Windows Azure Web サイトへの Visual Studio web プロジェクトを配置する方法については、次を参照してください。[を作成すると、ASP.NET web サイトと Visual Studio での SQL データベースを展開](https://docs.microsoft.com/dotnet/azure/)です。 そのチュートリアルでは、Entity Framework Code First Migrations を使用して Windows Azure SQL データベース (以前の SQL Azure) に、SQL Server データベースを展開する方法も示します。
> 
> このチュートリアルは、Rick Anderson によって書き込まれました ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。


## <a name="what-youll-build"></a>新機能のビルドします。

> [!NOTE]
> このチュートリアルでは使用可能な場合、更新されたバージョン[ここ](../../getting-started/introduction/getting-started.md)を使用して[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)です。 新しいチュートリアルでは、ASP.NET MVC 5 は、このチュートリアルを超える多くの機能強化を提供を使用します。


作成、編集、検索、およびデータベースからムービーの一覧表示をサポートする単純なムービー リスト アプリケーション実装します。 アプリケーションのビルドの 2 つのスクリーン ショットを次に示します。 データベースからムービーの一覧を表示するページが含まれています。

![](intro-to-aspnet-mvc-4/_static/image1.png)

アプリケーションでは、追加、編集、およびムービー、だけでなく 1 つずつに関する詳細を参照してくださいを削除することもできます。 データ エントリのすべてのシナリオには、データベースに格納されたデータが正しいことを確認する検証が含まれます。

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>作業の開始

Visual Studio Express 2012 または Visual Web Developer 2010 Express を実行して開始します。 この系列を Visual Studio Express 2012 がスクリーン ショットのほとんどは、Visual Studio 2010 SP1、Visual Studio 2012、Visual Studio Express 2012 または Visual Web Developer 2010 Express を使用してこのチュートリアルを完了できます。 選択**新しいプロジェクト**から、**開始**ページ。

Visual Studio は、IDE、または統合開発環境です。 Microsoft Word を使用してドキュメントを作成するのにのと同じようにアプリケーションを作成するのに、IDE を使用します。 Visual Studio を使用できるさまざまなオプションを示す上部にツールバーがあります。 IDE でのタスクを実行する別の方法を提供するメニューもあります。 (選択する代わりに、たとえば、**新しいプロジェクト**から、**開始** ページで、メニューを使用して選択**ファイル** &gt; **新しいプロジェクト**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>初めてアプリケーションの作成

プログラミング言語と Visual Basic または Visual c# を使用してアプリケーションを作成することができます。 左側の Visual c# を選択し、 **ASP.NET MVC 4 Web Application**です。 プロジェクトの名前を付けます&quot;MvcMovie&quot;  をクリックし、 **ok**です。

![](intro-to-aspnet-mvc-4/_static/image4.png)

**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**です。 ままにして**Razor**既定のビュー エンジンとして。

![](intro-to-aspnet-mvc-4/_static/image5.png)

**[OK]**をクリックします。 Visual Studio では、作成した ASP.NET MVC プロジェクトの既定のテンプレートが使用されるため、何もせず作業アプリケーションが現在ある! これは、単純な&quot;Hello World!&quot;プロジェクト、およびそのアプリケーションを起動するに適しています。

![](intro-to-aspnet-mvc-4/_static/image6.png)

**[デバッグ]** メニューの **[デバッグの開始]** をクリックします。

![](intro-to-aspnet-mvc-4/_static/image7.png)

デバッグを開始するキーボード ショートカットは、f5 キーであることを確認します。

F5 キーでは、IIS Express を起動し、web アプリケーションを実行する Visual Studio が発生します。 Visual Studio は、ブラウザーを起動し、アプリケーションのホーム ページが開きます。 ブラウザーのアドレス バーに表示される`localhost`などのメカニズムではありませんし`example.com`です。 これはため`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。 Visual Studio web プロジェクトの実行時に、ランダムなポートが、web サーバーに使用されます。 次の図では、ポート番号は、41788 です。 アプリケーションを実行するときに、別のポート番号をおそらく表示されます。

![](intro-to-aspnet-mvc-4/_static/image8.png)

すぐこの既定テンプレートを使用して自宅、連絡先、についてのページです。 また、登録および、ログ記録のサポートを提供し、Facebook、Twitter へのリンクします。 次の手順では、このアプリケーションの動作を変更し、ASP.NET MVC について少し説明します。 ブラウザーを閉じて、いくつかのコードを変更してみましょう。

>[!div class="step-by-step"]
[次へ](adding-a-controller.md)
