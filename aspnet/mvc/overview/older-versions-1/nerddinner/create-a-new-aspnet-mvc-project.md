---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: 新しい ASP.NET MVC プロジェクトの作成 |Microsoft Docs
author: microsoft
description: 手順 1 では、NerdDinner アプリケーションの基本的な構造を適切に配置する方法を示します。
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 88ef850503725cc57c92a11952729b4bd2205a69
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835095"
---
<a name="create-a-new-aspnet-mvc-project"></a>新しい ASP.NET MVC プロジェクトを作成します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 1 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。
> 
> 手順 1 では、NerdDinner アプリケーションの基本的な構造を適切に配置する方法を示します。
> 
> 次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner 手順 1: ファイル-&gt;新しいプロジェクト

まず、NerdDinner アプリケーションを選択すると、**ファイル -&gt;新しいプロジェクト**内で Visual Studio 2008 または、無料の Visual Web Developer 2008 Express のいずれかのメニュー項目。

[新しいプロジェクト] ダイアログ ボックスが表示されます。 新しい ASP.NET MVC アプリケーションを作成するには、ダイアログの左側にある"Web"ノードを選択し、右側の「ASP.NET MVC Web アプリケーション」プロジェクト テンプレートを選択しされます。

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*重要: ダウンロードして、ASP.NET MVC で、新しいプロジェクト ダイアログに表示されませんが、それ以外の場合にインストールされていることを確認してください。V2 を使用することができます、 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)まだインストールしていない場合 (ASP.NET MVC は内で使用できる、"Web プラットフォーム-&gt;フレームワークおよびランタイム"セクション)。*

"NerdDinner"を作成し、クリックして [ok] ボタンを作成する新しいプロジェクトという名前です。

[Ok] をクリックしたとき、Visual Studio は、必要に応じて、新しいアプリケーションの単体テスト プロジェクトを作成することを求める追加のダイアログが表示されます。 機能と、アプリケーションの動作を検証する自動テストを作成することにより、この単体テスト プロジェクト (何かについて説明する方法このチュートリアルの後半で to do)。

![](create-a-new-aspnet-mvc-project/_static/image2.png)

上記のダイアログ ボックスで、「テスト フレームワーク」ドロップダウン リストには、すべて使用可能な ASP.NET MVC 単体テスト プロジェクト テンプレートがコンピューターにインストールが設定されます。 NUnit や MBUnit、XUnit のバージョンをダウンロードできます。 組み込みの Visual Studio 単体テスト フレームワークもサポートされます。

*注: Visual Studio の単体テスト フレームワークは、Visual Studio 2008 Professional および以降のバージョンで使用できるのみです。VS 2008 Standard Edition または Visual Web Developer 2008 Express を使用している場合は、ダウンロードしてこのダイアログ ボックスに表示するために、ASP.NET MVC の NUnit や MBUnit XUnit 拡張機能をインストールする必要があります。インストールされているすべてのテスト フレームワークがない場合は、ダイアログは表示されません。*

作成するテスト プロジェクトの既定の"NerdDinner.Tests"名前を使用して、「Visual Studio 単体テスト」フレームワーク オプションを使用しておします。 クリックしたとき、[ok] ボタンの Visual Studio はソリューションを単体テストのため、web アプリケーションのいずれかとその中の 2 つのプロジェクトの作成します。

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>NerdDinner ディレクトリ構造の確認

Visual Studio で新しい ASP.NET MVC アプリケーションを作成するときに自動的にさまざまなファイルとディレクトリをプロジェクトに追加します。

![](create-a-new-aspnet-mvc-project/_static/image4.png)

既定では、ASP.NET MVC プロジェクトでは、6 つの最上位ディレクトリがあります。

| **ディレクトリ** | **目的** |
| --- | --- |
| **/コント ローラー** | URL 要求を処理するコント ローラー クラスを配置します。 |
| **/モデル** | データの操作を表すクラスを配置します。 |
| **/Views** | 出力のレンダリングを担当する UI テンプレート ファイルを配置します。 |
| **/Scripts** | JavaScript ライブラリのファイルとスクリプト (.js) を配置します。 |
| **/Content** | CSS とイメージ ファイル、およびその他の非動的または非 JavaScript コンテンツを配置します。 |
| **/アプリ\_データ** | データ ファイルの格納先は、読み取り/書き込みします。 |

ASP.NET MVC では、この構造体は必要ありません。 実際には、大規模なアプリケーションで作業する開発者は通常、アプリケーションの間でパーティション分割より管理しやすいように複数のプロジェクト (例: データ モデル クラスは多くの場合、別のクラス ライブラリ プロジェクトで web アプリケーションから移動) します。 ただし、既定のプロジェクト構造は、アプリケーションの問題をクリーンに維持するために使用できる便利な既定のディレクトリ規則を提供しています。

/Controllers ディレクトリを展開している場合は、Visual Studio が、プロジェクトに既定で – HomeController と AccountController – 2 つのコント ローラー クラスを追加する分かります。

![](create-a-new-aspnet-mvc-project/_static/image5.png)

/Views ディレクトリを展開しましたと – 内のファイルがプロジェクトに既定で追加もいくつかのテンプレートに加え、/Home、/Account および/Shared – 3 つのサブ ディレクトリを検索します。

![](create-a-new-aspnet-mvc-project/_static/image6.png)

/Content と/Scripts ディレクトリを展開しますと、アプリケーション内でをサポートできるように ASP.NET AJAX と jQuery JavaScript ライブラリと同様に、サイトのすべての HTML のスタイル設定に使用される Site.css ファイルを検索します。

![](create-a-new-aspnet-mvc-project/_static/image7.png)

NerdDinner.Tests プロジェクトを展開している場合、コント ローラー クラスの単体テストが含まれている 2 つのクラスを検索します。

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Visual Studio によって追加されたこれらの既定のファイルの基本的な構造での作業アプリケーションのホーム ページで、ページ、アカウントのログイン/ログアウト/登録ページ (すべてワイヤード (有線) アップ動作し、ボックス外) のハンドルされないエラー ページについての完全な提供します。

### <a name="running-the-nerddinner-application"></a>NerdDinner アプリケーションを実行します。

いずれかを選択してプロジェクトを実行できる、**デバッグ -&gt;デバッグの開始**または**デバッグ -&gt;デバッグなしで開始**メニュー項目。

![](create-a-new-aspnet-mvc-project/_static/image9.png)

組み込み ASP.NET Web サーバー、Visual Studio に付属する起動され、アプリケーションの実行します。

![](create-a-new-aspnet-mvc-project/_static/image10.png)

新しいプロジェクトのホーム ページを以下に示します (URL:「/」) 実行されるとき。

![](create-a-new-aspnet-mvc-project/_static/image11.png)

"About" タブをクリックすると、に関するページ (URL:「ホーム/についての/」)。

![](create-a-new-aspnet-mvc-project/_static/image12.png)

右上の [ログオン] リンクをクリックすると、ログイン ページに移動 (URL:「アカウント/ログオン」)

![](create-a-new-aspnet-mvc-project/_static/image13.png)

かどうかログイン アカウントが登録リンクをクリックすることはありません (URL:「アカウント/登録」) を作成します。

![](create-a-new-aspnet-mvc-project/_static/image14.png)

について、上記のホームを実装するコードとログアウト/を登録、新しいプロジェクトを作成したときに、既定では機能が追加されました。 そのアプリケーションの開始点として使用します。

### <a name="testing-the-nerddinner-application"></a>NerdDinner アプリケーションのテスト

Professional Edition または Visual Studio 2008 の新しいバージョンを使用している私たちは場合、プロジェクトをテストする組み込みの単体テストの Visual studio IDE のサポートを使用できます。

![](create-a-new-aspnet-mvc-project/_static/image15.png)

上記のオプションのいずれかを選択する、IDE 内にある [テスト結果] ウィンドウを開くし、組み込みの機能をカバーする、新しいプロジェクトに含まれる 27 単体テストの合格/不合格の状態を提供します。

![](create-a-new-aspnet-mvc-project/_static/image16.png)

このチュートリアルの後半で自動テストについては説明がされアプリケーションの機能の実装に対応する追加の単体テストを追加します。

### <a name="next-step"></a>次の手順

場所に基本的なアプリケーションの構造体を取得しました。 これにしましょう[、アプリケーションのデータを格納するデータベースを作成](create-a-database.md)です。

> [!div class="step-by-step"]
> [前へ](introducing-the-nerddinner-tutorial.md)
> [次へ](create-a-database.md)
