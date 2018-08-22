---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'パート 3: Views と ViewModels |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 第 3 部では、Views と ViewModels について説明します。
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 828ff18abcc5932f82be71a45ebde589eeb051fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827277"
---
<a name="part-3-views-and-viewmodels"></a>パート 3: Views と ViewModels
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。  
>   
> MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。  
>   
> このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 第 3 部では、Views と ViewModels について説明します。


これまでに私たちしただけされてを返す文字列コント ローラー アクションから。 コント ローラーのしくみを理解できる優れた手段ですが、いないする方法が、実際の web アプリケーションを構築します。 私たちのサイトにアクセスしてブラウザーに HTML を生成する方法の向上を目的に – 1 つの HTML コンテンツをより簡単にカスタマイズするテンプレート ファイルを使用できます返信します。 これには正確に表示します。

## <a name="adding-a-view-template"></a>ビュー テンプレートを追加します。

ビュー テンプレートを使用するには、ActionResult を返す HomeController の Index メソッドを変更し、View() が以下のように返されるように。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

上記の変更では、文字列を返すの代わりを示します、代わりに、結果を生成する"View"を使用します。

プロジェクトに適切なテンプレートの表示を追加します。 これを行うこと、インデックス アクション メソッド内にテキスト カーソルを配置しますし、右クリックし、「ビューの追加」を選択します。 ビューの追加 ダイアログが表示されます。

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

「ビューの追加」ダイアログでは、迅速かつ簡単には、ビュー テンプレート ファイルを生成できます。 既定では、「ビューの追加」ダイアログでは、それを使用するアクション メソッドに一致するように作成するビュー テンプレートの名前が事前設定します。 HomeController の Index() アクション メソッド内の「ビューの追加」コンテキスト メニューを使用したため、前述の"ビューの追加 ダイアログ ボックスは既定で事前作成ビューの名前として"Index"があります。 このダイアログ ボックスでオプションのいずれかを変更するには、そのため、[追加] をクリックしては不要です。

[追加] ボタンをクリックすると、Visual Web Developer は、フォルダーを作成する場合、\Views\Home ディレクトリ内のビュー テンプレートが存在しない新しい Index.cshtml を作成します。

![](mvc-music-store-part-3/_static/image2.png)

"Index.cshtml"ファイルの名前とフォルダーの場所は重要であり、既定の ASP.NET MVC の名前付け規則に従います。 ディレクトリ名、\Views\Home、コント ローラー - HomeController という名前に一致します。 インデックス、ビュー テンプレート名では、ビューを表示するコント ローラー アクション メソッドと一致します。

ASP.NET MVC では、この名前付け規則を使用してビューを返すときに、名前またはビュー テンプレートの場所を明示的に指定しなくてもすむようにできます。 既定で表示する \Views\Home\Index.cshtml テンプレートの表示、HomeController 内で次のようなコードを記述する場合。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer では、作成され、"ビューの追加 ダイアログ ボックスで 追加 ボタンをクリックした後に"Index.cshtml"テンプレートの表示を開きます。 Index.cshtml の内容は、以下に示します。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

このビューは、ASP.NET Web フォームと ASP.NET MVC の以前のバージョンで使用される Web フォーム ビュー エンジンよりも簡潔である Razor 構文を使用しています。 Web フォームのビュー エンジンは ASP.NET MVC 3 では使用できますが、多くの開発者は、Razor ビュー エンジンが ASP.NET MVC の開発を非常にうまく収まることを確認します。

最初の 3 つの行では、ViewBag.Title を使用して、ページ タイトルを設定します。 見てこのしくみについて詳しくがすぐに、まずみましょう更新テキストの見出しテキストがされページを表示します。 更新プログラム、 &lt;h2&gt;タグを次に示すように「このは、ホーム ページ」と答えるとします。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

新しいテキストがホーム ページに表示されるアプリケーションの表示を実行します。

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>サイトの一般的な要素のレイアウトを使用します。

ほとんどの web サイトは、多くのページ間で共有されるコンテンツがある: ナビゲーション、ページ フッター、ロゴのイメージ、スタイル シートの参照など。Razor ビュー エンジンは、これと呼ばれるページを使用して管理する簡単な\_Layout.cshtml/ビュー/共有フォルダー内を自動的に作成されました。

![](mvc-music-store-part-3/_static/image4.png)

次に示す、内容を表示するには、このフォルダーをダブルクリックします。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

によって、個々 のビューからのコンテンツが表示されます、 @RenderBody() コマンド、およびの外側に記述する必要がある共通の内容に追加できる、 \_Layout.cshtml マークアップ。 当社の MVC Music Store 上のテンプレートに追加する予定、サイト内のすべてのページで、ホーム ページとストアの領域へのリンクに共通のヘッダーを持つ必要があります@RenderBody() のステートメント。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>スタイル シートを更新しています

空のプロジェクト テンプレートには、検証メッセージを表示するために使用するスタイルが含まれている非常に簡素化された CSS ファイルが含まれています。 このデザイナーは、now でそれらを追加しますので、弊社サイトの外観を定義する追加 CSS とイメージを提供しています。

更新された CSS ファイルとイメージがの MvcMusicStore Assets.zip で利用可能なコンテンツのディレクトリ内に含まれる[MVC-ミュージック ストア](https://github.com/evilDave/MVC-Music-Store)します。 Windows エクスプ ローラーでこれらの両方を選択して、次に示すように Visual Web developer で、ソリューションのコンテンツのフォルダーにドロップしましたします。

![](mvc-music-store-part-3/_static/image5.png)

Site.css ファイルを上書きするかどうかの確認を求め。 [はい] をクリックします。

![](mvc-music-store-part-3/_static/image6.png)

アプリケーションのコンテンツのフォルダーは、次のように表示されます。

![](mvc-music-store-part-3/_static/image7.png)

これでアプリケーションを実行し、ホーム ページで、変更点の外観を確認してみましょう。

![](mvc-music-store-part-3/_static/image8.png)

- 変更内容を確認しましょう。「HomeController の Index アクション メソッドが見つかり、テンプレートの表示後、標準的な名前付け規則ので、コードに"戻り View()"と呼ばれる場合でも、\Views\Home\Index.cshtmlView テンプレートが表示されます。
- ホーム ページは \Views\Home\Index.cshtml ビュー テンプレート内で定義されている単純なようこそメッセージを表示しています。
- ホーム ページを使用して、 \_Layout.cshtml テンプレート、ようこそメッセージは、標準のサイトの HTML レイアウト内に含まれるためです。

## <a name="using-a-model-to-pass-information-to-our-view"></a>このビューに情報を渡すモデルを使用します。

単純にハードコーディングされた HTML を表示します。 テンプレートの表示はない非常に興味深い web サイトを作成しようとしています。 動的な web サイトを作成するには代わりにするために、ビュー テンプレートに、コント ローラー アクションから情報を渡す。

モデル-ビュー-コント ローラー パターンでモデルを指す用語オブジェクトをアプリケーション内のデータを表します。 多くの場合、モデル オブジェクトが、データベース内のテーブルに対応していますが、する必要はありません。

ActionResult を返すコント ローラー アクション メソッドは、モデル オブジェクトをビューに渡すことができます。 これにより、応答を生成し、ビュー テンプレートにこの情報を渡すために必要なすべての情報を明確にパッケージを使用して、適切な HTML 応答を生成するコント ローラーです。 これは、最も簡単に理解の動作確認することによって、開始しましょう。

まず、ストア内のジャンルとアルバムを表すいくつかのモデル クラスを作成します。 ジャンル クラスを作成してみましょう。 プロジェクト内で"Models"フォルダーを右クリックし、「クラスの追加」を選択して、および"Genre.cs"ファイルの名前します。

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

作成されたクラスをパブリック文字列プロパティの名前を追加します。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*注: 参考までに、、{get; 設定。} 表記は、# の自動実装プロパティの機能を使用します。これにより、プロパティの利点、バッキング フィールドを宣言することを必要とせず。*

次に、タイトルとジャンル プロパティを持つアルバム クラス (Album.cs という名前) を作成する同じ手順に従います。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

ここでモデルからの動的な情報を表示するビューを使用して StoreController を変更することができます。 デモンストレーションを目的として今 - という名前の要求 ID に基づく、アルバム場合、は、以下のビューのようにその情報を表示しますでした。

![](mvc-music-store-part-3/_static/image10.png)

1 つのアルバムの情報が表示されるように、ストアの詳細アクションを変更することで始めます。 先頭に"using"ステートメントを追加、 **StoreControllers**アルバム クラスを使用するたびに MvcMusicStore.Models.Album を入力する必要はありませんので、MvcMusicStore.Models 名前空間を含めるクラス。 そのクラスの"using"セクションが表示されます以下のとおりです。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

次に、更新します、Details コント ローラー アクション、文字列ではなく、ActionResult を返すように HomeController の Index メソッドと同様。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

ここでビューにアルバム オブジェクトを返すロジックを変更することができます。 このチュートリアルの後半で私たちがするからデータを取得: データベースが、今では使用「ダミー データ」を開始します。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*注: c# を使い慣れていない場合は、可能性がありますと想定すること、アルバムの変数がある遅延バインディングを意味 var の使用します。正しくありません-c# コンパイラを使用して型推論に基づいてどのアルバムがアルバムの種類を決定する、変数に代入しています、アルバムの種類として、アルバムのローカル変数をコンパイルするため、コンパイル時チェックと Visual Studio コード エディターを取得しますサポート。*

当社のアルバムを使用して、HTML 応答を生成するテンプレートの表示を今すぐ作成しましょう。 その前に、新しく作成したアルバム クラスの概要ビューの追加 ダイアログを認識するようにプロジェクトをビルドする必要があります。 プロジェクトをビルドする Debug⇨Build MvcMusicStore を選択してメニュー項目 (追加の手順を使用できます Ctrl + SHIFT+B ショートカット プロジェクトをビルドします)。

![](mvc-music-store-part-3/_static/image11.png)

これで、サポートするクラスを設定したは、ビュー テンプレートを作成する準備ができました。 Details メソッド内で右クリックし、コンテキスト メニューから [ビューの追加] を選択します。

![](mvc-music-store-part-3/_static/image12.png)

HomeController の前に行ったように、新しいビュー テンプレートを作成するでしょう。 StoreController から作成するためが既定で生成されます \Views\Store\Index.cshtml ファイル。

異なり、前にここ「厳密に型を作成する」ビューのチェック ボックスをオンします。 「View データ クラス」ドロップ ダウン リスト内で、「アルバム」クラスを選択し、ここです。 いるものと想定するビュー テンプレートを使用するように渡されるオブジェクトのアルバムを作成する「ビューの追加」ダイアログになります。

![](mvc-music-store-part-3/_static/image13.png)

[追加] ボタンをクリックしたとき、\Views\Store\Details.cshtml ビュー テンプレートが作成された、次のコードを格納しています。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

このビューがアルバム クラスに厳密に型であることを示す最初の行に注意してください。 渡されたアルバム オブジェクトでは、モデルのプロパティを簡単にアクセスしても、Visual Web Developer のエディターで IntelliSense の利点があるため、Razor ビュー エンジンが理解しています。

更新プログラム、 &lt;h2&gt;タグを次のように表示するには、その行を変更することで、アルバムのタイトルのプロパティが表示されるようにします。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

後にピリオドを入力すると、IntelliSense がトリガーされることに注意してください、@Modelキーワード、アルバム クラスをサポートするメソッドとプロパティを表示します。

みましょうこれで、プロジェクトを再実行してストア/詳細/5 URL を参照してください。 次のようなアルバムの詳細が表示されます。

![](mvc-music-store-part-3/_static/image14.png)

今すぐストア参照アクション メソッドのような更新をしましょう。 、ActionResult を返すように、メソッドを更新し、新しいジャンル オブジェクトを作成し、ビューに戻ることは、メソッドのロジックを変更します。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

厳密に型指定されたビューを追加し、参照メソッドを右クリックし、コンテキスト メニューから [ビューの追加] を選択して、厳密に型指定されたジャンル クラスに追加します。

![](mvc-music-store-part-3/_static/image15.png)

更新プログラム、 &lt;h2&gt;ジャンルの情報を表示する (/Views/Store/Browse.cshtml) でコードのビュー内の要素。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

これで、プロジェクトを再実行し、ストア/参照を参照してみましょうか。ジャンル ディスコの URL を = です。 以下のように表示される参照ページが表示されます。

![](mvc-music-store-part-3/_static/image16.png)

最後に、少し複雑な更新を行います、**インデックスの格納**アクション メソッドと、ストア内のすべてのジャンルの一覧を表示するビュー。 ジャンルを 1 つだけではなく、モデル オブジェクトとしてジャンルのリストを使用して、実行します。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

ストア インデックスのアクション メソッドを右クリックし、追加のビュー モデル クラスとジャンルを選択する前に、し、[追加] を選択します。

![](mvc-music-store-part-3/_static/image17.png)

最初は変更し、@modelビューがいくつかのジャンルを予想することを示す宣言が 1 つだけではなくオブジェクトします。 次のように/Store/Index.cshtml の最初の行を変更します。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

これは、いくつかのジャンル オブジェクトを保持するモデル オブジェクトで使用されることを Razor ビュー エンジンを指示します。 IEnumerable を使用して&lt;ジャンル&gt;一覧ではなく&lt;ジャンル&gt;より汎用的なので、IEnumerable インターフェイスをサポートするオブジェクトの種類を後で、モデルの種類を変更することを許可します。

次に、次のコードの完成した表示に示すように、モデル内のジャンル オブジェクトをループ処理します。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

ある完全な IntelliSense のサポート、このコードに入るときに入力するために注意してください"@Model"。 私たちは、すべてのメソッドと型ジャンルの IEnumerable でサポートされるプロパティを参照してください。

![](mvc-music-store-part-3/_static/image18.png)

"Foreach"ループ内では、Visual Web Developer は、各項目がジャンル、型の IntelliSense をそれぞれ確認ため、ジャンル型であるを認識します。

![](mvc-music-store-part-3/_static/image19.png)

次に、スキャフォールディング機能では、ジャンル オブジェクトを検査し、各必要がある、Name プロパティをループ処理し、それらを決定します。また、個々 の項目を編集、詳細、および削除のリンクも生成されます。 その後で、ストア マネージャーでの恩恵はすぐがここでは、単純なリストの代わりが今回は。

アプリケーションを実行して、/Store を参照、数とジャンルの一覧が表示されることがわかります。

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>ページ間のリンクの追加

/Store URL を現在のジャンルを一覧表示するには、単にプレーン テキストとしてジャンル名が一覧表示します。 みましょうかをプレーン テキストではなく代わりに、適切なストア/参照 URL へのジャンル名リンクされるように変更音楽のジャンル、ストア/参照に移動します"Disco"などをクリックするとしますか? ジャンル Disco URL を = です。 これらのリンクを使用して以下のようなコードの出力に \Views\Store\Index.cshtml ビュー テンプレート更新 **(これには入力しないでください - これを改善していきます)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

機能はしますが、ハードコーディングされた文字列があったために、後で問題になる可能性があります。 たとえば、コント ローラーの名前を変更する場合は、検索、コードを更新する必要があるリンクを検索して必要があります。

別のアプローチを使用して、HTML ヘルパー メソッドを利用することです。 ASP.NET MVC には、さまざまな次のように、一般的なタスクを実行する、コードの表示テンプレートから使用できる HTML ヘルパー メソッドが含まれています。 Html.ActionLink() のヘルパー メソッドは簡単に HTML を構築すること、便利な&lt;、&gt;リンク、URL パスが正しく URL エンコードを行うような面倒な詳細を管理します。

Html.ActionLink() には、リンクに必要な限り多くの情報を指定することをいくつかの異なるオーバー ロードがあります。 最も簡単な場合は、リンクのテキストのみと、クライアントで、ハイパーリンクがクリックされたときに移動するアクション メソッドを入力します。 たとえば、「ストア/」Index() メソッド リンク テキスト「移動するのストア インデックス」次の呼び出しを使用して、ストアの詳細ページにリンクできます。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*注: この場合、していない必要があります現在のビューをレンダリングしている同じコント ローラー内の別のアクション リンクしているだけであるため、コント ローラー名を指定します。*

[参照] ページに、リンクを 3 つのパラメーターを受け取る Html.ActionLink メソッドの別のオーバー ロードを使用するために、ただし、パラメーターを渡す必要があります。

- 1. リンク テキストは、ジャンル名が表示されます。
- 2. コント ローラー アクションの名前 (参照)
- 3. ルート パラメーターの値、(ジャンル) の名前と値 (ジャンル名) の両方を指定します。

すべてをまとめて、ここでは、ストアのインデックス ビューにこれらのリンクを書き込む方法を配置するには。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

これで、プロジェクトをもう一度実行して/Store/URL へのアクセス ジャンルの一覧が表示されます。 クリックされたときに、各ジャンルが – ハイパーリンク、ストア/参照にかかることですか? ジャンル =*[ジャンル]* URL。

![](mvc-music-store-part-3/_static/image3.jpg)

ジャンルの一覧については、HTML のようになります。

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [前へ](mvc-music-store-part-2.md)
> [次へ](mvc-music-store-part-4.md)
