---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'パート 2: コント ローラー |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 第 2 部では、コント ローラーについて説明します。
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8824d5d2f5670aee2df6dc6e74767e4a851dd4ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841096"
---
<a name="part-2-controllers"></a>パート 2: コント ローラー
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。  
>   
> MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。  
>   
> このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 第 2 部では、コント ローラーについて説明します。


従来の web フレームワークでは、受信した Url は通常ディスク上のファイルにマップされます。 例: のような要求 URL の"/ディスク上のファイル"または"/されず"、「繰り返し」または「されず」ファイルで処理される可能性があります。

Web ベースの MVC フレームワークは、少し異なる方法で Url をサーバー コードにマップします。 受信した Url をファイルにマップするには、代わりには、代わりに、クラスのメソッドに Url をマップします。 これらのクラスは「コント ローラー」と呼ばれますとユーザー入力を処理する受信 HTTP 要求を処理する責任が、クライアント バックアップを取得して、データの保存と送信する応答を決定する (HTML を表示、ダウンロード ファイルを別にリダイレクトURL など)。

## <a name="adding-a-homecontroller"></a>HomeController の追加

サイトのホーム ページに Url を処理するコント ローラー クラスを追加することで、MVC Music Store アプリケーションをまず。 ASP.NET MVC の既定の名前付け規則に従うがされ HomeController を付けます。

ソリューション エクスプ ローラー内でコント ローラー」フォルダーを右クリックし、"Add"し、[コント ローラー] コマンドを選択します。

![](mvc-music-store-part-2/_static/image1.jpg)

「コント ローラーの追加」ダイアログが表示されます。 「HomeController」コント ローラーを指定し、、追加ボタンを押します。

![](mvc-music-store-part-2/_static/image1.png)

これにより、次のコードで、HomeController.cs で、新しいファイルが作成されます。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

限りシンプルにしてみましょう文字列を返すだけ簡単な方法で Index メソッドを置き換えます。 私たちは 2 つの変更を行います。

- ActionResult ではなく文字列を返す方法を変更します。
- "こんにちはから Home"を返す return ステートメントを変更します。

メソッドは、次のようになります。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>アプリケーションの実行

今すぐサイトを実行してみましょう。 当社の web サーバーを起動し、次のいずれかを使用してサイトを試す:。

- デバッグ ⇨ デバッグの開始 メニュー項目を選択します。
- ツールバーの緑色の矢印ボタンをクリックします。 ![](mvc-music-store-part-2/_static/image2.jpg)
- キーボード ショートカット、f5 キーを使用します。

上記の手順のいずれかを使用して、このプロジェクトでのコンパイルし、組み込ま Visual Web Developer を開始するには、ASP.NET 開発サーバーと、します。 通知を ASP.NET 開発サーバーが開始されたことを示すために、画面の下隅にある表示され、下で実行されているポート番号が表示されます。

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer が、web サーバーを指す URL のブラウザー ウィンドウを自動的に開きます。 これにより、web アプリケーションを簡単に試すことが。

![](mvc-music-store-part-2/_static/image3.png)

では、非常に速かった – 新しい web サイトを作成しましたが、関数の場合、次の 3 つの行を追加して、ブラウザーでテキストがあります。 いないきわめてが、開始します。

*注: Visual Web Developer には、無料の「ポート」ランダムな番号で、web サイトを実行する ASP.NET 開発サーバーが含まれています。上記のスクリーン ショットでは、サイトが実行されている`http://localhost:26641/`26641 ポートを使用しているため、します。実際のポート番号は別になります。このチュートリアルでは URL の like/Store/Browse について説明と、は、ポート番号の後が変わります。26641 のポート番号と仮定すると、/ストア/参照への参照を参照する`http://localhost:26641/Store/Browse`します。*

## <a name="adding-a-storecontroller"></a>StoreController を追加します。

サイトのホーム ページを実装する単純な HomeController を追加しました。 閲覧、ミュージック ストアの機能を実装するために使用する別のコント ローラーを追加してみましょう。 ストア コント ローラーは、3 つのシナリオをサポートします。

- ミュージック ストアで音楽のジャンルのリスト ページ
- すべての音楽の album には、特定のジャンルを一覧表示する [参照] ページ
- 特定の音楽のアルバムに関する情報を表示するための詳細ページ

新しい StoreController クラスを追加してを起動します. まだインストールしていない場合、ブラウザーを閉じるか、デバッグ ⇨ デバッグの停止 メニュー項目を選択して、アプリケーションの実行を停止します。

新しい StoreController を追加します。 HomeController で行ったように、同様これは、ソリューション エクスプ ローラー内でコント ローラー」フォルダーを右クリックし、追加の選択によって&gt;コント ローラーのメニュー項目

![](mvc-music-store-part-2/_static/image4.png)

この新しい StoreController は既に"Index"メソッドを使用しています。 この"Index"メソッドを使用して、ミュージック ストア内のすべてのジャンルを一覧表示された、リスト ページを実装します。 シナリオを実装する、2 つの他の処理するために、StoreController する 2 つの追加メソッドも追加します。 参照と詳細。

コント ローラー内のこれらのメソッド (インデックス、参照、および詳細情報) を「コント ローラー アクション」と呼びます。 HomeController.Index () のアクション メソッドで既に説明した、仕事が URL 要求に応答し、(一般) どのようなコンテンツを決定するにはブラウザーまたは URL を呼び出すユーザーに送信する必要があります。

文字列"こんにちはから Store.Index()"を返す theIndex() メソッドを変更することで、StoreController 実装をまずし、Browse() と Details() の同様のメソッドを追加します。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

プロジェクトをもう一度実行して、次の Url を参照してください。

- /ストア
- /ストア/参照
- /保存/詳細

これらの Url にアクセスする、コント ローラー内のアクション メソッドを呼び出すし、文字列の応答を返します。

![](mvc-music-store-part-2/_static/image5.png)

ですしますが、これらは、単に定数文字列。 しましょうに動的で、URL から情報を取得し、出力ページに表示します。

まず、URL からクエリ文字列値を取得する参照アクション メソッドを変更します。 このメソッドは、アクション メソッドに"genre"パラメーターを追加することで実行できます。 この作業を行う ASP.NET MVC は自動的に、アクション メソッドには、"genre"という名前のメソッドが呼び出されたときにクエリ文字列またはフォーム post パラメーターを渡します。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*注: ユーザー入力をサニタイズするのに HttpUtility.HtmlEncode ユーティリティ メソッドを使用しています。これにより、ユーザーが/Store/Browse などのリンクを使用して、ビューに Javascript を挿入することからできないようにしますか。ジャンル =&lt;スクリプト&gt;window.location='http://hackersite.com'&lt;/script&gt;します。*

今すぐ/ストア/参照をブラウズしますか?ジャンル Disco を =

![](mvc-music-store-part-2/_static/image6.png)

読み取り、という名前の id。 入力パラメーターを表示、Details アクションを次に変更してみましょう 前のメソッドとは異なりしませんに埋め込む ID 値をクエリ文字列パラメーターとして。 代わりに URL 自体内で直接埋め込むことをしました。 例:/Store/Details/5 します。

ASP.NET MVC では、何も構成しなくても簡単に実行できます。 ASP.NET MVC の既定のルーティング規則では、"ID"という名前のパラメーターとしてアクション メソッド名の後、URL のセグメントを処理します。 アクション メソッドは、ID が付けられたパラメーターを持つ場合、ASP.NET MVC は自動的に URL セグメントにパラメーターとして渡します。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

アプリケーションを実行して、/Store/Details/5 を参照してください。

![](mvc-music-store-part-2/_static/image7.png)

これまでに実行した内容を確認しましょう。

- Visual Web Developer で新しい ASP.NET MVC プロジェクトを作成します。
- ASP.NET MVC アプリケーションの基本的なフォルダー構造を説明しました。
- ASP.NET 開発サーバーを使用して、web サイトを実行する方法を学習しました
- 2 つのコント ローラー クラスを作成しました: テンプレートを使用して、StoreController
- URL 要求に応答し、ブラウザーに返されるテキストが、コント ローラーにアクション メソッドを追加しました


> [!div class="step-by-step"]
> [前へ](mvc-music-store-part-1.md)
> [次へ](mvc-music-store-part-3.md)
