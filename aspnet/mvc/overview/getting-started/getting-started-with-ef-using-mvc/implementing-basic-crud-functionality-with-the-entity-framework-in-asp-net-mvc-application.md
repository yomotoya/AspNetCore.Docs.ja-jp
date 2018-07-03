---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: ASP.NET MVC アプリケーションで Entity Framework での基本的な CRUD 機能を実装する |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 6 Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/09/2015
ms.topic: article
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: eeed2b572ddecb66c3b95926b57dd783c26d5f0f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390218"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>ASP.NET MVC アプリケーションで Entity Framework での基本的な CRUD 機能を実装します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 6 Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。


前のチュートリアルでは、保存し、Entity Framework と SQL Server LocalDB を使用してデータを表示する MVC アプリケーションを作成しました。 このチュートリアルで確認およびカスタマイズ、CRUD (作成、読み取り、更新、削除)、MVC スキャフォールディングが自動的にコント ローラーとビューを作成するコードです。

> [!NOTE]
> コントローラーとデータ アクセス層の間に抽象化レイヤーを作成するためにリポジトリ パターンを実装することは、よく行われることです。 この一連のチュートリアルが複雑にならないようにし、Entity Framework 自体の使い方に集中できるように、チュートリアルではリポジトリは使われていません。 リポジトリを実装する方法については、次を参照してください。、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)します。


このチュートリアルでは、次の web ページを作成します。

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>詳細ページを作成します。

受講者のスキャフォールディング コードに`Index`ページを含めなかった、`Enrollments`プロパティ、そのプロパティには、コレクションが格納されているためです。 `Details`ページの HTML テーブルのコレクションの内容を表示します。

 *Controllers\StudentController.cs*、用のアクション メソッド、`Details`表示は、[検索](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx)、1 つを取得するメソッドを`Student`エンティティ。 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

キーの値としてメソッドに渡されます、`id`パラメーター由来*データ ルーティング*で、**詳細**Index ページにハイパーリンク。

### <a name="tip-route-data"></a>ヒント:**ルート データ**

ルート データは、モデル バインダーがルーティング テーブルで指定された URL セグメントであるデータです。 たとえば、既定のルートを指定します`controller`、 `action`、および`id`セグメント。

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

次の URL で既定のルートをマップ`Instructor`として、 `controller`、`Index`として、 `action` 1 を`id`。 これらは、ルート データの値。

`http://localhost:1230/Instructor/Index/1?courseID=2021`

"? courseID = 2021"クエリ文字列値です。 モデル バインダーは、渡した場合でも機能、`id`クエリ文字列の値として。

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Url がによって作成された`ActionLink`Razor ビュー内のステートメント。 次のコードで、`id`のでパラメーターに既定のルートと一致する`id`ルート データに追加されます。

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

次のコードで`courseID`既定のルートのパラメーターに一致しないため、クエリ文字列として追加されます。

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]


1. 開いている*Views\Student\Details.cshtml*します。 使用して各フィールドを表示、`DisplayFor`ヘルパーは、次の例に示すようにします。

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]
2. 後に、`EnrollmentDate`フィールドと、終了する直前に`</dl>`タグ付け、次の例に示すように、登録の一覧を表示する強調表示されたコードを追加します。

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    コードを貼り付けた後でコードのインデントが乱れた場合は、Ctrl + D + K キーを押して修正します。

    このコードは、`Enrollments` ナビゲーション プロパティ内のエンティティをループ処理します。 各`Enrollment`エンティティ、プロパティでは、コースのタイトルとグレードが表示されます。 コース タイトルがから取得した、`Course`エンティティに格納されている、`Course`のナビゲーション プロパティ、`Enrollments`エンティティ。 すべてのデータをデータベースから自動的に取得されます必要がある場合。 (つまり、使用する遅延読み込みは、ここです。 指定しなかった*一括読み込み*の`Courses`ナビゲーション プロパティ、加入契約は、受講者が同じクエリでは取得されませんでした。 代わりに、最初にアクセスすると、`Enrollments`ナビゲーション プロパティ、新しいクエリがデータベースに送信、データを取得します。 詳細については、遅延読み込みと一括読み込みで、[関連データの読み取り](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズの後半のチュートリアルです)。
3. 選択して、ページの実行、**学生**タブとクリックすると、**詳細**Alexander Carson のリンク。 (Details.cshtml ファイルが開いている間に ctrl キーを押しながら f5 キーを押すと、表示されます、HTTP 400 エラーの詳細ページを実行しようとする Visual Studio からのリンクを表示する受講者を指定することに達してためです。 見つからず、だけ、URL から「学生/詳細」を削除し、もう一度お試しまたはブラウザーを閉じ、プロジェクトを右クリックして をクリックして**ビュー**、 をクリックし、**ブラウザーで表示**)。

    選んだ受講者のコースとグレードの一覧が表示されます。

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="update-the-create-page"></a>更新プログラムの作成 ページ

1. *Controllers\StudentController.cs*、置換、`HttpPost``Create`アクション メソッドを次のコードを追加する、`try-catch`ブロックし、削除`ID`から、 [Bind 属性](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)のスキャフォールディングされたメソッド。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    このコードを追加、`Student`に ASP.NET MVC モデル バインダーによって作成されたエンティティ、`Students`エンティティが設定され、データベースに変更を保存します。 (*モデル バインダー*モデル バインダーに変換が投稿されたフォーム値の CLR 型とパラメーター内のアクション メソッドに渡します。 フォームによって送信されたデータを操作するために簡単には、ASP.NET MVC の機能を参照します。 この、モデル バインダーをインスタンス化、`Student`からプロパティを使用してエンティティの値、`Form`コレクションです)。

    削除する`ID`バインドから属性ため`ID`は SQL Server は、行が挿入されると自動的に設定されますが、主キーの値。 ユーザーからの入力が設定されていない、`ID`値。

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>セキュリティ警告 -、`ValidateAntiForgeryToken`属性を防ぎ[クロスサイト リクエスト フォージェリ](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)攻撃です。 対応する必要があります`Html.AntiForgeryToken()`ステートメントで、ビューでは、後でわかります。

    `Bind`属性から保護する方法の 1 つは、*オーバーポスティング*でシナリオを作成します。 たとえば、`Student`エンティティが含まれています、`Secret`プロパティを設定するには、この web ページをしたくないです。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    存在しない場合でも、`Secret`ハッカー、web ページ上のフィールドがなど、ツールを使用する可能性があります[fiddler](http://fiddler2.com/home)、JavaScript、投稿を記述または、`Secret`フォーム値。 なし、[バインド](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)属性の作成時にモデル バインダーが使用されるフィールドを制限すること、`Student`インスタンス<em>、</em>モデル バインダーを取得するよう`Secret`フォーム値と、それを使用作成、`Student`エンティティ インスタンス。 その場合、`Secret` フォーム フィールドに対してハッカーが指定した値はすべて、データベースで更新されます。 次の図は、fiddler ツールを追加する、`Secret`にポストされたフォーム値 (値"OverPost") を含むフィールド。

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)  

    値 "OverPost" は挿入される行の `Secret` プロパティに正常に追加されますが、Web ページがそのプロパティを設定できることは意図したものではありません。

    使用するセキュリティのベスト プラクティスは、`Include`パラメーター、`Bind`属性を*ホワイト リスト*フィールド。 使用することも、`Exclude`パラメーターを*ブラック リスト*を除外するフィールドを選択します。 理由`Include`方が安全ですが、エンティティに新しいプロパティを追加すると、新しいフィールドに自動的にによって保護されていない、`Exclude`一覧。

    回避できます過剰ポスティング編集シナリオでは、まずデータベースからエンティティを読み取りを呼び出すことによって`TryUpdateModel`、明示的に許可されているプロパティ リストを渡します。 これらのチュートリアルで使用される方法です。

    多くの開発者に好まれますポスティングを防ぐために別の方法では、モデル バインドでエンティティ クラスではなくビュー モデルを使用します。 更新するプロパティのみをビュー モデルに含めます。 MVC モデル バインダーが完了すると、ビュー モデルのプロパティを必要に応じてなどのツールを使用して、エンティティのインスタンスにコピー [AutoMapper](http://automapper.org/)します。 Db を使用します。状態を Unchanged に設定し、Property("PropertyName") を設定するエンティティ インスタンスのエントリ。IsModified ビュー モデルに含まれている各エンティティのプロパティを true に設定します。 この方法は、編集シナリオと作成シナリオの両方で利用できます。

    他にも、`Bind`属性、`try-catch`ブロックは、スキャフォールディングされたコードに加えた変更だけです。 派生した例外[DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx)は、変更の保存中にキャッチされた、一般的なエラー メッセージが表示されます。 [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx)例外が場合がありますが原因で、プログラミング エラーではなく、アプリケーションの外部の何かため、ユーザーがもう一度お試しするように求められます。 このサンプルでは実装されていませんが、運用品質のアプリケーションでは例外をログに記録します。 詳細については、「[Monitoring and Telemetry (Building Real-World Cloud Apps with Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log)」(監視とテレメトリ (Azure での実際のクラウド アプリの構築)) の「**Log for insight**」(洞察のためのログ) セクションをご覧ください。

    コードでは、 *Views\Student\Create.cshtml*で見たような*Details.cshtml*ことを除いて、`EditorFor`と`ValidationMessageFor`ヘルパーがではなく、各フィールドに使用される`DisplayFor`. 関連するコードを次に示します。

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml*も含まれています`@Html.AntiForgeryToken()`、効果的な`ValidateAntiForgeryToken`を防ぐため、コント ローラー属性[クロスサイト リクエスト フォージェリ](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)攻撃です。

    変更は必要ありません*Create.cshtml*します。
2. 選択して、ページの実行、**学生**タブとクリックして**新規作成**です。
3. 名前と、無効な日付を入力し、クリックして**作成**エラー メッセージを確認します。

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    これは、既定で作成されるサーバー側の検証です。後のチュートリアルでは、クライアント側検証用コードも生成する属性を追加する方法を示します。 次の強調表示されたコードでモデルの検証チェックを示しています、**作成**メソッド。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]
4. 日付を有効な値に変更し、**[Create]** をクリックして、新しい学生が **[Index]** ページに表示されることを確認します。

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

## <a name="update-the-edit-httppost-method"></a>更新プログラムの編集の HttpPost メソッド

*Controllers\StudentController.cs*、 `HttpGet` `Edit`メソッド (せず 1 つ、`HttpPost`属性) を使用して、`Find`メソッドを選択した取得`Student`見たこととして、エンティティ`Details`メソッド。 このメソッドを変更する必要はありません。

ただし、置換、 `HttpPost` `Edit`アクション メソッドを次のコード。

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

これらの変更を防ぐためにセキュリティのベスト プラクティスを実装する[過剰ポスティング](#overpost)、生成された scaffolder を`Bind`属性およびエンティティ セットに変更されたフラグにモデル バインダーによって作成されたエンティティを追加します。 に、コードが推奨されなく、`Bind`属性が記載されていないフィールド内の既存のデータ消去、`Include`パラメーター。 生成されないように、MVC コント ローラー scaffolder が更新されます、将来、 `Bind` Edit メソッドの属性。

新しいコードを読み取り、既存のエンティティと呼び出し[tryupdatemodel に渡します](https://msdn.microsoft.com/library/system.web.mvc.controller.tryupdatemodel(v=vs.118).aspx)ポストされたフォーム データでのユーザー入力からのフィールドを更新します。 Entity Framework の自動変更のセットの追跡、 [Modified](https://msdn.microsoft.com/library/system.data.entitystate.aspx)エンティティのフラグ。 ときに、 [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)メソッドを呼び出すと、`Modified`フラグによって、データベースの行を更新する SQL ステートメントを作成する Entity Framework。 [同時実行の競合](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)は無視されますと、ユーザーを変更していないものも含め、データベースの行のすべての列が更新されます。 (後のチュートリアルは、同時実行の競合を処理する方法を示していますおよび、データベースで更新する個々 のフィールドのみたい場合は、エンティティを Unchanged に設定と個々 のフィールドを変更します。)。

過剰ポスティングを防ぐために、ベスト プラクティスとして、フィールドの編集 ページで更新可能にするにはでホワイト リストに、`TryUpdateModel`パラメーター。 現在、他に保護しているフィールドはありませんが、モデル バインダーでバインドしたいフィールドをリストに入れておくと、後でデータ モデルにフィールドを追加した場合に、ここでフィールドを明示的に追加するまで、自動的にフィールドを保護できます。

これらの変更の結果として、HttpPost の Edit メソッドのメソッド シグネチャは、HttpGet edit メソッドと同じそのため EditPost メソッドの名前を変更しました。

> [!TIP] 
> 
> **エンティティの状態と、アタッチと SaveChanges メソッド**
> 
> データベース コンテキストは、メモリ内のエンティティがデータベースの対応する行と同期しているかどうかを追跡しており、この情報により、`SaveChanges` メソッドを呼び出したときの処理が決まります。 新しいエンティティを渡す場合など、[追加](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx)メソッドは、エンティティの状態に設定されている`Added`します。 その後呼び出す、 [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)メソッドでは、データベース コンテキストの問題、SQL`INSERT`コマンド。
> 
> エンティティは、のいずれかである可能性があります、[状態に従って](https://msdn.microsoft.com/library/system.data.entitystate.aspx):
> 
> - `Added`。 エンティティは、データベースにまだ存在しません。 `SaveChanges`メソッドを発行する必要があります、`INSERT`ステートメント。
> - `Unchanged`。 `SaveChanges` メソッドはこのエンティティに対し何も行う必要はありません。 データベースからエンティティを読み取ると、エンティティはこの状態で開始します。
> - `Modified`。 エンティティのプロパティ値の一部またはすべてが変更されています。 `SaveChanges`メソッドを発行する必要があります、`UPDATE`ステートメント。
> - `Deleted`。 エンティティには削除のマークが付けられています。 `SaveChanges`メソッドを発行する必要があります、`DELETE`ステートメント。
> - `Detached`。 エンティティはデータベース コンテキストによって追跡されていません。
> 
> デスクトップ アプリケーションにおいて、通常、状態の変更は自動的に設定されます。 デスクトップのタイプのアプリケーションでは、エンティティを読み取るし、そのプロパティ値の一部を変更します。 そのエンティティの状態は自動的に `Modified` に変更されます。 その後呼び出す`SaveChanges`、Entity Framework には、SQL が生成されます`UPDATE`実際に変更したプロパティのみを更新するステートメント。
> 
> この継続的なシーケンスでは、web アプリ、接続が確立は許可されていません。 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)を読み取るエンティティは、ページが表示された後で破棄されます。 ときに、 `HttpPost` `Edit`アクション メソッドが呼び出され、新しい要求が行われるの新しいインスタンスがある、 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)、エンティティの状態を手動で設定する必要があるため、`Modified.`を呼び出すと、 `SaveChanges`、Entity Framework は、コンテキストには、変更したプロパティを特定する方法があるないので、データベースの行のすべての列を更新します。
> 
> 場合は、SQL`Update`ステートメントは、ユーザーが実際に変更されたフィールドのみを更新するため、使用できるようにする場合に、(非表示フィールド) などのいくつかの方法で元の値を保存することができます、 `HttpPost` `Edit`メソッドが呼び出されます。 作成し、 `Student` 、呼び出し元の値を使用して、エンティティ、`Attach`メソッドは、エンティティの元のバージョンでは、エンティティの値を新しい値に更新を呼び出して`SaveChanges.`詳細については、次を参照してください[。エンティティの状態および SaveChanges](https://msdn.microsoft.com/data/jj592676)と[ローカル データ](https://msdn.microsoft.com/data/jj592872)MSDN データ デベロッパー センターでします。


HTML および Razor コード*Views\Student\Edit.cshtml*で見たような*Create.cshtml*、し、変更は必要ありません。

選択して、ページの実行、**学生** タブをクリックして、**編集**ハイパーリンク。

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

データをいくつか変更し、**[Save]** をクリックします。 Index ページで、変更されたデータを表示します。

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-delete-page"></a>Delete ページの更新

*Controllers\StudentController.cs*、テンプレート コードを`HttpGet``Delete`メソッドは、`Find`メソッドを取得、選択した`Student`でエンティティを見た、`Details`と`Edit`メソッド。 ただし、`SaveChanges` の呼び出しが失敗したときのカスタム エラー メッセージを実装するには、何らかの機能とその対応するビューをこのメソッドに追加します。

更新および作成操作で見たように、削除操作にも 2 つのアクション メソッドが必要です。 GET 要求に応答して呼び出されるメソッドには、ユーザーを承認または削除操作をキャンセルする機会を提供するビューが表示されます。 ユーザーが操作を承認すると、POST 要求が作成されます。 その場合、 `HttpPost` `Delete`メソッドが呼び出され、そのメソッドが実際には、削除操作を実行します。

追加、`try-catch`へのブロック、 `HttpPost` `Delete`データベースが更新されたときに発生するエラーを処理するメソッド。 エラーが発生する場合、 `HttpPost` `Delete`メソッドの呼び出し、 `HttpGet` `Delete`メソッドは、エラーが発生したことを示すパラメーターを渡します。 `HttpGet Delete`メソッドが再び表示し、確認ページと、エラー メッセージを取り消すか、もう一度お試しする機会をユーザーに提供します。

1. 置換、 `HttpGet` `Delete`アクション メソッドを次のコードは、エラー報告を管理します。 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    このコードは受け取ります、[省略可能なパラメーター](https://msdn.microsoft.com/library/dd264739.aspx)変更の保存に失敗した後、メソッドが呼び出されたかどうかを示します。 このパラメーターは`false`ときに、 `HttpGet` `Delete`以前失敗せずにメソッドが呼び出されます。 呼び出されたとき、 `HttpPost` `Delete`メソッド データベース更新エラーへの応答で、パラメーターが`true`エラー メッセージは、ビューに渡されます。
2. 置換、 `HttpPost` `Delete`アクション メソッド (名前付き`DeleteConfirmed`) を次のコードでは、実際の削除操作を実行して、データベース更新エラーをキャッチします。

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

     このコードでは、選択したエンティティを取得します。 を呼び出して、[削除](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx)エンティティの状態を設定するメソッドを`Deleted`します。 ときに`SaveChanges`を呼び出すと、SQL`DELETE`コマンドが生成されます。 また、アクション メソッドの名前を `DeleteConfirmed` から `Delete` に変更しています。 という名前のスキャフォールディングされたコード、 `HttpPost` `Delete`メソッド`DeleteConfirmed`提供する、`HttpPost`メソッドに一意のシグネチャ。 (CLR にオーバー ロードされたメソッドが、さまざまなメソッド パラメーターが必要です)。シグネチャが一意ではこれでは、MVC 規則に従うし、に同じ名前を使用、`HttpPost`と`HttpGet`メソッドを削除します。

     呼び出すコードの行を置き換えることで、行を取得する不必要な SQL クエリの問題を回避できれば量の多いアプリケーションでパフォーマンスの向上が優先度の場合、`Find`と`Remove`メソッドを次のコードで。

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

     このコードをインスタンス化、`Student`主キーの値のみを使用してエンティティ、エンティティの状態を設定および`Deleted`。 エンティティを削除するために Entity Framework に必要なものは主キーの値だけです 

     前述のように、 `HttpGet` `Delete`メソッド データは削除されません。 要求 (またはそのさらに言えば、任意の編集操作を実行する操作、または他のデータを変更する操作を作成)、GET に対する応答で削除操作を実行するセキュリティ上のリスクを作成します。 詳細については、次を参照してください。 [ASP.NET MVC ヒントと 46: セキュリティ ホールを作成するため、削除のリンクを使用しない](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)Stephen Walther のブログ。
3. *Views\Student\Delete.cshtml*、間にエラー メッセージを追加、`h2`見出しと`h3`見出しで、次の例に示すようにします。

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

     選択して、ページの実行、**学生**タブとクリックすると、**削除**ハイパーリンク。

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)
4. **[Delete]** をクリックします。 削除された学生を含まない [Index] ページが表示されます  (エラー処理のアクションにコードの例が表示されます、[同時実行チュートリアル](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md))。

## <a name="closing-database-connections"></a>データベース接続の終了

データベース接続を閉じ、リソースを保持します。 できるだけ早く解放する、終了するときに、コンテキスト インスタンスを破棄します。 スキャフォールディングされたコードは、理由は、 [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx)メソッドの末尾に、`StudentController`クラス*StudentController.cs*次の例のように。

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

基本`Controller`クラスが既に実装、`IDisposable`インターフェイスのため、このコードでは、上書きを単に追加します、`Dispose(bool)`コンテキスト インスタンスを明示的に破棄するメソッド。

<a id="transactions"></a>
## <a name="handling-transactions"></a>トランザクションの処理

既定では、Entity Framework はトランザクションを暗黙的に実装します。 複数の行またはテーブルに変更を加えるし、呼び出しのシナリオで`SaveChanges`、Entity Framework は自動的に、すべての変更は成功か、またはすべてが失敗することを確認します。 一部の変更が完了した後でエラーが発生した場合、それらの変更は自動的にロールバックされます。 必要があります詳細に制御が - たとえば、--トランザクションでは Entity Framework の外部で実行する操作を追加する場合のシナリオを参照してください[トランザクション操作](https://msdn.microsoft.com/data/dn456843)msdn です。

## <a name="summary"></a>まとめ

単純な CRUD 操作を実行するページの完全なセットがあるようになりました`Student`エンティティ。 MVC ヘルパーを使用すると、データ フィールドの UI 要素を生成します。 MVC ヘルパーの詳細については、次を参照してください。 [HTML ヘルパーを使用してフォームをレンダリング](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx)(ページは MVC 3 は、MVC 5 にも引き続き該当です)。

次のチュートリアルでは、並べ替えとページングを追加することで、インデックス ページの機能を拡張します。

このチュートリアルの立った方法で改善できましたフィードバックを送信してください。 また、新しいトピックを要求することもできます。[表示 Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)します。

その他の Entity Framework リソースへのリンクが記載[ASP.NET データ アクセス - 推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)します。

> [!div class="step-by-step"]
> [前へ](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [次へ](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
