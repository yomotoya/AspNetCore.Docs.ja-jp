---
title: "Razor ページの EF コア CRUD - 2 8"
author: rick-anderson
description: "作成、読み取り、更新、EF コアで削除する方法を示します"
keywords: "ASP.NET Core、Entity Framework Core、CRUD、作成、読み取り、更新、削除"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: 163bc35afed0bf1d9236935d5ce60e6975356594
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/15/2017
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a>作成、読み取り、更新、および削除 - EF コア Razor ページ (2/8)

によって[Tom Dykstra](https://github.com/tdykstra)、 [Jon P Smith](https://twitter.com/thereformedprog)、および[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

このチュートリアルでは、スキャフォールディング CRUD (作成、読み取り、更新、削除) コードの確認し、カスタマイズができます。

注: EF コアに重点を置いてこれらのチュートリアルの変更に複雑さを軽減するには、EF コア コードが使用 Razor ページの分離コード ファイルにします。 一部の開発者で、サービス レイヤー、またはリポジトリ パターンを使用して、UI (Razor ページ) とデータ アクセス層の間で抽象化レイヤーを作成します。

このチュートリアル、作成、編集、削除、および詳細 Razor ページで、*学生*フォルダーを変更します。

スキャフォールディングのコードでは、作成、編集、および削除のページの次のパターンを使用します。

* 取得し、HTTP GET メソッドを使用して、要求されたデータを表示`OnGetAsync`です。
* HTTP POST メソッドを使用してデータへの変更を保存`OnPostAsync`です。

インデックスおよび詳細ページが取得し、HTTP GET メソッドを使用して、要求されたデータを表示`OnGetAsync`

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a>FirstOrDefaultAsync で SingleOrDefaultAsync を置き換えます

生成されたコードを使用して[SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)を要求されたエンティティを取得します。 [FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) 1 つのエンティティのフェッチでより効率的です。

* コードがあることを確認する必要がありますいない限り、クエリから返される 1 つ以上のエンティティはありませんがあります。 
* `SingleOrDefaultAsync`多くのデータをフェッチし、不要な作業を行います。
* `SingleOrDefaultAsync`フィルター部品に適合する 1 つ以上のエンティティがある場合は、例外をスローします。
*  `FirstOrDefaultAsync`フィルター部品に適合する 1 つ以上のエンティティがある場合は、例外がスローされません。

グローバルに置き換える`SingleOrDefaultAsync`で`FirstOrDefaultAsync`です。 `SingleOrDefaultAsync`5 桁で使用されます。

* `OnGetAsync`[詳細] ページにします。
* `OnGetAsync`および`OnPostAsync`編集および削除のページにします。

<a name="FindAsync"></a>
### <a name="findasync"></a>FindAsync

スキャフォールディングのコードの多くで[FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___)の代わりに使用できる`FirstOrDefaultAsync`または`SingleOrDefaultAsync`です。 

`FindAsync`:

* 主キー (PK) を持つエンティティを検索します。 主キーを持つエンティティはコンテキストによって追跡されているが場合、は、要求なし、DB に返されます。
* 単純で簡潔なです。
* 1 つのエンティティの検索に最適化されます。
* できますが、一部の状況でパフォーマンスの利点があります、通常の web シナリオの再生になることはほとんどありません。
* 暗黙的に使用[FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)の代わりに[SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)です。
他のエンティティを追加するかどうかは、検索に不要になったが、適切なします。 つまりに検索を破棄し、アプリの進行状況に応じて、クエリに移動する必要があります。

## <a name="customize-the-details-page"></a>[詳細] ページをカスタマイズします。

参照`Pages/Students`ページ。 **編集**、**詳細**、および**削除**へのリンクがによって生成される、[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)で、*ページ/受講者/Index.cshtml*ファイル。

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

詳細情報のリンクを選択します。 URL の形式は、`http://localhost:5000/Students/Details?id=2`です。 クエリ文字列を使用して、学生 ID が渡されます (`?id=2`)。

更新を使用するには、編集、詳細、および Razor ページの削除、`"{id:int}"`ルート テンプレート。 これらの各ページのページ ディレクティブを `@page` から `@page "{id:int}"` に変更します。

"{Id: int}"のルート テンプレートを持つページへの要求**いない**整数ルート値を返します、HTTP 404 (見つかりません) エラーが含まれます。 たとえば、 `http://localhost:5000/Students/Details` 404 エラーが返されます。 ID を省略するには、次のように `?` をルート制約に追加します。

 ```cshtml
@page "{id:int?}"
```

アプリを実行、詳細リンクをクリックして、URL がルート データとして ID を渡すことを確認してください (`http://localhost:5000/Students/Details/2`)。

グローバルに変更しない`@page`に`@page "{id:int}"`ホームへのリンクの解除を行って、およびページを作成します。

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>関連するデータを追加します。

受講者インデックス ページのスキャフォールディング コードを含まない、`Enrollments`プロパティです。 このセクションの内容で、`Enrollments`コレクションは、詳細ページに表示されます。

`OnGetAsync`メソッドの*Pages/Students/Details.cshtml.cs*を使用して、 `FirstOrDefaultAsync` 、1 つを取得する方法を`Student`エンティティです。 次の強調表示されたコードを追加します。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

`Include`と`ThenInclude`メソッドで発生する読み込みコンテキスト、`Student.Enrollments`ナビゲーション プロパティ、および各登録内で、`Enrollment.Course`ナビゲーション プロパティ。 これらのメソッドは、データの読み取りに関連するチュートリアル」で詳しく examinied はします。

`AsNoTracking`メソッドによって返されるエンティティは、現在のコンテキストでは更新されない時のシナリオでパフォーマンスを向上します。 `AsNoTracking`このチュートリアルで後ほど説明します。

### <a name="display-related-enrollments-on-the-details-page"></a>[詳細] ページの関連する登録を表示します。

開いている*Pages/Students/Details.cshtml*です。 登録の一覧を表示する次の強調表示されたコードを追加します。

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

コードを貼り付けた後でコードのインデントが正しくない場合は、修正して CTRL-D-K キーを押します。

上記のコードをループ内のエンティティ、`Enrollments`ナビゲーション プロパティ。 各登録の場合は、コースのタイトルとグレードが表示されます。 格納されているコース エンティティからコース タイトルが取得される、`Course`登録エンティティのナビゲーション プロパティ。

アプリを実行する、選択、**受講者**タブをクリックし、をクリックして、**詳細**受講者をリンクします。 Courses に、選択した学生の成績の一覧が表示されます。

## <a name="update-the-create-page"></a>更新プログラムの作成 ページ

更新プログラム、`OnPostAsync`メソッド*Pages/Students/Create.cshtml.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

確認、 [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_)コード。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

上記のコードで`TryUpdateModelAsync<Student>`を更新しようとする、`emptyStudent`オブジェクトからポストされたフォーム値を使用して、 [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext)プロパティに、 [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0)です。 `TryUpdateModelAsync`プロパティを一覧表示の更新のみ (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`)。

上記のサンプル。

* 2 番目の引数 (` "student", // Prefix`) は、プレフィックスを使用して値を検索します。 大文字小文字を区別することはできません。
* ポストされたフォーム値の型に変換、`Student`モデルを使用して[モデル バインディング](xref:mvc/models/model-binding#how-model-binding-works)です。

<a id="overpost"></a>
### <a name="overposting"></a>過剰ポスティング

使用して`TryUpdateModel`ポストされた値を持つフィールドを更新するにはセキュリティのベスト プラクティスを overposting を防ぐために、します。 たとえば、受講者エンティティが含まれています、`Secret`プロパティをこの web ページを更新または追加する必要がありますいません。

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

アプリを持っていない場合でも、 `Secret` Razor ページでは、ハッカーの作成/更新でフィールドが設定でした、`Secret`過剰ポスティングによっての値。 ハッカーでした、Fiddler などのツールを使用してまたは投稿に、一部の JavaScript を書き込み、`Secret`値を作成します。 学生インスタンスの作成時にモデル バインダーが使用されるフィールドの制限は、元のコードがありません。

値の指定されたハッカー、 `Secret` DB のフォーム フィールドを更新します。 次の図は、Fiddler ツールを追加する、`Secret`ポストされたフォーム値へのフィールド (に値"OverPost") です。

![Fiddler シークレット フィールドの追加](../ef-mvc/crud/_static/fiddler.png)

値に"OverPost"を正常に追加、`Secret`挿入された行のプロパティです。 アプリケーション デザイナーを想定しなかった、`Secret`の作成 ページで設定されるプロパティです。

<a name="vm"></a>
### <a name="view-model"></a>ビュー モデル

ビュー モデルには、通常、アプリケーションによって使用されるモデルに含まれるプロパティのサブセットが含まれています。 アプリケーション モデルは、ドメイン モデルと呼ばれます。 ドメイン モデルには、通常、データベース内の対応するエンティティに必要なすべてのプロパティが含まれています。 ビュー モデルには、UI 層 (たとえば、作成 ページ) に必要なプロパティのみが含まれています。 ビュー モデルだけでなく、一部のアプリケーションは、Razor ページの分離コード クラスとブラウザー間でデータを受け渡すバインド モデルまたは入力モデルを使用します。 次の検討`Student`ビュー モデル。

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

ビューのモデルでは、overposting を防ぐために別の方法を提供します。 ビュー モデルには、(表示) を表示または更新するプロパティのみが含まれています。

次のコードでは、`StudentVM`ビュー モデルに新しい学生を作成します。

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

[SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_)メソッドでは、このオブジェクトの値を設定を別の値を読み取って[PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues)オブジェクト。 `SetValues`プロパティの名前の一致を使用します。 ビュー モデルの種類が、モデルの種類に関連している必要がある、一致するプロパティだけが必要です。

使用して`StudentVM`必要[CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml)に更新する`StudentVM`なく`Student`です。

Razor ページで、`PageModel`派生クラスは、ビュー モデルです。 

## <a name="update-the-edit-page"></a>更新プログラムの編集 ページ

編集ページの分離コード ファイルを更新します。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

コードの変更は、いくつかの例外の作成 ページに似ています。

* `OnPostAsync`省略可能な`id`パラメーター。
* 現在の受講者が、DB からフェッチされた空の学生を作成するのではなくです。
* `FirstOrDefaultAsync`置き換えられました[FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0)です。 `FindAsync`主キーからエンティティを選択するときに、適切な選択です。 参照してください[FindAsync](#FindAsync)詳細についてはします。

### <a name="test-the-edit-and-create-pages"></a>編集をテストし、ページを作成します。

作成し、いくつかの学生エンティティを編集します。

## <a name="entity-states"></a>エンティティの状態

DB コンテキストは、メモリ内のエンティティが、DB 内の対応する行との同期であるかどうかの追跡を保持します。 DB コンテキストの同期情報の動作を決定するときに`SaveChanges`と呼びます。 たとえば、新しいエンティティが渡されたときにする、`Add`メソッドに設定されているエンティティの状態を`Added`です。 ときに`SaveChanges`が呼び出されると、データベース コンテキストは、SQL の INSERT コマンドを発行します。

次の状態のいずれかでエンティティがあります。

* `Added`: エンティティは、db では、まだ存在しません。 `SaveChanges`メソッドは、INSERT ステートメントを発行します。

* `Unchanged`変更する必要があります: このエンティティで保存されます。 エンティティでは、データベースから読み取られるときにこの状態があります。

* `Modified`: 一部またはすべてのエンティティのプロパティの値が変更されています。 `SaveChanges`メソッドは、UPDATE ステートメントを発行します。

* `Deleted`: エンティティは、削除のマークされています。 `SaveChanges`メソッドは、DELETE ステートメントを発行します。

* `Detached`: エンティティは、DB コンテキストによって追跡されているはありません。

デスクトップ アプリでは、通常状態の変更から自動的に設定します。 エンティティの読み取り、変更を加えると、エンティティの状態に自動的に変更する`Modified`です。 呼び出す`SaveChanges`変更されたプロパティのみを更新する SQL UPDATE ステートメントを生成します。

Web アプリで、`DbContext`を読み取るエンティティと、ページが表示された後、データが破棄されて表示されます。 ときに、ページ`OnPostAsync`メソッドが呼び出されると、新しい web 要求が行われるの新しいインスタンスを使用して、`DbContext`です。 デスクトップの処理をシミュレートする新しいコンテキスト内のエンティティを再読み込みします。

## <a name="update-the-delete-page"></a>更新プログラムの削除 ページ

ここでは、カスタム エラーを実装するコードを追加メッセージへの呼び出し`SaveChanges`は失敗します。 Possile エラー メッセージを格納する文字列を追加します。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

`OnGetAsync` メソッドを次のコードで置き換えます。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

上記のコードには、省略可能なパラメーターが含まれています。`saveChangesError`です。 `saveChangesError`student オブジェクトの削除に失敗した後、メソッドが呼び出されたかどうかを示します。 一時的なネットワークの問題があるため、削除操作が失敗可能性があります。 ネットワークの一時的なエラーは、クラウドで可能性が高くなります。 `saveChangesError`ときに、false 削除ページ`OnGetAsync`UI から呼び出されます。 ときに`OnGetAsync`によって呼び出される`OnPostAsync`(ため、削除操作が失敗しました)、`saveChangesError`パラメーターが true です。

### <a name="the-delete-pages-onpostasync-method"></a>Delete ページ OnPostAsync メソッド

置換、`OnPostAsync`を次のコード。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

上記のコードは、選択したエンティティを取得し、呼び出して、`Remove`エンティティの状態を設定するメソッドを`Deleted`です。 ときに`SaveChanges`が呼び出されると、SQL の DELETE コマンドを生成します。 場合`Remove`は失敗します。

* DB 例外をキャッチします。
* Delete ページ`OnGetAsync`メソッドが呼び出された`saveChangesError=true`です。

### <a name="update-the-delete-razor-page"></a>Delete Razor ページを更新します。

Razor ページの削除するには、次の強調表示されたエラー メッセージを追加します。

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

削除をテストします。

## <a name="common-errors"></a>一般的なエラー

学生/ホームまたはその他のリンクは機能しません。

Razor ページが含まれていますが、正しいことを確認`@page`ディレクティブです。 など、受講者/ホーム Razor ページする必要があります**いない**ルート テンプレートを含めます。

```cshtml
@page "{id:int}"
```

各 Razor ページを含める必要があります、`@page`ディレクティブです。

>[!div class="step-by-step"]
[前へ](xref:data/ef-rp/intro)
[次へ](xref:data/ef-rp/sort-filter-page)
