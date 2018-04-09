---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: 最初にコードを移行し、Entity Framework、ASP.NET MVC アプリケーションと展開 |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 04d393edca0469df140f06a7d083a48aa8f84b65
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>最初にコードを移行し、Entity Framework、ASP.NET MVC アプリケーションと展開
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。

これまで、アプリケーションが実行されているローカル IIS Express で開発用コンピューター上。 実際のアプリケーションをインターネット経由で使用するには、他のユーザーに使用できるようにするには、web ホスティング プロバイダーに配置する必要があります。 このチュートリアルで、Contoso 大学アプリケーションを Azure でクラウドに展開します。

このチュートリアルには、次のセクションが含まれています。

- Code First Migrations を有効にします。 移行機能を使用すると、データ モデルを変更および削除し、データベースを再作成しなくても、データベース スキーマを更新することにより、実稼働環境に変更を配置できます。
- Azure に展開します。 この手順は省略可能です。残りのチュートリアルを続行するには、プロジェクトを配置することなしです。

ソース管理と展開については、継続的インテグレーション プロセスを使用するが、このチュートリアルではこれらのトピックについては説明しないことをお勧めします。 詳細については、次を参照してください。、[ソース コントロール](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)と[継続的インテグレーション](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery)の章、 [Azure と実際のクラウド アプリのビルド](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction)電子書籍します。

## <a name="enable-code-first-migrations"></a>Code First Migrations を有効にします。

新しいアプリケーションを開発して、データ モデルが頻繁に変更される場合、モデルが変更されるたびに、モデルはデータベースと同期されなくなります。 自動的に削除し、データ モデルを変更するたびにデータベースを再作成するには、Entity Framework を構成しておきます。 ときに、追加、削除、またはエンティティ クラスを変更または変更、`DbContext`クラスを次に、アプリケーションを実行したときに自動的に、既存のデータベースを削除すると、作成、モデルと一致し、テスト データのシードを設定するか、新しいです。

このメソッドは、実稼働環境にアプリケーションを展開するまで、データベースとデータ モデルの同期の維持がうまく機能します。 アプリケーションは、実稼働環境で実行中は、データを保持して、新しい列を追加するなどの変更を加えるたびにすべてのものが失われるしたくないが通常格納されます。 [Code First Migrations](https://msdn.microsoft.com/data/jj591621)機能が Code First を削除して、データベースを再作成ではなく、データベース スキーマを更新するようにすることでこの問題を解決します。 このチュートリアルでは、アプリケーションを展開してを準備するを移行できます。

1. 前のコメント アウトまたは削除すると、設定した初期化子を無効にする、`contexts`アプリケーションの Web.config ファイルに追加する要素です。

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. アプリケーションでも*Web.config*ファイル、ContosoUniversity2 に接続文字列でデータベースの名前を変更します。

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    この変更では、最初の移行で新しいデータベースが作成されるように、プロジェクトを設定します。 これには必要ありませんが、後述することをお勧めする理由。
3. **ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**し**Package Manager Console**です。

    ![Selecting_Package_Manager_Console](https://asp.net/media/4336350/1pm.png)
4. `PM>`プロンプトに次のコマンドを入力してください。

    `enable-migrations`  
    `add-migration InitialCreate`

    ![enable-migrations コマンド](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    `enable-migrations`コマンドを作成、*移行*ContosoUniversity プロジェクト内のフォルダーはそのフォルダーに格納、*される Configuration.cs*ファイルの移行を構成することができます。

    (データベース名を変更するように指示する上記の手順をされなかった場合に移行は、既存のデータベースを見つけるし、自動的には、`add-migration`コマンド。 これは問題ありません、これらのデータベースを展開する前に、移行コードのテストを実行しません。 後で実行するときに、`update-database`コマンドは、データベースが既に存在するため、何も起こりません)。

    ![Migrations フォルダー](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    先ほど、見た initializer クラスと同様に、`Configuration`クラスが含まれています、`Seed`メソッドです。

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    目的、[シード](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)メソッドを挿入または Code First を作成またはデータベースを更新した後、テスト データを更新することを有効にします。 メソッドは、データベースの作成時に、そのデータ モデルを変更した後、データベース スキーマが更新されるたびに呼び出されます。

### <a name="set-up-the-seed-method"></a>Seed メソッドを設定します。

削除しているし、初期化子クラスを使用するデータ モデルのすべての変更のデータベースを再作成するには、`Seed`モデル変更のたびに、データベースが削除されるため、テスト データを挿入するメソッドとすべてのテスト データは失われます。 テスト データを保持するデータベースの変更後に、Code First Migrations にテスト データを含むため、[シード](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)メソッド通常必要はありません。 実際には、したくない、`Seed`場合、使用する移行を実稼働でデータベースを配置するためにテスト データを挿入するメソッド、`Seed`メソッドは、実稼働環境で実行されます。 場合は、`Seed`実稼働環境で、データベースに必要なデータのみを挿入するメソッド。 たとえば、ことができます、データベース内の実際の部門名を含める、`Department`表に、アプリケーションは実稼働環境で使用可能です。

このチュートリアルで使用する移行展開についても`Seed`メソッドが大量のデータを手動で挿入することがなくアプリケーションの機能のしくみを確認しやすくためにテスト データをこのまま挿入されます。

1. 内容を置き換える、*される Configuration.cs*ファイルを次のコードは、新しいデータベースにテスト データが読み込まれます。 

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    [シード](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)メソッドを受け取り、入力パラメーターとして、データベース コンテキストのオブジェクト、メソッドのコードはそのオブジェクトを使用して、データベースに新しいエンティティを追加します。 エンティティ型ごとに、コードは新しいエンティティのコレクションを作成、適切なにそれらを追加[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx)プロパティ、および、データベースへの変更を保存します。 呼び出す必要はありません、 [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)メソッドのエンティティの各グループの後に、ここでは、として行われますが、これを行うコードは、データベースに書き込んでいるときに例外が発生した場合、問題の原因を特定できます。

    一部のデータを挿入するステートメントを使用して、 [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) "upsert"操作を実行するメソッド。 `Seed`メソッドの実行を実行するたびに、`update-database`追加しようとしている行が既にありますデータベースを作成する最初の移行後にあるため、データを挿入することはできませんだけを各移行後に通常コマンドします。 "Upsert"操作が既に存在する行を挿入しようとする場合に発生するとエラーを防ぐこと***オーバーライド***アプリケーションのテスト中に対して行ったデータに対する変更。 いくつかのテーブルでのテスト データを使用しない場合発生すること。 場合によってはテスト中にデータを変更すると、変更するデータベースの更新後に残します。 条件付きの挿入操作を実行する場合: 存在しない場合にのみ行を挿入します。 Seed メソッドは、両方のアプローチを使用します。

    渡される最初のパラメーター、 [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)メソッドが使用して、行が既に存在するかどうかを確認するプロパティを指定します。 次の情報を提供して、テスト学生データの`LastName`最後の各リストの名前は一意なので、この目的のプロパティを使用できます。

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    このコードでは、最後に名前が一意であることを前提としています。 重複する姓を持つ、受講者を手動で追加する場合は、次回の移行を実行する次の例外が表示されます。

    シーケンスには、複数の要素が含まれています。

    "Alexander Carson"という 2 つの受講者などの冗長なデータを処理する方法については、次を参照してください。[シード処理、およびデバッグ Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Rick Anderson のブログです。 詳細については、`AddOrUpdate`メソッドを参照してください[EF 4.3 AddOrUpdate メソッドを使用して注意](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman ブログ。

    作成するコード`Enrollment`エンティティには、必要、`ID`内のエンティティの値、`students`コレクション、コレクションを作成するコードでそのプロパティを設定しませんでした。

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    使用することができます、`ID`プロパティは、ここため、`ID`を呼び出すときに値が設定`SaveChanges`の`students`コレクション。 EF は、エンティティをデータベースに挿入し、更新時に自動的に主キーの値を取得、`ID`メモリ内のエンティティのプロパティです。

    各追加コード`Enrollment`エンティティを`Enrollments`エンティティ セットを使用していない、`AddOrUpdate`メソッドです。 これは、エンティティが既に存在しが存在しない場合は、エンティティを挿入します。 かどうかを確認します。 このアプローチには、アプリケーションの UI を使用して登録グレードに加えた変更が保存されます。 各メンバーをループ処理、コード、 `Enrollment`[リスト](https://msdn.microsoft.com/library/6sh2ey19.aspx)し、データベースの登録が見つからない場合、データベースに登録を追加します。 初めて、データベースを更新するデータベースは空になりますので、各登録を追加します。

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]
2. プロジェクトをビルドします。

### <a name="execute-the-first-migration"></a>最初の移行を実行します。

実行すると、`add-migration`コマンド、移行が最初からデータベースを作成するコードを生成します。 このコードもに、*移行*という名前のファイル内のフォルダー *&lt;タイムスタンプ&gt;\_InitialCreate.cs*です。 `Up`のメソッド、`InitialCreate`クラスは、データ モデルのエンティティ セットに対応するデータベース テーブルを作成し、`Down`メソッドでは、それらを削除します。

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

移行は、`Up` メソッドを呼び出して、移行のためのデータ モデルの変更を実装します。 更新をロールバックするためのコマンドを入力すると、移行が `Down` メソッドを呼び出します。

これは、入力したときに作成された最初の移行、`add-migration InitialCreate`コマンド。 パラメーター (`InitialCreate`例では)、ファイルで使用される単語または語句、移行で行われている新機能をまとめたものを選択する通常以外の名前を指定し、希望することができます。 たとえば、後で移行を名前可能性があります&quot;AddDepartmentTable&quot;です。

データベースが既に存在するときに、初期移行を作成した場合、データベースの作成コードが生成されますが、データベースは既にデータと一致しているため、作成コードを実行する必要はありません。 データベースがまだ存在しない別の環境にアプリを展開する場合、データベースを作成するために、このコードが実行されるため、最初にテストを行うことをお勧めします。 これが、移行で新しいデータベースを最初から作成できるように、前の接続文字列でデータベースの名前を変更した理由です。

1. **パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを入力します。

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    `update-database`コマンドを実行する、`Up`データベースし、それを作成するメソッドの実行、`Seed`メソッドは、データベースを設定します。 同じプロセス自動的に実行されます実稼働環境で、アプリケーションを展開するように、次のセクションが表示されます。
2. 使用して**サーバー エクスプ ローラー**を最初のチュートリアルで行ったように、データベースを調査し、動作を確認することのすべてが同じまでと同様、アプリケーションを実行します。

## <a name="deploy-to-azure"></a>Azure への配置します。

これまで、アプリケーションが実行されているローカル IIS Express で開発用コンピューター上。 使用可能にする他の人に、インターネット経由で使用して、web ホスティング プロバイダーに配置する必要です。 チュートリアルのこのセクションでは、Azure にデプロイします。 このセクションは省略可能です。この手順をスキップして次のチュートリアルを続行または任意の別のホスティング プロバイダーのこのセクションの手順を適合させることができます。

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Code First Migrations を使用して、データベースを展開するには

データベースを展開するには、Code First Migrations を使用します。 というチェック ボックスを選択する設定を構成する Visual Studio から展開するために使用する発行プロファイルを作成するときに**更新データベース**です。 この設定により、展開プロセスを自動的にアプリケーションを構成する*Web.config* Code First を使用するように、移行先サーバー上のファイル、`MigrateDatabaseToLatestVersion`初期化子のクラスです。

Visual Studio を処理しません、データベースと、展開プロセス中に、プロジェクトが移行先サーバーにコピー中にします。 展開済みアプリケーションを実行すると、展開後に最初に、データベースにアクセスする、Code First チェックがデータベースには、データ モデルが一致すます。 不一致がある場合 Code First 自動的にデータベースを作成します (まだ存在しない) 場合または (データベースが存在しますが、モデルと一致しない) 場合は、最新バージョンにデータベース スキーマを更新します。 アプリケーションには、移行が実装されている場合`Seed`メソッド、メソッドの実行後、データベースが作成されるか、スキーマを更新します。

使用して移行`Seed`メソッドは、テスト データを挿入します。 変更する必要がありますを実稼働環境に配置された場合、`Seed`メソッドが、実稼働データベースに挿入するデータの挿入のみようにします。 たとえば、現在のデータ モデルのことができますが、実際のコース、架空の受講者に、開発用データベースで。 記述することができます、`Seed`を読み込みの両方で、開発し、実稼働環境に展開する前に、架空の受講者をコメントします。 作成して、`Seed`コースのみを読み込むし、アプリケーションの UI を使用して手動でテスト データベースに架空の受講者を入力します。

### <a name="get-an-azure-account"></a>Azure アカウントを取得します。

Azure アカウントが必要です。 場合は、既に必要はありませんが、Visual Studio サブスクリプションが、 [、サブスクリプションの特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)です。 それ以外の場合、ほんの数分で無料の試用アカウントを作成できます。 詳細については、「 [Azure 無料評価版](https://azure.microsoft.com/free/)です。

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Azure で web サイトおよび SQL データベースを作成します。

Azure で web アプリは、Azure の他のクライアントと共有されている仮想マシン (Vm) で実行されることを意味する共有ホスティング環境で実行されます。 共有ホスティング環境は、クラウドで作業を開始する低コスト方法です。 その後、web トラフィックが増加すると、アプリケーションを専用の仮想マシンで実行して、ニーズに対応するスケールできます。 Azure App Service の料金オプションの詳細については、上のドキュメントを読み取る[Azure ドキュメント](https://azure.microsoft.com/pricing/details/app-service/)

Azure SQL Database にデータベースを配置します。 SQL データベースとは、SQL Server テクノロジに基づいて構築されたクラウド ベースのリレーショナル データベース サービスです。 ツールと SQL Server で動作するアプリケーションも、SQL Database で動作します。

1. [Azure Management Portal](https://portal.azure.com)、 をクリックして**新規**左側のタブをクリックして**すべてを参照してください**をクリックして新しいブレードで、 **Web アプリと SQL**で、**Web**セクションと最後に**作成**です。

    ![管理ポータルで新しいボタン](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/CreateWeb-Sql.png)

   **SQL の作成 (&)、新しい Web アプリ**ウィザードが開きます。

2. ブレードで、入力文字列に、**アプリ名**アプリケーションの一意の URL として使用するボックスです。 入力した内容は、ここと Azure アプリのサービスの既定のドメインの完全な URL では (. azurewebsites.net)。 場合、**アプリ名**は既に使用されて、ウィザードは赤いでこれを通知*アプリ名は使用できません*メッセージ。 場合、**アプリ名**が利用可能な表示される緑色のチェック マークします。

    ![管理ポータルでデータベースのリンクを作成します。](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-WebApp.png)

3. **サブスクリプション**ドロップダウン リストで、する Azure サブスクリプションを選択してください、 **App Service**を配置します。

4. **リソース グループ**テキスト ボックスで、リソース グループを選択するか、新しいものを作成します。 この設定は、データ センターで、web サイトは実行を指定します。 リソース グループの詳細については、ドキュメントをお読みに[Azure Docs](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)です。
5. 新規作成**App Service プラン** をクリックして、*アプリ サービス セクションで*、**新規作成**、入力と**App Service プラン**(と同じ名前を指定できますアプリ サービス)、**場所**、および**価格レベル**(は、無料のオプション)。

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-AppService.png)
6. クリックして、 **SQL データベース**を選択して*新規作成*または既存のデータベースを選択

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-Database.png)

7. **名前**ボックスで、データベースの名前を入力します。
8. クリックして、**ターゲット サーバー**ボックスで、**サーバーの新規作成**です。 また、サーバーを以前に作成した場合は、使用可能なサーバーの一覧からそのサーバーを選択できます。
9. 選択**価格レベル**セクションで、選択*空き*です。 その他のリソースが必要な場合は、データベースをいつでもにスケール アップが可能です。 Azure SQL の料金の詳細については、ドキュメントを読んで[Azure Docs](https://azure.microsoft.com/pricing/details/sql-database/)です。
10. 変更[照合](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support)に応じて。
11. 管理者の入力**SQL 管理者のユーザー名**と**SQL 管理者のパスワード**です。 選択した場合は**新しい SQL データベース サーバー**、既存の名前とパスワードをここに入力していない、新しい名前とデータベースにアクセスするときに後で使用するようになりました定義しているパスワードを入力しているか。 以前作成したサーバーを選択した場合は、そのサーバーの資格情報を入力します。
12. Application Insights を使用してサービスをアプリに製品利用統計情報のコレクションを有効にすることができます。 ほとんどの構成に application Insights は、重要なイベント、例外、依存関係、要求、およびトレース情報を収集します。 Application Insights の詳細についてを使い始める[Azure Docs](https://azure.microsoft.com/services/application-insights/)です。
13. をクリックして**作成**が完了したらを示すために、ブレードの下部にあります。
  
    管理ポータル ページに戻る、ダッシュ ボード、および**通知**ブレードで、ページの上部にあるが、サイトが作成されていることを示しています。 (通常よりも小さい 1 分間)、しばらくしてから、展開が成功したことを示す通知があります。 左側のナビゲーション バーで、新しい**App Service**に表示されます、 *App Services*セクションと、新しい**SQL データベース**に表示されます、 *SQL データベース*セクションです。

### <a name="deploy-the-application-to-azure"></a>Azure にアプリケーションを展開します。

1. Visual Studio でプロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**発行**コンテキスト メニューからです。
  
    ![プロジェクトのコンテキスト メニューを発行します。](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)
2. **プロファイル**のタブ、 **Web の発行**ウィザード、をクリックして**Microsoft Azure App Service**です。
  
    ![発行設定のインポート](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-ChooseTarget.png)
3. 以前 Visual Studio で Azure サブスクリプションを追加していない場合は、画面の手順を実行します。 次の手順が、Azure サブスクリプションに接続する Visual Studio のリストを有効にする**App Services** web サイトが含まれます。
 
4. 選択、**サブスクリプション**に、アプリ サービスを追加した、 **App Service プラン**アプリ サービスの一部であるフォルダーと最後に、 **App Service**続く自体**Ok**です。

    ![App Service を選択します。](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-AppService.png)
5. プロファイルを構成した後、**接続** タブが表示されます。 をクリックして**接続の検証**設定が正しいことを確認するには

    ![接続を検証します。](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Connection.png)
6. 緑のチェック マークが横に示すように、接続が検証されると、**接続の検証**ボタンをクリックします。 **[次へ]**をクリックします。
  
    ![正常に検証された接続](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-SettingsValidated.png)
7. 開く、**リモート接続文字列**下にあるドロップダウン リスト**SchoolContext**を作成したデータベースの接続文字列を選択します。
8. 選択**更新データベース**です。

    ![[設定] タブ](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Settings.png)

    この設定により、展開プロセスを自動的にアプリケーションを構成する*Web.config* Code First を使用するように、移行先サーバー上のファイル、`MigrateDatabaseToLatestVersion`初期化子のクラスです。
9. **[次へ]**をクリックします。
10. **プレビュー**  タブで、をクリックして**開始プレビュー**です。
  
    ![[プレビュー] タブで StartPreview ボタン](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Preview.png)
  
    タブには、サーバーにコピーされるファイルの一覧が表示されます。 プレビューを表示する、アプリケーションを発行するため必要はありませんですが便利関数を認識します。 この場合、表示されているファイルの一覧は何もする必要はありません。 このアプリケーションを配置するときに、[次へ] に変更されたファイルだけは、この一覧になります。
    ![StartPreview ファイル出力](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-PreviewLoaded.png)

11. **[発行]**をクリックします。
    Visual Studio では、Azure サーバーへのファイルのコピーのプロセスを開始します。
12. **出力**ウィンドウどのような展開アクションを実行した示し、展開の成功した完了を報告します。
  
    ![展開の成功を報告 [出力] ウィンドウ](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-BuildOutput.png)
13. 配置に成功したら、既定のブラウザーは、配置の web サイトの URL を自動的に開きます。
    作成したアプリケーションは、クラウドで実行されています。 
  
    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Site.png)

この時点で、 *SchoolContext*データベースが作成された Azure SQL データベースで選択したので、**実行 Code First Migrations (アプリの起動時に実行)**です。 *Web.config*配置済みの web サイト内のファイルが変更されたできるように、 [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)初期化子は初めてコード データ読み取りまたは書き込み (これが発生したデータベースでの実行選択した場合、**受講者** タブ)。

![](https://asp.net/media/4367421/mig.png)

展開プロセスは、新しい接続文字列も作成*(SchoolContext\_DatabasePublish*) の Code First Migrations データベース スキーマの更新と、データベースのシード処理に使用します。

![Database_Publish 接続文字列](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

自分のコンピューター上の Web.config ファイルの配置済みのバージョンを見つけることができます*ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*です。展開されたアクセスできる*Web.config* FTP を使用してファイル自体です。 手順については、次を参照してください。 [Visual Studio を使用した ASP.NET Web 展開: コードの更新を展開する](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update)です。 開始される指示に従って、"FTP ツールを使用する次の 3 つ必要があります: FTP の URL、ユーザー名とパスワードです"。

> [!NOTE]
> Web アプリは、すべての URL を検索するユーザー データを変更できるようにセキュリティを実装しません。 Web サイトをセキュリティで保護する方法については、次を参照してください。[メンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET MVC アプリケーションを Azure にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。 他の人が、Azure 管理ポータルを使用して、サイトを使用するようにまたは**サーバー エクスプ ローラー** Visual studio にサイトを停止します。


![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Stop-Service.png)

## <a name="advanced-migrations-scenarios"></a>高度な移行シナリオ

このチュートリアルで示すように自動的に移行を実行してデータベースを展開する複数のサーバーで実行される web サイトに配置する場合は、同時に移行を実行しようとしています。 複数のサーバーを取得するでした。 2 つのサーバーが同じへ移行を実行しようとすると、一方が成功して (場合、操作を 2 回実行することはできません) はできません、他の移行は atomic。 このシナリオでは、これらの問題を回避する場合の移行を手動で呼び出すできのみ 1 回行われるように、独自のコードを設定できます。 詳細については、次を参照してください。[実行とコードからのスクリプトの移行](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/)Rowan Miller ブログと[Migrate.exe](https://msdn.microsoft.com/data/jj618307) (コマンドラインからの移行を実行する) の MSDN のです。

その他の移行シナリオについては、次を参照してください。[移行スクリーン キャスト系列](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)です。

## <a name="code-first-initializers"></a>コードの最初の初期化子

展開に関するセクションで説明しました、 [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)初期化子が使用されています。 最初のコードなどの他の初期化子を提供[CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (既定)、 [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx)を使用する前) と[DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx)です。 `DropCreateAlways`初期化子は単体テストの条件を設定するため役に立ちます。 独自の初期化子を記述することもでき、アプリケーションからの読み取りまたはデータベースに書き込みます。 するまで待機したくない場合は、明示的に初期化子を呼び出すことができます。 このチュートリアルは、2013 年 11 月版で記述されている時に移行を有効にする前に作成および DropCreate 初期化子をのみ使用できます。 Entity Framework チームが作業にも移行で使用可能なこれらの初期化子を作成します。

初期化子の詳細については、次を参照してください[についてデータベース初期化子の Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm)と書籍の第 6 章[Entity Framework のプログラミング: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman によって。および Rowan Miller です。

## <a name="summary"></a>まとめ

このチュートリアルでは、移行を有効にしてアプリケーションを展開する方法を説明しました。 次のチュートリアルでは、データ モデルを展開してより高度なトピックを見るが始めます。

このチュートリアルをリンクする方法と、何を改善にフィードバックを送信してください。 新しいトピックを要求することもできます。 [Me 方法でコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)です。

その他の Entity Framework リソースへのリンクは含まれて[ASP.NET データ アクセス - リソースのことをお勧め](xref:whitepapers/aspnet-data-access-content-map)です。

> [!div class="step-by-step"]
> [前へ](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
> [次へ](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
