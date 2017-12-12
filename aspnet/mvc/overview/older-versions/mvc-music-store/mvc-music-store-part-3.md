---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: "パート 3: ビューと ViewModels |Microsoft ドキュメント"
author: jongalloway
description: "このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 パート 3 は、ビューと ViewModels について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 5b38f88283c5d2d93f0bab283dbd9451855d95e3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-3-views-and-viewmodels"></a>パート 3: ビューと ViewModels
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。  
>   
> MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。  
>   
> このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 パート 3 は、ビューと ViewModels について説明します。


これまでおしただけされた文字列から返すコント ローラーのアクション。 コント ローラーのしくみを理解する便利な方法がない方法する実際の web アプリケーションをビルドします。 サイト、ブラウザーに HTML を生成する優れた方法としての目的に – いずれかのテンプレート ファイルを使用して、HTML コンテンツをより簡単にカスタマイズおを返信します。 まさにビューが何を実行します。

## <a name="adding-a-view-template"></a>ビューのテンプレートを追加します。

ビュー テンプレートを使用するを返す、ActionResult HomeController インデックス メソッドを変更し、以下のように、View() が返されるようにおします。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

上記の変更では、文字列を返すのではなくを示します、代わりに、結果を生成する"View"を使用します。

プロジェクトに適切なビュー テンプレートを追加します。 これを行うおありますインデックス アクション メソッド内でテキストのカーソルの位置を右クリックし、「ビューの追加」です。 ビューの追加 ダイアログが表示されます。

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

「ビューの追加」ダイアログでは、迅速かつ簡単には、テンプレート ファイルの表示を生成できます。 既定では、「ビューの追加」ダイアログでは、それを使用するアクション メソッドに一致するように作成するビュー テンプレートの名前が事前に入力します。 当社 HomeController の Index() アクション メソッド内で「ビューの追加」のコンテキスト メニューを使用しているため「ビューの追加」このダイアログ ボックスは既定ではあらかじめ値が入力されたビュー名と"Index"を持ちます。 このダイアログ ボックスでオプションのいずれかを変更するために、[追加] ボタンをクリックして必要はありません。

[追加] ボタンをクリックしてお Visual Web Developer は、フォルダーを作成する場合、\Views\Home ディレクトリにご利用の米国ビュー テンプレートが存在しない新しい Index.cshtml を作成します。

![](mvc-music-store-part-3/_static/image2.png)

"Index.cshtml"ファイルの名前とフォルダーの場所は重要であり、既定の ASP.NET MVC の名前付け規則に従います。 ディレクトリ名、\Views\Home、コント ローラー - HomeController という名前に一致します。 ビューのテンプレート名、インデックス、ビューを表示するコント ローラー アクション メソッドに一致します。

ASP.NET MVC では、この名前付け規則を使用してビューを返すときに、名またはビュー テンプレートの場所を明示的に指定する必要があるようにすることができます。 既定で表示する \Views\Home\Index.cshtml ビュー テンプレート、HomeController 内で次のようなコードを記述するとき。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer は、作成され、「ビューの追加」ダイアログ ボックスで [追加] ボタンをクリックした後に"Index.cshtml"ビューのテンプレートを開きます。 Index.cshtml の内容は次のとおりです。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

このビューは、ASP.NET Web フォームおよび ASP.NET MVC の以前のバージョンで使用される、Web フォーム ビュー エンジンよりも簡潔である Razor 構文を使用しています。 Web フォーム ビュー エンジンは ASP.NET MVC 3 で使用できますが、多くの開発者が、Razor ビュー エンジンが ASP.NET MVC の開発を非常に収まることを確認します。

最初の 3 つの行は、ViewBag.Title を使用してページのタイトルを設定します。 拝見このしくみさらに詳しくすぐに、まずみましょう更新テキスト見出しのテキストし、ページを表示します。 更新プログラム、 &lt;h2&gt;を次に示すように、「このは、ホーム ページ」を言うタグ。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

新しいテキストがホーム ページに表示されているアプリケーションの表示を実行します。

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>一般的なサイトの要素のレイアウトを使用します。

ほとんどの web サイトは多くのページ間で共有されるコンテンツがある: ナビゲーション、フッター、ロゴのイメージ、スタイル シート参照などです。Razor ビュー エンジンにより、これと呼ばれるページを使用して管理しやすい\_Layout.cshtml/ビュー/共有フォルダー内にご利用の米国自動的に作成されました。

![](mvc-music-store-part-3/_static/image4.png)

下に表示される内容を表示するには、このフォルダーをダブルクリックします。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

個々 のビューからコンテンツを表示する、 @RenderBody() コマンドとの外側に記述する共通の内容に追加することができます、 \_Layout.cshtml マークアップ。 サイトでは、すべてのページで、ホーム ページおよびストアの領域にリンクに共通のヘッダーを持つテンプレートをすぐ上に追加する予定の MVC Music Store がいいでしょう@RenderBody() のステートメント。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>スタイル シートを更新

空のプロジェクト テンプレートには、検証メッセージを表示するために使用するスタイルだけを含む非常に簡素化された CSS ファイルが含まれています。 このデザイナーは、いくつか追加 CSS およびイメージを今すぐに追加されますので、サイトのルック アンド フィールを定義を提供しています。

更新された CSS ファイルとイメージが記載されている MvcMusicStore Assets.zip のコンテンツのディレクトリに含まれる[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)です。 Windows エクスプ ローラーで、これらの両方を選択し、次のように Visual Web Developer で、このソリューションのコンテンツ フォルダーにドロップおします。

![](mvc-music-store-part-3/_static/image5.png)

Site.css ファイルを上書きするかどうかを確認するよう求められます。 [はい] をクリックします。

![](mvc-music-store-part-3/_static/image6.png)

アプリケーションのコンテンツのフォルダーは次のように表示されます。

![](mvc-music-store-part-3/_static/image7.png)

今すぐアプリケーションを実行し、ホーム ページで、変更の外観を確認してみましょう。

![](mvc-music-store-part-3/_static/image8.png)

- 変更点を確認してみましょう。「HomeController の Index アクション メソッドを見つけて、テンプレートの表示後、標準の名前付け規則ので、コードに"戻り View()"が呼び出されていなくても、\Views\Home\Index.cshtmlView テンプレートが表示されます。
- ホーム ページには、\Views\Home\Index.cshtml ビュー テンプレート内で定義されている単純なへようこそ メッセージが表示されています。
- ホーム ページを使用して、 \_Layout.cshtml テンプレートおよびウェルカム メッセージが標準サイト HTML レイアウト内に含まれるようにします。

## <a name="using-a-model-to-pass-information-to-our-view"></a>モデルを使用して、ビューに情報を渡す

だけハードコードされた HTML を表示するビュー テンプレートがない非常に興味深い web サイトを作成しようとしています。 動的な web サイトを作成するには代わりにします、コント ローラーのアクションからテンプレートを表示する情報を渡します。

パターンでは、モデル ビュー コント ローラー、モデルを指す用語は、アプリケーションでデータを表現するオブジェクトです。 多くの場合、モデル オブジェクトは、データベースのテーブルに対応しますが、する必要はありません。

ActionResult を返すことがコント ローラー アクション メソッドは、ビューにモデル オブジェクトを渡すことができます。 これにより、コント ローラーをクリーンにパッケージを応答を生成し、ビュー テンプレートにこの情報を渡すために必要なすべての情報を使用して、適切な HTML 応答を生成します。 これは、最も簡単に理解操作で確認することによって、それでは始めましょう。

まず、ストア内のジャンルおよびアルバムを表すために一部のモデル クラスを作成します。 ジャンル クラスを作成して開始しましょう。 プロジェクト内で"Models"フォルダーを右クリックして、「クラスの追加」オプションを選択して、ファイルの名前を"Genre.cs"。

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

作成されたクラスにパブリックの文字列名プロパティを追加します。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*注: 参考までに、、{get; に設定} 表記が行っている # の自動実装プロパティの機能を使用します。これにより、プロパティの利点をバッキング フィールドを宣言することを必要とせずします。*

次に、タイトルとジャンル プロパティを持つアルバム クラス (Album.cs という名前) を作成する同じ手順に従います。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

今すぐ StoreController モデルからの動的な情報を表示するビューを使用して変更できます。 デモンストレーション用に今すぐおという名前の要求 ID に基づいた、アルバム場合は、以下のビューと同様にその情報を表示おでした。

![](mvc-music-store-part-3/_static/image10.png)

1 枚のアルバムの情報が表示されるように、ストアの詳細アクションを変更することで始めます。 先頭に"using"ステートメントを追加、 **StoreControllers**クラス MvcMusicStore.Models 名前空間を含めるために、アルバム クラスを使用するたびに、MvcMusicStore.Models.Album を入力する必要はありません。 そのクラスの"using"セクションが表示されます以下のようです。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

次に、更新されます詳細コント ローラーのアクション、文字列ではなく ActionResult を返すように HomeController のインデックスのメソッドで行ったようにします。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

今すぐをビューにアルバム オブジェクトを返すロジックを変更できます。 このチュートリアルで後ほどおはするデータを取得する、データベースからは、今すぐおは「ダミー データ」作業を開始します。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*注: c# 慣れていない場合は、可能性がありますを想定する、var を使用してあることを意味、アルバム変数遅延バインディング。正しくありません: - 内容に基づいておしている変数を割り当てると、そのアルバムがアルバムの種類を決定するため、コンパイル時のチェックと Visual Studio コード エディター、アルバムの種類として、アルバムのローカル変数をコンパイルする型の推定を使用して、c# コンパイラサポートしてください。*

HTML 応答を生成する、アルバムを使用する表示テンプレートを今すぐ作成してみましょう。 その前に、新しく作成したアルバム クラスの概要ビューの追加 ダイアログを認識するようにプロジェクトをビルドする必要があります。 ことができます、プロジェクトを選択してビルド Debug⇨Build MvcMusicStore メニュー項目 (、余分なクレジットのすることができます、ctrl キーを押し-b Shift ショートカットを使用してプロジェクトをビルドします)。

![](mvc-music-store-part-3/_static/image11.png)

これで、サポートするクラスを設定しました。 は、当社ビュー テンプレートをビルドする準備ができました。 詳細メソッド内で右クリックし、[ビューの追加] を選択 コンテキスト メニュー。

![](mvc-music-store-part-3/_static/image12.png)

同じように、テンプレートを使用するには、新しいビュー テンプレートを作成しようとしています。 StoreController から、作成するためが既定では生成されます \Views\Store\Index.cshtml ファイルで。

異なり前に、なっています「作成は、厳密に型指定された」ビューのチェック ボックスをオンにします。 「ビューのデータのクラス」ドロップ ダウン リスト内で、「アルバム」クラスを選択し、いきます。 これにより、「ビューの追加」ダイアログを使用するようにオブジェクトが渡されるアルバムを期待しているビュー テンプレートを作成します。

![](mvc-music-store-part-3/_static/image13.png)

[追加] ボタンをクリックしたとき、\Views\Store\Details.cshtml ビュー テンプレートが作成された、次のコードを含むです。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

このビューが厳密に型指定された、アルバム クラスであることを示します、最初の行に注意してください。 されて成功したアルバムのオブジェクト モデルのプロパティを簡単にアクセスしても、Visual Web Developer エディターの IntelliSense の利点があるため、Razor ビュー エンジンが認識しています。

更新プログラム、 &lt;h2&gt;タグを次のように表示するには、その行を変更することにより、アルバムのタイトルのプロパティが表示されます。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

後にピリオドを入力すると、IntelliSense がトリガーされることに注意してください、@Modelキーワード、プロパティとアルバム クラスでサポートされるメソッドを表示します。

みましょう今すぐプロジェクトを再実行してストア/詳細/5 URL を参照してください。 次のようなアルバムの詳細を会いしましょう。

![](mvc-music-store-part-3/_static/image14.png)

今すぐストア参照アクション メソッドのような更新プログラムにしましょう。 ActionResult、それを返すように、メソッドを更新し、新しいジャンル オブジェクトを作成し、ビューを返します、メソッドのロジックを変更します。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

参照メソッドを右クリックし、[ビューの追加] を選択 コンテキスト メニューからは厳密に型指定されたビューを追加し、厳密に型指定をジャンル クラスに追加します。

![](mvc-music-store-part-3/_static/image15.png)

更新プログラム、 &lt;h2&gt;ジャンル情報を表示する (/Views/Store/Browse.cshtml) でコードのビュー内の要素。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

今すぐみましょう、プロジェクトを再実行し、ストア/[参照] を参照しますか?ジャンル Disco URL を = です。 以下のように表示される [参照] ページは表示されます。

![](mvc-music-store-part-3/_static/image16.png)

最後に少し複雑な更新プログラムを確認してみましょう、**インデックスの格納**アクション メソッドと、ストア内のすべてのジャンルの一覧を表示するビュー。 1 つのジャンルだけではなく、モデル オブジェクトとしてジャンルの一覧を使用して、実行します。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

ストアのインデックス アクション メソッドを右クリックし、ビューのようにする前には、モデル クラスとしてジャンルを選択し、追加 を選択します。

![](mvc-music-store-part-3/_static/image17.png)

最初に変更します、@modelビューがいくつかのジャンルを予想することを示すために宣言が 1 つだけではなくオブジェクトします。 次のように/Store/Index.cshtml の最初の行を変更します。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

これは、これは、ジャンルのいくつかのオブジェクトが保持できるモデル オブジェクトを作成する、Razor ビュー エンジンを指示します。 お使いの IEnumerable&lt;ジャンル&gt;一覧ではなく&lt;ジャンル&gt;より汎用的なので、IEnumerable インターフェイスをサポートするオブジェクトの種類に後で、モデルの種類を変更することを許可します。

次に、下の完成したビューのコードで示すように、モデル内のジャンル オブジェクトをループ処理します。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

通知がある完全に IntelliSense サポートはこのコードを入力して、ときに入力できるように"@Model"。 ここには、すべてのメソッドと型ジャンルの IEnumerable でサポートされるプロパティが参照してください。

![](mvc-music-store-part-3/_static/image18.png)

"Foreach"ループ内では、Visual Web Developer は、各項目の型はジャンル、IntelliSence ごとに表示するため、ジャンル型を認識します。

![](mvc-music-store-part-3/_static/image19.png)

次に、スキャフォールディング機能は、ジャンル オブジェクトを検査し、それぞれが持つこと、Name プロパティをループ処理して出力するようにを決定します。また、個々 の項目を編集、詳細、および削除のリンクを生成します。 活用を後で、ストア マネージャーで、移動しますが、ここでは代わりに、単純なリストがあるしたい場合がします。

アプリケーションを実行お/Store を参照すると、数とジャンルの一覧の両方が表示されることがわかります。

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>ページ間のリンクの追加

当社/Store URL を現在ジャンルの一覧を表示するには、単にプレーン テキストとしてジャンル名が一覧表示します。 みましょうようにプレーン テキストではなく代わりに、ジャンル名へのリンク、適切なストア/参照 URL されるように変更音楽のジャンル"Disco"をストア/参照に移動などをクリックするとしますか? ジャンル Disco URL を = です。 当社 \Views\Store\Index.cshtml ビュー テンプレートを使用してこれらのリンクは以下のようなコードの出力を更新すること**(これには入力しないで - は、パフォーマンスを向上すること)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

機能するが、ハードコーディングされた文字列に依存しているので、後で問題になる可能性があります。 たとえば、コント ローラーの名前を変更する場合、更新する必要があるリンクを検索しているコード全体を検索お必要があります。

その他の方法を使用して HTML ヘルパー メソッドを利用を開始します。 ASP.NET MVC には、さまざまな次のように一般的なタスクを実行するビュー テンプレート コードから使用できる HTML ヘルパー メソッドが含まれています。 Html.ActionLink() ヘルパー メソッドは HTML をビルドしやすく、特に有用なものでは、 &lt;、&gt;リンク、URL パスは、正しく URL エンコードされていることを確認するのと同様に面倒な場合の詳細を管理します。

Html.ActionLink() には、リンクに必要な多くの情報を指定することをいくつかの異なるオーバー ロードがあります。 最も簡単な場合は、リンク テキストだけと、クライアントでハイパーリンクがクリックされたときに移動するアクション メソッドを指定します。 たとえば、「ストア/」というリンク テキストは"移動する、ストア"を使用してインデックスは次の呼び出しとストアの詳細ページの Index() メソッドにリンクできます。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*注: このケースでは、していない必要がありますおだけとリンクしている現在のビューをレンダリングは、同じコント ローラー内の別のアクション、コント ローラー名を指定します。*

[参照] ページに、リンクを 3 つのパラメーターを受け取る Html.ActionLink メソッドの別のオーバー ロードを使用して、、ただし、パラメーターを渡す必要があります。

- 1. リンク テキストは、ジャンル名前が表示されます。
- 2. コント ローラー アクションの名前 (参照)
- 3. ルートのパラメーター値、(ジャンル) の名前と値 (ジャンル名) の両方を指定します。

すべて同時には、ここでは、ストアのインデックス ビューにこれらのリンクを書き込む方法を配置します。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

今すぐ、プロジェクトをもう一度実行して/Store/URL へのアクセスとは、ジャンルの一覧を表示おされます。 クリックされたときに、各ジャンルにはハイパーリンク – us にかかる、ストア/参照? ジャンル =*[ジャンル]* URL。

![](mvc-music-store-part-3/_static/image3.jpg)

次のような一覧については、ジャンル HTML が表示。

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


>[!div class="step-by-step"]
[前へ](mvc-music-store-part-2.md)
[次へ](mvc-music-store-part-4.md)
