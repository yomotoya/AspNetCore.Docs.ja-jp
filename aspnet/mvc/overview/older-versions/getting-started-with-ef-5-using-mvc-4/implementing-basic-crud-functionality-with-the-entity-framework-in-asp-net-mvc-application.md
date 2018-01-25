---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: "ASP.NET MVC アプリケーション (10 の 2) に、Entity Framework を使用して基本的な CRUD 機能を実装する |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d031cd760fb578d29626933eed39fe987ef796d7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>ASP.NET MVC アプリケーション (10 の 2) に、Entity Framework を使用して基本的な CRUD 機能を実装します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。 一連のチュートリアルを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから開始します。
> 
> > [!NOTE] 
> > 
> > 解決できない場合、問題が発生した場合[ダウンロード完了章](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 一般に、コードを完成したコードを比較することによって、問題の解決策を検索できます。 一般的なエラーとそれらを解決する方法は、次を参照してください。[エラーと回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルを格納し、Entity Framework と SQL Server LocalDB を使用してデータを表示する MVC アプリケーションを作成します。 このチュートリアルでを確認およびカスタマイズしたりする、CRUD (作成、読み取り、更新、削除) MVC のスキャフォールディング自動的に作成するコント ローラーとビューのコード。

> [!NOTE]
> コント ローラーと、データ アクセス層の間で抽象化レイヤーを作成するのには、リポジトリ パターンを実装する一般的なことをお勧めします。 これらのチュートリアルを簡潔にするには、このシリーズの後のチュートリアルまでリポジトリを実装するされません。


このチュートリアルでは、次の web ページを作成します。

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>詳細ページを作成します。

受講者のスキャフォールディング コード`Index`ページは省略して、`Enrollments`プロパティ、そのプロパティには、コレクションが格納されているためです。 `Details`ページを HTML テーブルで、コレクションの内容を表示します。

 *Controllers\StudentController.cs*、用のアクション メソッド、`Details`使用方法を表示、 `Find` 、1 つを取得する方法を`Student`エンティティです。 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 キーの値としてメソッドに渡す、`id`パラメーターでルート データから取得し、**詳細**インデックス ページのハイパーリンクのです。 

1. 開いている*Views\Student\Details.cshtml*です。 使用して、各フィールドが表示されます、`DisplayFor`ヘルパーに渡し、次の例で示すようにします。 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. 後に、`EnrollmentDate`フィールドと、終了する直前に`fieldset`タグ付け、次の例で示すように、登録の一覧を表示するコードを追加します。

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    このコードは、内のエンティティをループ処理、`Enrollments`ナビゲーション プロパティ。 各`Enrollment`エンティティ プロパティには、コースのタイトルとグレードが表示されます。 コース タイトルが取得される、`Course`に格納されているエンティティ、`Course`のナビゲーション プロパティ、`Enrollments`エンティティです。 すべてのデータがデータベースから自動的に取得が必要な場合です。 (つまり、使用する遅延読み込みは、ここです。 指定しなかった*一括読み込み*の`Courses`ナビゲーション プロパティ、そのプロパティにアクセスすると、初めてので、クエリがデータベースに送信データを取得します。 詳細を読み取ることができます遅延読み込みと一括読み込みでは、[関連のデータの読み取り](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズの後でチュートリアルです)。
3. 選択して、ページの実行、**受講者** タブをクリックし、**詳細**Alexander Carson へのリンク。 選択した受講者コースとグレードの一覧を参照してください。

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>[作成] ページを更新

1. *Controllers\StudentController.cs*、置換、`HttpPost``Create`アクション メソッドを次のコードを追加する、`try-catch`ブロックおよび[Bind 属性](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)スキャフォールディング メソッドに。 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    このコードを追加、`Student`に ASP.NET MVC モデル バインダーによって作成されたエンティティ、`Students`エンティティが設定され、データベースに変更を保存します。 (*モデル バインダー*変換ポストされたフォーム値の CLR 型とパラメーター内のアクション メソッドに渡して、モデル バインダー; フォームによって送信されたデータを操作するために簡単には、ASP.NET MVC 機能を指します。 この、モデル バインダーをインスタンス化、`Student`からプロパティを使用してエンティティの値、`Form`コレクション)。

    `ValidateAntiForgeryToken`属性を防止[クロスサイト リクエスト フォージェリ](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)攻撃です。

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.chstml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. 選択して、ページの実行、**受講者** タブをクリックして**新規作成**です。

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    いくつかのデータ検証は、既定では動作します。 名前と無効な日付を入力し、をクリックして**作成**エラー メッセージを表示します。

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    次の強調表示されたコードでは、モデルの検証チェックを示します。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    9/1/2005 などの有効な値に日付を変更し、をクリックして**作成**に表示される新しい学生を表示する、**インデックス**ページ。

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>更新後の編集 ページ

*Controllers\StudentController.cs*、 `HttpGet` `Edit`メソッド (ことがなく 1 つ、`HttpPost`属性) を使用して、 `Find` 、選択した取得する方法を`Student`を説明したのと、エンティティ`Details`メソッドです。 この方法を変更する必要はありません。

ただし、置換、 `HttpPost` `Edit`アクション メソッドを次のコードを追加する、`try-catch`ブロックおよび[Bind 属性](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

このコードはで学習したような`HttpPost``Create`メソッドです。 ただし、エンティティ セットにモデル バインダーによって作成されたエンティティを追加するには、代わりにこのコードにフラグを設定が変更されたことを示すエンティティです。 ときに、 [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)メソッドが呼び出されると、 [Modified](https://msdn.microsoft.com/library/system.data.entitystate.aspx)フラグにより、Entity Framework データベースの行を更新する SQL ステートメントを作成します。 ユーザーが変更されなかったものも含め、データベースの行のすべての列を更新は、同時実行の競合が無視されます。 (学習このシリーズの後のチュートリアルでは同時実行を処理する方法です。)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>エンティティの状態と、アタッチと SaveChanges メソッド

メモリ内のエンティティは、データベース内の対応する行との同期と、この情報を呼び出すときの動作を決定するかどうかの追跡データベース コンテキスト、`SaveChanges`メソッドです。 新しいエンティティを渡す場合など、[追加](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx)メソッドに設定されているエンティティの状態を`Added`です。 その後呼び出す、 [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)メソッド、データベース コンテキストの問題、SQL`INSERT`コマンド。

1 つのエンティティがあります、[次の状態](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`。 エンティティは、データベースにまだ存在しません。 `SaveChanges`メソッドを発行する必要があります、`INSERT`ステートメントです。
- `Unchanged`。 何も行う必要がありますでこのエンティティと、`SaveChanges`メソッドです。 データベースからエンティティを読み取るときに、エンティティは、この状態を持つ開始します。
- `Modified`。 一部またはすべてのエンティティのプロパティの値が変更されています。 `SaveChanges`メソッドを発行する必要があります、`UPDATE`ステートメントです。
- `Deleted`。 エンティティは、削除のマークされています。 `SaveChanges`メソッドを発行する必要があります、`DELETE`ステートメントです。
- `Detached`。 エンティティは、データベース コンテキストによって追跡されているはありません。

デスクトップ アプリケーションでは、通常状態の変更から自動的に設定します。 デスクトップの種類のアプリケーションでは、エンティティの読み取りし、そのプロパティ値の一部を変更します。 これにより、そのエンティティの状態に自動的に変更する`Modified`です。 その後呼び出す`SaveChanges`、Entity Framework は、SQL を生成`UPDATE`を変更して実際のプロパティのみを更新するステートメント。

Web アプリの切断されているため、この連続した一連の許可されていません。 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)を読み取る、ページが表示されたら、エンティティが破棄されています。 ときに、 `HttpPost` `Edit`アクション メソッドが呼び出される、新しい要求が行われ、の新しいインスタンスを持つ、 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)にエンティティの状態を手動で設定する必要があるため、`Modified.`を呼び出すと、 `SaveChanges`、Entity Framework は、コンテキストに変更するプロパティを特定する方法があるないために、データベースの行のすべての列を更新します。

場合は、SQL`Update`ステートメントをユーザーが実際に変更されたフィールドのみを更新するため、使用できるようにするときに、何らかの方法 (非表示フィールドなど) で元の値を保存することができます、 `HttpPost` `Edit`メソッドが呼び出されます。 作成し、 `Student` 、呼び出し元の値を使用して、エンティティ、`Attach`メソッドをその元のバージョン、エンティティのエンティティの値を新しい値に更新およびを呼び出す`SaveChanges.`詳細については、次を参照してください[。エンティティの状態と SaveChanges](https://msdn.microsoft.com/data/jj592676)と[ローカル データ](https://msdn.microsoft.com/data/jj592872)MSDN データ デベロッパー センターにします。

内のコード*Views\Student\Edit.cshtml*で学習したような*Create.cshtml*、し、変更は必要ありません。

選択して、ページの実行、**受講者** タブをクリックして、**編集**ハイパーリンクです。

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

クリックして、データの一部を変更**保存**です。 [インデックス] ページで変更されたデータを表示します。

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>ページの削除 を更新

*Controllers\StudentController.cs*、テンプレート コードを`HttpGet``Delete`メソッドを使用、 `Find` 、選択した取得する方法を`Student`エンティティも従って、`Details`と`Edit`メソッドです。 ただし、実装するカスタム エラー メッセージへの呼び出し`SaveChanges`失敗した場合、このメソッドとその対応するビューには、一部の機能を追加します。

更新プログラムの操作を作成すると、削除操作は次の 2 つのアクション メソッドが必要です。 GET 要求に応答して呼び出されるメソッドは、ユーザーを承認または削除操作をキャンセルする機会を提供するビューを表示します。 ユーザーを承認すると、POST 要求が作成されます。 発生したときに、 `HttpPost` `Delete`メソッドが呼び出され、そのメソッドが実際には、削除操作を実行し、します。

追加、`try-catch`へのブロック、 `HttpPost` `Delete`データベースが更新されたときに発生したエラーを処理するメソッド。 エラーが発生する場合、 `HttpPost` `Delete`メソッドの呼び出し、 `HttpGet` `Delete`メソッド、エラーが発生したことを示すパラメーターを渡します。 `HttpGet Delete`メソッドは、取り消すか、もう一度実行する機会をユーザーに提供のエラー メッセージと共に確認 ページを再表示します。

1. 置換、 `HttpGet` `Delete`アクション メソッドを次のコードは、エラー報告を管理します。 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    このコードが受け付ける、[省略可能な](https://msdn.microsoft.com/library/dd264739.aspx)変更を保存に失敗した後に呼び出されたかどうかを示すブール値パラメーターです。 このパラメーターは`false`ときに、 `HttpGet` `Delete`以前失敗せずにメソッドが呼び出されます。 呼び出されたとき、 `HttpPost` `Delete`データベース更新エラーへの応答でメソッドをパラメーターが`true`エラー メッセージは、ビューに渡されます。
- 置換、 `HttpPost` `Delete`アクション メソッド (名前付き`DeleteConfirmed`) を次のコードを実際の削除操作を実行し、データベース更新エラーをキャッチします。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

    このコードは、選択したエンティティを取得し、呼び出して、[削除](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx)エンティティの状態を設定するメソッドを`Deleted`です。 ときに`SaveChanges`が呼び出されると、SQL`DELETE`コマンドが生成されます。 アクション メソッドの名前も変更した`DeleteConfirmed`に`Delete`です。 名前付きスキャフォールディング コード、 `HttpPost` `Delete`メソッド`DeleteConfirmed`を与える、`HttpPost`メソッド固有の署名。 (CLR には、別のメソッドのパラメーターを持つオーバー ロードされたメソッドが必要があります)。署名が一意ではこれでは、MVC 規則のオプションを使用と同じ名前を使用、`HttpPost`と`HttpGet`メソッドを削除します。

    不要な SQL クエリを呼び出すコードの行を置き換えることで、行を取得することで回避できます量の多いアプリケーションのパフォーマンスの向上がある場合、優先度、`Find`と`Remove`黄色で示したように、次のコードを持つメソッド強調表示します。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

    このコードをインスタンス化、`Student`主キーの値のみを使用して、エンティティ、エンティティの状態に設定し、`Deleted`です。 エンティティを削除するのには、Entity Framework が必要なすべてです。

    前述のように、 `HttpGet` `Delete`データは削除されません。 要求 (またはに言えば、任意の編集操作を実行する操作、またはデータが変更されたその他の操作を作成します)、GET に対する応答で delete 操作の実行、セキュリティ上のリスクを作成します。 詳細については、次を参照してください。 [ASP.NET MVC ヒント #46-セキュリティ ホールを作成するため、削除のリンクを使用しない](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)Stephen Walther ブログ。
- *Views\Student\Delete.cshtml*との間のエラー メッセージを追加、`h2`見出しと`h3`見出しで、次の例に示すように。

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

    選択して、ページの実行、**受講者** タブをクリックし、**削除**ハイパーリンク。

    ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
- をクリックして**削除**です。 削除された学生ないインデックス ページが表示されます。 (処理コードでの操作でエラーの例が表示されます、[同時実行の処理](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズの後でチュートリアルです)。

## <a name="ensuring-that-database-connections-are-not-left-open"></a>データベース接続が残っていないこと開いていることを確認

確認して適切なデータベース接続が閉じ、リソースを保持します。 を解放する必要がありますを参照してくださいにコンテキストのインスタンスが破棄されることです。 スキャフォールディングのコードは、理由は、 [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx)メソッドの最後に、`StudentController`クラス*StudentController.cs*次の例のように。

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

基本`Controller`既にクラスが実装する、`IDisposable`インターフェイスのため、このコードは単純にする上書きを追加、`Dispose(bool)`メソッドを明示的にコンテキストのインスタンスを破棄します。

## <a name="summary"></a>まとめ

単純な CRUD 操作を実行するページの完全なセットがあるようになりました`Student`エンティティです。 MVC ヘルパーを使用すると、データ フィールド用の UI 要素を生成します。 MVC ヘルパーの詳細については、次を参照してください。 [HTML ヘルパーを使用してフォームをレンダリング](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx)(ページが MVC 3 がは MVC 4 にも引き続き該当)。

次のチュートリアルでは、並べ替えとページングを追加することでインデックス ページの機能を展開します。

その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。

>[!div class="step-by-step"]
[前へ](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
[次へ](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
