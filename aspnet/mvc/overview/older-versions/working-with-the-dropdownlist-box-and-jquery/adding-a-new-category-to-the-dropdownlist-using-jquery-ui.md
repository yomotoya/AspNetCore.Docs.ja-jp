---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: "JQuery UI を使用した DropDownList に新しいカテゴリを追加する |Microsoft ドキュメント"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 0cc51fbe84124a62f0c1254faab796cbcdc7efd6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>JQuery UI を使用した DropDownList に新しいカテゴリを追加します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

HTML`Select`タグが固定のカテゴリのデータの一覧を表示するのに適していますが、多くの場合、新しいカテゴリを追加する必要があります。 データベース内のカテゴリをジャンル"Opera"を追加する必要があるとしますか。 このセクションを使用して、新しいカテゴリを追加 ダイアログ ボックスを追加するのに、jQuery UI を使用します。 次の図は、ブラウザーで UI を提示する方法を示しています。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

ユーザーが選択すると、**新しいジャンルの追加**リンク、ポップアップ ダイアログ ボックスは、新しいジャンル名前 (および必要に応じて説明) はユーザーを要求します。 次に示す、イメージ、**追加ジャンル**ポップアップ ダイアログ。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

新しいジャンル名が入力されている場合、**保存**ボタンが押されて、次の動作します。

1. AJAX 呼び出しは、ジャンル コント ローラー、新しいジャンルをデータベースに保存し、JSON として、新しいジャンル情報 (ジャンルの名前と ID) を返しますの Create メソッドにデータを送信します。
2. JavaScript では、選択リストに新しいジャンル データを追加します。
3. JavaScript は、選択した項目を新しいジャンルになります。

 下の画像で**Opera**データベースに追加されで選択されている、**ジャンル**ドロップ ダウン リスト。 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

開く、 *Views\StoreManager\Create.cshtml*ファイルを開き、次のマークアップをジャンルを置き換えます次のコード。

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

`_ChooseGenre`部分ビューには、JavaScript と jQuery の新しいジャンルの追加 機能を実装するために使用をフックするために、すべてのロジックにが含まれます。 コードが完成した後は、アーティスト UI で同じ操作を実行する単純なことはできます。

ソリューション エクスプ ローラーで右クリックし、 *Views\StoreManager*フォルダーと選択**追加**、し**ビュー**です。 **ビュー名**入力、入力`_ChooseGenre`を選択し、**追加**です。 内のマークアップを置き換える、 *Views\StoreManager\\_ChooseGenre.cshtml*を次のファイル。

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

最初の行で渡していることを宣言、`Album`モデルとビューを作成するステートメントをモデルとまったく同じです。 次の数行、**ラベル**ヘルパー マークアップ。 次の行は、 **DropDownList**ヘルパーを呼び出すと、元のビューを作成するものとまったく同じです。 次の行の名前を持つリンクを追加`Add New Genre`、およびボタンのようなスタイルします。 行を含む`ValidationMessageFor`作成ビューから直接コピーされます。 次の行では:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

ID を持つ非表示の div を作成`genreDialog`です。 JQuery を使用してフック、**追加ジャンル**ID を持つ ダイアログ ボックス`genreDialog`この位置の div で 最後の 2 つのスクリプト タグには、追加新しいジャンル機能の実装を使用して、JavaScript ファイルへのリンクが含まれています。 */Scripts/chooseGenre.js*ファイルがプロジェクト内の見ていきますが、チュートリアルの後半で提供されています。

アプリケーションを実行し、をクリックして、**新しいジャンルの追加**ボタンをクリックします。 **追加ジャンル** ダイアログ ボックスで、入力**Opera**で、**名前**入力ボックス。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

クリックして、**保存**ボタンをクリックします。 AJAX 呼び出しは Opera カテゴリを作成およびし、Opera、ドロップダウン リストを設定し、選択したジャンルとして Opera を設定します。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

アーティスト、タイトル、および価格を入力し、選択、**作成**ボタンをクリックします。 8.99 ドル未満の価格を入力する場合は、新しいアルバムがインデックス ビューの上部に表示されます。 新しいアルバム エントリがデータベースに保存されたことを確認します。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

1 つだけの文字で新しいジャンルを作成してください。 次のコードで、 *Models\Genre.cs*ファイルはジャンル名の最小値と最大の長さを設定します。

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

クライアント側の検証は、2 ~ 20 文字、文字列を入力する必要がありますを報告します。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>新しいジャンル方法を調べることが、データベースと、選択リストに追加されます。

開く、 *Scripts\chooseGenre.js*ファイルし、コードを調べます。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

2 番目の行は、ID を使用して`genreDialog`の div タグのダイアログ ボックスを作成する、 *Views\StoreManager\\_ChooseGenre.cshtml*ファイル。 名前付きパラメーターのほとんどは自明です。 `autoOpen`パラメーターが false に設定を選択すると、**作成ジャンル**ボタンがダイアログ ボックスを明示的に開く (後者にこの説明は)。 ダイアログ ボックスが 2 つのボタン**保存**と**キャンセル**です。 **キャンセル**ボタンがダイアログを閉じます。 次のコードは、**保存**関数のボタンをクリックします。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

`var createGenreForm`からが選択されている、 `createGenreForm` id です。 `createGenreForm`で見つかった次のコードで ID が設定されて、 *Views\Genre\\_CreateGenre.cshtml*ファイル。

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

[Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd492714.aspx)ヘルパーのオーバー ロードで使用される、 *Views\Genre\\_CreateGenre.cshtml*ファイルは、フォームを送信する URL を含むアクション属性を持つ HTML を生成します。 これは、ブラウザーでアルバムの作成 ページを表示して、ブラウザーで表示するソースを選択して確認できます。 次のマークアップでは、生成された HTML フォーム タグを含むを示します。

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery`$.post`線は、action 属性への AJAX 呼び出しを作成 (`/StoreManager/Create`) からのデータで渡すと、**ジャンルの作成** ダイアログ ボックス。 データは、新しいジャンルとオプションの説明の名前で構成されます。 AJAX 呼び出しが成功した場合は、ジャンルの新しい名前と値は、Select のマークアップに追加され、新しいジャンルが選択した値に設定されています。 これは動的に生成されたマークアップであるため、ブラウザーで、ソースを表示して、新しいオプションを選択を参照してくださいことはできません。 IE 9 F12 開発者ツールを使用して新しい HTML を表示できます。 表示するには、新しいオプションを選択、Internet Explorer 9 では、F12 開発者ツールを起動する F12 キーを押します。 [作成] ページに移動し、新しいジャンルがジャンルの選択リストで選択されているために、新しいジャンルを追加します。 F12 開発者ツールで。

1. [HTML] タブを選択します。
2. 更新アイコンをクリックします。  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. [検索] ボックスで、GenreID を入力します。
4. [次へ] のアイコンを使用   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
 次の select タグに移動します。

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. 最後のオプションの値を展開します。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

次のコードで、 *Scripts\chooseGenre.js*ファイル、方法を示します、**新しいジャンルの追加**ボタン クリック イベントに接続方法、および**新しいジャンルの追加** ダイアログ ボックスが作成されます。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

最初の行にアタッチされているをクリックして関数を作成する、**新しいジャンルの追加**ボタンをクリックします。 Views\StoreManager から次のマークアップ\\_ChooseGenre.cshtml ファイルの表示方法、**新しいジャンルの追加** ボタンを作成します。

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Load メソッドの作成およびジャンルの追加 ダイアログを開く、し、jQuery を呼び出す`parse`メソッド、ダイアログ ボックスに入力されたデータにクライアントの検証が発生するようにします。

このセクションでは、選択リストに新しいカテゴリのデータを追加するために使用するダイアログを作成する方法を学習しました。 新しいアーティストをアーティストの選択リストに追加するための UI を作成する同じ手順を実行できます。 このチュートリアルには、ASP.NET MVC の HTML ヘルパーの使用の概要から与えられた**DropDownList**です。 操作の詳細については、 **DropDownList**、以下の追加の参照セクションを参照してください。 お知らせかどうか、このチュートリアルは便利なされました。

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>その他のリファレンス

- [ASP.NET MVC – カスケード ドロップダウン一覧チュートリアル](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx)によって[Radu enuca が](https://weblogs.asp.net/raduenuca/default.aspx)
- [選択した](http://harvesthq.github.com/chosen/)複数選択やフィルター処理をサポートする JavaScript プラグインします。

### <a name="contributors"></a>貢献者

- [Radu enuca が](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>校閲者

- Jean Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike 教皇
- Tom Dykstra

>[!div class="step-by-step"]
[前へ](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
