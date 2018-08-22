---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: プログラミングの ASP.NET Web Pages (Razor) を使用して Visual Studio |Microsoft Docs
author: tfitzmac
description: この付録では、Visual Studio 2010 または Visual Web Developer 2010 Express を Razor 構文を使用して ASP.NET Web Pages をプログラムに使用する方法について説明します。
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 41cb1048b9dab21516e38cfff0772b8b690d474f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835160"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Visual Studio を使用して ASP.NET Web ページ (Razor) のプログラミング
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、Visual Studio または Visual Web Developer Express を web サイトの ASP.NET Web Pages (Razor) のプログラムに使用する方法について説明します。
> 
> 学習内容
> 
> - Visual Studio のバージョンの ASP.NET Web ページを操作する (ある場合) をインストールするために必要とします。
> - Visual Web Developer 2010 Express を ASP.NET Web Pages のサポートを追加する方法。
> - Visual Studio で ASP.NET Razor ページ, IntelliSense や、デバッガーを使用する機能を使用する方法。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2、Visual Studio 2012、Visual Studio 2010、および WebMatrix 2 により連携します。


WebMatrix またはその他の多くのコード エディターを使用して、Razor 構文を使用する ASP.NET Web pages をプログラミングできます。 さまざまな種類のアプリケーション (web サイトがだけでなく) を作成するための強力なツール一式を提供するすべての機能を備えた統合開発環境 (IDE) である Microsoft Visual Studio を使用することもできます。 ASP.NET Razor ページを使用するか、Visual Studio のエディションのいずれかまたは無料でく[Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express)エディション。

ASP.NET Razor web ページを使用したプログラミングの Visual Studio が提供されている 2 つの特に便利な機能は次のとおりです。

- *IntelliSense*します。 Visual Studio に組み込まれている IntelliSense 機能は、WebMatrix では、IntelliSense のより包括的なです。
- *デバッガー*します。 デバッガーでは、実行、変数を調べること、およびコード 1 行ずつステップ実行中にプログラムを停止することで、コードのトラブルシューティングを行うことができます。

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Visual Studio を使用して、さまざまなバージョンの ASP.NET Web ページ

Visual Studio 2012 と Visual Studio 2013 には、ASP.NET Web Pages のサポートが含まれます。 (ASP.NET Web Pages をサポートするために必要なパッケージは、Visual Studio をインストールするときにインストールされます)。

Visual Studio 2010 サポートは含まれません既定の ASP.NET Web ページ。 Visual Studio 2010 では、ASP.NET Web Pages を使用するには、ASP.NET MVC パッケージをインストールする必要があります。 ASP.NET Web Pages 2 を取得するには、ASP.NET MVC 4 をインストールします。

次の表では、異なるバージョンの Visual Studio で ASP.NET Web Pages のサポートをまとめたものです。

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web ページ 2** | ASP.NET MVC 4 をインストールします。 | (含まれています) | (含まれています) |
| **ASP.NET Web ページ 3** |  | 更新プログラムを ASP.NET Web ページから 3 つの NuGet | (含まれています) |

Visual Studio 2010 を使用する、次を参照してください。 [Visual Studio 2010 で ASP.NET Web Pages のサポートのインストール](#vs2010support)します。

## <a name="launching-visual-studio-from-webmatrix"></a>WebMatrix から Visual Studio を起動します。

WebMatrix で、プロジェクトの開始が Visual Studio に切り替える場合、WebMatrix が簡単に Visual Studio でプロジェクトを開く ボタンを提供します。 有効にするには、このボタンは、コンピューターにインストールされている Visual Studio が必要です。 次の図では、WebMatrix で、ボタンを示します。

![Visual Studio を起動します。](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

ボタンをクリックすると、プロジェクトは Visual Studio で開きます。 切り替えることができますを行き来 WebMatrix や Visual Studio は問題なく。 すべてのファイルが他の環境で変更された最新の変更の取得を再読み込みする必要がある場合に通知されます。

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Visual Studio で ASP.NET Razor のサイトを作成します。

Visual Studio で ASP.NET Razor の web サイトを作成します。

1. Visual Studio または Visual Web Developer を開始します。
2. **ファイル** メニューのをクリックして**新しい Web サイト**します。

    ![新しい web サイトを作成します。](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. **新しい Web サイト** ダイアログ ボックスで、(Visual c# または Visual Basic) を使用する言語を選択します。
4. 選択、 **ASP.NET Web サイト (Razor)** テンプレート。

    ![razor のサイト](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. **[OK]** をクリックします。

新しいプロジェクトが存在し、既定の web ページを簡単には格納されます。

### <a name="using-intellisense"></a>Using IntelliSense

サイトを作成するので、Visual Studio で IntelliSense がどのように動作するかが確認できます。

1. 作成した web サイトで開く、 *Default.cshtml*ページ。
2. 後に、 `<h3>`  ページで、タグを入力`@ServerInfo.`(ドットを含む)。 IntelliSense の使用可能なメソッドの表示方法に注意してください、`ServerInfo`ドロップダウン リストでヘルパー。 

    ![intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. 選択、`GetHtml`メソッドを一覧し、Enter キーを押します。 IntelliSense は、メソッドで自動的に入力されます。 (C# での任意のメソッドに追加する必要がありますと`()`メソッドの後の文字)。  
   完成したコード、`GetHtml`メソッドは次のようになります。  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Ctrl + f5 キーを押してページを実行します。 これは、ページがどのようにと、ブラウザーに表示されます。 

    ![ブラウザーで既定のページ](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. ブラウザーを閉じます。

### <a name="using-the-debugger"></a>デバッガーを使用します。

1. 上部にある、 *Default.cshtml*ページで後で始まる行`Page.Title`、次のコード行を追加します。 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. エディター、コードの左側の灰色の余白を追加するにはこの新しい行の横にあるクリックして、*ブレークポイント*します。 ブレークポイントは、何が起こっているかを確認できるようにその時点で、プログラムの実行を停止するデバッガーを示すマーカーです。

    ![ブレークポイントの設定](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. 呼び出しを削除、`ServerInfo.GetHtml`メソッド呼び出しを追加し、`@myTime`代わりに変数。 この呼び出しは、新しい行のコードによって返される現在の時刻値を表示します。
4. F5 キーを押して、デバッガーでページを実行します。 ページは、設定したブレークポイントで停止します。 次の図は、ページ内でどのようにエディター (黄色) でブレークポイントを設定しました。 

    ![デバッグのブレークポイント](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. [デバッグ] ツールバーをクリックして、**ステップ イン**次のコード行を実行するボタン (または F11 キーを押します)。 このボタンをクリックするたびに、実行を次のコード行に進みます。

    ![ステップ イン ボタン](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. 値を調べて、`myTime`変数の上にマウス ポインターを保持しているかに表示される値を調べることによって、**ローカル**と**呼び出し履歴**windows。 Visual Studio では、変数の値を表示します。

    ![表示時刻の値](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. 変数を調べると、コードのステップが完了したら、f5 キーを押して 1 行ずつ停止することがなく、ページの実行を継続します。 すべてのコードのステップが完了したら、ブラウザーにページが表示されます。

デバッガーについてと、Visual Studio でコードをデバッグする方法についての詳細についてを参照してください。[チュートリアル: Visual Web Developer で Web ページのデバッグ](https://msdn.microsoft.com/library/z9e7w6cs.aspx)します。

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Visual Studio を使用した ASP.NET MVC プロジェクトでの Razor の使用

ASP.NET MVC プロジェクトで幅広く、Razor 構文が使用されます。 MVC は、動的な web サイトを構築する、パターン ベースの強力な方法です。 ASP.NET Web Pages サイトの保守が困難になると、ASP.NET MVC アプリケーションに変換することを検討する可能性があります。 MVC アプリケーションの作成の例は、次を参照してください。 [ASP.NET MVC 5 の概要](../../../mvc/overview/getting-started/introduction/getting-started.md)します。

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Visual Studio 2010 での ASP.NET Web ページのサポートのインストール

このセクションでは、Visual Studio の Visual Web Developer Express 2010 と ASP.NET Web ページのツールをインストールする方法を示します。

1. Web Platform Installer がまだしていない場合は、次の URL からダウンロードします。

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Web Platform Installer を実行します。
3. をクリックして、**製品**タブ。

    ![WebPI 製品 タブ](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. 検索**ASP.NET MVC 4** (ASP.NET Web ページ 2) の順にクリックします**追加**します。 これらの製品には、ASP.NET Razor web サイトを構築するための Visual Studio ツールが含まれます。

    ![WebPi のインストール オプション](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. クリックして**インストール**インストールを完了します。
