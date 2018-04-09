---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Async および Entity Framework、ASP.NET MVC アプリケーションでのストアド プロシージャ |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 84cf427c7da7905444568ac34534e9ed98a7d8c8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Async および Entity Framework、ASP.NET MVC アプリケーションでのストアド プロシージャ
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。


前のチュートリアルでは、読み取りし、同期プログラミング モデルを使用してデータを更新する方法について学習しました。 このチュートリアルでは、非同期プログラミング モデルを実装する方法について参照してください。 非同期コードには、サーバーのリソースを効率的に使用可能になったためにでパフォーマンスが向上するアプリケーションが役立ちます。

このチュートリアルでは、insert、update、および削除操作、エンティティのストアド プロシージャを使用する方法も表示されます。

最後に、と共に展開した最初の時刻以降に実装しているデータベースの変更のすべての Azure にアプリケーションを再展開します。

以下の図は、使用するページの一部を示しています。

![部門ページ](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![部門を作成します。](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>非同期コードの必要はありません。

Web サーバーでは、利用できるスレッド数に限りがあります。負荷が高い状況では、利用できるスレッドが全部使われる可能性があります。 その場合、スレッドが解放されるまでサーバーは新しい要求を処理できません。 同期コードの場合、たくさんのスレッドが関連付けられていても、I/O の完了を待っているため、実際には何の作業も行っていないということがあります。 非同期コードの場合、あるプロセスが I/O の完了を待っているとき、多の要求の処理にサーバーが利用できるようにそのスレッドが解放されます。 その結果、非同期のコードより効率的に使用し、遅延なしのより多くのトラフィックを処理するサーバーを有効にするサーバーのリソースが有効です。

.NET の以前のバージョンで作成および非同期コードのテストが複雑で、エラーが発生しやすく、デバッグが困難にします。 .NET 4.5、記述、テスト、および非同期コードのデバッグはずっと簡単に理由がない限り、非同期コードを記述する、一般にする必要があります。 非同期のコードは少量のオーバーヘッドを導入がトラフィックが少ない場合は、パフォーマンスに影響はごくわずかであり、中に大量のトラフィックの場合、潜在的なパフォーマンスが向上大きくします。

非同期プログラミングの詳細については、次を参照してください。[呼び出しがブロックされないようにする非同期のサポートを使用する .NET 4.5 の](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async)します。

## <a name="create-the-department-controller"></a>部署コント ローラーを作成します。

部門コント ローラーが以前コント ローラーで、この時間以外で実行したのと同様の選択を作成、**を使用して非同期コント ローラー**チェック ボックスを操作します。

![部門コント ローラーのスキャフォールディング](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

次の点には同期コードに追加された内容を表示する、`Index`メソッドを非同期になります。

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

非同期的に実行する Entity Framework データベース クエリを有効にする 4 つの変更が適用されました。

- メソッドが付いて、`async`キーワード、メソッド本体の各部分のコールバックを生成して自動的に作成するコンパイラに指示する、`Task<ActionResult>`返されるオブジェクト。
- 戻り値の型がから変更された`ActionResult`に`Task<ActionResult>`です。 `Task<T>`型が型の結果で進行中の作業を表す`T`です。
- `await`キーワードは、web サービスの呼び出しに適用されました。 コンパイラは、このキーワードを見て、バック グラウンドでそのメソッドに分割 2 つの部分です。 最初の部分は、非同期的に開始される操作を終了します。 2 番目の部分は、操作が完了したときに呼び出されるコールバック メソッドに配置されます。
- 非同期バージョンの`ToList`拡張メソッドが呼び出されました。

理由は、`departments.ToList`ステートメントを変更しない場合は、`departments = db.Departments`ステートメントか。 クエリまたはコマンドのデータベースに送信されるステートメントだけが非同期的に実行されることです。 `departments = db.Departments`ステートメントがクエリを設定するが、まで、クエリが実行されない、`ToList`メソッドが呼び出されます。 したがって、のみ、`ToList`メソッドが非同期的に実行します。

`Details`メソッドおよび`HttpGet``Edit`と`Delete`メソッド、`Find`メソッドは非同期的に実行を取得するメソッドになるように、データベースに送信されるクエリが発生しました。

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

`Create`、 `HttpPost Edit`、および`DeleteConfirmed`メソッドでは、`SaveChanges`メソッドの呼び出しを実行するコマンドの原因となるなどのステートメントではない`db.Departments.Add(department)`のみ変更するメモリ内のエンティティが発生します。

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

開いている*Views\Department\Index.cshtml*、テンプレート コードを次のコードに置き換えます。

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

管理者名を右に移動このコードは、部門をインデックスからタイトルを変更し、管理者の完全な名前を提供します。

作成、削除、詳細については、ビューの編集、およびのキャプションを変更、`InstructorID`フィールドを"Administrator"コース ビューで、部門名 フィールドを"Department"に変更したのと同様にします。

作成および編集には、ビューは、次のコードを使用します。

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

削除と詳細ビューで、次のコードを使用します。

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

アプリケーションを実行し、**部門**タブです。

![部門ページ](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

すべてが他のコント ローラーのものと同じ機能が、このコント ローラーで、すべての SQL クエリは非同期に実行します。

Entity framework 非同期プログラミングを使用する場合の注意すべき点がいくつか:

- 非同期コードは、スレッド セーフではありません。 つまり、つまり、しないでください、同じコンテキスト インスタンスを使用して並列に複数の操作を実行します。
- 非同期コードのパフォーマンス上の利点を最大限に活用する場合、(ページングなどのために) ライブラリ パッケージを利用しているのであれば、それがクエリをデータベースに送信させる Entity Framework メソッドを呼び出す場合、非同期を利用する必要があります。

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>挿入、更新、および削除のストアド プロシージャを使用します。

一部の開発者と Dba は、データベースへのアクセスのストアド プロシージャの使用を優先します。 以前のバージョンの Entity Framework によってストアド プロシージャを使用してデータを取得できます[生 SQL クエリを実行する](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)、更新操作のためのストアド プロシージャを使用する EF を指示することはできませんが、します。 EF 6 ストアド プロシージャを使用して Code First の構成が容易になります。

1. *DAL\SchoolContext.cs*、強調表示されたコードを追加、`OnModelCreating`メソッドです。

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    このコードでは、Entity Framework を指示挿入、使用するストアド プロシージャに、更新、および delete 操作を`Department`エンティティです。
2. パッケージの管理コンソールで、次のコマンドを入力します。

    `add-migration DepartmentSP`

    開いている*移行\&lt; タイムスタンプ&gt;\_DepartmentSP.cs*でコードを確認する、`Up`を作成するメソッドの挿入、更新、および Delete ストアド プロシージャ。

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. パッケージの管理コンソールで、次のコマンドを入力します。

     `update-database`
4. アプリケーションをデバッグ モードで実行 をクリックして、**部門** タブをクリックして**新規作成**です。
5. 新しい部門の場合は、データを入力し、クリックして**作成**です。

     ![部門を作成します。](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
6. Visual Studio でのログを確認、**出力**を新しい部門行を挿入するストアド プロシージャが使用されたことを表示するウィンドウです。

     ![部門挿入 SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

コードは、最初に格納されている既定のプロシージャ名を作成します。 既存のデータベースを使用している場合は、データベースで既に定義されているストアド プロシージャを使用するためにストアド プロシージャ名をカスタマイズする必要があります。 実行する方法については、次を参照してください。 [Entity Framework コード最初挿入/更新/削除ストアド プロシージャ](https://msdn.microsoft.com/data/dn468673)です。

ストアド プロシージャは生成された新機能をカスタマイズする場合は、移行のためスキャフォールディング コードを編集することができます`Up`ストアド プロシージャを作成するメソッド。 このようにして、変更が反映されるたびに移行が実行され、展開後に、実稼働環境で移行を自動的に実行すると、実稼働データベースに適用されます。

前の移行で作成された既存のストアド プロシージャを変更する場合は、空白の移行を生成する Add-migration コマンドを使用してし、を呼び出すコードを手動で作成、 [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx)メソッド.

## <a name="deploy-to-azure"></a>Azure への配置します。

このセクションでは、省略可能な完了している必要があります**Azure へのアプリの展開**」の「、[移行と配置](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズ チュートリアルです。 ローカル プロジェクトでデータベースを削除することによって解決移行エラーがある場合は、このセクションをスキップします。

1. Visual Studio でプロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**発行**コンテキスト メニューからです。
2. **[発行]**をクリックします。

    Visual Studio は、azure アプリケーションを展開し、アプリケーションを Azure で実行されている、既定のブラウザーで開きます。
3. アプリケーションをテストすることを確認して動作します。

    初めてページを実行するデータベースにアクセスする場合は、Entity Framework 実行のすべての移行の`Up`データベースが現在のデータ モデルに最新の状態に必要なメソッドです。 これですべての前回のこのチュートリアルで追加した部門ページなど、展開した後に追加した web ページを使用できます。

## <a name="summary"></a>まとめ

このチュートリアルでは、非同期的に実行されるコードを記述して、サーバーの効率性を向上させる方法と用のストアド プロシージャを使用する方法は、挿入、更新、および削除操作を確認しました。 次のチュートリアルでは、複数のユーザーが同時に同じレコードを編集しようとデータ損失を回避する方法が表示されます。

その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。

> [!div class="step-by-step"]
> [前へ](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
