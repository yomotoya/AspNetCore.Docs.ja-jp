---
title: "Razor ページと EF Core - 移行 - 4/8"
author: rick-anderson
description: "このチュートリアルでは、ASP.NET Core MVC アプリでデータ モデルの変更を管理するための EF Core の移行機能の使用を開始します。"
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: e89d95702cb94556bc6e5dc73253c51acaa11578
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/31/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a>移行 - EF コアと Razor ページのチュートリアル (4/8)

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog)、[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

このチュートリアルでは、データ モデルの変更を管理するための EF Core の移行機能を使用します。

解決できない問題が発生した場合は、[このステージの完成したアプリ](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)をダウンロードしてください。

新しいアプリを開発するときには、頻繁にデータ モデルを変更します。 モデルを変更するたびに、モデルはデータベースと同期されなくなります。 このチュートリアルでは、最初にデータベースが存在しない場合にデータベースを作成するように Entity Framework を構成します。 データ モデルが変更されるたびに次の処理を実行します。

* データベースが削除されます。
* EF がモデルと一致する新しいデータベースを作成します。
* アプリがテスト データを DB に格納します。

このアプローチは、実稼働環境にアプリを展開するまで、データベースとデータ モデルの同期の維持がうまく機能します。 アプリが実稼働環境で実行されているときには、通常は維持する必要があるデータをアプリが格納します。 変更 (新しい列の追加など) が加えられるたびにテスト データベースを使用してアプリを開始することはできません。 EF コア移行機能は、新しいデータベースを作成する代わりに EF Core でデータベース スキーマを更新できるようにすることでこの問題を解決します。

データ モデルが変更されたときにデータベースを削除して再作成する代わりに、移行によってスキーマを更新し、既存のデータを維持します。

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>移行用の Entity Framework Core NuGet パッケージ

移行を使用するには、**パッケージ マネージャー コンソール** (PMC) またはコマンドライン インターフェイス (CLI) を使用します。 これらのチュートリアルでは、CLI コマンドを使用する方法を示します。 PMC については、[このチュートリアルの最後](#pmc)に説明します。

コマンド ライン インターフェイス (CLI) の EF Core ツールが [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) で提供されています。 このパッケージをインストールするには、下に示すように、*.csproj* ファイルの `DotNetCliToolReference` コレクションにパッケージを追加します。 **注:** このパッケージをインストールするには、*.csproj* ファイルを編集する必要があります。 `install-package` コマンドまたはパッケージ マネージャー GUI を使用してこのパッケージをインストールすることはできません。 *.csproj* ファイルを編集するには、**ソリューション エクスプローラー**でプロジェクト名を右クリックし、**[ContosoUniversity.csproj の編集]** を選択します。

次のマークアップは、更新された *.csproj* ファイルと強調表示された EF Core CLI ツールを示しています。

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
上記の例では、バージョン番号は、チュートリアルが記述された時点で最新でした。 他のパッケージで見つかった EF Core CLI ツールの同じバージョンを使用します。

## <a name="change-the-connection-string"></a>接続文字列を変更する

*appsettings.json* ファイルで、接続文字列内のデータベースの名前を ContosoUniversity2 に変更します。

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

接続文字列でデータベース名を変更すると、最初の移行で新しいデータベースが作成されます。 新しいデータベースが作成されるのは、その名前のデータベースが存在していないためです。 移行で開始するには、接続文字列を変更する必要はありません。

データベース名を変更する代わりにデータベースを削除することもできます。 **SQL Server オブジェクト エクスプローラー** (SSOX) または `database drop` CLI コマンドを使用します。

 ```console
 dotnet ef database drop
 ```

次のセクションでは、CLI コマンドを実行する方法について説明します。

## <a name="create-an-initial-migration"></a>初期移行を作成する

プロジェクトをビルドします。

コマンド ウィンドウを開き、プロジェクト フォルダーに移動します。 プロジェクト フォルダーには *Startup.cs* ファイルが含まれます。

コマンド ウィンドウで次のコマンドを入力します。

```console
dotnet ef migrations add InitialCreate
```

コマンド ウィンドウには、次のような情報が表示されます。

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

移行が失敗した場合は、"*ファイル ...ContosoUniversity.dll は、別のプロセスで使用されているため、アクセスできません。*" というメッセージが表示されます。

* IIS Express を終了する

   * Visual Studio を終了して再起動します。または、
   * Windows のシステム トレイで IIS Express アイコンを見つけます。
   * IIS Express アイコンを右クリックし、**[ContosoUniversity] > [サイトの停止]** をクリックします。

"ビルドに失敗しました" というエラー メッセージが 表示された場合、コマンドを再度実行します。 このエラーが発生する場合は、このチュートリアルの最後にメモを残してください。

### <a name="examine-the-up-and-down-methods"></a>Up および Down メソッドを確認する

EF Core コマンド `migrations add` は、データベースの作成元のコードを生成しました。 この移行コードは *Migrations\<timestamp>_InitialCreate.cs* ファイルにあります。 `InitialCreate` クラスの `Up` メソッドは、データ モデルのエンティティ セットに対応するデータベース テーブルを作成します。 次の例で示すように、`Down` メソッドは、それらを削除します。

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

移行は、`Up` メソッドを呼び出して、移行のためのデータ モデルの変更を実装します。 更新をロールバックするためのコマンドを入力すると、移行が `Down` メソッドを呼び出します。

上記のコードは、初期の移行のためのコードです。 そのコードは、`migrations add InitialCreate` コマンドが実行されたときに作成されます。 移行の name パラメーター (この例では "InitialCreate") は、ファイル名に使用されます。 移行名には、任意の有効なファイル名を指定できます。 移行で行われている処理をまとめた単語または語句を選択することをお勧めします。 たとえば、department テーブルを追加する移行に "AddDepartmentTable" という名前を付けることができます。

最初の移行が作成され、データベースが存在している場合:

* データベース作成コードが生成されます。
* データベースは既にデータ モデルと一致しているため、データベース作成コードを実行する必要はありません。 データベース作成コードが実行された場合、データベースは既にデータ モデルと一致しているため、何も変更しません。

新しい環境にアプリが展開されるときには、データベースを作成するためのデータベース作成コードを実行する必要があります。

接続文字列は前にデータベースの新しい名前を使用するように変更されました。 指定されたデータベースが存在しないため、移行は、データベースを作成します。

### <a name="examine-the-data-model-snapshot"></a>データ モデルのスナップショットを確認する

移行は、現在のデータベース スキーマの*スナップショット*を *Migrations/SchoolContextModelSnapshot.cs* 内に作成します。

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

現在のデータベース スキーマはコードで表されるため、移行を作成するために、EF Core がデータベースとやり取りする必要はありません。 移行を追加するときに、EF Core は、スナップショット ファイルとデータ モデルを比較することによって変更内容を判断します。 EF Core は、データベースを更新する必要がある場合にのみ、データベースとやり取りします。

スナップショット ファイルは、その作成元の移行と同期されている必要があります。 *\<timestamp>_\<migrationname>.cs* というファイルを削除することによって移行を削除することはできません。 そのファイルが削除された場合、残りの移行は、データベース スナップショット ファイルと同期されなくなります。 最後に追加された移行を削除するには、[dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) コマンドを使用します。

## <a name="remove-ensurecreated"></a>Remove EnsureCreated

初期の開発では、`EnsureCreated` コマンドが使用されました。 このチュートリアルでは、migrations を使用します。 `EnsureCreated` には次の制限が適用されます。

* 移行をバイパスし、データベースとスキーマを作成します。
* 移行テーブルは作成しません。
* 移行と共に使用することは*できません*。
* データベースが頻繁に削除および再作成されるようなテストや迅速なプロトタイプのために設計されています。

`DbInitializer` から次の行を削除します。

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>開発中のデータベースに移行を適用する

コマンド ウィンドウで、次のコマンドを入力してデータベースとテーブルを作成します。

```console
dotnet ef database update
```

メモ: `update` コマンドが "ビルドに失敗しました" というエラーを返した場合は、次の手順を実行します。

* コマンドをもう一度実行します。
* 再度失敗した場合は、Visual Studio を終了し、`update` コマンドを実行します。
* ページの下部にメッセージを残します。

コマンドの出力は、`migrations add` コマンドの出力に似ています。 上記のコマンドでは、データベースをセットアップする SQL コマンドのログが表示されます。 次のサンプル出力ではログのほとんどは省略されています。

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

ログ メッセージの詳細レベルを下げるために、 *appsettings.Development.json* ファイルでログ レベルを変更できます。 詳細については、「[Introduction to Logging](xref:fundamentals/logging/index)」 (ログ記録の概要) を参照してください。

**SQL Server オブジェクト エクスプローラー**を使用してデータベースを検査します。 `__EFMigrationsHistory` テーブルが追加されていることに注意してください。 `__EFMigrationsHistory` テーブルは、どの移行がデータベースに適用されたかを追跡します。 `__EFMigrationsHistory` テーブルのデータを表示すると、最初の移行の 1 つの行が表示されます。 前の CLI の出力例の最後のログは、この行を作成する INSERT ステートメントを示しています。

アプリを実行して、すべてが適切に機能していることを確認します。

## <a name="appling-migrations-in-production"></a>実稼働環境で移行を適用する

実稼働アプリケーションでは、アプリケーションの起動時に [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) を**呼び出さない**ことをお勧めします。 `Migrate` をサーバー ファームのアプリから呼び出すことはできません。 たとえば、アプリがスケールアウト (アプリの複数のインスタンスを実行する) を使用してクラウドに展開されている場合があります。

データベースの移行は、展開の一部として制御された方法で行う必要があります。 実稼働データベースの移行には次の方法があります。

* 移行を使用して SQL スクリプトを作成し、展開で SQL スクリプトを使用します。
* 制御された環境から `dotnet ef database update` を実行します。

EF Core は、`__MigrationsHistory` テーブルを使用して、移行を実行する必要があるかどうかを確認します。 データベースが最新の状態になっている場合、移行は実行されません。

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>コマンド ライン インターフェイス (CLI) とパッケージ マネージャー コンソール (PMI) の比較

移行を管理するための EF Core ツールを次の方法で使用できます。

* .NET Core CLI コマンド。
* Visual Studio **パッケージ マネージャー コンソール** (PMC) ウィンドウの PowerShell コマンドレット。

このチュートリアルでは、CLI を使用する方法を示します。PMC を好む開発者もいます。

PMC の EF Core コマンドは、[Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) パッケージ内にあります。 このパッケージは [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) メタパッケージに含まれています。インストールする必要はありません。

**重要:** このパッケージは、*.csproj* ファイルを編集することで、CLI 用にインストールするパッケージと同じものではありません。 `Tools.DotNet` で終わる CLI パッケージ名とは異なり、このパッケージの名前は `Tools` で終わります。

CLI コマンドの詳細については、「[.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)」を参照してください。

PMC コマンドの詳細については、「[パッケージ マネージャー コンソール (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)」を参照してください。

## <a name="troubleshooting"></a>トラブルシューティング

[このステージの完成したアプリ](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)をダウンロードします。

アプリは次の例外を生成します。

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Solution: Run `dotnet ef database update`

`update` コマンドが "ビルドに失敗しました" というエラーを返した場合は、次の手順を実行します。

* コマンドをもう一度実行します。
* ページの下部にメッセージを残します。

>[!div class="step-by-step"]
[前へ](xref:data/ef-rp/sort-filter-page)
[次へ](xref:data/ef-rp/complex-data-model)
