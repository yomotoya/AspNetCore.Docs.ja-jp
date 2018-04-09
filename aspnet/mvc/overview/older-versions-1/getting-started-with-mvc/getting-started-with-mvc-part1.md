---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: ASP.NET mvc |Microsoft ドキュメント
author: shanselman
description: これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みする単純な web アプリケーションを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 476d832e389b9b5a26fe2d552ca648c79b100056
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc"></a>ASP.NET mvc
====================
によって[Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > このチュートリアルでは使用可能な場合、更新されたバージョン[ここ](../../getting-started/introduction/getting-started.md)を使用して[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)です。 新しいチュートリアルでは、ASP.NET MVC 5 は、このチュートリアルを超える多くの機能強化を提供を使用します。
> 
> 
> これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みを単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。


ASP.NET MVC Web アプリケーションを使用して最初を加えてみましょう[Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/)です。 少しのムービー リスト アプリケーションをお知らせを作成し、映画を一覧表示するしましょう。

## <a name="what-youll-build"></a>新機能のビルドします。

アプリケーションのビルドの 2 つのスクリーン ショットを次に示します。 さまざまな列を持つムービーの単純なテーブルがあります。

[![ムービーの一覧]-[Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

ムービーの一覧に追加できるように、フォームの作成が必要があります。

[![-ムービーを作成するには、Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>スキルの学習

このチュートリアルでは、Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 について説明します。

- 新しい ASP.NET MVC プロジェクトを作成する方法
- SQL Server の新しいデータベースを作成する方法
- ASP.NET MVC のコント ローラーとビューを作成する方法
- データ取得して表示する方法
- データを編集し、データの検証を有効にする方法
- データベース スキーマを更新する方法

## <a name="get-started"></a>開始するには

Visual Web Developer 2010 Express (名前を付けますが"VWD"今後) と新しいプロジェクトを選択を実行して、スタート画面から、開始します。

Visual Web Developer は、IDE、または開発者の統合環境です。 Microsoft Word を使用してドキュメントを作成するのにのと同じようにアプリケーションを作成するのに、IDE を使用します。 だけでなく、ファイルの選択を使用してもでしたメニューを使用できるさまざまなオプションを示す上部にあるツールバー |新規のプロジェクトです。

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>初めてアプリケーションの作成

Visual Basic または Visual c# を使用してアプリケーションを作成することができます。 ここでは、選択 Visual C# の場合、左側の「ASP.NET MVC 2 Web アプリケーション」を選択し、 「映画」をプロジェクトに名前し、[ok] をクリックします。

[![新しいプロジェクト](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

右側にあるすべてのファイルとフォルダーを表示、アプリケーションで、ソリューション エクスプ ローラーです。 大規模なウィンドウの中間には、コードを編集し、ほとんどの時間を費やす場所です。 Visual Studio では、作成した ASP.NET MVC プロジェクトの既定のテンプレートが使用されるため、何もせず作業アプリケーションが現在ある! これは、単純な"Hello World! プロジェクト、これは、アプリケーションを起動するに適してします。

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

ツールバーに「再生」ボタンを選択します。

![デバッグの開始](getting-started-with-mvc-part1/_static/image11.png)

緑色の矢印は、プログラムをコンパイルし、web ブラウザーで、アプリケーションを起動する右 をポイントすることをお勧めします。

*注: する代わりに、キーボードの f5 キーを押すか、選択デバッグ-&gt;[デバッグ] メニューからデバッグを開始します。*

開発 web サーバーを起動し、(はありません構成や手動手順がこれを有効にするために必要)、web アプリケーションを実行する Visual Web Developer は、なります。 ブラウザーを起動し、アプリケーションのホーム ページを参照するように構成されます。 下"localhost"、「example.com」のようなものでありブラウザーのアドレス バーに表示されるに注意してください。-ここでは、作成したアプリケーションを実行している自分のローカル コンピューターを常に localhost がポイントするためです。

[![ホーム ページ](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

すぐは、この既定のテンプレートは、する 2 つのページにアクセスし、基本的なログイン ページを示します。 このアプリケーションの動作を変更して、プロセスで ASP.NET MVC について少し説明しましょう。 ブラウザーを閉じて、いくつかのコードを変更することができます。

> [!div class="step-by-step"]
> [次へ](getting-started-with-mvc-part2.md)
