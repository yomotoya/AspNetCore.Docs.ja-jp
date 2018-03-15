---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: "新しい ASP.NET MVC プロジェクトを作成 |Microsoft ドキュメント"
author: microsoft
description: "手順 1 では、基本的な NerdDinner アプリケーション構造を導入する方法を示します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 4d30a6803b1478014a2afb814ac317df27394446
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
<a name="create-a-new-aspnet-mvc-project"></a>新しい ASP.NET MVC プロジェクトを作成します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料のステップ 1 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。
> 
> 手順 1 では、基本的な NerdDinner アプリケーション構造を導入する方法を示します。
> 
> ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner 手順 1: ファイル-&gt;新しいプロジェクト

まず NerdDinner アプリケーションを選択すると、**ファイル -&gt;新しいプロジェクト**メニュー項目内で Visual Studio 2008 または、空き Visual Web Developer 2008 Express のいずれか。

これは、"新しいプロジェクト ダイアログ ボックスが表示されます。 新しい ASP.NET MVC アプリケーションを作成するには、ダイアログ ボックスの左側にある"Web"ノードを選択し、右側の「ASP.NET MVC Web アプリケーション」のプロジェクト テンプレートを選択しておします。

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*重要: ダウンロードし、ASP.NET MVC で、新しいプロジェクト ダイアログに表示されませんが、それ以外の場合をインストールしたことを確認してください。V2 を使用することができます、 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)まだインストールしていない場合 (ASP.NET MVC は内で使用できる、"Web Platform の&gt;フレームワークおよびランタイム"セクション)。*

新しいプロジェクトの"NerdDinner"を作成し、それを作成する"ok"ボタンをクリックするという名前です。

[Ok] をクリックしたとき、Visual Studio は、必要に応じて、新しいアプリケーションの単体テスト プロジェクトを作成することを要求する追加のダイアログが表示されます。 アプリケーションの動作と機能を検証する自動テストを作成することにより、この単体テスト プロジェクト (で取り上げるもの方法このチュートリアルで後ほど to do)。

![](create-a-new-aspnet-mvc-project/_static/image2.png)

上記のダイアログ ボックスで、「テスト フレームワーク」ドロップダウンには、すべて使用可能な ASP.NET MVC 単体テスト プロジェクト テンプレートがコンピューターにインストールが格納されます。 バージョンは、NUnit、XUnit、MBUnit、ダウンロードできます。 組み込みの Visual Studio 単体テスト フレームワークもサポートされます。

*注意: Visual Studio の単体テスト フレームワークが Visual Studio 2008 Professional と以降のバージョンで使用可能ではのみです。VS 2008 Standard Edition または Visual Web Developer 2008 Express を使用している場合をダウンロードして、ASP.NET MVC のこのダイアログ ボックスに表示されるためには、NUnit、MBUnit、XUnit 拡張機能をインストールする必要があります。インストールされているすべてのテスト フレームワークがない場合は、ダイアログ ボックスは表示されません。*

おを名前を使用して、既定値"NerdDinner.Tests"を作成するテスト プロジェクトのオプションを使用「Visual Studio 単体テスト」フレームワークです。 クリックしたとき、"ok"ボタン Visual Studio で作成されますソリューションご利用の米国内の web アプリケーション用と、単体テストのための 2 つのプロジェクト。

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>NerdDinner ディレクトリ構造の確認

Visual Studio で新しい ASP.NET MVC アプリケーションを作成するときに自動的に、プロジェクトにいくつかのファイルとディレクトリを追加します。

![](create-a-new-aspnet-mvc-project/_static/image4.png)

既定では ASP.NET MVC プロジェクトでは、最上位 6 つのディレクトリがあります。

| **ディレクトリ** | **目的** |
| --- | --- |
| **/コント ローラー** | URL 要求を処理するコント ローラー クラスを配置します。 |
| **/Models** | データの操作を表すクラスを配置します。 |
| **/Views** | 出力のレンダリングを担当する UI テンプレート ファイルを配置します。 |
| **/Scripts** | JavaScript ライブラリのファイルとスクリプト (.js) を配置します。 |
| **/Content** | CSS、画像ファイル、およびその他の非動的/非 JavaScript コンテンツを配置します。 |
| **/App\_Data** | データ ファイルを保存する場合は、読み取り/書き込みします。 |

ASP.NET MVC には、この構造体は不要です。 実際には、大規模なアプリケーションで作業する開発者は通常のパーティション、アプリケーションをより管理しやすいに複数のプロジェクト (例: データ モデル クラスは多くの場合、別のクラス ライブラリ プロジェクトで、web アプリケーションから移動) します。 ただし、既定のプロジェクト構造は、とらえてアプリケーションをクリーンして使用できる便利な既定のディレクトリ規約を提供しています。

/Controllers ディレクトリを展開してときに Visual Studio が、プロジェクトに既定で – HomeController および AccountController – 2 つのコント ローラー クラスを追加するを検索します。

![](create-a-new-aspnet-mvc-project/_static/image5.png)

/Views ディレクトリを展開して、ときに 3 つのサブ ディレクトリ –/Home、/Account/Shared – とそれらに含まれるファイルがプロジェクトに既定では追加もいくつかのテンプレートを検索します。

![](create-a-new-aspnet-mvc-project/_static/image6.png)

お展開と/Content と/Scripts ディレクトリ、サイトで、すべての HTML のスタイル設定に使用される Site.css ファイルだけでなく JavaScript ライブラリが ASP.NET AJAX と jQuery を有効にすることができるアプリケーション内でサポートを検索します。

![](create-a-new-aspnet-mvc-project/_static/image7.png)

NerdDinner.Tests プロジェクトを展開している場合、コント ローラー クラスの単体テストを含む 2 つのクラスを検索します。

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Visual Studio によって追加されたこれらの既定のファイルでは、us 基本的な構造を持つ作業アプリケーションのホーム ページで、に関するページ、アカウントのログインとログアウト/登録ページ、および未処理のエラー ページ (すべてワイヤード (有線) し、すぐ作業) に完了しませんでした。

### <a name="running-the-nerddinner-application"></a>NerdDinner アプリケーションを実行します。

いずれかを選択してプロジェクトを実行できる、**デバッグ -&gt;デバッグの開始**または**デバッグ -&gt;デバッグなしで開始**メニュー項目。

![](create-a-new-aspnet-mvc-project/_static/image9.png)

これでは、組み込み ASP.NET Web サーバー、Visual Studio に付属しているを起動して、アプリケーションの実行します。

![](create-a-new-aspnet-mvc-project/_static/image10.png)

新しいプロジェクトのホーム ページを次に示します (URL:「/」) 実行されるとき。

![](create-a-new-aspnet-mvc-project/_static/image11.png)

"About" タブをクリックすると、に関するページ (URL:「ホーム/についての/」)。

![](create-a-new-aspnet-mvc-project/_static/image12.png)

右上の [ログオン] リンクをクリックして、ログイン ページに移動 (URL:「/アカウント/ログオン」)

![](create-a-new-aspnet-mvc-project/_static/image13.png)

かどうかレジスタ」リンクをクリックしてログイン アカウントがありません (URL:「/アカウント/レジスタ」) を作成します。

![](create-a-new-aspnet-mvc-project/_static/image14.png)

上記のホームを実装するコードとログアウトを登録/、新しいプロジェクトを作成した際、既定では機能が追加されました。 そのアプリケーションの開始点として使用されます。

### <a name="testing-the-nerddinner-application"></a>NerdDinner アプリケーションのテスト

Professional Edition またはより新しいバージョンの Visual Studio 2008 を使用して場合プロジェクトをテストするテストの Visual Studio 内で IDE サポート組み込み単位を使用できます。

![](create-a-new-aspnet-mvc-project/_static/image15.png)

上記のオプションのいずれかを選択する、IDE 内で"テスト結果 ウィンドウを開くし、組み込みの機能をカバーする新しいプロジェクトに含まれている 27 単体テストの成功/失敗ステータスをお聞かせ。

![](create-a-new-aspnet-mvc-project/_static/image16.png)

このチュートリアルで後ほどおを自動テストについて詳しく説明し、実装のアプリケーションの機能に対応する追加の単体テストを追加します。

### <a name="next-step"></a>次の手順

場所にアプリケーションの基本構造を取得しました。 これにしましょう[、アプリケーションのデータを格納するデータベースを作成する](create-a-database.md)です。

>[!div class="step-by-step"]
[前へ](introducing-the-nerddinner-tutorial.md)
[次へ](create-a-database.md)
