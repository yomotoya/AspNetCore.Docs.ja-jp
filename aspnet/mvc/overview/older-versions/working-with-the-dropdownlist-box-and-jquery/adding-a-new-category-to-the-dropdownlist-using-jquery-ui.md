---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: JQuery UI を使用した DropDownList に新しいカテゴリの追加 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: e2528b74b714a3f691b07ed2429b3fe9eb3c2074
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802527"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>JQuery UI を使用した DropDownList に新しいカテゴリを追加します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

HTML`Select`タグは、固定のカテゴリのデータの一覧を表示するのに最適ですが、新しいカテゴリを追加する必要があることがよくあります。 ジャンル"Opera"をデータベース内のカテゴリに追加することにしますか。 このセクションを使用して、新しいカテゴリの追加 ダイアログ ボックスを追加するのに、jQuery UI を使用します。 次の図は、ブラウザーで、UI を提示する方法を示します。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

ユーザーが選択すると、**新しいジャンルの追加**リンク、ポップアップ ダイアログ ボックスに新しいジャンル名 (および必要に応じて説明) のユーザー メッセージが表示されます。 次に示すイメージ、**追加ジャンル**ポップアップ ダイアログ。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

新しいジャンル名を入力するときに、**保存**ボタンが押されて、次の動作します。

1. AJAX 呼び出しを新しいジャンルをデータベースに保存して、JSON として、新しいジャンル情報 (ジャンル名と ID) を返す、ジャンル コント ローラーの Create メソッドにデータを送信します。
2. JavaScript では、選択リストに新しいジャンルのデータを追加します。
3. JavaScript は、選択した項目に新しいジャンルにします。

   、次の図で**Opera**データベースに追加され、で選択されている、**ジャンル**ドロップダウン リスト。 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

開く、 *Views\StoreManager\Create.cshtml*ファイルを開き、次のジャンルのマークアップに置き換えます次のコード。

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

`_ChooseGenre`部分ビューには、すべてのロジック [JavaScript と jQuery の新しいジャンルの追加] 機能を実装するためのフックにはが含まれます。 コードが完成したアーティストの UI で同じ処理を実行する簡単なことになります。

ソリューション エクスプ ローラーで右クリックし、 *Views\StoreManager*フォルダーと選択**追加**、し**ビュー**します。 **ビュー名**入力、入力`_ChooseGenre`選び**追加**します。 内のマークアップを置き換える、 *Views\StoreManager\\_ChooseGenre.cshtml*を次のファイル。

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

最初の行に渡していることを宣言して、`Album`私たちのモデルとして、作成ビューにあるステートメントをモデルとまったく同じです。 次の数行は、**ラベル**ヘルパーのマークアップ。 次の行が、 **DropDownList**ヘルパーが呼び出し元のビューを作成すると同じです。 次の行は、名前のリンクを追加します。 `Add New Genre`、およびボタンのように、スタイル。 行を含む`ValidationMessageFor`作成ビューから直接コピーされます。 次の行。

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

ID を非表示の div を作成します。`genreDialog`します。 JQuery を使用してフック、**追加ジャンル**id ダイアログ ボックス`genreDialog`でこの div です。 最後の 2 つのスクリプト タグには、新しいジャンルの追加 機能を実装するために使用するサンプルの JavaScript ファイルへのリンクが含まれます。 */Scripts/chooseGenre.js*ファイルがプロジェクトを見ていきますが、チュートリアルの後半で指定します。

アプリケーションを実行し、をクリックして、**新しいジャンルの追加**ボタンをクリックします。 **追加ジャンル** ダイアログ ボックスに、入力**Opera**で、**名前**入力ボックス。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

**[保存]** ボタンをクリックします。 AJAX 呼び出しは、Opera のカテゴリを作成し、Opera をドロップダウン リストを設定し、として選択されているジャンル Opera を設定します。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

アーティスト、タイトル、および価格を入力し、選択、**作成**ボタンをクリックします。 8.99 ドル未満の価格を入力すると、インデックス ビューの上部にある新しいアルバムが表示されます。 新しいアルバム エントリがデータベースに保存されたことを確認します。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

1 つだけの文字を持つ新しいジャンルを作成してください。 次のコードで、 *Models\Genre.cs*ファイルは、ジャンル名の最小値と最大の長さを設定します。

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

クライアント側の検証では、2 ~ 20 文字の文字列を入力する必要がありますを報告します。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>新しいジャンル方法を調べることは、データベースと選択リストに追加されます。

開く、 *Scripts\chooseGenre.js*ファイルを開き、コードを確認します。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

2 行目は、ID を使用して`genreDialog`の div タグにダイアログ ボックスを作成する、 *Views\StoreManager\\_ChooseGenre.cshtml*ファイル。 名前付きパラメーターのほとんどは、一目瞭然です。 `autoOpen`パラメーターが false に設定を選択すると、**作成ジャンル**ボタンがダイアログ ボックスを明示的に開く (後者にこの説明は)。 ダイアログに 2 つのボタンがあります**保存**と**キャンセル**します。 **キャンセル**ボタンがダイアログを閉じます。 次のコードは、**保存**関数のボタンをクリックします。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

`var createGenreForm`からが選択されている、 `createGenreForm` id。 `createGenreForm` ID は、次のコードで設定された、 *Views\Genre\\_CreateGenre.cshtml*ファイル。

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx)ヘルパーのオーバー ロードで使用される、 *Views\Genre\\_CreateGenre.cshtml*ファイル、フォームを送信する URL を含むアクション属性を持つ HTML が生成されます。 これは、ブラウザーでアルバムの作成 ページを表示して、ブラウザーで表示するソースの選択を確認できます。 次のマークアップは、生成される HTML フォーム タグを含むを示します。

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery`$.post`線は、action 属性への AJAX 呼び出しを作成する (`/StoreManager/Create`) からデータを渡すと、**ジャンルの作成** ダイアログ ボックス。 データは、新しいジャンルとオプションの説明の名前で構成されます。 AJAX 呼び出しが成功した場合は、新しいジャンル名と値は、Select のマークアップに追加され、新しいジャンルが、選択した値に設定されています。 これは動的に生成されたマークアップであるため、ブラウザーで、ソースを表示して、新しいオプションを選択を表示できません。 IE 9 F12 開発者ツールで新しい HTML を表示できます。 Internet Explorer 9 で、新しい選択オプションを表示するには、F12 開発者ツールを開始する F12 キーを押します。 [作成] ページに移動し、新しいジャンルがジャンルの選択リストで選択されているために、新しいジャンルを追加します。 F12 開発者ツール。

1. [HTML] タブを選択します。
2. 更新アイコンをクリックします。  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. 検索ボックスに、GenreID を入力します。
4. [次へ] のアイコンを使用します。   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   次の select タグに移動します。

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. 最後のオプションの値を展開します。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

次のコードで、 *Scripts\chooseGenre.js*ファイルは、方法を示しています、**新しいジャンルの追加**ボタンは、click イベントに接続を取得する方法と、**新しいジャンルの追加** ダイアログ ボックスが作成されます。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

最初の行にアタッチ をクリック関数を作成する、**新しいジャンルの追加**ボタンをクリックします。 Views\StoreManager から次のマークアップ\\_ChooseGenre.cshtml ファイルの表示方法、**新しいジャンルの追加**ボタンが作成されます。

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Load メソッドを作成しますジャンルの追加 ダイアログを開くし、jQuery を呼び出す`parse`メソッド、ダイアログ ボックスに入力されたデータのクライアント検証が行われるようにします。

このセクションでは、選択リストに新しいカテゴリのデータを追加するために使用できるダイアログを作成する方法を説明しました。 アーティストの選択リストに新しいアーティストを追加する UI を作成する同じ手順を行うことができます。 このチュートリアルは、ASP.NET MVC の HTML ヘルパーの操作の概要を与え**DropDownList**します。 操作の詳細については、 **DropDownList**、以下の追加の参照セクションを参照してください。 ご連絡くださいかどうか、このチュートリアルは便利なされました。

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>その他の参照

- [ASP.NET MVC – カスケード ドロップダウンは、チュートリアルを一覧表示](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx)によって[Radu enuca が](https://weblogs.asp.net/raduenuca/default.aspx)
- [選択した](http://harvesthq.github.com/chosen/)複数選択やフィルター処理をサポートする JavaScript のプラグイン。

### <a name="contributors"></a>共同作成者

- [Radu enuca が](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>校閲者

- Jean Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike 教皇
- Tom Dykstra

> [!div class="step-by-step"]
> [前へ](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
