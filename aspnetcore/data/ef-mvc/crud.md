---
title: "ASP.NET MVC を持つコアを EF コア CRUD - 2 10"
author: tdykstra
description: 
keywords: "ASP.NET Core、Entity Framework Core、CRUD、作成、読み取り、更新、削除"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 6e1cd570-40f1-4b24-8b6e-7d2d27758f18
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/crud
ms.openlocfilehash: 855f060a6404dedff310b288ada9738689069ceb
ms.sourcegitcommit: 5355c96a1768e5a1d5698a98c190e7addcc4ded5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/05/2017
---
# <a name="create-read-update-and-delete---ef-core-with-aspnet-core-mvc-tutorial-2-of-10"></a>作成、読み取り、更新、および削除の ASP.NET Core MVC のチュートリアル (10 の 2) と EF コア

によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。

前のチュートリアルを格納し、Entity Framework と SQL Server LocalDB を使用してデータを表示する MVC アプリケーションを作成します。 このチュートリアルでを確認およびカスタマイズしたりする、CRUD (作成、読み取り、更新、削除) MVC のスキャフォールディング自動的に作成するコント ローラーとビューのコード。

> [!NOTE] 
> コント ローラーと、データ アクセス層の間で抽象化レイヤーを作成するのには、リポジトリ パターンを実装する一般的なことをお勧めします。 単純なおよび教育自体 Entity Framework を使用する方法に焦点を当ててをこれらのチュートリアルを維持するには、リポジトリを使用しないでください。 EF のリポジトリについては、次を参照してください。[このシリーズの前回のチュートリアル](advanced.md)です。

このチュートリアルでは、次の web ページを使用します。

![学生の詳細 ページ](crud/_static/student-details.png)

![学生の作成 ページ](crud/_static/student-create.png)

![学生の編集 ページ](crud/_static/student-edit.png)

![学生の削除 ページ](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a>[詳細] ページをカスタマイズします。

スキャフォールディング コード受講者インデックス ページを省略して、`Enrollments`プロパティ、そのプロパティには、コレクションが格納されているためです。 **詳細** ページで、HTML テーブルのコレクションの内容を表示します。

*Controllers/StudentsController.cs*、詳細については、アクション メソッドの表示は、 `SingleOrDefaultAsync` 、1 つを取得する方法を`Student`エンティティです。 呼び出すコードを追加`Include`です。 `ThenInclude`、および`AsNoTracking`メソッドは、次の強調表示されたコードに示すようにします。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

`Include`と`ThenInclude`メソッドで発生する読み込みコンテキスト、`Student.Enrollments`ナビゲーション プロパティ、および各登録内で、`Enrollment.Course`ナビゲーション プロパティ。  これらのメソッドの詳細を学習、[関連データを読み取る](read-related-data.md)チュートリアルです。

`AsNoTracking`メソッドで返されるエンティティは更新されません、現在のコンテキストの有効期間内のシナリオでパフォーマンスが向上します。 詳しくは`AsNoTracking`このチュートリアルの最後。

### <a name="route-data"></a>ルート データ

渡されるキーの値、`Details`メソッドに由来*データ ルーティング*です。 ルート データは、URL のセグメント内のモデル バインダーにあるデータです。 たとえば、既定のルートは、コント ローラー、アクション、および id のセグメントを指定します。

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

次の URL で既定のルートは、コント ローラー、アクションとしてインデックスと、id として 1 とインストラクターをマップします。これらは、ルート データの値です。

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

URL の最後の部分 ("? courseID 2021 を =") のクエリ文字列値です。 モデル バインダーは、ID 値を渡すことも、`Details`メソッド`id`パラメーター クエリ文字列の値として渡す場合。

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

インデックス ページにハイパーリンクの Url は、Razor ビューでタグ ヘルパーのステートメントによって作成されます。 次の Razor コードで、`id`ためパラメーターに既定のルートと一致する`id`ルート データを追加します。

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

これにより、次の HTML が生成されます。 ときに`item.ID`は 6。

```html
<a href="/Students/Edit/6">Edit</a>
```

次の Razor コードで`studentID`クエリ文字列として追加されたために、既定のルートのパラメーターと一致しません。

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

これにより、次の HTML が生成されます。 ときに`item.ID`は 6。

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

タグ ヘルパーの詳細については、次を参照してください。 [ASP.NET Core でタグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。

### <a name="add-enrollments-to-the-details-view"></a>詳細ビューに登録を追加します。

開いている*Views/Students/Details.cshtml*です。 使用して、各フィールドが表示される`DisplayNameFor`と`DisplayFor`ヘルパーを次の例で示すようにします。

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

最後のフィールド後と終わりの前にすぐに`</dl>`タグは、「登録」一覧を表示する次コードを追加します。

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

コードを貼り付けた後にコードのインデントが正しくない場合は、修正して CTRL-D-K キーを押します。

このコードは、内のエンティティをループ処理、`Enrollments`ナビゲーション プロパティ。 各登録の場合は、コースのタイトルとグレードが表示されます。 格納されているコース エンティティからコース タイトルが取得される、`Course`登録エンティティのナビゲーション プロパティ。

アプリケーションの実行で、選択、**受講者**タブをクリックし、をクリックして、**詳細**受講者をリンクします。 選択した受講者コースとグレードの一覧を参照してください。

![学生の詳細 ページ](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>更新プログラムの作成 ページ

*StudentsController.cs*、変更、HttpPost `Create` 、try-catch ブロックを追加してからの ID を削除することによって、`Bind`属性。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

このコードは、セットされ、データベースに変更を保存、受講者エンティティに ASP.NET MVC モデル バインダーによって作成された学生エンティティを追加します。 (モデル バインダーのフォームから送信されたデータを操作するが容易 ASP.NET MVC 機能を指す。 モデル バインダーは、CLR 型にポストされたフォーム値を変換し、パラメーター内のアクション メソッドに渡します。 ここでは、モデル バインダーをインスタンス化学生エンティティのフォーム コレクションからプロパティ値を使用している。)

削除する`ID`から、`Bind`属性の ID が SQL Server は、行が挿入されると自動的に設定されますが、主キーの値であるためです。 ユーザーからの入力は、ID 値を設定しません。

以外の場合、`Bind`属性、try-catch ブロックがスキャフォールディング コードに加えたのみ変更します。 派生した例外`DbUpdateException`は、変更の保存中にキャッチされ、汎用的なエラー メッセージが表示されます。 `DbUpdateException`例外は場合もありますが原因でプログラミング エラーではなく、アプリケーションを外部の何かため、ユーザーが再試行することをお勧めします。 このサンプルでは実装されていませんが実稼働品質アプリケーションは、例外を記録します。 詳細については、次を参照してください。、**について理解を深めるログ**」の「[監視と遠隔測定 (実際のクラウド アプリのビルドと Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry)です。

`ValidateAntiForgeryToken`属性、クロスサイト リクエスト フォージェリ (CSRF) 攻撃の防止に役立ちます。 トークンは自動的に挿入され別に表示、 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)フォームがユーザーによって送信されるときに含まれるとします。 トークンを検証、`ValidateAntiForgeryToken`属性。 CSRF の詳細については、次を参照してください。[対策要求の偽造](../../security/anti-request-forgery.md)です。

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a>Overposting に関するセキュリティに関する注意

`Bind`上のスキャフォールディング コードを含む属性、`Create`メソッドは overposting から保護する方法の 1 つのシナリオを作成します。 たとえば、受講者エンティティが含まれています、`Secret`プロパティを設定するには、この web ページをたくないです。

```csharp
public class Student
{
    public int ID { get; set; }
    public string LastName { get; set; }
    public string FirstMidName { get; set; }
    public DateTime EnrollmentDate { get; set; }
    public string Secret { get; set; }
}
```

存在しない場合でも、 `Secret` web ページで、ハッカー フィールドでした、Fiddler などのツールを使用してまたは投稿に、一部の JavaScript を書き込み、`Secret`値を作成します。 なし、`Bind`学生のインスタンスでは、モデル バインダーの作成時にモデル バインダーが使用されるフィールドを制限する属性がピックアップされる`Secret`値の形式し、学生エンティティ インスタンスの作成に使用します。 値の指定されたハッカーし、`Secret`フォーム フィールドが、データベースで更新されます。 次の図は、Fiddler ツールを追加する、`Secret`ポストされたフォーム値へのフィールド (に値"OverPost") です。

![Fiddler シークレット フィールドの追加](crud/_static/fiddler.png)

"OverPost"は、正常に追加する値、`Secret`プロパティの挿入された行を web ページが、そのプロパティを設定できることを意図したものことはありませんが。

まず、データベースからエンティティを読み取り、呼び出すことで、編集のシナリオで overposting を防ぐことができます`TryUpdateModel`、明示的に許可されているプロパティ リストに渡します。 これらのチュートリアルで使用する方法です。

多くの開発者に好ま overposting を防ぐために別の方法では、モデル バインディングでエンティティ クラスではなく、ビュー モデルを使用します。 ビュー モデルで更新するプロパティのみが含まれます。 MVC モデル バインダーが終了すると、モデル プロパティの表示を必要に応じて AutoMapper などのツールを使用して、エンティティ インスタンスにコピーします。 使用して`_context.Entry`にその状態を設定するエンティティのインスタンスで`Unchanged`、し、設定`Property("PropertyName").IsModified`ビュー モデルに含まれる各エンティティ プロパティを true に設定します。 このメソッドは両方のでは、編集し、シナリオを作成します。

### <a name="test-the-create-page"></a>テストの作成 ページ

内のコード*Views/Students/Create.cshtml*を使用して`label`、 `input`、および`span`の検証メッセージ タグ ヘルパーの各フィールドにします。

選択して、ページの実行、**受講者** タブをクリックして**新規作成**です。

名前と日付を入力します。 お使いのブラウザーを実行できる場合は、無効な日付を入力してください。 (一部のブラウザーを強制する日付の選択を使用します。)をクリックして**作成**エラー メッセージを表示します。

![日付の妥当性確認エラー](crud/_static/date-error.png)

これは、既定では取得するサーバー側の検証後のチュートリアルもクライアント側の検証のコードを生成する属性を追加する方法が表示されます。 次の強調表示されたコードで、モデルの検証チェックを示しています、`Create`メソッドです。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

有効な値に日付を変更し、をクリックして**作成**に表示される新しい学生を表示する、**インデックス**ページ。

## <a name="update-the-edit-page"></a>更新プログラムの編集 ページ

*StudentController.cs*、HttpGet`Edit`メソッド (ことがなく 1 つ、`HttpPost`属性) を使用して、`SingleOrDefaultAsync`で学習したように、選択した学生エンティティを取得する方法を`Details`メソッドです。 この方法を変更する必要はありません。

### <a name="recommended-httppost-edit-code-read-and-update"></a>HttpPost 編集コードをお勧めします読み取りと更新。

HttpPost 編集アクション メソッドを次のコードに置き換えます。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

これらの変更は、overposting を防ぐためにセキュリティのベスト プラクティスを実装します。 生成された scaffolder、`Bind`属性し、エンティティのセットにモデル バインダーによって作成されたエンティティを追加、`Modified`フラグ。 コードは多くのシナリオの推奨されないこと、`Bind`属性に表示されていないフィールド内の既存のデータをクリアして、`Include`パラメーター。

新しいコードを読み取り、既存のエンティティと呼び出し`TryUpdateModel`取得されたエンティティのフィールドを更新する[ポストされたフォーム データでのユーザー入力に基づいて](xref:mvc/models/model-binding#how-model-binding-works)です。 Entity Framework の自動変更のセットを追跡、`Modified`フォーム入力によって変更されたフィールドのフラグ。 ときに、`SaveChanges`メソッドが呼び出されると、Entity Framework は、データベースの行を更新する SQL ステートメントを作成します。 同時実行の競合は無視され、ユーザーによって更新されたテーブルの列のみが、データベースで更新されます。 (後のチュートリアルは、同時実行の競合を処理する方法を示しています)。

過剰ポスティング、によって更新可能にフィールドを防ぐために、ベスト プラクティスとして、**編集**ページは、ホワイト リストに登録で、`TryUpdateModel`パラメーター。 (前に、パラメーター リストのフィールドの一覧は空の文字列はフォーム フィールド名を使用するプレフィックスです。)現在保護している、余分なフィールドはありませんが、後で、データ モデルにフィールドを追加する場合、自動的に保護されていることに明示的に追加するには、ここまでにバインドするモデル バインダーを使用するフィールドを一覧表示することを確認します。

HttpPost のメソッド シグネチャのこれらの変更の結果として`Edit`メソッドは、HttpGet と同じ`Edit`メソッドです。 したがって、メソッドを名前変更した`EditPost`です。

### <a name="alternative-httppost-edit-code-create-and-attach"></a>代替 HttpPost 編集コード: を作成し、アタッチ

推奨される HttpPost 編集コードでは、変更された列のみが更新され、モデル バインディングに含まれる必要のないプロパティのデータを保持をによりします。 ただし、読み取り優先のアプローチには、追加のデータベースの読み取り、および同時実行の競合を処理するためのより複雑なコードで発生することができますが必要です。 代わりには、EF コンテキストにモデル バインダーによって作成されたエンティティをアタッチし、修正としてマークするにです。 (プロジェクトを更新しない、次のコードでは、それがのみ表示を省略可能な方法を示しています)。 

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

Web ページの UI は、エンティティのすべてのフィールドが含まれています、それらを更新できます、このアプローチを使用することができます。

スキャフォールディングのコードが作成およびアタッチによる方法を使用しますが、のみキャッチ`DbUpdateConcurrencyException`404 エラー コードを例外とを返します。  例では、すべてデータベース更新の例外をキャッチし、エラー メッセージが表示されます。

### <a name="entity-states"></a>エンティティの状態

メモリ内のエンティティは、データベース内の対応する行との同期と、この情報を呼び出すときの動作を決定するかどうかの追跡データベース コンテキスト、`SaveChanges`メソッドです。 新しいエンティティを渡す場合など、`Add`メソッドに設定されているエンティティの状態を`Added`です。 その後呼び出す、`SaveChanges`メソッド、データベース コンテキストは、SQL の INSERT コマンドを発行します。

次の状態のいずれかでエンティティがあります。

* `Added`。 エンティティは、データベースにまだ存在しません。 `SaveChanges`メソッドは、INSERT ステートメントを発行します。

* `Unchanged`。 何も行う必要がありますでこのエンティティと、`SaveChanges`メソッドです。 データベースからエンティティを読み取るときに、エンティティは、この状態を持つ開始します。

* `Modified`。 一部またはすべてのエンティティのプロパティの値が変更されています。 `SaveChanges`メソッドは、UPDATE ステートメントを発行します。

* `Deleted`。 エンティティは、削除のマークされています。 `SaveChanges`メソッドは、DELETE ステートメントを発行します。

* `Detached`。 エンティティは、データベース コンテキストによって追跡されているはありません。

デスクトップ アプリケーションでは、通常状態の変更から自動的に設定します。 エンティティを読み取るし、そのプロパティ値の一部を変更します。 これにより、そのエンティティの状態に自動的に変更する`Modified`です。 その後呼び出す`SaveChanges`、Entity Framework を変更して実際のプロパティのみを更新する SQL の UPDATE ステートメントが生成されます。

Web アプリで、`DbContext`エンティティと、ページが表示されたら、そのデータを編集することが破棄される表示を最初に読み込みます。 ときに、HttpPost`Edit`アクション メソッドが呼び出される、新しい web 要求が行われ、の新しいインスタンスを持つ、`DbContext`です。 その新しいコンテキスト内のエンティティを再読み込みする場合は、デスクトップの処理をシミュレートします。

余分な読み取り操作を行うにはたくない場合は、モデル バインダーによって作成されたエンティティ オブジェクトを使用する必要があります。  これを行う最も簡単な方法では、前に示した代替 HttpPost 編集コードのように、modified、エンティティの状態を設定します。 その後呼び出す`SaveChanges`、Entity Framework がコンテキストに変更するプロパティを特定する方法があるないために、データベースの行のすべての列を更新します。

読み取り-優先のアプローチを避けたいユーザーが実際に変更されたフィールドのみを更新する SQL UPDATE ステートメントをする場合、コードは複雑です。 何らかの方法で元の値を保存する必要が (など、隠しフィールドを使用して) ため、使用できるようにするときに、HttpPost`Edit`メソッドが呼び出されます。 呼び出し元の値を使用して学生エンティティを作成してから、`Attach`メソッドをその元のバージョン、エンティティのエンティティの値を新しい値に更新およびを呼び出す`SaveChanges`です。

### <a name="test-the-edit-page"></a>テストの編集 ページ

アプリケーションの実行を選択して、**受講者** タブのをクリックして、**編集**ハイパーリンクです。

![[編集] ページの受講者](crud/_static/student-edit.png)

クリックして、データの一部を変更**保存**です。 **インデックス**ページが開かれ、変更されたデータを参照してください。

## <a name="update-the-delete-page"></a>更新プログラムの削除 ページ

*StudentController.cs*、テンプレート コード、HttpGet を`Delete`メソッドを使用、`SingleOrDefaultAsync`詳細ビューと編集方法に説明したとおりに、選択した学生エンティティを取得する方法です。 ただし、実装するカスタム エラー メッセージへの呼び出し`SaveChanges`失敗した場合、このメソッドとその対応するビューには、一部の機能を追加します。

更新プログラムの操作を作成すると、削除操作は次の 2 つのアクション メソッドが必要です。 GET 要求に応答して呼び出されるメソッドは、ユーザーを承認または削除操作をキャンセルする機会を提供するビューを表示します。 ユーザーを承認すると、POST 要求が作成されます。 このような場合、HttpPost`Delete`メソッドが呼び出され、そのメソッドが実際には、削除操作を実行し、します。

場合、try-catch ブロックに追加、HttpPost`Delete`データベースが更新されたときに発生したエラーを処理するメソッド。 エラーが発生する場合、HttpPost Delete メソッドは、エラーが発生したことを示すパラメーターを渡す、HttpGet Delete メソッドを呼び出します。 HttpGet Delete メソッドには、確認ページを取り消すか、もう一度実行する機会をユーザーに提供のエラー メッセージとし、再表示されます。

HttpGet を置き換える`Delete`アクション メソッドを次のコードは、エラー報告を管理します。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

このコードは、変更を保存する障害の発生後、メソッドが呼び出されたかどうかを示す省略可能なパラメーターを受け入れます。 このパラメーターが false 場合に、HttpGet`Delete`以前失敗せずにメソッドが呼び出されます。 これが呼び出されたとき、HttpPost によって`Delete`データベース更新エラーへの応答でメソッドをパラメーターが true であり、エラー メッセージは、ビューに渡されます。

### <a name="the-read-first-approach-to-httppost-delete"></a>HttpPost Delete に読み取り優先のアプローチ

HttpPost を置き換える`Delete`アクション メソッド (名前付き`DeleteConfirmed`) を次のコードを実際の削除操作を実行し、データベース更新エラーをキャッチします。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

このコードは、選択したエンティティを取得し、呼び出し、`Remove`エンティティの状態を設定するメソッドを`Deleted`です。 ときに`SaveChanges`が呼び出されると、SQL の DELETE コマンドを生成します。

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>HttpPost の削除 を作成およびアタッチによる方法

大規模なアプリケーションのパフォーマンスの向上が、優先順位の場合、ことで回避できます不必要な SQL クエリのみを使用して学生エンティティをインスタンス化してキーの値と、エンティティの状態に設定し、`Deleted`です。 エンティティを削除するのには、Entity Framework が必要なすべてです。 (プロジェクトにこのコードを配置しないです。 ここで、代替手段を説明するためにのみ)

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

エンティティには関連するデータも削除する必要がありますが場合、は、その連鎖削除がデータベースで構成されていることを確認してください。 エンティティの削除に対するこのアプローチで EF が付かないかもしれませんを削除する関連エンティティが存在します。

### <a name="update-the-delete-view"></a>削除ビューを更新します。

*Views/Student/Delete.cshtml*、次の例に示すように h2 見出しと h3 見出しの間でのエラー メッセージを追加します。

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

選択して、ページの実行、**受講者** タブをクリックし、**削除**ハイパーリンク。

![削除の確認 ページ](crud/_static/student-delete.png)

をクリックして**削除**です。 削除された学生ないインデックス ページが表示されます。 (同時実行のチュートリアルではアクション内のコードの処理エラーの例を参照します)。

## <a name="closing-database-connections"></a>データベース接続を閉じる

データベース接続を保持するリソースを解放するには、コンテキスト インスタンス破棄する必要ができるだけ早くが完了したこととします。 組み込みの ASP.NET Core[依存性の注入](../../fundamentals/dependency-injection.md)をそのタスクの行われます。

*Startup.cs*を呼び出す、 [AddDbContext 拡張メソッド](https://github.com/aspnet/EntityFramework/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs)をプロビジョニング、 `DbContext` ASP.NET DI コンテナー内のクラスです。 メソッドでは、サービスの有効期間を設定`Scoped`既定です。 `Scoped`コンテキスト オブジェクトの有効期間は、web 要求の存続期間と一致することを意味し、`Dispose`メソッドは、web 要求の最後に自動的に呼び出されます。

## <a name="handling-transactions"></a>トランザクションの処理

既定では、Entity Framework では、トランザクションが暗黙的に実装しています。 ここで複数の行またはテーブルを変更してを呼び出すシナリオで`SaveChanges`、Entity Framework が自動的に作成するすべての変更成功または失敗することを確認します。 いくつかの変更を最初に完了して、エラーが発生し、それらの変更は自動的にロールバックします。 必要な複数を制御する--など--トランザクションでは Entity Framework の外部で実行する操作を追加する場合のシナリオを参照してください[トランザクション](https://docs.microsoft.com/ef/core/saving/transactions)です。

## <a name="no-tracking-queries"></a>No 追跡クエリ

データベース コンテキストは、テーブルの行を取得し、それらを表すエンティティ オブジェクトを作成、ときに既定では、追跡データベース内にメモリ内のエンティティが同期されているかどうか。 メモリ内のデータはキャッシュとして機能し、エンティティを更新するときに使用します。 このキャッシュではありません多くの場合、web アプリケーションで必要なコンテキストをインスタンス化は通常短時間 (新しい 1 つが作成され、破棄要求ごとに) とコンテキストを読み取るそのエンティティが再度使用される前に、通常、エンティティが破棄されています。

呼び出してメモリ内のエンティティ オブジェクトの追跡を無効にすることができます、`AsNoTracking`メソッドです。 実行する一般的なシナリオを以下に示します。

* コンテキストの有効期間中に、エンティティを更新する必要はありません必要し、しないに EF[別々 のクエリによって取得されたエンティティのナビゲーション プロパティを自動的に読み込む](read-related-data.md)します。 頻繁にこれらの条件がコント ローラーの HttpGet アクション メソッドで満たされます。

* 大量のデータを取得するクエリを実行しているし、返されるデータの小さな部分だけが更新されます。 大規模なクエリでは、追跡をオフにして、後で更新する必要があるいくつかのエンティティのクエリを実行する方が効率的がある可能性があります。

* 更新するためにエンティティをアタッチするが、別の目的のため、同じエンティティを取得する前。 エンティティは、データベースのコンテキストによって既に追跡されているが、ために、変更するエンティティをアタッチできません。 この状況に対処する方法の 1 つが呼び出されて`AsNoTracking`以前のクエリにします。

詳細については、次を参照してください。 [vs を追跡します。No 追跡](https://docs.microsoft.com/ef/core/querying/tracking)です。

## <a name="summary"></a>概要

学生のエンティティの単純な CRUD 操作を実行するページの完全なセットがあるようになりました。 次のチュートリアルでは、機能を拡張します、**インデックス**並べ替え、フィルター処理、およびページングを追加することによってページ。

>[!div class="step-by-step"]
[前へ](intro.md)
[次へ](sort-filter-page.md)  
