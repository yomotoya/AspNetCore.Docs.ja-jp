---
title: "Razor ページの EF コアな移行 - 4 8"
author: rick-anderson
description: "このチュートリアルでは、ASP.NET Core MVC アプリでデータ モデルの変更を管理するための EF 中核となる移行機能の使用を開始します。"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/migrations
ms.openlocfilehash: 9a0fb52a1d1a62bce3f11c7e0394c00b9d544ab3
ms.sourcegitcommit: 3d512ea991ac36dfd4c800b7d1f8a27bfc50635e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a>移行 - EF コア Razor ページのチュートリアル (8 4)

によって[Tom Dykstra](https://github.com/tdykstra)、 [Jon P Smith](https://twitter.com/thereformedprog)、および[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

このチュートリアルでは、データ モデルの変更を管理するための EF 中核となる移行機能を使用します。

問題を解決できない場合に実行する場合は、ダウンロード、[この段階でのアプリを完成](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)です。

新しいアプリの開発時に、データ モデルの変更頻度。 毎回モデルの変更、モデルを取得、データベースと同期します。 このチュートリアルでは、Entity Framework が存在しない場合、データベースの作成を構成することによって開始します。 たびに、データ モデルの変更。

* データベースは削除されます。
* EF は、モデルと一致するか、新しいを作成します。
* アプリでは、テスト データを使用して DB シード処理されます。

DB のデータ モデルとの同期を維持するためにこのアプローチは、実稼働環境にアプリを展開するまでにも動作します。 アプリは、実稼働環境で実行中は、データを維持する必要がある通常格納されます。 アプリは、DB のたびに変更 (新しい列を追加する) など、テストを始めることはできません。 EF コア移行機能は、EF コアを新しいデータベースを作成する代わりにデータベースのスキーマの更新を有効にすると、この問題を解決します。

削除して、データ モデルの変更時にデータベースを再作成するではなくは、移行は、スキーマを更新し、既存のデータを保持します。

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>移行用の Entity Framework Core NuGet パッケージ

移行を使用するを使用して、 **Package Manager Console** (PMC) またはコマンド ライン インターフェイス (CLI)。 これらのチュートリアルでは、CLI コマンドを使用する方法を示します。 PMC に関するについては、「[このチュートリアルの最後の](#pmc)します。

コマンド ライン インターフェイス (CLI) の EF の主要なツールがで提供される[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet)です。 このパッケージをインストールする追加して、`DotNetCliToolReference`内のコレクション、 *.csproj*ファイルを示すようにします。 **注:**を編集して、このパッケージをインストールする必要があります、 *.csproj*ファイル。 `install-package`このパッケージをインストールするコマンドまたはパッケージ マネージャー GUI を使用できません。 編集、 *.csproj*でプロジェクト名を右クリックしてファイル**ソリューション エクスプ ローラー**を選択して**編集 ContosoUniversity.csproj**です。

次のマークアップを示しています、更新された*.csproj*強調表示されて、EF コア CLI ツール ファイル。

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
上記の例では、バージョン番号は、チュートリアルに書き込まれたときに現在でした。 他のパッケージで見つかった EF コア CLI ツールの同じバージョンを使用します。

## <a name="change-the-connection-string"></a>接続文字列を変更します。

*される appsettings.json*ファイル、ContosoUniversity2 を接続文字列でデータベースの名前を変更します。

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

接続文字列でデータベース名を変更すると、新しいデータベースを作成する最初の移行がさせます。 その名前を持つ 1 つが存在しないために、新しいデータベースが作成されます。 接続文字列を変更するには必要ありません移行の概要です。

DB 名を変更する代わりには、データベースを削除しています。 使用して**SQL Server オブジェクト エクスプ ローラー** (SSOX) または`database drop`CLI コマンド。

 ```console
 dotnet ef database drop
 ```

次のセクションでは、CLI コマンドを実行する方法について説明します。

## <a name="create-an-initial-migration"></a>最初の移行を作成します。

プロジェクトをビルドします。

コマンド ウィンドウを開き、プロジェクトのフォルダーに移動します。 プロジェクト フォルダーに含まれる、 *Startup.cs*ファイル。

[コマンド] ウィンドウで、次を入力します。

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

メッセージで、移行が失敗した場合は"*... ファイルにアクセスできませんContosoUniversity.dll 別のプロセスによって使用されているためです。*" 表示されます。

* IIS Express を停止します。

   * 終了し、Visual Studio を再起動または
   * IIS Express アイコンは、Windows のシステム トレイにあります。
   * IIS Express のアイコンを右クリックし、をクリックして**ContosoUniversity > サイトの停止**です。

エラー メッセージ"ビルドが失敗した場合です。" 表示される場合、コマンドを再度実行します。 このエラーが発生する場合は、このチュートリアルの下部にあるメモにままにします。

### <a name="examine-the-up-and-down-methods"></a>上矢印を確認し、下矢印メソッド

EF コア コマンド`migrations add`からデータベースを作成するコードを生成します。 この移行コードは、*移行\<タイムスタンプ > _InitialCreate.cs*ファイル。 `Up`のメソッド、`InitialCreate`クラスは、データ モデルのエンティティ セットに対応するデータベース テーブルを作成します。 `Down`メソッドでは、それらが削除される次の例で示すようにします。

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

移行の呼び出し、`Up`移行のためのデータ モデルの変更を実装するメソッド。 更新プログラム、移行の呼び出しをロールバックするためのコマンドを入力すると、`Down`メソッドです。

上記のコードでは、初期の移行のためです。 そのコードが作成されたときに、`migrations add InitialCreate`コマンドを実行します。 移行の name パラメーター (この例では"InitialCreate") は、ファイル名に使用されます。 名前の移行は、任意の有効なファイル名を指定できます。 単語または語句、移行で行われている新機能をまとめたものを選択することをお勧めします。 たとえば、department テーブルを追加する移行という"AddDepartmentTable"

最初の移行が作成され、DB が終了した場合。

* DB 作成コードが生成されます。
* DB 作成コードは、DB に既にデータ モデルと一致するために実行する必要はありません。 場合は、DB 作成コードを実行すると、DB に既にデータ モデルと一致するので、変更を加えますしません。

新しい環境にアプリが展開されると、データベースを作成する DB 作成コードを実行する必要があります。

以前、DB の新しい名前を使用する接続文字列が変更されました。 指定されたデータベースが存在しないため、移行は、データベースを作成します。

### <a name="examine-the-data-model-snapshot"></a>データ モデルのスナップショットを確認します。

移行を作成、*スナップショット*で現在のデータベースのスキーマの*Migrations/SchoolContextModelSnapshot.cs*:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

現在のデータベースのスキーマは、コードで表される、ための移行を作成するデータベースと対話する EF コアがありません。 移行を追加すると、EF コアは、スナップショット ファイルにデータ モデルを比較することによって変更内容を判断します。 EF コアは、データベースを更新する必要がある場合にのみ、DB とやり取りします。

スナップショット ファイルは、その作成元の移行での同期である必要があります。 という名前のファイルを削除することによって、移行を削除することはできません*\<タイムスタンプ > _\<migrationname > .cs*です。 そのファイルが削除された場合、残りのマイグレーションは、データベース スナップショット ファイルと同期です。 削除する追加の前回の移行を使用して、 [dotnet ef 移行削除](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove)コマンド。

## <a name="remove-ensurecreated"></a>Remove EnsureCreated

初期の開発のため、`EnsureCreated`コマンドが使用されました。 このチュートリアルでは、migrations を使用します。 `EnsureCreated`次の制限があります。

* 移行をバイパスし、データベースとスキーマを作成します。
* 移行テーブルは作成されません。
* できます*いない*の移行で使用します。
* テストや迅速なプロトタイプを作成、データベースは削除して再作成頻繁にします。

次の行を削除して`DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>開発中、DB に、移行を適用します。

コマンド ウィンドウで、データベースとテーブルを作成するには、次を入力します。

```console
dotnet ef database update
```

メモ: 場合、`update`コマンドは、「失敗したビルドします。」というエラーを返します。

* コマンドを再度実行します。
* 再度失敗した場合は、Visual Studio を終了し、実行、`update`コマンド。
* ページの下部にあるメッセージのままにします。

コマンドの出力がに似ていますが、`migrations add`コマンドを出力します。 上記のコマンドは、DB をセットアップする SQL コマンドのログが表示されます。 ログのほとんどは、次のサンプル出力で除外されます。

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

ログ メッセージの詳細のレベルを下げることができます、ログでレベルを変更、 *appsettings です。Development.json*ファイル。 詳細については、次を参照してください。[ログ記録の概要](xref:fundamentals/logging/index)です。

使用して**SQL Server オブジェクト エクスプ ローラー** DB の検査をします。 追加に注意してください、`__EFMigrationsHistory`テーブル。 `__EFMigrationsHistory`テーブルの追跡データベースに適用された移行の種類。 データを表示、 `__EFMigrationsHistory` 1 行の最初の移行データを示します。 CLI の例の出力の最後のログは、この行が作成される INSERT ステートメントを示しています。

アプリを実行して、すべてが適切に機能していることを確認します。

## <a name="appling-migrations-in-production"></a>実稼働環境での移行の適用

運用アプリにする必要がありますをお勧め**いない**呼び出す[Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_)アプリケーションの起動時にします。 `Migrate`サーバー ファームでのアプリから呼び出すできません必要があります。 たとえば、次のように、アプリが (アプリの複数のインスタンスを実行する) スケール アウトによる展開クラウドをされている場合です。

データベースの移行は、制御された方法で、展開の一部として行う必要があります。 実稼働データベースの移行方法は次のとおりです。

* 移行を使用して SQL スクリプトを作成して、環境で SQL スクリプトを使用します。
* 実行している`dotnet ef database update`制御された環境からです。

EF コアを使用して、`__MigrationsHistory`テーブルを参照してを実行するかどうか、移行が必要があります。 データベースが最新では、移行は実行されません。

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>コマンド ライン インターフェイス (CLI) とします。Package Manager Console (PMC)

移行を管理するためのツールを EF コアはから利用できます。

* .NET core CLI コマンド。
* Visual Studio での PowerShell コマンドレット**Package Manager Console** (PMC) ウィンドウです。

PMC を使用して一部の開発者が必要に応じて、このチュートリアルは CLI を使用する方法を示します。

PMC の EF コア コマンドが、 [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)パッケージです。 このパッケージに含まれる、 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage、ためをインストールする必要はありません。

**重要:**を編集して、CLI をインストールするものと同じパッケージではない、 *.csproj*ファイル。 この種類の名前で終わる`Tools`、終わる CLI パッケージ名とは異なり`Tools.DotNet`です。

CLI コマンドの詳細については、次を参照してください。 [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)です。

PMC コマンドの詳細については、次を参照してください。 [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)です。

## <a name="troubleshooting"></a>トラブルシューティング

ダウンロード、[この段階でのアプリを完成](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)です。

アプリには、次の例外が生成されます。

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

ソリューション: を実行`dotnet ef database update`

場合、`update`コマンドは、「失敗したビルドします。」というエラーを返します。

* コマンドを再度実行します。
* ページの下部にあるメッセージのままにします。

>[!div class="step-by-step"]
[前へ](xref:data/ef-rp/sort-filter-page)
[次へ](xref:data/ef-rp/complex-data-model)
