---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: ASP.NET MVC で DropDownList ヘルパーをスキャフォールディングする方法を調べること |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 542790b7f475cc641ed26ff3187c25c25118e0ed
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577848"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>ASP.NET MVC で DropDownList ヘルパーをスキャフォールディングする方法を調べる
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

**ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*クリックしてフォルダー**コント ローラーの追加**します。 コント ローラー **StoreManagerController**します。 オプションを設定、**コント ローラーの追加**ダイアログの次の図に示すようにします。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

編集、 *StoreManager\Index.cshtml*表示および削除`AlbumArtUrl`します。 削除する`AlbumArtUrl`は、プレゼンテーションを読みやすくします。 完成したコードを以下に示します。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

開く、 *Controllers\StoreManagerController.cs*ファイルを見つけて、`Index`メソッド。 追加、`OrderBy`句のため、アルバムは価格で並べ替えられます。 完全なコードは、以下に示します。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

価格で並べ替えが容易にできるようにデータベースへの変更をテストします。 編集、テスト メソッドを作成すると、保存されたデータを最初に表示されます、低価格を使用できます。

開く、 *StoreManager\Edit.cshtml*ファイル。 凡例のタグの直後後には、次の行を追加します。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

次のコードは、この変更のコンテキストを示しています。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

`AlbumId`アルバムのレコードを変更するが必要です。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 選択する、**管理者**をリンクし、選択、**新規作成**新しいアルバムを作成するリンク。 アルバム情報の保存を確認します。 アルバムを編集し、加えた変更が永続化されることを確認します。

### <a name="the-album-schema"></a>アルバム スキーマ

`StoreManager` MVC スキャフォールディング メカニズムによって作成されたコント ローラーは、ミュージック ストア データベースのアルバムの CRUD (作成、読み取り、更新、削除) へのアクセスを許可します。 アルバム情報のスキーマは、以下に示します。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

`Albums`アルバムのジャンルと説明は、テーブルが格納されていないへの外部キーを格納、`Genres`テーブル。 `Genres`テーブルには、ジャンル名と説明が含まれています。 同様に、`Albums`テーブルには、アルバム アーティスト名がへの外部キーが含まれていない、`Artists`テーブル。 `Artists`テーブルには、アーティスト名が含まれています。 内のデータを確認する場合、`Albums`テーブル、各行への外部キーが含まれています。 参照できます、`Genres`テーブルと外部キーを、`Artists`テーブル。 次の図の表示から一部のテーブル データ、`Albums`テーブル。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>HTML Select タグ

HTML`<select>`要素 (HTML によって作成された[DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)ヘルパー) (ジャンルのリスト) などの値の一覧を表示するために使用します。 編集フォームでは、現在の値がわかっている場合、選択リストを表示できます、現在の値。 見たこの以前に選択した値を設定すると**コメディ**します。 選択リストは、カテゴリまたは外部キーのデータを表示するために最適です。 `<select>`ジャンルの外部キーの要素には、可能なジャンル名の一覧が表示されますが、フォームを保存すると、ジャンル プロパティは、ジャンル外部キーの値、表示されているジャンル名ではなくで更新されます。 次の図では、選択されたジャンルは**Disco** 、アーティスト、 **Donna 夏**します。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>スキャフォールディングされたコードを ASP.NET MVC の確認

開く、 *Controllers\StoreManagerController.cs*ファイルを見つけて、`HTTP GET Create`メソッド。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

`Create`メソッドは、2 つ追加[SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx)オブジェクトを`ViewBag`ジャンルの情報を格納する 1 つ、アーティスト情報が含まれる 1 つ。 [SelectList](https://msdn.microsoft.com/library/dd505286.aspx)上記で使用するコンス トラクター オーバー ロードは 3 つの引数を受け取ります。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *項目*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx)リスト内の項目を格納します。 上記の例では、ジャンルの一覧がによって返される`db.Genres`します。
2. *dataValueField*: でプロパティの名前、 **IEnumerable**キーの値を含む一覧。 上記の例で`GenreId`と`ArtistId`します。
3. *dataTextField*: でプロパティの名前、 **IEnumerable**一覧を表示する情報を格納します。 アーティストとジャンルのテーブルの両方で、`name`フィールドを使用します。

開く、 *Views\StoreManager\Create.cshtml*ファイルを調べ、 `Html.DropDownList` genre フィールドのマークアップをヘルパー。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

ビューを作成するには、最初の行を示しています、`Album`モデル。 `Create`上記の方法、モデルが渡されなかったので、ビューを取得、 **null** `Album`モデル。 この時点で作成する新しいアルバムがある存在しないこと、`Album`のデータ。

[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)上記のオーバー ロードが、モデルにバインドするフィールドの名前を受け取ります。 この名前を検索するも使用、 **ViewBag**オブジェクトを含む、 [SelectList](https://msdn.microsoft.com/library/dd505286.aspx)オブジェクト。 このオーバー ロードを使用する必要がある名前、 **ViewBag SelectList**オブジェクト`GenreId`します。 2 番目のパラメーター (`String.Empty`) は、項目が選択されていない場合に表示されるテキストです。 これこそ新しいアルバムを作成するときに、次のものが必要です。 2 番目のパラメーターを削除してから、次のコードを使用します。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

選択リストのサンプルでは、最初の要素または岩を既定値は。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

確認、`HTTP POST Create`メソッド。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

このオーバー ロード、`Create`メソッドは、`album`ポストされたフォーム値からは、ASP.NET MVC モデル バインド システムによって作成されたオブジェクト。 モデルの状態が有効であり、データベース エラーがない場合に、新しいアルバムを送信するときに、データベースに新しいアルバムが追加されます。 次の図は、新しいアルバムの作成を示しています。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

使用することができます、 [fiddler ツール](http://www.fiddler2.com/fiddler2/)ポストされたフォーム値を確認するその ASP.NET MVC のモデル バインドを使用してアルバム オブジェクトを作成します。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png)。

### <a name="refactoring-the-viewbag-selectlist-creation"></a>ViewBag SelectList 作成のリファクタリング

両方の`Edit`メソッドと`HTTP POST Create`メソッドを設定するのと同じコードがある、 **SelectList**で、 **ViewBag**します。 精神[ドライ](http://en.wikipedia.org/wiki/Don't_repeat_yourself)、このコードのリファクタリングしました。 します。 このソフトウェアの使用が後でコードをリファクタリングします。

ジャンル、アーティストを追加する新しいメソッドを作成**SelectList**を**ViewBag**します。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

設定 2 つの行を置き換える、`ViewBag`のそれぞれで、`Create`と`Edit`メソッドを呼び出して、`SetGenreArtistViewBag`メソッド。 完成したコードを以下に示します。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

新しいアルバムを作成し、作業、変更を確認するアルバムを編集します。

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>DropDownList を SelectList を明示的に渡す

次の ASP.NET MVC のスキャフォールディングを使用して作成された create と edit ビュー **DropDownList**オーバー ロードします。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

`DropDownList`作成ビューのマークアップを次に示します。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

`ViewBag`プロパティを`SelectList`という`GenreId`、 **DropDownList**ヘルパーを使用して、 `GenreId` **SelectList**で、 **ViewBag**. 次に**DropDownList** 、オーバー ロード、`SelectList`が明示的に渡されます。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

開く、 *Views\StoreManager\Edit.cshtml*ファイルを開き、変更、 **DropDownList**に明示的に渡す呼び出し、 **SelectList**、上記のオーバー ロードを使用します。 これは、ジャンル カテゴリを行います。 完成したコードは、以下に示します。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

アプリケーションを実行し、をクリックして、**管理者**リンク、Jazz アルバムに移動し、選択して、**編集**リンク。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

現在選択されているジャンルとして Jazz ではなく、岩が表示されます。 ときに、文字列引数 (バインドするプロパティ) および**SelectList**オブジェクトが同じ名前を持つ、選択した値は使用されません。 ブラウザーの既定の最初の要素に指定された選択した値がない場合に、 **SelectList**(これは**Rock**上記の例では)。 これは、既知の制限、 **DropDownList**ヘルパー。

開く、 *Controllers\StoreManagerController.cs*ファイルし、変更、 **SelectList**オブジェクト名を`Genres`と`Artists`します。 完成したコードは、以下に示します。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

各カテゴリの ID だけが含まれているジャンル、アーティスト名は、カテゴリのわかりやすい名前です。 先ほどのリファクタリングは、支払われます。 変更する代わりに、 **ViewBag** 4 つの方法で変更されたに分離、`SetGenreArtistViewBag`メソッド。

変更、 **DropDownList**の作成を呼び出すし、新しいビューの編集**SelectList**名。 編集ビューの新しいマークアップは、以下に示します。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

ビューを作成するには、空の文字列、SelectList の最初の項目が表示されないようにする必要があります。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

新しいアルバムを作成し、作業、変更を確認するアルバムを編集します。 編集のコードをテストするには、ロック以外のジャンルのアルバムを選択します。

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>DropDownList ヘルパーをビュー モデルを使用します。

という名前のビューモデル フォルダーで新しいクラスを作成`AlbumSelectListViewModel`です。 コードに置き換えます、`AlbumSelectListViewModel`を次のクラス。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

`AlbumSelectListViewModel`コンス トラクターは、アルバム、アーティスト、ジャンルのリストを受け取り、アルバムを含むオブジェクトを作成して、`SelectList`ジャンル、アーティストの。

プロジェクトをビルドするため、`AlbumSelectListViewModel`は次の手順で、ビューを作成したときに使用できます。

追加、`EditVM`メソッドを`StoreManagerController`します。 完成したコードを以下に示します。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

右クリックして`AlbumSelectListViewModel`を選択します**解決**、し**MvcMusicStore.ViewModels; を使用して**します。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

または、次を追加するステートメントを使用します。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

右クリックして`EditVM`選択**ビューの追加**します。 次に示すオプションを使用します。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

選択**追加**の内容を置き換え、 *Views\StoreManager\EditVM.cshtml*を次のファイル。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

`EditVM`マークアップは、元によく似た`Edit`次の例外を使用するマークアップ。

- モデルのプロパティ、`Edit`の形式のビューは`model.property`(たとえば、 `model.Title` )。 モデルのプロパティ、`EditVm`の形式のビューは`model.Album.property`(たとえば、 `model.Album.Title`)。 です、`EditVM`ビューのコンテナーが渡されます、`Album`ではなく、`Album`ように、`Edit`ビュー。
- **DropDownList**されませんが、ビュー モデルから 2 番目のパラメーター、 **ViewBag**します。
- **BeginForm**でヘルパー、`EditVM`に明示的に投稿を表示、`Edit`アクション メソッド。 投稿することでバックアップ、`Edit`アクションにしなくても書き込み、`HTTP POST EditVM`アクションと再利用できる、 `HTTP POST` `Edit`アクション。

アプリケーションを実行し、アルバムを編集します。 使用する URL を変更する`EditVM`します。 フィールドを変更して、ヒット、**保存**ボタン コードの動作を検証します。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>どちらのアプローチを使用する必要がありますか。

表示されるすべての 3 つのアプローチは許容します。 多くの開発者が好む explictily パスに、`SelectList`を`DropDownList`を使用して、`ViewBag`します。 このアプローチでは、コレクションのより適切な名前を使用する柔軟がもたらさの長所があります。 1 つの注意事項は、名前を`ViewBag SelectList`モデルのプロパティと同じ名前のオブジェクトします。

ViewModel のアプローチを好む開発者もいます。 他のユーザーより詳細なマークアップを考慮し、HTML ViewModel アプローチの欠点を生成します。

このセクションでは、3 つのアプローチを使用してのについてを説明しましたが、 **DropDownList**カテゴリ データ。 次のセクションでは、新しいカテゴリを追加する方法を紹介します。

> [!div class="step-by-step"]
> [前へ](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [次へ](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
