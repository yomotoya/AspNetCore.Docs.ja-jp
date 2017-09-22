---
title: "ASP.NET MVC を持つコアを EF Core - 並べ替え、フィルター、ページング - 10 3"
author: tdykstra
description: "このチュートリアルでは、並べ替え、フィルター、およびページング ASP.NET Core および Entity Framework のコアを使用して 1 ページに機能を追加します。"
keywords: "ASP.NET Core、Entity Framework Core、並べ替え、フィルター、ページング、グループ化"
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: e6c1ff3c-5673-43bf-9c2d-077f6ada1f29
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 59fff4dbf4736f0776aac4072f3f4e2d40119842
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a>並べ替え、フィルター、ページング、およびグループ化 - ASP.NET Core MVC のチュートリアル (10 の 3) と EF コア

によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。

前のチュートリアルでは、Student エンティティの基本的な CRUD 操作用の web ページのセットを実装します。 このチュートリアルでは、受講者インデックス ページに並べ替え、フィルター、およびページング機能を追加します。 単純なグループ化を実行するページを作成することもあります。

次の図は、ページがどのようにしたらを示します。 列見出しとは、その列で並べ替えを行うユーザーがクリックするリンクです。 列見出しを繰り返しクリックすると、昇順と降順の並べ替え順序を切り替えます。

![インデックス ページの受講者](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>受講者インデックス ページに列の並べ替えのリンクを追加します。

学生インデックス ページに並べ替えを追加するを変更、`Index`受講者コント ローラーのメソッド学生インデックス ビューにコードを追加します。

### <a name="add-sorting-functionality-to-the-index-method"></a>Index メソッドに並べ替え機能を追加します。

*StudentsController.cs*、置換、`Index`メソッドを次のコード。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

このコードを受け取る、 `sortOrder` URL 内のクエリ文字列からのパラメーターです。 クエリ文字列の値は、アクション メソッドにパラメーターとして ASP.NET Core MVC によって提供されます。 パラメーターが"Name"または「日付」、必要に応じて後にアンダー スコアと降順の順序を指定するには、"desc"文字列を文字列になります。 既定の並べ替え順序は昇順です。

インデックス ページが要求されると、最初にクエリ文字列はありません。 受講者は、姓、名、フォール スルー大文字小文字によって設定されるは、既定で昇順に表示される、`switch`ステートメントです。 ユーザーが列見出し、ハイパーリンク、適切な`sortOrder`クエリ文字列の値を指定します。

2 つ`ViewData`要素 (NameSortParm および DateSortParm) を使用して、ビューによって適切なクエリ文字列の値を列見出しのハイパーリンクを構成します。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

これらは、三項ステートメントです。 最初の 1 つを指定する場合、`sortOrder`パラメーターが null または空で、NameSortParm に設定してください"name_desc"以外の場合は、空の文字列にそれ以外の場合、設定する必要があります。 これら 2 つのステートメントでは、ビュー、列見出しのハイパーリンクの次のように設定を有効にします。

|  現在の並べ替え順序  | 名の最後のハイパーリンク | 日付のハイパーリンク |
|:--------------------:|:-------------------:|:--------------:|
| 最後の名前昇順  | descending          | ascending      |
| 最後の名前が降順 | ascending           | ascending      |
| 日付の昇順       | ascending           | descending     |
| 日付 (降順)      | ascending           | ascending      |

メソッドは、並べ替える列を指定するのに LINQ to Entities を使用します。 このコードを作成、 `IQueryable` switch ステートメントの前に変数を呼び出し、switch ステートメントで変更、`ToListAsync`メソッドした後に、`switch`ステートメントです。 作成および変更するときに`IQueryable`変数、クエリがないデータベースに送信します。 変換するまでに、クエリが実行されていません、`IQueryable`などのメソッドを呼び出すことで、コレクションにオブジェクト`ToListAsync`です。 そのため、このコードなりますまで実行されない 1 つのクエリで、`return View`ステートメントです。

このコードは、列の数が多い verbose 取得でした。 [このシリーズの前回のチュートリアル](advanced.md#dynamic-linq)コードの名前を渡すことができますを記述する方法を示します、`OrderBy`文字列変数内の列です。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>学生インデックス ビューに列見出しのハイパーリンクを追加します。

コードに置き換えます*Views/Students/Index.cshtml*、列見出しのハイパーリンクを追加する次のコードにします。 変更された行が強調表示されます。

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

このコード内の情報を使用して`ViewData`適切なクエリでハイパーリンクを設定するプロパティの文字列値です。

アプリを実行する、選択、**受講者**タブをクリックし、をクリックして、**姓**と**登録日**を並べ替えることを確認する列見出しが動作します。

![受講者名の順にページをインデックスします。](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>受講者インデックス ページに検索ボックスを追加します。

受講者インデックス ページにフィルターを追加するにをビューにテキスト ボックスと [送信] ボタンを追加しで対応する変更を加え、`Index`メソッドです。 テキスト ボックスを使用すると、姓と名の最後のフィールドで検索する文字列を入力できます。

### <a name="add-filtering-functionality-to-the-index-method"></a>Index メソッドにフィルター処理機能を追加します。

*StudentsController.cs*、置換、`Index`メソッドを次のコード (変更が強調表示されます)。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

追加した、`searchString`パラメーターを`Index`メソッドです。 インデックス ビューに追加するテキスト ボックスから、検索する文字列値を受信します。 またに追加した LINQ ステートメントの where 句検索文字列を含む文字列名または姓を持つ受講者のみを選択します。 Where を追加するステートメントを検索する値がある場合にのみ、この句を実行します。

> [!NOTE]
> ここでの呼び出しには、`Where`メソッドを`IQueryable`オブジェクト、およびフィルターは、サーバーで処理されます。 一部のシナリオでする可能性がありますを呼び出すことの`Where`メモリ内コレクションの拡張メソッドとしてメソッドです。 (たとえばへの参照を変更すると仮定`_context.Students`、EF の代わりに`DbSet`リポジトリ メソッドを表すオブジェクトを参照して、`IEnumerable`コレクション)。結果は、通常同じになりますが、場合によっては異なる場合があります。
>
>.NET Framework の実装など、`Contains`メソッドが既定では、大文字小文字の比較を実行しますが、SQL Server では、SQL Server インスタンスの照合順序の設定によって決定これはします。 その設定は、大文字と小文字を既定値です。 呼び出すことも、`ToUpper`を明示的に大文字と小文字、テストを作成するメソッド:*場所 (s = > s.LastName.ToUpper() です。Contains(searchString.ToUpper())*です。 保つ結果、同じを返すリポジトリを使用するには、後でコードを変更する場合、`IEnumerable`コレクションの代わりに、`IQueryable`オブジェクト。 (を呼び出すと、`Contains`メソッドを`IEnumerable`.NET Framework の実装を取得する、コレクション以外のときに呼び出す、`IQueryable`オブジェクト、データベース プロバイダーの実装を取得します)。ただし、このソリューションのパフォーマンスの低下があります。 `ToUpper`コードは、TSQL の SELECT ステートメントの WHERE 句で関数を格納します。 ため、オプティマイザーはインデックスを使用できなくなります。 大文字と小文字 SQL がインストールされているほとんどの場合、あることをお勧めを回避するのには`ToUpper`大文字小文字を区別データ ストアに移行するまでのコードします。

### <a name="add-a-search-box-to-the-student-index-view"></a>学生インデックス ビューに検索ボックスを追加します。

*Views/Student/Index.cshtml*、キャプション、テキスト ボックスを作成するためにテーブル タグを開始する直前に強調表示されたコードを追加し、**検索**ボタンをクリックします。

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

このコードを使用して、 `<form>` [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)検索テキスト ボックスとボタンを追加します。 既定では、`<form>`タグ ヘルパーが、パラメーターとして渡される HTTP メッセージの本文では、URL ではなくクエリ文字列は、投稿にフォーム データを送信します。 HTTP GET を指定すると、フォームのデータに渡されます URL クエリ文字列としてユーザーの URL をブックマークすることができます。 アクションが、更新プログラムにならない場合に、W3C のガイドラインをお勧めしますが使用するを取得します。

アプリを実行する、選択、**受講者** タブで、検索文字列を入力し、検索フィルターが機能していることを確認する をクリックします。

![フィルター処理の受講者インデックス ページ](sort-filter-page/_static/filtering.png)

URL に検索文字列が含まれることに注意してください。

```html
http://localhost:5813/Students?SearchString=an
```

このページのブックマークを設定した場合に、ブックマークを使用する場合は、フィルター処理された一覧が表示されます。 追加`method="get"`を`form`はタグを生成するクエリ文字列の原因です。

この段階で、列見出しのソートのリンクをクリックすると失われますで指定したフィルター値、**検索**ボックス。 次のセクションで修正します。

## <a name="add-paging-functionality-to-the-students-index-page"></a>受講者インデックス ページにページング機能を追加します。

ページングに追加する、受講者インデックス ページを作成、`PaginatedList`を使用してクラス`Skip`と`Take`を常に、テーブルのすべての行を取得する代わりに、サーバー上のデータをフィルター処理するステートメント。 他の変更を加えるされます、`Index`メソッド ページング ボタンを追加し、`Index`ビュー。 次の図は、ページング ボタンを示しています。

![受講者とページングのリンクのページをインデックスします。](sort-filter-page/_static/paging.png)

プロジェクト フォルダーに作成`PaginatedList.cs`、テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

`CreateAsync`このコードでメソッドがページ サイズとページ番号を受け取り、適切な適用`Skip`と`Take`ステートメントを`IQueryable`です。 ときに`ToListAsync`で呼び出されると、 `IQueryable`、要求されたページのみを含むリストが返されます。 プロパティ`HasPreviousPage`と`HasNextPage`を有効または無効にするために使用できる**前**と**[次へ]**ボタンをページングします。

A`CreateAsync`メソッドを使用するコンス トラクターではなくを作成、`PaginatedList<T>`オブジェクトのコンス トラクターは、非同期コードを実行できないためです。

## <a name="add-paging-functionality-to-the-index-method"></a>Index メソッドにページング機能を追加します。

*StudentsController.cs*、置換、`Index`メソッドを次のコード。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

このコードは、メソッド シグネチャにページ番号パラメーター、現在の並べ替え順序パラメーター、および現在のフィルター パラメーターを追加します。

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

最初に、ページが表示されたら、またはユーザーが、ページングや並べ替えのリンクをクリックしていない場合、すべてのパラメーターは null になります。  ページングのリンクをクリックすると、ページ変数は、表示するページ番号が含まれます。

`ViewData` CurrentSort をという名前の要素がこのページング中に同じ並べ替え順序を維持するために、ページングのリンクに含める必要がありますので、現在の並べ替え順序でビューを提供します。

`ViewData` CurrentFilter をという名前の要素が現在のフィルター文字列で使用してビューを提供します。 ページング、中に、フィルターの設定を維持するために、ページングのリンクでこの値を含める必要があるし、ページが表示されときに、テキスト ボックスに復元する必要があります。

ページングの中に検索文字列を変更する場合は、新しいフィルターが、別のデータを表示するため、ページを 1 にリセットします。 テキスト ボックスに値を入力し、[送信] ボタンが押された検索文字列が変更されます。 その場合は、`searchString`パラメーターが null ではありません。

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

最後に、 `Index` 、メソッド、`PaginatedList.CreateAsync`メソッドは、ページングをサポートするコレクション型の生徒の 1 つのページを学生クエリを変換します。 その 1 つのページの受講者は、ビューに渡されます。

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

`PaginatedList.CreateAsync`メソッドは、ページ数を取得します。 2 つの疑問符は、null 合体演算子を表します。 Null 合体演算子を null 許容型以外の既定値を定義します式`(page ?? 1)`の値を返すことを意味`page`かどうかは、値を持つまたは 1 を返す`page`が null です。

## <a name="add-paging-links-to-the-student-index-view"></a>学生インデックス ビューにページングのリンクを追加します。

*Views/Students/Index.cshtml*、既存のコードを次のコードに置き換えます。 変更が強調表示されます。

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

`@model`ページの上部にあるステートメントは、ビューが現在を取得するを指定します、`PaginatedList<T>`オブジェクトの代わりに、`List<T>`オブジェクト。

列ヘッダーへのリンクは、フィルターの結果内で、ユーザーが並べ替えられるように、コント ローラーに現在の検索文字列を渡すクエリ文字列を使用します。

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

タグ ヘルパーによってページング ボタンが表示されます。

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

アプリを実行して、受講者のページに移動します。

![受講者とページングのリンクのページをインデックスします。](sort-filter-page/_static/paging.png)

ページング動作のことを確認する異なる並べ替え順のページングのリンクをクリックします。 検索文字列を入力し、ページング ページングも正常に動作する並べ替えとフィルター処理のことを確認するには、もう一度やり直してください。

## <a name="create-an-about-page-that-shows-student-statistics"></a>学生の統計情報を表示するバージョン情報 ページを作成します。

Contoso 大学の web サイトの**に関する** ページで、登録日付ごとに登録した数の受講者を表示することもできます。 これには、グループにグループ化と簡単な計算が必要です。 これを実現するには、次を行います。

* ビューに渡す必要があるデータのビュー モデル クラスを作成します。

* Home コント ローラーで、バージョン情報メソッドを変更します。

* バージョン情報の表示を変更します。

### <a name="create-the-view-model"></a>ビュー モデルを作成します。

作成、 *SchoolViewModels*内のフォルダー、*モデル*フォルダーです。

クラス ファイルを追加、新しいフォルダーに*EnrollmentDateGroup.cs*テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>Home コント ローラーの変更します。

*HomeController.cs*次の追加ファイルの上部にあるステートメントを使用します。

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

クラスの始め中かっこの直後に、データベース コンテキストのクラスの変数を追加し、ASP.NET Core DI からコンテキストのインスタンスを取得します。

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

`About` メソッドを次のコードで置き換えます。

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

LINQ ステートメントの登録日で学生エンティティをグループ化、各グループ内のエンティティの数を計算し、結果のコレクションに格納`EnrollmentDateGroup`モデル オブジェクトを表示します。
> [!NOTE] 
> 1.0 のバージョンの Entity Framework Core では、結果セット全体がクライアントに返され、クライアントでグループ化が行われます。 一部のシナリオでは、パフォーマンスの問題を作成このでした。 必ず、データの実稼働ボリュームとパフォーマンスをテストし、必要な場合をサーバーをグループ化を行うには生の SQL を使用してください。 生の SQL を使用する方法については、次を参照してください。[このシリーズの前回のチュートリアル](advanced.md)です。

### <a name="modify-the-about-view"></a>変更、ビューについて

コードで置き換え、 *Views/Home/About.cshtml*を次のコード ファイル。

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

アプリを実行してバージョン情報 ページに移動します。 登録の日付ごとの生徒の数は、テーブルに表示されます。

![ページについて](sort-filter-page/_static/about.png)

## <a name="summary"></a>概要

このチュートリアルでは、並べ替え、フィルター、ページング、およびグループ化を実行する方法を説明しました。 次のチュートリアルでは、移行を使用して、データ モデルの変更を処理する方法を学習します。

>[!div class="step-by-step"]
[前へ](crud.md)
[次へ](migrations.md)  
