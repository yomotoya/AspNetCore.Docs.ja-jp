---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: "手順 5: 編集フォームやテンプレート |Microsoft ドキュメント"
author: jongalloway
description: "このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 パート 5 は、編集フォームとテンプレートについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: cde6fe133291254531a797a434a4b2cdd226dd5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-5-edit-forms-and-templating"></a>手順 5: 編集フォームとテンプレート
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。  
>   
> MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。
> 
> このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 パート 5 は、編集フォームとテンプレートについて説明します。


過去の章では、データベースからデータを読み込むと、表示するおされました。 この章でも、データの編集を有効にしています。

## <a name="creating-the-storemanagercontroller"></a>StoreManagerController を作成します。

まずと呼ばれる新しいコント ローラーの作成、 **StoreManagerController**です。 このコント ローラーで取得 ASP.NET MVC 3 Tools Update で利用できるスキャフォールディング機能を活用します。 次に示すように、コント ローラーの追加 ダイアログのオプションを設定します。

![](mvc-music-store-part-5/_static/image1.png)

[追加] ボタンをクリックすると、ASP.NET MVC 3 のスキャフォールディング機構は、十分な量の作業が表示されます。

- Entity Framework のローカル変数を新しい StoreManagerController に作成されます。
- StoreManager フォルダー、プロジェクトのビュー フォルダーに追加します。
- アルバム クラスを厳密に型指定された Create.cshtml、Delete.cshtml、Details.cshtml、Edit.cshtml、および Index.cshtml のビューを追加します。

![](mvc-music-store-part-5/_static/image2.png)

新しい StoreManager コント ローラー クラスには、CRUD (作成、読み取り、更新、削除)、アルバムを操作する方法を知っているコント ローラーのアクションがモデル クラスとデータベース アクセスに対して、Entity Framework コンテキストを使用します。

## <a name="modifying-a-scaffolded-view"></a>スキャフォールディング ビューを変更します。

ことが重要うえで、このコードが生成された、ときに、このチュートリアル全体での執筆と同じように、標準の ASP.NET MVC コードがあることに注意してください。 定型的なコント ローラーのコードを記述し、厳密に型指定されたビューを手動で作成するのには、時間を保存するためのものしますが、不吉な警告に照準を変更する方法についてのコメントで始まるみたことがある生成されたコードの種類はありません、コードです。 これは、コードであり、変更する必要が。

そのため、クイック編集 StoreManager インデックス ビューを使用して開始しましょう (/Views/StoreManager/Index.cshtml)。 このビューは、ストア内で編集のアルバムの一覧を表示するテーブルを表示//リンクの削除を詳細アルバムのパブリック プロパティが含まれています。 AlbumArtUrl フィールドに、この表示で非常に効果的ではなく、削除されます。 &lt;テーブル&gt;セクション コードの表示 の削除、 &lt;th&gt;と&lt;td&gt;以下の強調表示された行が示すとおり、AlbumArtUrl 参照を囲む要素。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

コードの変更の表示は、次のように表示されます。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>ストア マネージャーで最初に表示

ここで、アプリケーションを実行し、/StoreManager を参照します。 これには、格納マネージャーのインデックスが変更、編集、詳細、および削除へのリンクを使用してストアのアルバムの一覧を表示が表示されます。

![](mvc-music-store-part-5/_static/image3.png)

[編集] リンクをクリックすると、アルバム、ジャンル、アーティストのドロップダウン リストを含むフィールドの編集フォームが表示されます。

![](mvc-music-store-part-5/_static/image4.png)

下部で、「リストに戻る」リンクをクリックし、アルバムの詳細 をクリックします。 これには、個々 のアルバムの詳細情報が表示されます。

![](mvc-music-store-part-5/_static/image5.png)

もう一度、一覧のリンクに戻る をクリックし、削除 リンクをクリックします。 これには、アルバムの詳細を表示し、それを削除することを確認しているかを確認する、確認ダイアログ ボックスが表示されます。

![](mvc-music-store-part-5/_static/image6.png)

下部にある [削除] ボタンをクリックすると、アルバムの削除し、削除アルバムを表示するインデックス ページに戻ります。

ストア マネージャーで終わっていませんがが動作しているコント ローラーおよびからを起動する CRUD 操作のコードの表示。

## <a name="looking-at-the-store-manager-controller-code"></a>ストア マネージャー コント ローラーのコードを見る

ストア マネージャー コント ローラーには、十分な量コードにはが含まれています。 みましょうこの上から下にします。 コント ローラーには、ある MVC コント ローラーだけでなく、モデルの名前空間への参照の一部の標準的な名前空間が含まれています。 コント ローラーは、データ アクセス用の各コント ローラー アクションで使用されている MusicStoreEntities のプライベート インスタンスを持ちます。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>ストア マネージャーのインデックスと詳細の操作

インデックス ビューは、アルバム、情報を含む各アルバムの参照先ジャンル、アーティスト以前見たようストア参照メソッドを使用する場合の一覧を取得します。 インデックス ビューが効率を実現し、元の要求にこの情報の照会、コント ローラーが表示されるように各アルバムのジャンル名、アーティスト名、表示できるように後は、リンクされたオブジェクトへの参照に制限します。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

StoreManager コント ローラーのコント ローラー アクションの詳細とまったく同じ動作に書いた以前のアルバムの問い合わせストア コント ローラーの詳細アクションとして Find() メソッドを使用して ID を使用して、ビューに戻ることです。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>アクション メソッドを作成

アクション メソッドを作成するは、フォーム入力を処理するためにここまでは、見たものとは少し異なります。 ユーザーが初めてアクセス/StoreManager/作成/それらが表示されます、空のフォーム。 この HTML ページには、&lt;フォーム&gt;をドロップダウン リストとテキスト ボックスを含む要素の入力要素のアルバムの詳細を入力することができます。

アルバムのフォーム値に入力した後は、これらの変更がデータベース内で保存するようにアプリケーションに戻す [保存] ボタンを押して、ことができます。 ユーザーが [保存] ボタンを押したとき、&lt;フォーム&gt;/StoreManager/作成/URL に HTTP POST を実行し、送信、&lt;フォーム&gt;HTTP POST の一部として値。

ASP.NET MVC では、–、/StoreManager/Create を初期の HTTP GET 参照を処理する 1 つ、StoreManagerController クラス内で 2 つ別々 の「作成」操作メソッドを実装することを有効にすると、これら 2 つの URL 呼び出しシナリオのロジックを簡単に分割することができます。/URL、および、その他の送信された変更の HTTP POST を処理します。

### <a name="passing-information-to-a-view-using-viewbag"></a>ViewBag を使用してビューに情報を渡す.

このチュートリアルで先ほど ViewBag を使用したしましたが多くについて説明します。 ViewBag を使用して、厳密に型指定されたモデル オブジェクトを使用せず、ビューに情報を渡すことができます。 ここで、編集の HTTP GET コント ローラーのアクションをドロップダウン リストを作成するフォームに両方のジャンル、アーティストの一覧を渡す必要がありますを行うには最も簡単な方法 ViewBag 項目として値を返すにします。

ViewBag は動的オブジェクトでは、これらのプロパティを定義するコードを記述することがなく ViewBag.Foo または ViewBag.YourNameHere を入力できます。 この場合、コント ローラーのコードを使用して ViewBag.GenreId と ViewBag.ArtistId GenreId と ArtistId、アルバムのプロパティの設定を行うには、フォームの送信 ドロップダウンの値ができるようにします。

目的にのみ構築された SelectList オブジェクトを使用してフォームには、これらのドロップダウン リストの値が返されます。 これは、次のようにコードを使用します。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

アクション メソッドのコードからわかるように、このオブジェクトを作成する 3 つのパラメーターを使用します。

- ドロップダウン リストが表示されている項目の一覧。 これは単なる文字列 - ジャンルの一覧を渡していることに注意してください。
- 次に、SelectList に渡されるパラメーターは、選択した値です。 この方法、SelectList は、リスト内の項目を事前に選択する方法を認識しています。 非常に類似した編集フォームに詳しく見てみるとわかりやすくなります。
- 最後のパラメーターは、表示されるプロパティです。 この場合、Genre.Name プロパティがユーザーに表示される内容を示すこのです。

念頭に、HTTP GET 作成操作は非常に単純な - SelectLists の 2 つは、ViewBag に追加し、モデル オブジェクトが渡されないフォーム (まだ作成されていない) からします。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>ビューの作成で、ドロップ ダウンを表示する HTML ヘルパー

ビューに、ドロップダウンの値を渡す方法について説明した、ためそれらの値を表示する方法を表示するビューを簡単に見てをみましょう。 コードの表示で (/Views/StoreManager/Create.cshtml)、ジャンル ボックスの一覧を表示する次の呼び出しが行われたわかりますダウンします。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

これは、HTML ヘルパーの一般的なタスクの表示を実行するユーティリティ メソッドと呼ばれます。 HTML ヘルパーは簡潔で読みやすく、コードの表示を維持するのに便利です。 Html.DropDownList ヘルパーは、ASP.NET MVC によって提供されるが、後でおわかりのよう、アプリケーションで再利用してコードの表示に独自のヘルパーを作成すること。

Html.DropDownList 呼び出しだけおく必要があります次の 2 つの場所を get 一覧表示するにはどのような値 (存在する場合) があらかじめ選択する必要があります。 GenreId、最初のパラメーターは、モデルまたは ViewBag GenreId をという名前の値を検索する DropDownList を指示します。 2 番目のパラメーターは、ドロップダウン リストで最初に、選択を表示する値を示すために使用されます。 このフォームがフォームを作成するためは、あらかじめ選択されている値はありません、String.Empty が渡されます。

### <a name="handling-the-posted-form-values"></a>フォームのポストされた値の処理

前に説明したよう各フォームに関連付けられている 2 つのアクション メソッドがあります。 1 つ目は、HTTP GET 要求を処理し、フォームを表示します。 2 つ目は、送信されたフォーム値を含む HTTP POST 要求を処理します。 コント ローラーのアクションが HTTP POST 要求に応答する必要がありますのみ ASP.NET MVC に指示する、[HttpPost] 属性を持つことに注意してください。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

この操作には、次の 4 つの責任があります。

- 1. フォームの値を読み取る
- 2. フォームの値がその検証ルールを渡すかどうかの確認します。
- 3. フォームの送信が有効な場合は、データを保存し、更新されたリストを表示
- 4. フォームの送信が有効でない場合は、検証エラーを含むフォームを再表示します

#### <a name="reading-form-values-with-model-binding"></a>モデル バインディングでのフォーム値の読み取り

コント ローラーのアクションはタイトル、価格、および AlbumArtUrl GenreId および ArtistId (、ドロップダウン リストから) 値およびテキスト ボックスの値を含むフォームの送信を処理しています。 フォームの値に直接アクセスすることはできますより適切な方法では、ASP.NET MVC に組み込まれているモデル バインド機能を使用します。 コント ローラーのアクションは、モデルの型をパラメーターとして受け取り、ASP.NET MVC はフォーム入力 (およびそのルートおよびクエリ文字列の値) を使用してその型のオブジェクトを設定しようとします。 これは、名前に一致するモデル オブジェクトのプロパティなどの値を探してで新しいアルバム オブジェクトの GenreId 値を設定するには、検索 GenreId の名前を入力します。 ASP.NET MVC での標準メソッドを使用して、ビューを作成するときに、フォームは常にレンダリングされますフィールドの名前だけが照合されますので、プロパティ名を使用して入力フィールド名として。

#### <a name="validating-the-model"></a>モデルの検証

ModelState.IsValid を単純な呼び出しでは、モデルが検証されます。 任意の検証規則、アルバム クラスに追加していないまだ - ビット - ため今すぐこのチェックはありませんはあまりすることによって行います。 重要な点は、この ModelStat.IsValid チェックは、検証規則に将来の変更は、コント ローラーのアクション コードへの更新を必要としないので、モデルの配置、検証規則に適用されます。

#### <a name="saving-the-submitted-values"></a>送信された値を保存します。

フォームの送信には、検証が成功した場合、値をデータベースに保存する時間を勧めします。 で Entity Framework、アルバム コレクションにモデルを追加して、SaveChanges を呼び出すことが必要です。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework には、値を保存する適切な SQL コマンドが生成されます。 データを保存した後おアルバムの一覧に戻るようにリダイレクトは、更新プログラムを参照してください。 これは、ここに表示するコント ローラー アクションの名前を持つ RedirectToAction を返すことによって行います。 この場合、インデックス メソッドです。

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>検証エラーを含む無効なフォームの送信を表示します。

場合は、入力の形式が無効です (HTTP GET 以外の場合) と同じ ViewBag にドロップダウン リストの値を追加および表示用のビューにバインドされたモデルの値が渡されます。 検証エラーが自動的に表示されるを使用して、 @Html.ValidationMessageFor HTML ヘルパー。

#### <a name="testing-the-create-form"></a>テスト フォームを作成します。

これをテストするには、実行/StoreManager/作成/- を参照しをアプリケーションが表示されます、StoreController 作成 HTTP-GET メソッドによって返されたを空白のフォーム。

いくつかの値を入力し、フォームを送信する作成ボタンをクリックします。

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>編集の処理

編集操作のペア (HTTP-GET および HTTP-POST) は、お見ただけでアクション メソッドを作成するとよく似ています。 編集シナリオでは、既存のアルバム、編集 HTTP の GET メソッドは、"id"パラメーターに基づくアルバムを読み込みます。 操作のために経由で渡されたルート。 AlbumId のアルバムを取得するためには、このコードは、詳細のコント ローラー アクションでを見てみました以前と同じです。 作成と同様に HTTP GET メソッドで、ドロップダウンの値は、ViewBag を介して返されます。/です。 これにより、モデル オブジェクトとして (これは厳密に型指定されたアルバム クラス) ビューにアルバムを返す ViewBag を使用して追加のデータ (例: ジャンルの一覧) を渡す際にできます。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

編集の HTTP POST 操作では、作成の HTTP POST 操作によく似ています。 唯一の違いは、db に新しいアルバムを追加する代わりにします。アルバム コレクションおを進めて、アルバムの現在のインスタンス db を使用します。Entry(album) および modified 状態に設定します。 これにより、Entity Framework 新規に作成するのではなく既存のアルバムを変更することがなります。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

この出力は、アプリケーションの実行/StoreManger/を参照し、アルバムの編集リンクをクリックすると、おテストできます。

![](mvc-music-store-part-5/_static/image9.png)

これには、編集の HTTP GET メソッドで表示される編集フォームが表示されます。 いくつかの値を入力し、[保存] ボタンをクリックします。

![](mvc-music-store-part-5/_static/image10.png)

フォームをポストこの、値を保存し、アルバム リストに値が更新されたことを示す us を返します。

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>削除の処理

削除では、編集および作成する、確認フォームを表示する 1 つのコント ローラーのアクションとフォームの送信を処理する別のコント ローラー アクションを使用すると同様のパターンに従います。

コント ローラーの HTTP GET Delete アクションは、以前のストア マネージャーの詳細コント ローラーのアクションとまったくです。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

私たちは、削除ビューのコンテンツ テンプレートを使用して、アルバムの型に厳密に型指定されたフォームを表示します。

![](mvc-music-store-part-5/_static/image12.png)

そのダウンを少しお簡略化できますが、削除のテンプレートには、モデルのすべてのフィールドが表示されます。 /Views/StoreManager/Delete.cshtml でコードの表示を次に変更します。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

これには、簡略化された削除の確認が表示されます。

![](mvc-music-store-part-5/_static/image13.png)

DeleteConfirmed アクションを実行すると、サーバーに送り返すにフォームと削除 をクリックします。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

HTTP POST 削除コント ローラーのアクションは、次のアクションを受け取ります。

- 1. ID でアルバムを読み込みます
- 2. アルバムを削除し、変更を保存
- 3. アルバムを一覧から削除されたことを示す、インデックスへのリダイレクト

これをテストするには、アプリケーションを実行し、/StoreManager を参照します。 一覧からアルバムを選択し、[削除] リンクをクリックします。

![](mvc-music-store-part-5/_static/image14.png)

これには、削除の確認画面が表示されます。

![](mvc-music-store-part-5/_static/image15.png)

[削除] ボタンをクリックすると、アルバムを削除し、ストア マネージャーのインデックスのページでは、アルバムが削除されたことを示しています us 返します。

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>カスタム HTML ヘルパーを使用してテキストを切り捨てる

ストア マネージャーのインデックス ページとして潜在的な問題の 1 つをしました。 アルバム タイトル、アーティスト名プロパティは両方とも十分に長い書式が、テーブル スロー可能性があります。 これらおよびその他のプロパティで、ビューを簡単に切り捨てることができるようにするためのカスタム HTML ヘルパーを作成します。

![](mvc-music-store-part-5/_static/image17.png)

Razor の@helper構文が行われたら、ビューで使用するための独自のヘルパー関数を作成する非常に簡単です。 /Views/StoreManager/Index.cshtml ビューを開き、次のコードを追加直接後に、@model行です。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

このヘルパー メソッドは、文字列とを許可する最大長を使用します。 ヘルパーが出力として指定されたテキストが指定された長さより短い場合は、-はします。 長い場合は、テキストが切り捨てられ、残りの部分の「...」を表示します。

アルバム タイトル、アーティスト名の両方のプロパティが 25 文字未満であることを確認、切り捨てのヘルパーを使用できます。 下の新しい切り捨てヘルパーを使用して完全なビュー コードが表示されます。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

今すぐ/StoreManager/URL を参照して、アルバムとタイトルが、最大長以下保持されます。

![](mvc-music-store-part-5/_static/image18.png)

注: を作成して、1 つのビューで、ヘルパーの使用の簡単な例を示します。 サイト全体で使用できるヘルパーの作成に関する詳細については、個人用ブログの投稿を参照してください: [http://bit.ly/mvc3-helper-options。](http://bit.ly/mvc3-helper-options)


>[!div class="step-by-step"]
[前へ](mvc-music-store-part-4.md)
[次へ](mvc-music-store-part-6.md)
