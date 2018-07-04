---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: ASP.NET MVC 4 の概要 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Visual Studio 2013 を使用して、ここで使用可能な場合は、更新されたバージョン。 新しいチュートリアルでは、t に多くの機能強化を提供する ASP.NET MVC 5 を使用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 51e469e131b083325bc565530d91173887769ab0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395815"
---
<a name="intro-to-aspnet-mvc-4"></a>ASP.NET MVC 4 の概要
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、使用可能な場合は、更新されたバージョン[ここ](../../getting-started/introduction/getting-started.md)を使用して[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)します。 新しいチュートリアルでは、このチュートリアルで多くの機能強化を提供する ASP.NET MVC 5 を使用します。
> 
> このチュートリアルが Microsoft を使用して ASP.NET MVC 4 Web アプリケーションの構築の基礎を講義[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)または Visual Web Developer 2010 Express Service Pack 1。 Visual Studio 2012 がお勧め、チュートリアルを完了する何もインストールする必要はありません。 Visual Studio 2010 を使用している場合は、以下のコンポーネントをインストールする必要があります。 次のリンクをクリックして、それらのすべてをインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 4 の WPI インストーラー](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、インストール、 [ASP.NET MVC 4 の WPI インストーラー](https://go.microsoft.com/fwlink/?LinkId=243392)と: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> C# ソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。 [C# バージョンをダウンロード](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip)します。
> 
> チュートリアルでは、Visual Studio でアプリケーションを実行します。 利用できるアプリケーション、インターネット経由でホスティング プロバイダーへのデプロイで。 マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、[無料試用版アカウントを Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)します。 Visual Studio web プロジェクトを Windows Azure の Web サイトにデプロイする方法については、次を参照してください。[作成し、ASP.NET web サイトと Visual Studio での SQL Database のデプロイ](https://docs.microsoft.com/dotnet/azure/)します。 そのチュートリアルでは、Entity Framework Code First Migrations を使用して Windows Azure SQL Database (旧 SQL Azure) に、SQL Server データベースをデプロイする方法も示します。
> 
> このチュートリアルの執筆者は、Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。


## <a name="what-youll-build"></a>構築します

> [!NOTE]
> このチュートリアルでは、使用可能な場合は、更新されたバージョン[ここ](../../getting-started/introduction/getting-started.md)を使用して[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)します。 新しいチュートリアルでは、このチュートリアルで多くの機能強化を提供する ASP.NET MVC 5 を使用します。


作成、編集、検索、およびデータベースからムービーを一覧表示をサポートする単純なムービーの一覧をアプリケーションを実装します。 アプリケーションをビルドの 2 つのスクリーン ショットを次に示します。 データベースからムービーの一覧を表示するページが含まれています。

![](intro-to-aspnet-mvc-4/_static/image1.png)

アプリケーションでは、追加、編集、および個別の詳細を参照するくださいと同様に、ムービーを削除することもできます。 すべてのデータ入力シナリオには、データベースに格納されたデータが正しいことを確認する検証が含まれます。

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>作業の開始

Visual Studio Express 2012 または Visual Web Developer 2010 Express を実行して開始します。 このシリーズを Visual Studio Express 2012 がスクリーン ショットのほとんどは、Visual Studio 2010 SP1、Visual Studio 2012、Visual Studio Express 2012 または Visual Web Developer 2010 Express で、このチュートリアルを完了できます。 選択**新しいプロジェクト**から、**開始**ページ。

Visual Studio は、IDE、または統合開発環境です。 Microsoft Word を使用してドキュメントを記述するのにのと同じようにアプリケーションを作成、IDE を使用します。 Visual Studio を使用できるさまざまなオプションを示す上部のツールバーがあります。 また、IDE でタスクを実行する別の方法を提供するメニューがあります。 (選択ではなく、たとえば、**新しいプロジェクト**から、**開始** ページで、メニューを使用して選択**ファイル** &gt; **の新しいプロジェクト**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>最初のアプリケーションを作成します。

プログラミング言語として Visual Basic または Visual c# を使用してアプリケーションを作成することができます。 左側の Visual c# を選択し、 **ASP.NET MVC 4 Web アプリケーション**します。 プロジェクトに名前を&quot;MvcMovie&quot;  をクリックし、 **OK**します。

![](intro-to-aspnet-mvc-4/_static/image4.png)

**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**します。 まま**Razor**既定のビュー エンジンとして。

![](intro-to-aspnet-mvc-4/_static/image5.png)

**[OK]** をクリックします。 Visual Studio は、何もせず、実用的なアプリケーションが現在あるため、作成した ASP.NET MVC プロジェクトの既定のテンプレートを使用! これは、単純な&quot;Hello World!&quot;プロジェクト、およびそのアプリケーションを起動することをお勧めします。

![](intro-to-aspnet-mvc-4/_static/image6.png)

**[デバッグ]** メニューの **[デバッグの開始]** をクリックします。

![](intro-to-aspnet-mvc-4/_static/image7.png)

デバッグを開始するキーボード ショートカットは、f5 キーであることを確認します。

F5 キーは、Visual Studio IIS Express を起動して、web アプリケーションを実行します。 Visual Studio は、ブラウザーを起動し、アプリケーションのホーム ページを開きます。 ブラウザーのアドレス バーが表示されますが`localhost`ようなものではありません`example.com`します。 だ`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。 Visual Studio web プロジェクトを実行すると、web サーバーのランダムなポートが使用されます。 次の図では、ポート番号は、41788 です。 アプリケーションを実行するときに、別のポート番号をおそらく表示されます。

![](intro-to-aspnet-mvc-4/_static/image8.png)

すぐに使えるこの既定テンプレートを使用してホーム、連絡先についてのページ。 登録および、ログ記録のサポートを提供し、Facebook と Twitter へのリンクします。 次の手順では、このアプリケーションの動作を変更し、ASP.NET MVC についてもう少し説明します。 ブラウザーを閉じて、いくつかのコードを変更してみましょう。

> [!div class="step-by-step"]
> [次へ](adding-a-controller.md)
