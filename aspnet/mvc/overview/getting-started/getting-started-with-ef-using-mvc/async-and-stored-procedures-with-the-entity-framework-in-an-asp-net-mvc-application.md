---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'チュートリアル: ASP.NET MVC アプリでの EF で非同期とストアド プロシージャを使用します。'
description: このチュートリアルでは、非同期プログラミング モデルを実装して、ストアド プロシージャを使用する方法を説明する方法を参照してください。
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 0896664174bc2fee65b73ecf256d994f2abacc0a
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444364"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a>チュートリアル: ASP.NET MVC アプリでの EF で非同期とストアド プロシージャを使用します。

前のチュートリアルでは、読み取りし、同期プログラミング モデルを使用してデータを更新する方法について説明しました。 このチュートリアルでは、非同期プログラミング モデルを実装する方法について参照してください。 非同期コードは、アプリケーション サーバーのリソースを効率的に使用できるのでパフォーマンスが向上に役立ちます。

このチュートリアルでは、insert、update、および削除の各エンティティに対して操作をストアド プロシージャを使用する方法も参照します。

最後に、初めてデプロイした後を実装しているデータベースの変更はすべてと共に、Azure にアプリケーションを再デプロイします。

以下の図は、使用するページの一部を示しています。

![部門のページ](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![学科を作成します。](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * 非同期コードについて説明します
> * 部門のコント ローラーを作成します。
> * ストアド プロシージャを使用します。
> * Azure に配置する

## <a name="prerequisites"></a>必須コンポーネント

* [関連データの更新](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a>非同期コードを使用する理由

Web サーバーでは、利用できるスレッド数に限りがあります。負荷が高い状況では、利用できるスレッドが全部使われる可能性があります。 その場合、スレッドが解放されるまでサーバーは新しい要求を処理できません。 同期コードの場合、たくさんのスレッドが関連付けられていても、I/O の完了を待っているため、実際には何の作業も行っていないということがあります。 非同期コードの場合、あるプロセスが I/O の完了を待っているとき、そのスレッドは解放され、サーバーによって他の要求の処理に利用できます。 結果として、非同期コードより効率的に使用すると、遅延なしの多くのトラフィックを処理するために、サーバーが有効になっているサーバーのリソースができます。

.NET の以前のバージョンで非同期コードのテストの記述とは、複雑なエラーが発生しやすく、デバッグが困難にします。 .NET 4.5 での書き込み、テスト、および非同期コードのデバッグはずっと簡単にしないようにする理由がない限り非同期コードを記述する、一般にする必要があります。 非同期のコードで少量のオーバーヘッド、挿入されたが、パフォーマンスに影響はごくわずかであり、中にトラフィックが多い場合のトラフィックが少ない場合、潜在的なパフォーマンスの向上は実体です。

非同期プログラミングの詳細については、次を参照してください。[ブロッキング呼び出しを回避するために .NET 4.5 の非同期サポート](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async)します。

## <a name="create-department-controller"></a>部門のコント ローラーを作成します。

この時間を除く、以前のコント ローラーを実行した同じ方法を選択部門コント ローラーを作成、**非同期コント ローラー アクションを使用して、** チェック ボックスをオンします。

次の点には、同期コードに追加された内容を表示する、`Index`メソッドを非同期になります。

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

非同期的に実行する Entity Framework データベース クエリを有効にする 4 つの変更が適用されました。

- メソッドが付いて、`async`キーワード、メソッド本体の一部にコールバックを生成して、自動的に作成するコンパイラに指示する、`Task<ActionResult>`返されるオブジェクト。
- 戻り値の型がから変更された`ActionResult`に`Task<ActionResult>`します。 `Task<T>`型が型の結果で進行中の作業を表す`T`します。
- `await`キーワードは、web サービス呼び出しに適用されました。 コンパイラでは、このキーワードを見て、バック グラウンドでそのメソッドに分割 2 つの部分。 最初の部分は、非同期的に開始される操作を終了します。 2 番目の部分は、操作が完了したときに呼び出されるコールバック メソッドに配置されます。
- 非同期バージョン、`ToList`拡張機能メソッドが呼び出されました。

理由は、`departments.ToList`ステートメントを変更しない場合は、`departments = db.Departments`ステートメントか? 理由は、クエリやコマンドをデータベースに送信するステートメントのみが非同期的に実行されることです。 `departments = db.Departments`ステートメントがクエリの設定が、まで、クエリが実行されない、`ToList`メソッドが呼び出されます。 そのため、のみ、`ToList`メソッドは非同期的に実行されます。

`Details`メソッドと`HttpGet``Edit`と`Delete`メソッド、`Find`メソッドは非同期的に実行されるメソッドがあるため、データベースに送信されるクエリが発生しました。

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

`Create`、 `HttpPost Edit`、および`DeleteConfirmed`メソッドは、`SaveChanges`コマンドを実行するメソッドの呼び出しなどのステートメントではない`db.Departments.Add(department)`のみ変更するメモリ内のエンティティが発生します。

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

開いている*Views\Department\Index.cshtml*、テンプレート コードを次のコードに置き換えます。

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

管理者名を右側に移動の部門にこのコードがインデックスからタイトルを変更し、管理者の完全名を提供します。

作成、削除、詳細、およびビューの編集のキャプションを変更、`InstructorID`フィールドを「管理者」Course ビューで、部門名 フィールドを"Department"に変更して同じようにします。

Create と Edit のビューは次のコードを使用します。

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

Details と Delete ビューでは、次のコードを使用します。

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

アプリケーションを実行し、**部門**タブ。

すべて他のコント ローラーの場合と同様に動作しますが、このコント ローラーのすべての SQL クエリに非同期で実行します。

非同期プログラミングを Entity Framework を使用しているときの注意すべき点がいくつか:

- 非同期コードはスレッド セーフではありません。 つまり、つまり、しようとしないでください、同じコンテキスト インスタンスを使用して並列で複数の操作を実行します。
- 非同期コードのパフォーマンス上の利点を最大限に活用する場合、(ページングなどのために) ライブラリ パッケージを利用しているのであれば、それがクエリをデータベースに送信させる Entity Framework メソッドを呼び出す場合、非同期を利用する必要があります。

## <a name="use-stored-procedures"></a>ストアド プロシージャを使用します。

一部の開発者と Dba は、データベースへのアクセスのストアド プロシージャを使用できます。 以前のバージョンの Entity Framework でストアド プロシージャを使用してデータを取得できます[生 SQL クエリを実行する](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)、更新操作のストアド プロシージャを使用して EF に指示することはできませんが、します。 EF 6 Code First ストアド プロシージャを使用して構成が容易になります。

1. *DAL\SchoolContext.cs*、強調表示されたコードを追加、`OnModelCreating`メソッド。

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    このコードでは、Entity Framework を指示挿入、使用してストアド プロシージャに、更新、および delete 操作を`Department`エンティティ。
2. パッケージの管理コンソールで、次のコマンドを入力します。

    `add-migration DepartmentSP`

    開いている*移行\&lt; タイムスタンプ&gt;\_DepartmentSP.cs*でコードを表示する、`Up`を作成するメソッドの挿入、更新、および Delete ストアド プロシージャ。

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. パッケージの管理コンソールで、次のコマンドを入力します。

     `update-database`
4. アプリケーションをデバッグ モードで実行をクリックして、**部門**タブをクリックし、をクリックし、**新規作成**です。
5. 新しい部署のデータを入力し、クリックして**作成**です。

6. ログを見て、Visual Studio で、**出力**ウィンドウを新しい Department 行を挿入するストアド プロシージャが使用されたことを確認します。

     ![部門 Insert SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

コードは、最初に格納されている既定のプロシージャ名を作成します。 既存のデータベースを使用している場合は、データベースで既に定義されているストアド プロシージャを使用するには、ストアド プロシージャの名前をカスタマイズする必要があります。 その方法については、次を参照してください。[エンティティ フレームワーク コード最初挿入/更新/削除ストアド プロシージャ](https://msdn.microsoft.com/data/dn468673)します。

ストアド プロシージャは生成されたものをカスタマイズする場合は、移行のためのスキャフォールディングされたコードを編集できます`Up`ストアド プロシージャを作成するメソッド。 これにより変更が反映されますたびに移行が実行され、デプロイ後に運用環境で移行を自動的に実行すると、実稼働データベースに適用されます。

以前の移行で作成された既存のストアド プロシージャを変更する場合は、Add-migration コマンドを使用して、空の移行を生成して呼び出すコードを手動で作成、 [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx)メソッド.

## <a name="deploy-to-azure"></a>Azure に配置する

このセクションでは、省略可能なが完了する必要があります**アプリを Azure にデプロイ**セクション、 [Migrations と展開](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズのチュートリアル。 ローカル プロジェクトでデータベースを削除することによって解決された移行エラーがある場合は、このセクションをスキップします。

1. Visual Studio でのプロジェクトを右クリックして**ソリューション エクスプ ローラー**選択と**発行**コンテキスト メニュー。
2. **[発行]** をクリックします。

    Visual Studio を Azure にアプリケーションを配置して、アプリケーションを Azure で実行されている、既定のブラウザーで開きます。
3. アプリケーションをテストして確認することが動作します。

    初めてページを実行するデータベースにアクセスする、Entity Framework で実行されるすべての移行`Up`データベースの現在のデータ モデルで最新の状態に必要なメソッドです。 このチュートリアルで追加した部門ページなど、デプロイした最終時刻以降に追加する web ページのすべてを使用することができますようになりました。

## <a name="get-the-code"></a>コードを取得する

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a>その他の技術情報

その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス - 推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)します。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * 非同期コードについて説明しました
> * 部門のコント ローラーの作成
> * ストアド プロシージャの使用
> * Azure にデプロイ

複数のユーザーが同時に同じエンティティを更新するときの競合を処理する方法については、次の記事に進んでください。
> [!div class="nextstepaction"]
> [同時実行の処理](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
