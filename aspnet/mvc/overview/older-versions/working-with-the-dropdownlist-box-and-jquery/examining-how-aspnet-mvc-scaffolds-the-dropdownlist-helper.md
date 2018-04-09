---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: ASP.NET MVC での DropDownList ヘルパーの scaffolds 方法を調べて |Microsoft ドキュメント
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 09d2d7a0df5e8ffa14160b7d3c16b1e9da905fa1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>ASP.NET MVC での DropDownList ヘルパーの scaffolds 方法を確認します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

**ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*クリックしてフォルダー**コント ローラーの追加**です。 コント ローラーの名前を付けます**StoreManagerController**です。 オプションを設定、**コント ローラーの追加**ダイアログの次の図に示すようにします。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

編集、 *StoreManager\Index.cshtml*表示および削除`AlbumArtUrl`です。 削除する`AlbumArtUrl`は、プレゼンテーションを読みやすくします。 完成したコードを以下に示します。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

開く、 *Controllers\StoreManagerController.cs*ファイルを見つけて、`Index`メソッドです。 追加、`OrderBy`句価格別、アルバムにソートされるようにします。 完全なコードは、以下に示します。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

価格を順に並べ替えが容易にできるようにデータベースへの変更をテストします。 編集をテストするメソッドを作成するを使用して低価格、保存したデータを最初に表示されます。

開く、 *StoreManager\Edit.cshtml*ファイル。 凡例のタグの直後に次の行を追加します。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

次のコードは、この変更のコンテキストを示しています。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

`AlbumId`アルバム レコードに変更を加えるために必要なです。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 選択、 **Admin**をリンクし、選択、**新規作成**新しいアルバムの作成へのリンク。 アルバム情報の保存を確認します。 アルバムを編集して、行った変更は永続化を確認してください。

### <a name="the-album-schema"></a>アルバム スキーマ

`StoreManager` MVC のスキャフォールディング メカニズムによって作成されたコント ローラーは、音楽ストア データベースのアルバムを CRUD (Create、Read、Update、Delete) のアクセスを許可します。 アルバム情報のスキーマを次に示します。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

`Albums`アルバム ジャンルと説明は、テーブルが格納されていないへの外部キーを格納、`Genres`テーブル。 `Genres`テーブルには、ジャンルの名前と説明が含まれています。 同様に、`Albums`アルバム アーティスト名がへの外部キー テーブルが含まれていない、`Artists`テーブル。 `Artists`テーブルには、アーティスト名が含まれています。 内のデータを確認する場合、`Albums`テーブル、行ごとへの外部キーが含まれています。 参照ことができます、`Genres`テーブルと外部キーを、`Artists`テーブル。 次の図がいくつかのテーブル データを表示する、`Albums`テーブル。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>HTML 選択タグ

HTML`<select>`要素 (HTML によって作成された[DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)ヘルパー) (ジャンルの一覧) などの値の完全な一覧を表示するために使用します。 編集フォームは、現在の値がわかっている場合、選択リストを表示できます、現在の値。 見たこの以前に選択した値を設定すると**コメディ**です。 選択リストは、カテゴリ、または外部キーのデータを表示するために最適です。 `<select>`ジャンルの外部キーの要素には、考えられるジャンル名の一覧が表示されますが、ジャンル プロパティは、ジャンル外部キー値を持つ、ジャンルの表示名ではなく更新は、フォームを保存するときにします。 選択した genre は、次の図で**Disco** 、アーティスト、 **Donna 夏**です。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>コードをスキャフォールディングされた ASP.NET MVC を確認します。

開く、 *Controllers\StoreManagerController.cs*ファイルを見つけて、`HTTP GET Create`メソッドです。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

`Create`メソッドは、2 つ追加[SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx)オブジェクトを`ViewBag`、ジャンル情報が含まれる 1 つとアーティスト情報が含まれるいずれか。 [SelectList](https://msdn.microsoft.com/library/dd505286.aspx)上記で使用するコンス トラクター オーバー ロードは、3 つの引数を受け取ります。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *項目*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx)リスト内の項目を格納します。 上記の例では、ジャンルの一覧がによって返される`db.Genres`です。
2. *dataValueField*: でプロパティの名前、 **IEnumerable**キーの値を含む一覧。 上記の例で`GenreId`と`ArtistId`です。
3. *dataTextField*: でプロパティの名前、 **IEnumerable**を表示する情報を含むリスト。 アーティスト、ジャンル テーブルで、`name`フィールドを使用します。

開く、 *Views\StoreManager\Create.cshtml*ファイルを確認、 `Html.DropDownList` genre フィールドのヘルパー マークアップ。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

ビューを作成するには、最初の行を示しています、`Album`モデル。 `Create`上記の方法、モデルが渡されなかったため、ビューを取得、 **null** `Album`モデル。 この時点では作成する新しいアルバムのでこのいずれかの`Album`データ。

[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)上に示したオーバー ロードは、モデルにバインドするフィールドの名を取得します。 使用してこの名前を探して、 **ViewBag**オブジェクトを含む、 [SelectList](https://msdn.microsoft.com/library/dd505286.aspx)オブジェクト。 このオーバー ロードを使用する必要がある名前、 **ViewBag SelectList**オブジェクト`GenreId`です。 2 番目のパラメーター (`String.Empty`) 項目が選択されていないときに表示されるテキストです。 これは、正確に新しいアルバムを作成するときに、次の新機能が必要です。 2 番目のパラメーターを削除して、次のコードを使用します。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

選択リストのサンプルの最初の要素、またはロックに既定値は。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

確認する、`HTTP POST Create`メソッドです。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

このオーバー ロード、`Create`メソッドは、`album`ポストされたフォーム値からは、ASP.NET MVC モデル バインド システムで作成されたオブジェクト。 モデルの状態が有効では、データベース エラーがない場合に、新しいアルバムを送信するときに新しいアルバムがデータベースに追加されます。 次の図は、新しいアルバムの作成を示しています。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

使用することができます、 [fiddler ツール](http://www.fiddler2.com/fiddler2/)ポストされたフォーム値を確認する ASP.NET MVC モデル バインディングを使用してアルバム オブジェクトを作成します。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png)。

### <a name="refactoring-the-viewbag-selectlist-creation"></a>ViewBag SelectList 作成のリファクタリング

両方の`Edit`メソッドおよび`HTTP POST Create`メソッドを設定するのと同じコードがある、 **SelectList**で、 **ViewBag**です。 基本[ドライ](http://en.wikipedia.org/wiki/Don't_repeat_yourself)、このコードをリファクタリングことができます。 します。 このソフトウェアの使用が後でコードをリファクタリングします。

ジャンル、アーティストを追加する新しいメソッドを作成する**SelectList**を**ViewBag**です。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

設定する 2 つの行を置き換える、`ViewBag`内の各、`Create`と`Edit`メソッドを呼び出して、`SetGenreArtistViewBag`メソッドです。 完成したコードを以下に示します。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

新しいアルバムの作成および変更に問題を確認するアルバムを編集します。

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>次のドロップダウン リストに、SelectList を明示的に渡すこと

次の ASP.NET MVC のスキャフォールディングを使用して作成されたを作成および編集ビュー **DropDownList**オーバー ロードします。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

`DropDownList`作成ビューのマークアップを次に示します。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

`ViewBag`プロパティを`SelectList`という`GenreId`、 **DropDownList**ヘルパーを使用して、 `GenreId` **SelectList**で、 **ViewBag**. 次に**DropDownList**過負荷、`SelectList`に明示的に渡されます。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

開く、 *Views\StoreManager\Edit.cshtml*ファイルを開き、変更、 **DropDownList**で明示的に渡すへの呼び出し、 **SelectList**、上記のオーバー ロードを使用します。 このジャンル カテゴリに対して行います。 完成したコードは、次に示します。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

アプリケーションを実行し、をクリックして、 **Admin**をリンクし、ジャズ アルバムに移動し、選択、**編集**リンクします。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

現在選択されているジャンルとしてジャズではなく、ロックが表示されます。 ときに、文字列引数 (にバインドするプロパティ) および**SelectList**オブジェクトが同じ名前を持つ、選択した値は使用されません。 ブラウザーが既定の最初の要素に指定された選択した値がない場合に、 **SelectList**(これは**ロック**上記の例で)。 これは、既知の制限事項、 **DropDownList**ヘルパー。

開く、 *Controllers\StoreManagerController.cs*ファイルし、変更、 **SelectList**オブジェクト名を`Genres`と`Artists`です。 完成したコードは、次に示します。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

ジャンル、アーティスト名は、各カテゴリの ID だけを含めるようにのカテゴリより優れた名です。 オフを有料リファクタリング前いました。 変更する代わりに、 **ViewBag** 4 つの方法で変更されたに分離、`SetGenreArtistViewBag`メソッドです。

変更、 **DropDownList**の作成を呼び出すし、新しいビューの編集**SelectList**名。 新しいマークアップ編集ビューを次に示します。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

ビューを作成するには、空の文字列に、SelectList の最初の項目が表示されないようにする必要があります。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

新しいアルバムの作成および変更に問題を確認するアルバムを編集します。 編集のコードをテストするには、ロック以外ジャンルのアルバムを選択します。

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>DropDownList ヘルパーのビュー モデルを使用します。

名前が付いた ViewModels フォルダーで新しいクラスを作成`AlbumSelectListViewModel`です。 コードで置き換え、`AlbumSelectListViewModel`を次のクラス。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

`AlbumSelectListViewModel`コンス トラクターは、アルバム、アーティスト、ジャンルの一覧を受け取り、アルバムを含むオブジェクトを作成し、`SelectList`ジャンル、アーティストのです。

プロジェクトをビルドしますので、`AlbumSelectListViewModel`は次の手順で、ビューを作成した場合に使用できます。

追加、`EditVM`メソッドを`StoreManagerController`です。 完成したコードを以下に示します。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

右クリックして`AlbumSelectListViewModel`**を解決する**、し**MvcMusicStore.ViewModels; を使用して**です。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

次を追加する代わりに、ステートメントを使用します。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

右クリックして`EditVM`選択**ビューの追加**です。 次に示すオプションを使用します。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

選択**追加**の内容を置き換える、 *Views\StoreManager\EditVM.cshtml*を次のファイル。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

`EditVM`マークアップは、元によく似た`Edit`マークアップで、次の例外。

- モデルのプロパティの`Edit`ビュー形式は次の`model.property`(たとえば、 `model.Title` )。 モデルのプロパティの`EditVm`ビュー形式は次の`model.Album.property`(たとえば、 `model.Album.Title`)。 `EditVM`メソッドから制御がコンテナー`Album`ではなく、`Album`ように、`Edit`ビュー。
- **DropDownList**ないビュー モデルから 2 番目のパラメーターを取得、 **ViewBag**です。
- **BeginForm**ヘルパーを`EditVM`に明示的に投稿を表示、`Edit`アクション メソッド。 投稿し、バックアップ、`Edit`アクション、する必要はない書き込み、`HTTP POST EditVM`アクションを再利用できると、 `HTTP POST` `Edit`アクション。

アプリケーションを実行し、アルバムを編集します。 使用する URL を変更する`EditVM`です。 フィールドを変更して、ヒット、**保存**ボタン コードが動作を確認します。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>どの方法を使用する必要がありますか。

表示されるすべての 3 つの方法は、承認します。 Explictily パスに多くの開発者が必要に応じて、`SelectList`を`DropDownList`を使用して、`ViewBag`です。 この方法より適切なコレクションの名前を使用する柔軟性を提供するという追加の利点があります。 1 つの注意点は、名前を付けることはできません、`ViewBag SelectList`モデル プロパティと同じ名前のオブジェクトします。

一部の開発者は、ViewModel アプローチを選択します。 他のユーザーより詳細なマークアップを考慮し、欠点は、ViewModel アプローチの HTML を生成します。

このセクションを使用する 3 つの方法を学習おしました、 **DropDownList**カテゴリ データを使用します。 次のセクションでは、新しいカテゴリを追加する方法を紹介します。

> [!div class="step-by-step"]
> [前へ](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [次へ](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
