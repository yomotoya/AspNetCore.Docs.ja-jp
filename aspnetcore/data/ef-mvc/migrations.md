---
title: "ASP.NET Core MVC と EF Core - 移行 - 4/10"
author: tdykstra
description: "このチュートリアルでは、ASP.NET Core MVC アプリケーションでデータ モデルの変更を管理するための EF Core の移行機能の使用を開始します。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/migrations
ms.openlocfilehash: fd466af8a73bf4c568fafe7e7fdcaa82021624da
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/31/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a>移行 - EF Core と ASP.NET Core MVC のチュートリアル (4/10)

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University のサンプル Web アプリケーションでは、Entity Framework Core と Visual Studio を使用して ASP.NET Core MVC Web アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](intro.md)をご覧ください。

このチュートリアルでは、データ モデルの変更を管理するための EF Core の移行機能の使用を開始します。 チュートリアルの後半では、データ モデルを変更するときに移行を追加します。

## <a name="introduction-to-migrations"></a>移行の概要

新しいアプリケーションを開発して、データ モデルが頻繁に変更される場合、モデルが変更されるたびに、モデルはデータベースと同期されなくなります。 データベースが存在しない場合にデータベースを作成するように Entity Framework を構成して、これらのチュートリアルを開始しました。 そのため、データ モデルを変更する (エンティティ クラスを追加、削除、または変更するか、DbContext クラスを変更する) たびに、データベースを削除することができ、EF でモデルに一致する新しいデータベースを作成し、テスト データを使ってシードを設定します。

このメソッドは、実稼働環境にアプリケーションを展開するまで、データベースとデータ モデルの同期の維持がうまく機能します。 実稼働環境でアプリケーションを実行している場合、通常、保持する必要があるデータが保存され、新しい列の追加などの変更を加えるたびにすべてが失われないようにする必要があります。 EF Core の移行機能は、新しいデータベースを作成する代わりに、EF でデータベース スキーマを更新できるようにすることで、この問題を解決します。

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>移行用の Entity Framework Core NuGet パッケージ

移行を使用するには、**パッケージ マネージャー コンソール** (PMC) またはコマンド ライン インターフェイス (CLI) を使用できます。  このチュートリアルでは、CLI コマンドを使用する方法を示します。 PMC については、[このチュートリアルの最後](#pmc)に説明します。

コマンド ライン インターフェイス (CLI) の EF ツールが [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) で提供されています。 このパッケージをインストールするには、以下のように、*.csproj* ファイルの `DotNetCliToolReference` コレクションにパッケージを追加します。 **注:** *.csproj* ファイルを編集して、このパッケージをインストールする必要があります。`install-package` コマンドやパッケージ マネージャー GUI を使用することはできません。 *.csproj* ファイルを編集するには、**[ソリューション エクスプローラー]** でプロジェクト名を右クリックし、**[ContosoUniversity.csproj の編集]** を選択します。

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(この例のバージョン番号は、チュートリアルが記述された時点で最新でした)。 

## <a name="change-the-connection-string"></a>接続文字列を変更する

*appsettings.json* ファイルで、接続文字列のデータベース名を ContosoUniversity2 に変更するか、使用しているコンピューターではまだ使用していない別の名前に変更します。

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

この変更では、最初の移行で新しいデータベースが作成されるように、プロジェクトを設定します。 これは移行の作業を開始するために必要ではありませんが、後でこの設定をお勧めする理由がわかります。

> [!NOTE]
> データベース名を変更する代わりに、データベースを削除することもできます。 **SQL Server オブジェクト エクスプローラー** (SSOX) または `database drop` CLI コマンドを使用します。
> ```console
> dotnet ef database drop
> ```
> 次のセクションでは、CLI コマンドを実行する方法について説明します。

## <a name="create-an-initial-migration"></a>初期移行を作成する

変更を保存し、プロジェクトをビルドします。 次に、コマンド ウィンドウを開き、プロジェクト フォルダーに移動します。 この操作を行う簡単な方法を次に示します。

* **[ソリューション エクスプローラー]** で、プロジェクトを右クリックし、コンテキスト メニューの **[エクスプローラーで開く]** を選びます。

  ![[エクスプローラーで開く] メニュー項目](migrations/_static/open-in-file-explorer.png)

* アドレス バーに「cmd」と入力して、Enter キーを押します。

  ![コマンド ウィンドウを開く](migrations/_static/open-command-window.png)

コマンド ウィンドウで次のコマンドを入力します。

```console
dotnet ef migrations add InitialCreate
```

コマンド ウィンドウに次のような出力が表示されます。

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> "*"dotnet-ef" コマンドに一致する実行可能ファイルが見つかりませんでした*" というエラー メッセージが表示された場合、トラブルシューティングのヘルプについては、[このブログ記事](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/)を参照してください。

"*別のプロセスによって使用されているため、ContosoUniversity.dll ファイルにアクセスできません。*" というエラー メッセージが表示された場合は、Windows のシステム トレイの IIS Express アイコンを見つけて右クリックし、**[ContosoUniversity]、[サイトの停止]** の順にクリックします。

## <a name="examine-the-up-and-down-methods"></a>Up および Down メソッドを確認する

`migrations add` コマンドを実行したときに、EF では最初からデータベースを作成するコードが生成されています。 このコードは、*\<タイムスタンプ>_InitialCreate.cs* という名前のファイルにある *[Migrations]* フォルダー内にあります。 `InitialCreate` クラスの `Up` メソッドでは、データ モデルのエンティティ セットに対応するデータベース テーブルを作成し、`Down` メソッドでは、次の例に示すようにそれらを削除します。

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

移行は、`Up` メソッドを呼び出して、移行のためのデータ モデルの変更を実装します。 更新をロールバックするためのコマンドを入力すると、移行が `Down` メソッドを呼び出します。

このコードは、`migrations add InitialCreate` コマンドを入力したときに作成された初期移行向けのものです。 移行の name パラメーター (この例では "InitialCreate") は、ファイル名に使用され、どのような名前でも付けることができます。 移行で行われている処理をまとめた単語または語句を選択することをお勧めします。 たとえば、後で移行に "AddDepartmentTable" という名前を付ける可能性があります。

データベースが既に存在するときに、初期移行を作成した場合、データベースの作成コードが生成されますが、データベースは既にデータと一致しているため、作成コードを実行する必要はありません。 データベースがまだ存在しない別の環境にアプリを展開する場合、データベースを作成するために、このコードが実行されるため、最初にテストを行うことをお勧めします。 これが、移行で新しいデータベースを最初から作成できるように、前の接続文字列でデータベースの名前を変更した理由です。

## <a name="examine-the-data-model-snapshot"></a>データ モデルのスナップショットを確認する

移行は、現在のデータベース スキーマの*スナップショット*を *Migrations/SchoolContextModelSnapshot.cs* 内にも作成します。 コードは次のようになります。

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

現在のデータベース スキーマはコードで表されるため、移行を作成するために、EF Core がデータベースとやり取りする必要はありません。 移行を追加するときに、EF は、スナップショット ファイルとデータ モデルを比較することによって変更内容を判断します。 EF は、データベースを更新する必要がある場合にのみ、データベースとやり取りします。 

スナップショット ファイルは、ファイルを作成する移行と同期され続ける必要があるため、*\<タイムスタンプ>_\<移行>.cs* という名前のファイルを削除するだけでは、移行を削除することはできません。 ファイルを削除すると、残りの移行はデータベースのスナップショット ファイルと同期されなくなります。 最後に追加された移行を削除するには、[dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) コマンドを使用します。

## <a name="apply-the-migration-to-the-database"></a>移行をデータベースに適用する

コマンド ウィンドウで、次のコマンドを入力してデータベースとデータベース内のテーブルを作成します。

```console
dotnet ef database update
```

コマンドからの出力は、データベースを設定する SQL コマンドのログを表示する以外は、`migrations add` コマンドと同様です。 次のサンプル出力では、ログのほとんどは省略されています。 ログ メッセージの詳細レベルを表示しない場合は、*appsettings.Development.json* ファイルでログ レベルを変更できます。 詳細については、[ログ記録の概要](xref:fundamentals/logging/index)に関するページを参照してください。

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

**SQL Server オブジェクト エクスプローラー**を使用して、最初のチュートリアルで行ったように、データベースを調べます。  データベースに適用されている移行を記録する __EFMigrationsHistory テーブルに追加があることがわかります。 テーブルのデータを表示すると、最初の移行の 1 行が表示されます。 (前の CLI の出力例の最後のログは、この行を作成する INSERT ステートメントを示しています)。

アプリケーションを実行して、すべてが以前と同じように動作することを確認します。

![Students インデックス ページ](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>コマンド ライン インターフェイス (CLI) とパッケージ マネージャー コンソール (PMC) の比較

移行を管理するための EF ツールは、.NET Core CLI コマンドから、または Visual Studio **パッケージ マネージャー コンソール** (PMC) ウィンドウの PowerShell コマンドレットから利用できます。 このチュートリアルでは、CLI の使用方法を示しますが、好みに応じて PMC を使用できます。

PMC コマンドの EF コマンドは、[Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) パッケージ内にあります。 このパッケージは既に [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) メタパッケージに含まれているため、インストールする必要はありません。

**重要:** このパッケージは、*.csproj* ファイルを編集することで、CLI 用にインストールするパッケージと同じものではありません。 `Tools.DotNet` で終わる CLI パッケージ名とは異なり、このパッケージの名前は `Tools` で終わります。

CLI コマンドの詳細については、「[.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)」を参照してください。 

PMC コマンドの詳細については、「[パッケージ マネージャー コンソール (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)」を参照してください。

## <a name="summary"></a>まとめ

このチュートリアルでは、最初の移行を作成して適用する方法が示されました。 次のチュートリアルでは、データ モデルを展開して、詳細なトピックについて確認します。 その途中で、追加の移行を作成して適用することになります。

>[!div class="step-by-step"]
[前へ](sort-filter-page.md)
[次へ](complex-data-model.md)  
