---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: ASP.NET MVC アプリケーションで Entity Framework で基本的な CRUD 機能を実装する |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/09/2015
ms.topic: article
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 14f5143bb5086890d4a2f2fb3b98f1be88a549a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876645"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>ASP.NET MVC アプリケーションで Entity Framework で基本的な CRUD 機能を実装します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。


前のチュートリアルを格納し、Entity Framework と SQL Server LocalDB を使用してデータを表示する MVC アプリケーションを作成します。 このチュートリアルでを確認およびカスタマイズしたりする、CRUD (作成、読み取り、更新、削除) MVC のスキャフォールディング自動的に作成するコント ローラーとビューのコード。

> [!NOTE]
> コントローラーとデータ アクセス層の間に抽象化レイヤーを作成するためにリポジトリ パターンを実装することは、よく行われることです。 この一連のチュートリアルが複雑にならないようにし、Entity Framework 自体の使い方に集中できるように、チュートリアルではリポジトリは使われていません。 リポジトリを実装する方法については、次を参照してください。、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。


このチュートリアルでは、次の web ページを作成します。

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>詳細ページを作成します。

受講者のスキャフォールディング コード`Index`ページは省略して、`Enrollments`プロパティ、そのプロパティには、コレクションが格納されているためです。 `Details`ページを HTML テーブルで、コレクションの内容を表示します。

 *Controllers\StudentController.cs*、用のアクション メソッド、`Details`使用方法を表示、[検索](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx)、1 つを取得する方法を`Student`エンティティです。 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

キーの値は、メソッドに、`id`パラメーターを起源と*データをルーティング*で、**詳細**インデックス ページのハイパーリンクのです。

### <a name="tip-route-data"></a>ヒント:**データのルーティング**

ルート データは、ルーティング テーブルに指定された URL セグメント内のモデル バインダーにあるデータです。 たとえば、既定のルートを指定して`controller`、 `action`、および`id`セグメント。

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

既定のルートでは、次の URL では、マップ`Instructor`として、 `controller`、`Index`として、 `action` 1 を`id`です。 これらは、ルート データの値。

`http://localhost:1230/Instructor/Index/1?courseID=2021`

"? courseID 2021 ="クエリ文字列値です。 モデル バインダーは、合格した場合でも機能、`id`クエリ文字列値として。

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Url がによって作成された`ActionLink`Razor ビュー内のステートメント。 次のコードで、`id`ためパラメーターに既定のルートと一致する`id`ルート データを追加します。

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

次のコードに`courseID`クエリ文字列として追加されたために、既定のルートのパラメーターと一致しません。

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]


1. 開いている*Views\Student\Details.cshtml*です。 使用して、各フィールドが表示されます、`DisplayFor`ヘルパーに渡し、次の例で示すようにします。

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]
2. 後に、`EnrollmentDate`フィールドと、終了する直前に`</dl>`タグ、コードを追加、強調表示された登録の一覧を表示する次の例で示すようにします。

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    コードを貼り付けた後でコードのインデントが乱れた場合は、Ctrl + D + K キーを押して修正します。

    このコードは、`Enrollments` ナビゲーション プロパティ内のエンティティをループ処理します。 各`Enrollment`エンティティ プロパティには、コースのタイトルとグレードが表示されます。 コース タイトルが取得される、`Course`に格納されているエンティティ、`Course`のナビゲーション プロパティ、`Enrollments`エンティティです。 すべてのデータがデータベースから自動的に取得が必要な場合です。 (つまり、使用する遅延読み込みは、ここです。 指定しなかった*一括読み込み*の`Courses`ナビゲーション プロパティ、受講者を取得したのと同じクエリ内で、登録が取得されませんでした。 代わりに、最初にアクセスすると、`Enrollments`ナビゲーション プロパティ、新しいクエリがデータベースに送信データを取得します。 詳細を読み取ることができます遅延読み込みと一括読み込みでは、[関連のデータの読み取り](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズの後でチュートリアルです)。
3. 選択して、ページの実行、**受講者** タブをクリックし、**詳細**Alexander Carson へのリンク。 (Details.cshtml ファイルが開いているときに ctrl キーを押しながら f5 キーを押すと、表示されます、HTTP 400 エラーの詳細 ページを実行しようとする Visual Studio が、受講者を表示するを指定するためのリンクからに達したされなかったためです。 その場合、だけ URL から「生徒/詳細」を削除し、もう一度やり直してまたはブラウザーを閉じて、プロジェクトを右クリックしておよび をクリックして**ビュー**、クリックして**ブラウザーで表示**)。

    選んだ受講者のコースとグレードの一覧が表示されます。

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="update-the-create-page"></a>更新プログラムの作成 ページ

1. *Controllers\StudentController.cs*、置換、`HttpPost``Create`アクション メソッドを次のコードを追加する、`try-catch`をブロックし、削除`ID`から、 [Bind 属性](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)のスキャフォールディング メソッド:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    このコードを追加、`Student`に ASP.NET MVC モデル バインダーによって作成されたエンティティ、`Students`エンティティが設定され、データベースに変更を保存します。 (*モデル バインダー*変換ポストされたフォーム値の CLR 型とパラメーター内のアクション メソッドに渡して、モデル バインダー; フォームによって送信されたデータを操作するために簡単には、ASP.NET MVC 機能を指します。 この、モデル バインダーをインスタンス化、`Student`からプロパティを使用してエンティティの値、`Form`コレクション)。

    削除する`ID`属性、バインドから`ID`は、SQL Server は、行が挿入されると自動的に設定されますが、主キーの値。 ユーザーからの入力が設定されていない、`ID`値。

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>セキュリティ警告 -、`ValidateAntiForgeryToken`属性を防止[クロスサイト リクエスト フォージェリ](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)攻撃です。 対応する必要があります`Html.AntiForgeryToken()`ステートメントで、ビューでは、後で表示されます。

    `Bind`属性を防ぐために 1 つの方法は、*過剰転記*でシナリオを作成します。 たとえば、`Student`エンティティが含まれています、`Secret`プロパティを設定するには、この web ページをたくないです。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    存在しない場合でも、 `Secret` web ページで、ハッカー フィールドでしたツールを使用してなど[fiddler](http://fiddler2.com/home)、または書き込みを行ういくつか JavaScript、`Secret`値を作成します。 なし、[バインド](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)属性の作成時にモデル バインダーが使用されるフィールドを制限する、`Student`インスタンス<em>、</em>モデル バインダーがピックアップされる`Secret`値を作成し、それを使用作成、`Student`エンティティのインスタンス。 その場合、`Secret` フォーム フィールドに対してハッカーが指定した値はすべて、データベースで更新されます。 次の図は、fiddler ツールを追加する、`Secret`ポストされたフォーム値へのフィールド (に値"OverPost") です。

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)  

    値 "OverPost" は挿入される行の `Secret` プロパティに正常に追加されますが、Web ページがそのプロパティを設定できることは意図したものではありません。

    使用するセキュリティのベスト プラクティスは、`Include`を持つパラメーター、`Bind`属性を*ホワイト リスト*フィールドです。 使用することも、`Exclude`パラメーターを*ブラック リスト*フィールドを除外します。 理由`Include`方が安全ですが、エンティティに新しいプロパティを追加すると、新しいフィールドに自動的にによって保護されていない、 `Exclude`  ボックスの一覧です。

    防ぐことができます overposting 編集のシナリオでは、まず、データベースからエンティティを読み取り、呼び出すことで、 `TryUpdateModel`、明示的に許可されているプロパティ リストに渡します。 これらのチュートリアルで使用する方法です。

    多くの開発者に好ま overposting を防ぐために別の方法では、モデル バインディングでエンティティ クラスではなく、ビュー モデルを使用します。 更新するプロパティのみをビュー モデルに含めます。 MVC モデル バインダーが終了すると、モデル プロパティの表示を必要に応じてなどのツールを使用して、エンティティ インスタンスにコピー [AutoMapper](http://automapper.org/)です。 Db を使用します。エンティティのエントリは、インスタンスの状態を Unchanged に設定して、Property("PropertyName") を設定します。IsModified ビュー モデルに含まれる各エンティティ プロパティを true に設定します。 この方法は、編集シナリオと作成シナリオの両方で利用できます。

    以外の場合、`Bind`属性、`try-catch`ブロックはスキャフォールディング コードに加えたのみ変更します。 派生した例外[DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx)は、変更の保存中にキャッチされ、汎用的なエラー メッセージが表示されます。 [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx)例外が場合がありますが原因で、プログラミング エラーではなく、アプリケーションを外部の何かため、ユーザーが再試行することをお勧めします。 このサンプルでは実装されていませんが、運用品質のアプリケーションでは例外をログに記録します。 詳細については、「[Monitoring and Telemetry (Building Real-World Cloud Apps with Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log)」(監視とテレメトリ (Azure での実際のクラウド アプリの構築)) の「**Log for insight**」(洞察のためのログ) セクションをご覧ください。

    内のコード*Views\Student\Create.cshtml*で学習したような*Details.cshtml*する点を除いて、`EditorFor`と`ValidationMessageFor`ではなく、各フィールドの使用はヘルパー`DisplayFor`. 関連するコードを次に示します。

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml*も含まれています`@Html.AntiForgeryToken()`で機能する、`ValidateAntiForgeryToken`を防ぐため、コント ローラー属性[クロスサイト リクエスト フォージェリ](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)攻撃です。

    変更は必要ありません*Create.cshtml*です。
2. 選択して、ページの実行、**受講者** タブをクリックして**新規作成**です。
3. 名前と無効な日付を入力し、をクリックして**作成**エラー メッセージを表示します。

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    これは、既定で作成されるサーバー側の検証です。後のチュートリアルでは、クライアント側検証用コードも生成する属性を追加する方法を示します。 次の強調表示されたコードで、モデルの検証チェックを示しています、**作成**メソッドです。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]
4. 日付を有効な値に変更し、**[Create]** をクリックして、新しい学生が **[Index]** ページに表示されることを確認します。

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

## <a name="update-the-edit-httppost-method"></a>Update 編集 HttpPost メソッド

*Controllers\StudentController.cs*、 `HttpGet` `Edit`メソッド (ことがなく 1 つ、`HttpPost`属性) を使用して、 `Find` 、選択した取得する方法を`Student`を説明したのと、エンティティ`Details`メソッドです。 このメソッドを変更する必要はありません。

ただし、置換、 `HttpPost` `Edit`アクション メソッドを次のコード。

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

これらの変更を防ぐためにセキュリティのベスト プラクティスを実装する[過剰ポスティング](#overpost)、生成された scaffolder、`Bind`属性を更新日時を示すフラグを設定、エンティティにモデル バインダーによって作成されたエンティティを追加します。 のコードは推奨されなく、`Bind`属性に表示されていないフィールド内の既存のデータをクリアして、`Include`パラメーター。 これを生成しないようにの MVC コント ローラー scaffolder の更新、将来、`Bind`編集メソッドの属性です。

新しいコードを読み取り、既存のエンティティと呼び出し[TryUpdateModel](https://msdn.microsoft.com/library/system.web.mvc.controller.tryupdatemodel(v=vs.118).aspx)ポストされたフォーム データでのユーザー入力からのフィールドを更新します。 Entity Framework の自動変更のセットを追跡、 [Modified](https://msdn.microsoft.com/library/system.data.entitystate.aspx)エンティティのフラグ。 ときに、 [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)メソッドが呼び出されると、`Modified`フラグにより、Entity Framework データベースの行を更新する SQL ステートメントを作成します。 [同時実行の競合](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)は無視し、ユーザーが変更されなかったものも含め、データベースの行のすべての列が更新されます。 (後のチュートリアルは、同時実行の競合を処理する方法を示していますとする場合のみ、データベースで更新する個別のフィールド、エンティティを Unchanged に設定し、設定できます個々 のフィールドを変更します。)。

Overposting を回避するベスト プラクティスとして、フィールドの編集 ページで更新可能にするは、ホワイト リストに登録で、`TryUpdateModel`パラメーター。 現在、他に保護しているフィールドはありませんが、モデル バインダーでバインドしたいフィールドをリストに入れておくと、後でデータ モデルにフィールドを追加した場合に、ここでフィールドを明示的に追加するまで、自動的にフィールドを保護できます。

HttpPost Edit メソッドのメソッド シグネチャでは、これらの変更の結果としては、HttpGet 編集メソッドと同じしたがって EditPost メソッドの名前を変更しました。

> [!TIP] 
> 
> **エンティティの状態と、アタッチと SaveChanges メソッド**
> 
> データベース コンテキストは、メモリ内のエンティティがデータベースの対応する行と同期しているかどうかを追跡しており、この情報により、`SaveChanges` メソッドを呼び出したときの処理が決まります。 新しいエンティティを渡す場合など、[追加](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx)メソッドに設定されているエンティティの状態を`Added`です。 その後呼び出す、 [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)メソッド、データベース コンテキストの問題、SQL`INSERT`コマンド。
> 
> 1 つのエンティティがあります、[次の状態](https://msdn.microsoft.com/library/system.data.entitystate.aspx):
> 
> - `Added`。 エンティティは、データベースにまだ存在しません。 `SaveChanges`メソッドを発行する必要があります、`INSERT`ステートメントです。
> - `Unchanged`。 `SaveChanges` メソッドはこのエンティティに対し何も行う必要はありません。 データベースからエンティティを読み取ると、エンティティはこの状態で開始します。
> - `Modified`。 エンティティのプロパティ値の一部またはすべてが変更されています。 `SaveChanges`メソッドを発行する必要があります、`UPDATE`ステートメントです。
> - `Deleted`。 エンティティには削除のマークが付けられています。 `SaveChanges`メソッドを発行する必要があります、`DELETE`ステートメントです。
> - `Detached`。 エンティティはデータベース コンテキストによって追跡されていません。
> 
> デスクトップ アプリケーションにおいて、通常、状態の変更は自動的に設定されます。 デスクトップの種類のアプリケーションでは、エンティティの読み取りし、そのプロパティ値の一部を変更します。 そのエンティティの状態は自動的に `Modified` に変更されます。 その後呼び出す`SaveChanges`、Entity Framework は、SQL を生成`UPDATE`を変更して実際のプロパティのみを更新するステートメント。
> 
> Web アプリの切断されているため、この連続した一連の許可されていません。 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)を読み取る、ページが表示されたら、エンティティが破棄されています。 ときに、 `HttpPost` `Edit`アクション メソッドが呼び出される、新しい要求が行われ、の新しいインスタンスを持つ、 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)にエンティティの状態を手動で設定する必要があるため、`Modified.`を呼び出すと、 `SaveChanges`、Entity Framework は、コンテキストに変更するプロパティを特定する方法があるないために、データベースの行のすべての列を更新します。
> 
> 場合は、SQL`Update`ステートメントをユーザーが実際に変更されたフィールドのみを更新するため、使用できるようにするときに、何らかの方法 (非表示フィールドなど) で元の値を保存することができます、 `HttpPost` `Edit`メソッドが呼び出されます。 作成し、 `Student` 、呼び出し元の値を使用して、エンティティ、`Attach`メソッドをその元のバージョン、エンティティのエンティティの値を新しい値に更新およびを呼び出す`SaveChanges.`詳細については、次を参照してください[。エンティティの状態と SaveChanges](https://msdn.microsoft.com/data/jj592676)と[ローカル データ](https://msdn.microsoft.com/data/jj592872)MSDN データ デベロッパー センターにします。


HTML および Razor コード*Views\Student\Edit.cshtml*で学習したような*Create.cshtml*、し、変更は必要ありません。

選択して、ページの実行、**受講者** タブをクリックして、**編集**ハイパーリンクです。

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

データをいくつか変更し、**[Save]** をクリックします。 [インデックス] ページで変更されたデータを表示します。

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-delete-page"></a>ページの削除 を更新

*Controllers\StudentController.cs*、テンプレート コードを`HttpGet``Delete`メソッドを使用、 `Find` 、選択した取得する方法を`Student`エンティティも従って、`Details`と`Edit`メソッドです。 ただし、`SaveChanges` の呼び出しが失敗したときのカスタム エラー メッセージを実装するには、何らかの機能とその対応するビューをこのメソッドに追加します。

更新および作成操作で見たように、削除操作にも 2 つのアクション メソッドが必要です。 GET 要求に応答して呼び出されるメソッドは、ユーザーを承認または削除操作をキャンセルする機会を提供するビューを表示します。 ユーザーが操作を承認すると、POST 要求が作成されます。 発生したときに、 `HttpPost` `Delete`メソッドが呼び出され、そのメソッドが実際には、削除操作を実行し、します。

追加、`try-catch`へのブロック、 `HttpPost` `Delete`データベースが更新されたときに発生したエラーを処理するメソッド。 エラーが発生する場合、 `HttpPost` `Delete`メソッドの呼び出し、 `HttpGet` `Delete`メソッド、エラーが発生したことを示すパラメーターを渡します。 `HttpGet Delete`メソッドは、取り消すか、もう一度実行する機会をユーザーに提供のエラー メッセージと共に確認 ページを再表示します。

1. 置換、 `HttpGet` `Delete`アクション メソッドを次のコードは、エラー報告を管理します。 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    このコードが受け付ける、[省略可能なパラメーター](https://msdn.microsoft.com/library/dd264739.aspx)変更を保存する障害の発生後、メソッドが呼び出されたかどうかを示すです。 このパラメーターは`false`ときに、 `HttpGet` `Delete`以前失敗せずにメソッドが呼び出されます。 呼び出されたとき、 `HttpPost` `Delete`データベース更新エラーへの応答でメソッドをパラメーターが`true`エラー メッセージは、ビューに渡されます。
2. 置換、 `HttpPost` `Delete`アクション メソッド (名前付き`DeleteConfirmed`) を次のコードを実際の削除操作を実行し、データベース更新エラーをキャッチします。

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

     このコードは、選択したエンティティを取得し、呼び出して、[削除](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx)エンティティの状態を設定するメソッドを`Deleted`です。 ときに`SaveChanges`が呼び出されると、SQL`DELETE`コマンドが生成されます。 また、アクション メソッドの名前を `DeleteConfirmed` から `Delete` に変更しています。 名前付きスキャフォールディング コード、 `HttpPost` `Delete`メソッド`DeleteConfirmed`を与える、`HttpPost`メソッド固有の署名。 (CLR には、別のメソッドのパラメーターを持つオーバー ロードされたメソッドが必要があります)。署名が一意ではこれでは、MVC 規則のオプションを使用と同じ名前を使用、`HttpPost`と`HttpGet`メソッドを削除します。

     不要な SQL クエリを呼び出すコードの行を置き換えることで、行を取得することで回避できます量の多いアプリケーションのパフォーマンスの向上がある場合、優先度、`Find`と`Remove`メソッドを次のコード。

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

     このコードをインスタンス化、`Student`主キーの値のみを使用して、エンティティ、エンティティの状態に設定し、`Deleted`です。 エンティティを削除するために Entity Framework に必要なものは主キーの値だけです 

     前述のように、 `HttpGet` `Delete`データは削除されません。 要求 (またはに言えば、任意の編集操作を実行する操作、またはデータが変更されたその他の操作を作成します)、GET に対する応答で delete 操作の実行、セキュリティ上のリスクを作成します。 詳細については、次を参照してください。 [ASP.NET MVC ヒント #46-セキュリティ ホールを作成するため、削除のリンクを使用しない](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)Stephen Walther ブログ。
3. *Views\Student\Delete.cshtml*との間のエラー メッセージを追加、`h2`見出しと`h3`見出しで、次の例に示すように。

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

     選択して、ページの実行、**受講者** タブをクリックし、**削除**ハイパーリンク。

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)
4. **[Delete]** をクリックします。 削除された学生を含まない [Index] ページが表示されます  (処理コードでの操作でエラーの例が表示されます、[同時実行のチュートリアル](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md))。

## <a name="closing-database-connections"></a>データベース接続の終了

データベース接続を閉じ、できるだけ早くを保持するリソースを解放する、関連付けしたらコンテキストのインスタンスを破棄します。 スキャフォールディングのコードは、理由は、 [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx)メソッドの最後に、`StudentController`クラス*StudentController.cs*次の例のように。

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

基本`Controller`既にクラスが実装する、`IDisposable`インターフェイスのため、このコードは単純にする上書きを追加、`Dispose(bool)`メソッドを明示的にコンテキストのインスタンスを破棄します。

<a id="transactions"></a>
## <a name="handling-transactions"></a>トランザクションの処理

既定では、Entity Framework はトランザクションを暗黙的に実装します。 ここで複数の行またはテーブルを変更してを呼び出すシナリオで`SaveChanges`、Entity Framework は自動的に、すべての変更が成功するか、またはすべて失敗したことを確認します。 一部の変更が完了した後でエラーが発生した場合、それらの変更は自動的にロールバックされます。 必要な複数を制御する--など--トランザクションでは Entity Framework の外部で実行する操作を追加する場合のシナリオを参照してください[トランザクションを使用](https://msdn.microsoft.com/data/dn456843)msdn です。

## <a name="summary"></a>まとめ

単純な CRUD 操作を実行するページの完全なセットがあるようになりました`Student`エンティティです。 MVC ヘルパーを使用すると、データ フィールド用の UI 要素を生成します。 MVC ヘルパーの詳細については、次を参照してください。 [HTML ヘルパーを使用してフォームをレンダリング](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx)(ページが MVC 3 がは MVC 5 の場合にも引き続き該当)。

次のチュートリアルでは、並べ替えとページングを追加することでインデックス ページの機能を展開します。

このチュートリアルをリンクする方法と、何を改善にフィードバックを送信してください。 新しいトピックを要求することもできます。 [Me 方法でコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)です。

その他の Entity Framework リソースへのリンクは含まれて[ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。

> [!div class="step-by-step"]
> [前へ](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [次へ](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
