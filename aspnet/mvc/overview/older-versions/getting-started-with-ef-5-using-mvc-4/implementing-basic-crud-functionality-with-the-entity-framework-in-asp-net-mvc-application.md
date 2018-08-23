---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: ASP.NET MVC アプリケーション (2/10) で Entity Framework での基本的な CRUD 機能を実装する |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: riande
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 999a598b6f9c9a16c596cb6c8d7bb46439876f01
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834817"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>ASP.NET MVC アプリケーション (2/10) で Entity Framework での基本的な CRUD 機能を実装します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。 チュートリアルのシリーズを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから始めてください。
> 
> > [!NOTE] 
> > 
> > を解決できない問題が生じた場合[章では、完了したダウンロード](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 問題の解決策は、完成したコードにコードを比較することによって一般的に見つかります。 一般的なエラーとその解決方法は、次を参照してください。[エラーと回避策。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルでは、保存し、Entity Framework と SQL Server LocalDB を使用してデータを表示する MVC アプリケーションを作成しました。 このチュートリアルで確認およびカスタマイズ、CRUD (作成、読み取り、更新、削除)、MVC スキャフォールディングが自動的にコント ローラーとビューを作成するコードです。

> [!NOTE]
> コントローラーとデータ アクセス層の間に抽象化レイヤーを作成するためにリポジトリ パターンを実装することは、よく行われることです。 これらのチュートリアルを簡潔にするには、このシリーズの後のチュートリアルまでリポジトリを実装するされません。


このチュートリアルでは、次の web ページを作成します。

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>詳細ページを作成します。

受講者のスキャフォールディング コードに`Index`ページを含めなかった、`Enrollments`プロパティ、そのプロパティには、コレクションが格納されているためです。 `Details`ページの HTML テーブルのコレクションの内容を表示します。

 *Controllers\StudentController.cs*、アクション メソッド、`Details`表示は、 `Find` 、1 つを取得するメソッドを`Student`エンティティ。 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 キーの値としてメソッドに渡されます、`id`パラメーター内のルート データから取得し、**詳細**Index ページにハイパーリンク。 

1. 開いている*Views\Student\Details.cshtml*します。 使用して各フィールドを表示、`DisplayFor`ヘルパーは、次の例に示すようにします。 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. 後に、`EnrollmentDate`フィールドと、終了する直前に`fieldset`タグは、次の例に示すように、登録の一覧を表示するコードを追加します。

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    このコードは、`Enrollments` ナビゲーション プロパティ内のエンティティをループ処理します。 各`Enrollment`エンティティ、プロパティでは、コースのタイトルとグレードが表示されます。 コース タイトルがから取得した、`Course`エンティティに格納されている、`Course`のナビゲーション プロパティ、`Enrollments`エンティティ。 すべてのデータをデータベースから自動的に取得されます必要がある場合。 (つまり、使用する遅延読み込みは、ここです。 指定しなかった*一括読み込み*の`Courses`ナビゲーション プロパティ、そのプロパティにアクセスしよう初めてのため、クエリがデータベースに送信、データを取得します。 詳細については、遅延読み込みと一括読み込みで、[関連データの読み取り](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズの後半のチュートリアルです)。
3. 選択して、ページの実行、**学生**タブとクリックすると、**詳細**Alexander Carson のリンク。 選んだ受講者のコースとグレードの一覧が表示されます。

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>[作成] ページを更新しています

1. *Controllers\StudentController.cs*、置換、`HttpPost``Create`アクション メソッドを次のコードを追加する、`try-catch`ブロックと[Bind 属性](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)スキャフォールディング メソッド。 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    このコードを追加、`Student`に ASP.NET MVC モデル バインダーによって作成されたエンティティ、`Students`エンティティが設定され、データベースに変更を保存します。 (*モデル バインダー*モデル バインダーに変換が投稿されたフォーム値の CLR 型とパラメーター内のアクション メソッドに渡します。 フォームによって送信されたデータを操作するために簡単には、ASP.NET MVC の機能を参照します。 この、モデル バインダーをインスタンス化、`Student`からプロパティを使用してエンティティの値、`Form`コレクションです)。

    `ValidateAntiForgeryToken`属性を防ぎ[クロスサイト リクエスト フォージェリ](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)攻撃です。

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
2. 選択して、ページの実行、**学生**タブとクリックして**新規作成**です。

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    いくつかのデータ検証は、既定では動作します。 名前と、無効な日付を入力し、クリックして**作成**エラー メッセージを確認します。

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    次の強調表示されたコードでは、モデルの検証チェックを示します。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    9/1/2005 などの有効な値に日付を変更し、をクリックして**作成**で表示される新しい学生を表示する、**インデックス**ページ。

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>投稿の編集 ページを更新しています

*Controllers\StudentController.cs*、 `HttpGet` `Edit`メソッド (せず 1 つ、`HttpPost`属性) を使用して、`Find`メソッドを選択した取得`Student`見たこととして、エンティティ`Details`メソッド。 このメソッドを変更する必要はありません。

ただし、置換、 `HttpPost` `Edit`アクション メソッドを次のコードを追加する、`try-catch`ブロックと[Bind 属性](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

このコードはで確認したものと似ています、 `HttpPost` `Create`メソッド。 ただし、エンティティ セットにモデル バインダーによって作成されたエンティティを追加する代わりにこのコードにフラグを設定が変更されたことを示すエンティティです。 ときに、 [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)メソッドを呼び出すと、 [Modified](https://msdn.microsoft.com/library/system.data.entitystate.aspx)フラグによって、データベースの行を更新する SQL ステートメントを作成する Entity Framework。 ユーザーを変更していないものも含め、データベースの行のすべての列を更新は、同時実行の競合が無視されます。 (このシリーズの以降のチュートリアルでは、同時実行を処理する方法を説明します) します。

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>エンティティの状態と、アタッチと SaveChanges メソッド

データベース コンテキストは、メモリ内のエンティティがデータベースの対応する行と同期しているかどうかを追跡しており、この情報により、`SaveChanges` メソッドを呼び出したときの処理が決まります。 新しいエンティティを渡す場合など、[追加](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx)メソッドは、エンティティの状態に設定されている`Added`します。 その後呼び出す、 [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)メソッドでは、データベース コンテキストの問題、SQL`INSERT`コマンド。

エンティティは、のいずれかである可能性があります、[状態に従って](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`。 エンティティは、データベースにまだ存在しません。 `SaveChanges`メソッドを発行する必要があります、`INSERT`ステートメント。
- `Unchanged`。 `SaveChanges` メソッドはこのエンティティに対し何も行う必要はありません。 データベースからエンティティを読み取ると、エンティティはこの状態で開始します。
- `Modified`。 エンティティのプロパティ値の一部またはすべてが変更されています。 `SaveChanges`メソッドを発行する必要があります、`UPDATE`ステートメント。
- `Deleted`。 エンティティには削除のマークが付けられています。 `SaveChanges`メソッドを発行する必要があります、`DELETE`ステートメント。
- `Detached`。 エンティティはデータベース コンテキストによって追跡されていません。

デスクトップ アプリケーションにおいて、通常、状態の変更は自動的に設定されます。 デスクトップのタイプのアプリケーションでは、エンティティを読み取るし、そのプロパティ値の一部を変更します。 そのエンティティの状態は自動的に `Modified` に変更されます。 その後呼び出す`SaveChanges`、Entity Framework には、SQL が生成されます`UPDATE`実際に変更したプロパティのみを更新するステートメント。

この継続的なシーケンスでは、web アプリ、接続が確立は許可されていません。 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)を読み取るエンティティは、ページが表示された後で破棄されます。 ときに、 `HttpPost` `Edit`アクション メソッドが呼び出され、新しい要求が行われるの新しいインスタンスがある、 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)、エンティティの状態を手動で設定する必要があるため、`Modified.`を呼び出すと、 `SaveChanges`、Entity Framework は、コンテキストには、変更したプロパティを特定する方法があるないので、データベースの行のすべての列を更新します。

場合は、SQL`Update`ステートメントは、ユーザーが実際に変更されたフィールドのみを更新するため、使用できるようにする場合に、(非表示フィールド) などのいくつかの方法で元の値を保存することができます、 `HttpPost` `Edit`メソッドが呼び出されます。 作成し、 `Student` 、呼び出し元の値を使用して、エンティティ、`Attach`メソッドは、エンティティの元のバージョンでは、エンティティの値を新しい値に更新を呼び出して`SaveChanges.`詳細については、次を参照してください[。エンティティの状態および SaveChanges](https://msdn.microsoft.com/data/jj592676)と[ローカル データ](https://msdn.microsoft.com/data/jj592872)MSDN データ デベロッパー センターでします。

コードでは、 *Views\Student\Edit.cshtml*で見たような*Create.cshtml*、し、変更は必要ありません。

選択して、ページの実行、**学生** タブをクリックして、**編集**ハイパーリンク。

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

データをいくつか変更し、**[Save]** をクリックします。 Index ページで、変更されたデータを表示します。

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Delete ページの更新

*Controllers\StudentController.cs*、テンプレート コードを`HttpGet``Delete`メソッドは、`Find`メソッドを取得、選択した`Student`でエンティティを見た、`Details`と`Edit`メソッド。 ただし、`SaveChanges` の呼び出しが失敗したときのカスタム エラー メッセージを実装するには、何らかの機能とその対応するビューをこのメソッドに追加します。

更新および作成操作で見たように、削除操作にも 2 つのアクション メソッドが必要です。 GET 要求に応答して呼び出されるメソッドには、ユーザーを承認または削除操作をキャンセルする機会を提供するビューが表示されます。 ユーザーが操作を承認すると、POST 要求が作成されます。 その場合、 `HttpPost` `Delete`メソッドが呼び出され、そのメソッドが実際には、削除操作を実行します。

追加、`try-catch`へのブロック、 `HttpPost` `Delete`データベースが更新されたときに発生するエラーを処理するメソッド。 エラーが発生する場合、 `HttpPost` `Delete`メソッドの呼び出し、 `HttpGet` `Delete`メソッドは、エラーが発生したことを示すパラメーターを渡します。 `HttpGet Delete`メソッドが再び表示し、確認ページと、エラー メッセージを取り消すか、もう一度お試しする機会をユーザーに提供します。

1. 置換、 `HttpGet` `Delete`アクション メソッドを次のコードは、エラー報告を管理します。 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    このコードは受け取ります、[省略可能な](https://msdn.microsoft.com/library/dd264739.aspx)変更の保存に失敗した後に呼び出されたかどうかを示すブール型パラメーター。 このパラメーターは`false`ときに、 `HttpGet` `Delete`以前失敗せずにメソッドが呼び出されます。 呼び出されたとき、 `HttpPost` `Delete`メソッド データベース更新エラーへの応答で、パラメーターが`true`エラー メッセージは、ビューに渡されます。
2. 置換、 `HttpPost` `Delete`アクション メソッド (名前付き`DeleteConfirmed`) を次のコードでは、実際の削除操作を実行して、データベース更新エラーをキャッチします。

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     このコードでは、選択したエンティティを取得します。 を呼び出して、[削除](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx)エンティティの状態を設定するメソッドを`Deleted`します。 ときに`SaveChanges`を呼び出すと、SQL`DELETE`コマンドが生成されます。 また、アクション メソッドの名前を `DeleteConfirmed` から `Delete` に変更しています。 という名前のスキャフォールディングされたコード、 `HttpPost` `Delete`メソッド`DeleteConfirmed`提供する、`HttpPost`メソッドに一意のシグネチャ。 (CLR にオーバー ロードされたメソッドが、さまざまなメソッド パラメーターが必要です)。シグネチャが一意ではこれでは、MVC 規則に従うし、に同じ名前を使用、`HttpPost`と`HttpGet`メソッドを削除します。

     呼び出すコードの行を置き換えることで、行を取得する不必要な SQL クエリの問題を回避できれば量の多いアプリケーションでパフォーマンスの向上が優先度の場合、`Find`と`Remove`黄色で示すように、次のコードでメソッド強調表示します。

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     このコードをインスタンス化、`Student`主キーの値のみを使用してエンティティ、エンティティの状態を設定および`Deleted`。 エンティティを削除するために Entity Framework に必要なものは主キーの値だけです 

     前述のように、 `HttpGet` `Delete`メソッド データは削除されません。 要求 (またはそのさらに言えば、任意の編集操作を実行する操作、または他のデータを変更する操作を作成)、GET に対する応答で削除操作を実行するセキュリティ上のリスクを作成します。 詳細については、次を参照してください。 [ASP.NET MVC ヒントと 46: セキュリティ ホールを作成するため、削除のリンクを使用しない](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)Stephen Walther のブログ。
3. *Views\Student\Delete.cshtml*、間にエラー メッセージを追加、`h2`見出しと`h3`見出しで、次の例に示すようにします。

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     選択して、ページの実行、**学生**タブとクリックすると、**削除**ハイパーリンク。

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. **[Delete]** をクリックします。 削除された学生を含まない [Index] ページが表示されます  (エラー処理のアクションにコードの例が表示されます、[同時実行の処理](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズの後半のチュートリアルです)。

## <a name="ensuring-that-database-connections-are-not-left-open"></a>データベース接続が開いたままいないにことを確認

適切なデータベース接続を閉じ、リソースを解放された保持することを確認する必要がありますを参照してくださいにコンテキストのインスタンスが破棄されたことです。 スキャフォールディングされたコードは、理由は、 [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx)メソッドの末尾に、`StudentController`クラス*StudentController.cs*次の例のように。

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

基本`Controller`クラスが既に実装、`IDisposable`インターフェイスのため、このコードでは、上書きを単に追加します、`Dispose(bool)`コンテキスト インスタンスを明示的に破棄するメソッド。

## <a name="summary"></a>まとめ

単純な CRUD 操作を実行するページの完全なセットがあるようになりました`Student`エンティティ。 MVC ヘルパーを使用すると、データ フィールドの UI 要素を生成します。 MVC ヘルパーの詳細については、次を参照してください。 [HTML ヘルパーを使用してフォームをレンダリング](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx)(ページは MVC 3 は、MVC 4 にも引き続き該当です)。

次のチュートリアルでは、並べ替えとページングを追加することで、インデックス ページの機能を拡張します。

その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)します。

> [!div class="step-by-step"]
> [前へ](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [次へ](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
