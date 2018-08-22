---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'パート 5: 編集フォームとテンプレート |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 5 では、編集フォームとテンプレートについて説明します。
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: c1065dcb45b6d28672edba32b95c7fc476c8b944
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823920"
---
<a name="part-5-edit-forms-and-templating"></a>パート 5: 編集フォームとテンプレート
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。  
>   
> MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。
> 
> このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 5 では、編集フォームとテンプレートについて説明します。


過去の章では、データベースからデータの読み込みがお表示。 この章でも、データの編集を有効にしますします。

## <a name="creating-the-storemanagercontroller"></a>StoreManagerController を作成します。

まずと呼ばれる新しいコント ローラーの作成、 **StoreManagerController**します。 このコント ローラーで取得した ASP.NET MVC 3 Tools Update のスキャフォールディング機能を活用します。 次に示すように、コント ローラーの追加 ダイアログのオプションを設定します。

![](mvc-music-store-part-5/_static/image1.png)

[追加] ボタンをクリックすると、ASP.NET MVC 3 のスキャフォールディング機能によってため、作業量は良いことが表示されます。

- Entity Framework のローカル変数を新しい StoreManagerController に作成されます。
- StoreManager フォルダー、プロジェクトの Views フォルダーに追加します。
- アルバム クラスを厳密に型指定された Create.cshtml、Delete.cshtml、Details.cshtml、Edit.cshtml、および Index.cshtml のビューを追加します

![](mvc-music-store-part-5/_static/image2.png)

新しい StoreManager コント ローラー クラスには、CRUD (作成、読み取り、更新、削除)、アルバムを操作する方法がわかっているコント ローラー アクションは、モデル クラスと、データベースへのアクセスを Entity Framework コンテキストを使用します。

## <a name="modifying-a-scaffolded-view"></a>スキャフォールディングされたビューを変更します。

これは私たちにとって、このコードが生成されたときに、このチュートリアル全体での執筆と同様、標準の ASP.NET MVC コードがあることに注意してください。 コント ローラーの定型コードを記述および厳密に型指定されたビューを手動で作成するのには時間を節約するためのものが、そうでは変更しないでください方法についてのコメントに悲惨な警告接頭辞かもしれません生成されたコードの種類が、コードです。 これは、コードと、これを変更する必要があります。

そのため、まず StoreManager インデックス ビューにすばやく編集から始めます (/Views/StoreManager/Index.cshtml)。 このビューが表示、編集、ストア内のアルバムの一覧を表示するテーブル/詳細/リンクを削除およびアルバムのパブリック プロパティが含まれています。 この表示で非常に便利ですがないため AlbumArtUrl フィールド、削除する予定です。 &lt;テーブル&gt;セクションの コードの表示、削除、 &lt;th&gt;と&lt;td&gt;以下の強調表示された行が示すとおり、AlbumArtUrl 参照を囲む要素。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

コードの変更の表示は次のように表示されます。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>ストア マネージャーの概要

これでアプリケーションを実行し、StoreManager/を参照します。 ストア マネージャーのインデックスが変更、編集、詳細、および削除へのリンクを使用してストアで、アルバムの一覧を表示が表示されます。

![](mvc-music-store-part-5/_static/image3.png)

[編集] リンクをクリックすると、アルバム、ジャンル、アーティストのドロップダウン リストを含むフィールドの編集フォームが表示されます。

![](mvc-music-store-part-5/_static/image4.png)

下部で、「リストに戻る」リンクをクリックし、アルバムは詳細情報リンクをクリックします。 これには、個々 のアルバムの詳細情報が表示されます。

![](mvc-music-store-part-5/_static/image5.png)

ここでも、リンクの一覧に戻る をクリックし、削除のリンクをクリックします。 これには、アルバムの詳細を表示しそれを削除することを確認しているかどうかを求める確認のダイアログが表示されます。

![](mvc-music-store-part-5/_static/image6.png)

下部にある [削除] ボタンをクリックすると、アルバムは削除し、削除、アルバムを表示する Index ページに戻ります。

ストア マネージャーで終わっていませんが、コント ローラーとの CRUD 操作からを起動するには、コードの表示の操作があります。

## <a name="looking-at-the-store-manager-controller-code"></a>ストア マネージャー コント ローラーのコードを見てください。

ストア マネージャー コント ローラーには、十分な量コードにはが含まれています。 それではこの上から下へ。 コント ローラーには、モデルの名前空間への参照と同様に、MVC コント ローラーの一部の標準名前空間が含まれています。 コント ローラーは、コント ローラー アクションの各データ アクセスに使用される MusicStoreEntities のプライベート インスタンスを持ちます。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>ストア マネージャー Index と Details アクション

インデックス ビューには、情報を含む各アルバムの参照されているジャンル、アーティスト、ストアの参照方法に取り組むときに以前説明したように、アルバムの一覧を取得します。 インデックス ビューが効率的で元の要求では、この情報の照会、コント ローラーが表示されるように各アルバムのジャンル名とアーティスト名、表示できるように、リンク オブジェクトへの参照をフォローします。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

StoreManager コント ローラーの詳細のコント ローラー アクションとまったく同じ動作フィクション以前は、アルバムのクエリ ストアのコント ローラーの詳細アクションとして Find() メソッドを使用して ID を使用して、ビューに戻ることです。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>アクション メソッドの作成

アクション メソッドを作成するは、フォームの入力を処理するため、これまでに見たものとは少し異なります。 ユーザーが初めてアクセス/StoreManager/作成/が表示されますの空のフォーム。 この HTML ページが表示されます、&lt;フォーム&gt;ドロップダウンと textbox を含む要素の入力要素のアルバムの詳細を入力することができます。

アルバムのフォーム値に入力した後、データベース内で保存するようにアプリケーションをこれらの変更を送信する [保存] ボタンを押すことができます。 ユーザーが [保存] ボタンを押すと、&lt;フォーム&gt;/StoreManager/作成/URL に HTTP POST を実行し、送信、&lt;フォーム&gt;HTTP POST の一部として値。

ASP.NET MVC では、–、/StoreManager/Create に初期 HTTP GET の参照を処理するために 1 つ、StoreManagerController クラス内で 2 つ個別の「作成」操作メソッドの実装を有効にすると、これら 2 つの URL の呼び出しのシナリオのロジックを簡単に分割できます。/URL、および他の送信された変更の HTTP POST を処理します。

### <a name="passing-information-to-a-view-using-viewbag"></a>ViewBag を使用してビューに情報を渡す

このチュートリアルで先ほど ViewBag を使用したしましたがそれについて詳しく説明していませんでした。 ViewBag を使用して、厳密に型指定されたモデル オブジェクトを使用せず、ビューに情報を渡すできます。 ここで、編集の HTTP GET のコント ローラー アクションが、ドロップダウン リストからを設定するためのフォームに両方のジャンル、アーティストの一覧を渡す必要があり、ViewBag 項目として返すことを行う最も簡単な方法は。

ViewBag は動的オブジェクトをこれらのプロパティを定義するコードを記述することがなく ViewBag.Foo または ViewBag.YourNameHere を入力できます。 ここで、コント ローラーと使用して ViewBag.GenreId ViewBag.ArtistId GenreId、ArtistId、アルバムのプロパティの設定を行うには、フォームで送信 ドロップダウンの値ができるように、します。

目的のためだけに組み込まれている SelectList オブジェクトを使用してフォームには、これらのドロップダウンの値が返されます。 これは、このようなコードを使用します。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

アクション メソッドのコードからわかる、このオブジェクトを作成する 3 つのパラメーターを使用します。

- ドロップダウン リストが表示されている項目の一覧。 そうでは単なる文字列 - ジャンルのリストを渡しているだけのことに注意してください。
- [次へ]、SelectList に渡されるパラメーターは、選択されている値です。 この、SelectList が事前に、リスト内の項目を選択する方法を認識する方法。 これは非常に似ている編集フォームを見たときに理解しやすくなります。
- 最後のパラメーターは、表示されるプロパティです。 この場合、Genre.Name プロパティは、ユーザーに表示される内容はこれを示すです。

念頭には、HTTP GET の作成操作は非常に単純 - SelectLists の 2 つは、ViewBag に追加し、モデル オブジェクトが渡されないフォーム (まだ作成されていない) からします。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>ビューの作成に、ドロップ リストを表示する HTML ヘルパー

ドロップダウンの値が、ビューに渡される方法について話してきました、ためこれらの値を表示する方法を表示するビューを簡単に見てをみましょう。 コードの表示 (/Views/StoreManager/Create.cshtml)、ジャンルのドロップを表示する次の呼び出しが行われますが表示されますダウンします。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

これは、HTML ヘルパーの一般的なタスクの表示を実行するためのユーティリティ メソッドと呼ばれます。 HTML ヘルパーは、簡潔で読みやすく、コードの表示を維持することで非常に便利です。 Html.DropDownList ヘルパーは、ASP.NET MVC、によって提供されますが、アプリケーションで再利用してコードの表示の独自のヘルパーを作成することは後でおわかりです。

だけ、Html.DropDownList 呼び出しは、次の 2 つの場所を表示するには、ボックスの一覧を取得し (ある場合) は、どのような値は事前に選択する必要があります通知する必要があります。 最初のパラメーターでは、GenreId では、DropDownList、モデルまたは ViewBag GenreId をという名前の値を検索するように指示します。 2 番目のパラメーターは、ドロップダウン リストで最初に、選択を表示する値を示すために使用されます。 このフォームがフォームを作成するためは、あらかじめ選択されている値はありません、String.Empty が渡されます。

### <a name="handling-the-posted-form-values"></a>ポストされたフォーム値の処理

前に説明した各フォームに関連付けられている 2 つのアクション メソッドがあります。 1 つ目は、HTTP GET 要求を処理し、フォームを表示します。 2 つ目は、送信されたフォーム値を含む HTTP POST 要求を処理します。 コント ローラー アクションが HTTP POST 要求に応答する必要がありますのみ ASP.NET MVC に伝えますが、[HttpPost] 属性を持つことに注意してください。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

このアクションでは、次の 4 つの責任があります。

- 1. フォームの値を読み取る
- 2. フォームの値が、検証規則を渡すかどうかの確認します。
- 3. フォームの送信が有効な場合は、データを保存し、更新された一覧を表示
- 4. フォームの送信が有効でない場合は、検証エラーを含むフォームを再表示します。

#### <a name="reading-form-values-with-model-binding"></a>モデル バインドでのフォーム値の読み取り

コント ローラー アクションのタイトル、価格、および AlbumArtUrl GenreId、ArtistId の (ドロップダウン リスト) からの値とテキスト ボックスの値を含むフォームの送信を処理します。 フォームの値に直接アクセスすることはできますが、ASP.NET MVC に組み込まれているモデル バインド機能を使用するほうが効果的です。 コント ローラーのアクションは、モデルの型をパラメーターとして受け取り、ASP.NET MVC はフォームの入力 (およびそのルートとクエリ文字列の値) を使用してその型のオブジェクトを設定しようとします。 これは名前が一致するモデル オブジェクトのプロパティなどの値を探すことによって、GenreId という名前の入力に対して GenreId に新しいアルバム オブジェクトの値を設定するときに見えます。 ASP.NET MVC で標準的なメソッドを使用してビューを作成するときに、入力フィールドの名前と一致するようにこのフィールド名はだけプロパティ名を使用して、フォーム、常に表示されます。

#### <a name="validating-the-model"></a>モデルの検証

ModelState.IsValid に対する単純な呼び出しで、モデルが検証されます。 アルバム クラスにその検証ルールを追加していませんをまだ持たない少し - ここにこのチェックはあまり行います。 どのようなことが重要ですが、この ModelStat.IsValid チェックが検証規則に将来の変更は、コント ローラーのアクション コードの更新を必要としないので、私たちのモデルの配置、検証規則に適用されます。

#### <a name="saving-the-submitted-values"></a>送信された値を保存しています

フォームの送信には、検証が成功した場合、値をデータベースに保存する時間を勧めします。 Entity framework、アルバムのコレクションにモデルを追加して、SaveChanges を呼び出すことが必要ですが。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework には、値を保持する適切な SQL コマンドが生成されます。 データを保存した後リダイレクト アルバムの一覧に戻り、更新プログラムを確認するためです。 これは、ここに表示するコント ローラー アクションの名前を持つ RedirectToAction を返すことによって行います。 この場合、Index メソッドです。

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>検証エラーを含む無効なフォームの送信を表示します。

無効な形式の入力の場合は、ドロップダウンの値は (HTTP GET の場合) のように ViewBag に追加され、表示のビューにバインドされたモデルの値が渡されました。 検証エラーを使用して自動的に表示されます、 @Html.ValidationMessageFor HTML ヘルパー。

#### <a name="testing-the-create-form"></a>テスト フォームを作成します。

/StoreManager/作成/- を参照しをアプリケーションを実行してこれをテストするには、これがわかります StoreController 作成 HTTP GET メソッドによって返された空白のフォーム。

いくつかの値を入力し、フォームを送信して、作成ボタンをクリックします。

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>編集の処理

編集操作のペア (HTTP GET と HTTP POST) は、見たので、Create アクション メソッドとよく似ています。 編集シナリオでは、既存のアルバム、編集 HTTP GET メソッドは、"id"パラメーターに基づいて、アルバムを読み込みますの操作のため、ルート経由で渡さ。 AlbumId のアルバムを取得するためには、このコードは、詳細のコント ローラーのアクションで紹介しました以前と同じです。 同様の作成]、[HTTP GET メソッドでは、ViewBag を使用して、ドロップダウンの値が返されます。 これにより、ViewBag を使用して追加データ (ジャンルのリストなど) を渡す際にビュー (これは、アルバム クラスを厳密に型指定) に、モデル オブジェクトとしてのアルバムを返すことができます。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

編集の HTTP POST アクションは、作成の HTTP POST アクションによく似ています。 唯一の違いは、データベースに新しいアルバムを追加する代わりにです。アルバムのコレクションを見いだし、アルバムの現在のインスタンス db を使用します。Entry(album) とその状態を Modified に設定します。 これにより、Entity Framework 新規に作成するのではなく既存のアルバム変更を加えていることができます。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

この出力をテスト/StoreManger/を参照し、アルバムの編集リンクをクリックすると、アプリケーションを実行できます。

![](mvc-music-store-part-5/_static/image9.png)

これには、編集の HTTP GET メソッドで表示される編集フォームが表示されます。 いくつかの値を入力し、[保存] ボタンをクリックします。

![](mvc-music-store-part-5/_static/image10.png)

値を保存、およびアルバムのリストに値が更新されたことを示すことを返しますこれがフォームを投稿します。

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>削除の処理

削除では、編集および作成、確認のフォームを表示する 1 つのコント ローラー アクションと、フォームの送信を処理するために別のコント ローラー アクションを使用すると同様のパターンに従います。

HTTP GET Delete コント ローラー アクションは、前のマネージャーの保存詳細コント ローラー アクションと正確にします。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

削除ビュー コンテンツ テンプレートを使用して、アルバムの型に厳密に型指定されたフォームが表示されます。

![](mvc-music-store-part-5/_static/image12.png)

削除のテンプレートが、モデルのすべてのフィールドを示していますが、そのダウン大幅に簡略化できます。 次に、/Views/StoreManager/Delete.cshtml でコードの表示を変更します。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

これには、簡略化された削除の確認が表示されます。

![](mvc-music-store-part-5/_static/image13.png)

DeleteConfirmed アクションを実行すると、サーバーに投稿するフォームと削除 ボタンをクリックします。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

この HTTP POST 削除コント ローラー アクションは、次の操作を受け取ります。

- 1. ID でアルバムを読み込みます
- 2. アルバムを削除し、変更を保存
- 3. アルバムを一覧から削除されたことを示す、インデックスへのリダイレクト

これをテストするには、アプリケーションを実行し、/StoreManager を参照します。 一覧から、アルバムを選択し、[削除] リンクをクリックします。

![](mvc-music-store-part-5/_static/image14.png)

これには、削除の確認画面が表示されます。

![](mvc-music-store-part-5/_static/image15.png)

削除ボタンをクリックするとでは、アルバムを削除し、アルバムが削除されたことを示しています。 ストア マネージャーのインデックスのページに私たちを返します。

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>カスタム HTML ヘルパーを使用してテキストを切り捨てる

ストア マネージャーのインデックス ページは、潜在的な問題の 1 つ作成しました。 アルバム タイトル、アーティスト名のプロパティは両方長くして、テーブルを書式設定をスローできます。 これらおよびその他のプロパティ ビューを簡単に切り捨てることができるようにするためのカスタム HTML ヘルパーを作成します。

![](mvc-music-store-part-5/_static/image17.png)

Razor の@helper構文はできるようにして、ビューで使用するため、独自のヘルパー関数を作成する非常に簡単です。 /Views/StoreManager/Index.cshtml ビューを開き、次のコードを追加直接後、@model行。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

このヘルパー メソッドは、文字列およびを許可する最大の長さを受け取ります。 ヘルパーを出力として指定されたテキストが指定された長さよりも短い場合は、-です。 長い場合は、テキストが切り捨てられ、残りの部分の「...」をレンダリングします。

アルバムのタイトル、アーティスト名の両方のプロパティが 25 文字未満であることを確認、切り捨てのヘルパーを使用できます。 下に、新規切り捨てヘルパーを使用してコードの完全な表示が表示されます。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

今すぐ/StoreManager/URL を参照しますアルバムとタイトルが、最大長以下保持されます。

![](mvc-music-store-part-5/_static/image18.png)

注: を作成して、ヘルパーを使用して、1 つのビューの単純な例が表示されます。 詳細については、サイト全体で使用できるヘルパーの作成は、私のブログ記事を参照してください。 [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


> [!div class="step-by-step"]
> [前へ](mvc-music-store-part-4.md)
> [次へ](mvc-music-store-part-6.md)
