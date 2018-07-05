---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: ASP.NET MVC 3 (c#) の概要 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: b0ff8da1911b36e6e74e5c7057f27d891ad57f61
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832290"
---
<a name="intro-to-aspnet-mvc-3-c"></a>ASP.NET MVC 3 (c#) の概要
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全ではるかに簡単に従うしより多くの機能を示します。
> 
> 
> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 始める前に、以下の前提条件がインストールされていることを確認します。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。 または、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。
> 
> C# ソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。 [C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。 Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルの。


## <a name="what-youll-build"></a>構築します

作成、編集、およびデータベースからムービーを一覧表示をサポートする簡単なムービー リスト アプリケーションを実装します。 アプリケーションをビルドの 2 つのスクリーン ショットを次に示します。 データベースからムービーの一覧を表示するページが含まれています。

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

アプリケーションでは、追加、編集、および個別の詳細を参照するくださいと同様に、ムービーを削除することもできます。 すべてのデータ入力シナリオには、データベースに格納されたデータが正しいことを確認する検証が含まれます。

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>学習内容

学習内容を次に示します。

- 新しい ASP.NET MVC プロジェクトを作成する方法。
- ASP.NET MVC のコント ローラーとビューを作成する方法。
- Entity Framework Code First パラダイムを使用して新しいデータベースを作成する方法。
- 取得する方法とデータを表示します。
- データを編集し、データの検証を有効にする方法。

## <a name="getting-started"></a>作業の開始

Visual Web Developer 2010 Express ("Visual Web Developer"短縮形) を実行して起動し、選択**新しいプロジェクト**から、**開始**ページ。

Visual Web Developer は、IDE、または統合開発環境です。 Microsoft Word を使用してドキュメントを記述するのにのと同じようにアプリケーションを作成、IDE を使用します。 Visual Web Developer を使用できるさまざまなオプションを示す上部のツールバーがあります。 また、IDE でタスクを実行する別の方法を提供するメニューがあります。 (選択ではなく、たとえば、**新しいプロジェクト**から、**開始** ページで、メニューを使用して選択**ファイル** &gt; **の新しいプロジェクト**.)

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>最初のアプリケーションを作成します。

プログラミング言語として Visual Basic または Visual c# を使用してアプリケーションを作成することができます。 左側の Visual c# を選択し、 **ASP.NET MVC 3 Web アプリケーション**します。 クリックして、プロジェクトに"MvcMovie" **OK**します。 (Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルの)。

![](intro-to-aspnet-mvc-3/_static/image5.png)

**新しい ASP.NET MVC 3 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**します。 確認**使用 HTML5 マークアップ**して**Razor**既定のビュー エンジンとして。

![](intro-to-aspnet-mvc-3/_static/image6.png)

**[OK]** をクリックします。 Visual Web Developer では、作成した ASP.NET MVC プロジェクトの既定のテンプレートが使用されるので何もせず、実用的なアプリケーションを今すぐ必要する! これは、単純です"Hello World!" プロジェクト、およびそのアプリケーションにお勧めです。

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

**[デバッグ]** メニューの **[デバッグの開始]** をクリックします。

![](intro-to-aspnet-mvc-3/_static/image9.png)

デバッグを開始するキーボード ショートカットは、f5 キーであることを確認します。

F5 キーは、Visual Web Developer を開発 web サーバーを起動し、web アプリケーションを実行します。 Visual Web Developer は、ブラウザーを起動し、アプリケーションのホーム ページを開きます。 ブラウザーのアドレス バーが表示されますが`localhost`ようなものではありません`example.com`します。 だ`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。 Visual Web Developer で web プロジェクトを実行すると、web サーバーのランダムなポートが使用されます。 次の図では、ランダムのポート番号は、43246 です。 アプリケーションを実行するときに、別のポート番号をおそらく表示されます。

![](intro-to-aspnet-mvc-3/_static/image10.png)

すぐに使えるこの既定のテンプレートは 2 つのページにアクセスして、基本的なログイン ページ。 次の手順では、このアプリケーションの動作を変更し、プロセスで ASP.NET MVC についてもう少し説明します。 ブラウザーを閉じて、いくつかのコードを変更してみましょう。

> [!div class="step-by-step"]
> [次へ](adding-a-controller.md)
