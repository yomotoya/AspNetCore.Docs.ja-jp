---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: プログラミングの ASP.NET Web Pages (Razor) を使用してクトリ |Microsoft ドキュメント
author: tfitzmac
description: この付録では、Razor 構文を使用するプログラミング ASP.NET Web Pages をな Visual Studio 2010 または Visual Web Developer 2010 Express を使用する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: eb17c8cc1fab5b552c8495e74bb86ae9dbc5b972
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896584"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Visual Studio を使用してプログラミングする ASP.NET Web Pages (Razor)
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、Visual Studio または Visual Web Developer Express を web サイトの ASP.NET Web Pages (Razor) のプログラムに使用する方法について説明します。
> 
> 学習する内容
> 
> - Visual Studio のバージョンの ASP.NET Web Pages を使用する (ある場合) をインストールするために必要とします。
> - Visual Web Developer 2010 Express を ASP.NET Web Pages のサポートを追加する方法です。
> - され、デバッガー、IntelliSense など、ASP.NET Razor ページを使用する Visual Studio の機能を使用する方法。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2、Visual Studio 2012、Visual Studio 2010、および WebMatrix 2 のでも動作します。


WebMatrix またはその他の多くのコード エディターを使用して、Razor 構文を使用する ASP.NET Web pages をプログラミングできます。 また、さまざまな種類のアプリケーション (だけでなく web サイト) を作成するための強力なツール セットを提供する全機能を備えた統合開発環境 (IDE) である Microsoft Visual Studio を使用することができます。 ASP.NET Razor ページを使用するには、Visual Studio の全エディションのいずれかを使用または無料[Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express)エディションです。

Visual Studio は、ASP.NET Razor web ページを使用したプログラミングを提供する 2 つの特に便利な機能は次のとおりです。

- *IntelliSense*です。 Visual Studio に組み込まれている、IntelliSense 機能は、WebMatrix では、IntelliSense より包括的なです。
- *デバッガー*です。 デバッガーでは、実行、変数を調べること、およびコードの 1 行ずつステップ実行中に、プログラムを停止することによって、コードのトラブルシューティングを行うことができます。

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>ASP.NET Web ページのさまざまなバージョンの Visual Studio の使用

Visual Studio 2012 と Visual Studio 2013 には、ASP.NET Web Pages のサポートが含まれます。 (ASP.NET Web Pages をサポートするために必要なパッケージがインストールされている Visual Studio をインストールするときにします。)

Visual Studio 2010 サポートは含まれません既定では ASP.NET Web Pages のです。 Visual Studio 2010 では、ASP.NET Web Pages を使用するには、ASP.NET MVC のパッケージをインストールする必要があります。 ASP.NET Web Pages 2 を取得するには、ASP.NET MVC 4 をインストールします。

次の表は、ASP.NET Web Pages の異なるバージョンの Visual Studio でのサポートをまとめたものです。

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web Pages 2** | ASP.NET MVC 4 をインストールします。 | (インクルードされる) | (インクルードされる) |
| **ASP.NET Web Pages 3** |  | 更新プログラムを ASP.NET Web Pages 3 から NuGet | (インクルードされる) |

Visual Studio 2010 を使用する、次を参照してください。 [Visual Studio 2010 の ASP.NET Web Pages のサポートのインストール](#vs2010support)です。

## <a name="launching-visual-studio-from-webmatrix"></a>WebMatrix から Visual Studio を起動します。

WebMatrix で、プロジェクトを開始する場合に、Visual Studio に切り替えする WebMatrix は、簡単に Visual Studio でプロジェクトを開く ボタンを提供します。 有効にするには、このボタンは、コンピューターにインストールされている Visual Studio が必要です。 次の図では、WebMatrix で、ボタンを示しています。

![Visual Studio を起動します。](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

ボタンをクリックすると、プロジェクトが Visual Studio で開きます。 切り替えることができます前後 WebMatrix や Visual Studio、問題なくです。 すべてのファイルが、他方の環境で変更された、最新の変更の取得を再読み込みする必要がある場合、通知されます。

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Visual Studio での ASP.NET Razor のサイトの作成

Visual Studio で、ASP.NET Razor web サイトを作成します。

1. Visual Studio または Visual Web Developer を開始します。
2. **ファイル** メニューをクリックして**新しい Web サイト**です。

    ![新しい web サイトを作成します。](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. **新しい Web サイト** ダイアログ ボックスで、(Visual c# または Visual Basic) を使用する言語を選択します。
4. 選択、 **ASP.NET Web サイト (Razor)** テンプレート。

    ![razor サイト](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. **[OK]** をクリックします。

新しいプロジェクトが存在し、いくつか作業を開始するための既定の web ページが表示されます。

### <a name="using-intellisense"></a>Using IntelliSense

これで、サイトを作成した、Visual Studio での IntelliSense の操作を確認できます。

1. 作成した web サイトで開く、 *Default.cshtml*ページ。
2. 後に、 `<h3>`  ページで、タグを入力`@ServerInfo.`(ドットを含む)。 IntelliSense で使用できるメソッドを表示する方法に注意してください、`ServerInfo`ドロップダウン リストでヘルパー。 

    ![intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. 選択、`GetHtml`し、Enter キーを押して、一覧からのメソッドです。 IntelliSense は、メソッドで自動的に入力します。 (C# での任意のメソッドに追加する必要がありますと`()`メソッドの後に文字です)。  
   完全なコード、`GetHtml`メソッドは次の例のようになります。  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Ctrl + f5 キーを押してページを実行します。 これは、ページの外観と、ブラウザーに表示されます。 

    ![ブラウザーで既定のページ](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. ブラウザーを閉じます。

### <a name="using-the-debugger"></a>デバッガーの使用方法

1. 上部にある、 *Default.cshtml*ページで後で始まる行`Page.Title`、次のコード行を追加します。 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. 追加するためにこの新しい行の横にあるをクリックして、エディターで、コードの左側の灰色の余白で、*ブレークポイント*です。 ブレークポイントは、何が起こっているかを確認できるようにその時点で、プログラムの実行を停止するデバッガーに指示するマーカーです。

    ![ブレークポイントの設定](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. 呼び出しを削除、`ServerInfo.GetHtml`メソッド呼び出しを追加し、`@myTime`代わりに変数。 この呼び出しは、新しい行のコードによって返される現在の時刻値を表示します。
4. F5 キーを押して、デバッガーでページを実行します。 ページは、設定したブレークポイントで停止します。 次の図は、ページ内でどのようにエディター (黄) 内のブレークポイントとします。 

    ![デバッグのブレークポイント](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. デバッグ ツールバーで、をクリックして、**ステップ イン**を次のコード行を実行するボタン (または F11 キーを押します)。 このボタンをクリックするたびに実行を次のコード行に進みます。

    ![ボタンにステップ インします。](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. 値を調べて、`myTime`変数の上にマウス ポインターを保持しているかに表示される値を調べることによって、**ローカル**と**呼び出し履歴**windows です。 Visual Studio では、変数の値を表示します。

    ![時間を示さない値](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. 変数を確認して、コードのステップ実行を完了したら、f5 キーを押して 1 行ずつ停止することがなく、ページの実行を継続します。 すべてのコードをステップ実行が完了したら、ブラウザーには、ページが表示されます。

詳細については、デバッガーおよび Visual Studio でコードをデバッグする方法について、次を参照してください。[チュートリアル: Visual Web Developer で Web ページをデバッグ](https://msdn.microsoft.com/library/z9e7w6cs.aspx)です。

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Visual Studio での ASP.NET MVC プロジェクトで Razor を使用します。

ASP.NET MVC プロジェクトで Razor 構文が広範囲に使用されます。 MVC は、動的な web サイトを構築する、パターンに基づく強力な方法です。 ASP.NET Web Pages サイトが管理するが困難になると、ASP.NET MVC アプリケーションに変換することを検討することができます。 MVC アプリケーションの作成の例は、次を参照してください。 [ASP.NET MVC 5 の概要](../../../mvc/overview/getting-started/introduction/getting-started.md)です。

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Visual Studio 2010 の ASP.NET Web Pages のサポートのインストール

このセクションでは、Visual Studio の Visual Web Developer Express 2010 と ASP.NET Web Pages のツールをインストールする方法を示します。

1. Web Platform Installer がいない場合は、次の URL からダウンロードします。

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Web Platform Installer を実行します。
3. クリックして、**製品**タブです。

    ![WebPI 製品 タブ](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. 検索**ASP.NET MVC 4** (ASP.NET Web Pages 2) の順にクリック**追加**です。 これらの製品には、ASP.NET Razor web サイトを構築するための Visual Studio のツールが含まれます。

    ![WebPi のインストール オプション](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. をクリックして**インストール**インストールを完了します。
