---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'パート 2: コント ローラー |Microsoft ドキュメント'
author: jongalloway
description: このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 第 2 部では、コント ローラーについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 680cdea388d9b01961bd626643c0fd91c9205ed7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="part-2-controllers"></a>パート 2: コント ローラー
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。  
>   
> MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。  
>   
> このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 第 2 部では、コント ローラーについて説明します。


従来の web フレームワークで受信した Url は通常、ディスク上のファイルにマップします。 例: などの URL に対する要求"/繰り返し"または"/Products.php"、「繰り返し」または"Products.php"ファイルによって処理される可能性があります。

MVC フレームワークの web ベースでは、少し異なる方法で、Url がサーバー コードをマップします。 受信 Url をファイルにマップするには、代わりには、代わりに、クラスのメソッドに Url をマップします。 これらのクラスは「コント ローラー」と呼ばれますとユーザー入力の処理、入力方向の HTTP 要求の処理を担当している、クライアント バックアップを取得して、データを保存および送信する応答を決定する (HTML を表示、ファイルをダウンロードする、別のサーバーに対するリダイレクトURL など)。

## <a name="adding-a-homecontroller"></a>HomeController の追加

マイクロソフトのサイトのホーム ページに Url を処理するコント ローラー クラスを追加することによって、MVC 音楽ストア アプリケーションをまずします。 ASP.NET MVC の既定の名前付け規則に従うし、HomeController を呼び出すおします。

ソリューション エクスプ ローラー内でコント ローラー」フォルダーを右クリックし、"Add"および「コント ローラー…」選択 コマンド:

![](mvc-music-store-part-2/_static/image1.jpg)

これは、「コント ローラーの追加」ダイアログが表示されます。 コント ローラー"HomeController"という名前を追加 ボタンを押します。

![](mvc-music-store-part-2/_static/image1.png)

これにより、次のコードで HomeController.cs、新しいファイルが作成されます。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

開始するには限りシンプルにしてみましょう文字列を返すだけ単純なメソッドを使用してインデックス メソッドに置き換えます。 2 つの変更を確認しましょう。

- ActionResult ではなく文字列を返す方法を変更します。
- 「こんにちはからホーム」を返す return ステートメントを変更します。

メソッドは、次のようになります。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>アプリケーションの実行

今すぐサイトを実行してみましょう。 当社の web サーバーを起動し、次のいずれかを使用してサイトを実行してください。

- デバッグ ⇨ デバッグの開始 メニュー項目を選択します。
- ツールバーの緑色の矢印ボタンをクリックします。 ![](mvc-music-store-part-2/_static/image2.jpg)
- キーボード ショートカット、f5 キーを使用します。

上記の手順のいずれかの方法は、プロジェクトをコンパイルしに組み込ま Visual Web Developer を開始である ASP.NET 開発サーバーです。 通知は、下隅にある ASP.NET 開発サーバーが、開始されたことを示すために、画面の表示され、下で実行されているポート番号が表示されます。

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer は自動的に開き、web サーバーを指す URL をブラウザー ウィンドウです。 これにより、web アプリケーションを迅速に試すことがします。

![](mvc-music-store-part-2/_static/image3.png)

さて、かなりクイック – 新しい web サイトを作成しましたが 3 つのインライン関数を追加し、ブラウザーでテキストをしました。 いないきわめてが、開始します。

*注意: Visual Web Developer には、ランダム空き"port"番号で、web サイトを実行する ASP.NET 開発サーバーが含まれます。上記のスクリーン ショットでは、サイトが実行されているで`http://localhost:26641/`ポート 26641 使用されているため、します。使用するポート番号は別になります。このチュートリアルでは、URL の like/Store/Browse 方法について説明、ポート番号の後は変わります。26641 のポート番号を想定すると、ストア/参照する参照は意味を参照して`http://localhost:26641/Store/Browse`です。*

## <a name="adding-a-storecontroller"></a>StoreController を追加します。

マイクロソフトのサイトのホーム ページを実装する単純な HomeController が追加されました。 使用して、参照、音楽ストアの機能を実装する別のコント ローラーを追加してみましょう。 このストアのコント ローラーは、3 つのシナリオをサポートします。

- 音楽のジャンル、音楽ストア内のリスト ページ
- すべての音楽アルバムには、特定のジャンルの一覧を表示する [参照] ページ
- 特定の音楽のアルバムに関する情報が表示される詳細ページ

まず、新しい StoreController クラスの追加. まだの場合は、ブラウザーを閉じる、またはデバッグ ⇨ デバッグの停止 メニュー項目を選択して、アプリケーションの実行を停止します。

新しい StoreController を追加します。 HomeController で行ったのと同じようになりますこの作業を行う、ソリューション エクスプ ローラー内の「コント ローラー」フォルダーを右クリックして、追加の を選択して&gt;コント ローラーのメニュー項目

![](mvc-music-store-part-2/_static/image4.png)

この新しい StoreController は既に"Index"メソッドを使用しています。 この"Index"メソッドを使用して、音楽ストア内のすべてのジャンルの一覧を示す、リスト ページを実装します。 シナリオを実装する 2 つの他のマイクロソフト StoreController を処理する 2 つの追加メソッドも追加されます: 参照および詳細。

当社のコント ローラー内でのこれらのメソッド (インデックス、参照および詳細) は「コント ローラー アクション」と呼ばれます、HomeController.Index () のアクション メソッドで既に説明したように、ジョブを URL 要求に応答し、どのような内容の決定、(一般に)ブラウザーや URL を呼び出したユーザーに送信する必要があります。

文字列"こんにちはから Store.Index()"を返す theIndex() メソッドを変更することによって、StoreController 実装をまずし、Browse() および Details() 同様のメソッドを追加します。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

プロジェクトをもう一度実行し、次の Url を参照します。

- /ストア
- /ストア/[参照]
- /ストア/詳細

これらの Url にアクセスする、当社のコント ローラー内でアクション メソッドを呼び出すし、文字列の応答を返します。

![](mvc-music-store-part-2/_static/image5.png)

ですしますが、これらは、単に定数文字列。 みましょう動的で、URL から情報を取得し、ページの出力に表示します。

まず URL からクエリ文字列値を取得する参照アクション メソッドを変更します。 「ジャンル」パラメーターをアクション メソッドに追加することによってこの作業を行うことができます。 この作業を行うときに ASP.NET MVC では、クエリ文字列パラメーターまたはフォーム ポスト パラメーターが呼び出されると、アクション メソッドに「ジャンル」をという名前を自動的に渡されます。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*注: おしているメソッドを使用 HttpUtility.HtmlEncode ユーティリティ、ユーザー入力を不要部分を削除します。これにより、ユーザーが/Store/Browse のようなリンクを使用して、ビューに Javascript を挿入できないようにしますか。ジャンル =&lt;スクリプト&gt;window.location='http://hackersite.com'&lt;/script&gt;です。*

ここでストア/参照をブラウズしますか?ジャンル Disco を =

![](mvc-music-store-part-2/_static/image6.png)

読み取りおよび id。 をという名前の入力パラメーターを表示する、Details アクションを次に変更してみましょう 前のメソッドとは異なりはクエリ文字列パラメーターとして ID 値お埋め込みではありません。 代わりに、URL 自体内で直接埋め込むおします。 例:/Store/Details/5 です。

ASP.NET MVC により、何も構成することがなく簡単に実行します。 ASP.NET MVC の既定のルーティング規則では、"ID"という名前のパラメーターとしてアクション メソッド名の後に URL のセグメントを処理します。 アクション メソッドに ID をという名前のパラメーターがある場合、ASP.NET MVC は自動的に URL セグメントをパラメーターとして渡します。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

アプリケーションを実行し、/Store/Details/5 を参照します。

![](mvc-music-store-part-2/_static/image7.png)

これまで実行したの要約します。

- Visual Web Developer で、新しい ASP.NET MVC プロジェクトを作成します。
- ASP.NET MVC アプリケーションの基本的なフォルダー構造について説明しました
- ASP.NET 開発サーバーを使用して、web サイトを実行する方法を学習しました
- 2 つのコント ローラー クラスを作成しました: テンプレートを使用して、StoreController
- URL 要求に応答し、ブラウザーにテキストを返しますが、コント ローラーにアクション メソッドが追加されました


> [!div class="step-by-step"]
> [前へ](mvc-music-store-part-1.md)
> [次へ](mvc-music-store-part-3.md)
