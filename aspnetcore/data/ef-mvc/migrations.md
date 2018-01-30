---
title: "ASP.NET MVC を持つコアを EF コアな移行 - 4 10"
author: tdykstra
description: "このチュートリアルでは、ASP.NET Core MVC アプリケーションでデータ モデルの変更を管理するための EF 中核となる移行機能の使用を開始します。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/migrations
ms.openlocfilehash: fd466af8a73bf4c568fafe7e7fdcaa82021624da
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a>移行 - ASP.NET Core MVC のチュートリアル (10 の 4) と EF コア

によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。

このチュートリアルでは、データ モデルの変更を管理するための EF 中核となる移行機能の使用を開始します。 後のチュートリアルでは、データ モデルを変更すると、複数の移行を追加します。

## <a name="introduction-to-migrations"></a>移行の概要

新しいアプリケーションを開発するときに、データ モデルの変更するたびにし、多くの場合、モデルの変更、データベースとの同期を取得します。 これらのチュートリアルを開始するには、Entity Framework が存在しない場合、データベースの作成を構成します。 データ モデルの変更--追加、削除、またはエンティティ クラスを変更または--DbContext クラスを変更するたびにデータベースを削除することができ、EF を新規作成、モデルと一致し、テスト データのシードを設定します。

データベースのデータ モデルとの同期を維持するには、このメソッドは、実稼働環境にアプリケーションを配置するまでに適切に動作します。 アプリケーションが、保持して、毎回すべてが失われるしたくないデータを格納するは、通常実稼働環境で実行されている場合は、新しい列を追加するなど変更を加えます。 EF コア移行機能を新しいデータベースを作成する代わりにデータベース スキーマを更新する EF を有効にしてこの問題を解決します。

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>移行用の Entity Framework Core NuGet パッケージ

移行を使用して作業を行うこともできます、 **Package Manager Console** (PMC) またはコマンド ライン インターフェイス (CLI)。  これらのチュートリアルでは、CLI コマンドを使用する方法を示します。 PMC に関するについては、「[このチュートリアルの最後の](#pmc)します。

コマンド ライン インターフェイス (CLI) の EF ツールが [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) で提供されています。 このパッケージをインストールする追加して、`DotNetCliToolReference`内のコレクション、 *.csproj*ファイルを示すようにします。 **注:** *.csproj* ファイルを編集して、このパッケージをインストールする必要があります。`install-package` コマンドやパッケージ マネージャー GUI を使用することはできません。 編集することができます、 *.csproj*でプロジェクト名を右クリックしてファイル**ソリューション エクスプ ローラー**を選択して**編集 ContosoUniversity.csproj**です。

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(この例では、バージョン番号は、このチュートリアルに書き込まれたときに現在でした)。 

## <a name="change-the-connection-string"></a>接続文字列を変更します。

*される appsettings.json*ファイル、ContosoUniversity2 または使用しているコンピューターで使用していない別の名前に、接続文字列にデータベースの名前を変更します。

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

この変更は、最初の移行が新しいデータベースを作成できるように、プロジェクトを設定します。 これは、移行には、使用を開始するために必要はありませんが、後述する理由をお勧めします。

> [!NOTE]
> データベース名を変更する代わりに、データベースを削除することができます。 使用して**SQL Server オブジェクト エクスプ ローラー** (SSOX) または`database drop`CLI コマンド。
> ```console
> dotnet ef database drop
> ```
> 次のセクションでは、CLI コマンドを実行する方法について説明します。

## <a name="create-an-initial-migration"></a>最初の移行を作成します。

変更を保存し、プロジェクトをビルドします。 コマンド ウィンドウを開き、プロジェクト フォルダーに移動します。 実行する簡単な方法を次に示します。

* **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**エクスプ ローラーで開く**コンテキスト メニュー。

  ![ファイル エクスプ ローラーのメニュー項目で開く](migrations/_static/open-in-file-explorer.png)

* アドレス バーに「cmd」を入力し、、Enter キーを押します。

  ![開いているコマンド ウィンドウ](migrations/_static/open-command-window.png)

コマンド ウィンドウで次のコマンドを入力します。

```console
dotnet ef migrations add InitialCreate
```

コマンド ウィンドウで次のような出力を参照してください。

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> エラー メッセージを表示する場合は*実行可能ファイルに一致するコマンド"dotnet-ef"が見つかりませんでした*を参照してください[このブログの投稿](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/)トラブルシューティングのヘルプ。

エラー メッセージを表示する場合は"*... ファイルにアクセスできませんContosoUniversity.dll 別のプロセスによって使用されているためです。*"] し、Windows システム トレイに、IIS Express のアイコンを検索し、右クリックして、[ **ContosoUniversity > Stop サイト**です。

## <a name="examine-the-up-and-down-methods"></a>上矢印を確認し、下矢印メソッド

実行すると、`migrations add`コマンド、EF は最初からデータベースを作成するコードを生成します。 このコードは、*移行*という名前のファイル内のフォルダー *\<タイムスタンプ > _InitialCreate.cs*です。 `Up`のメソッド、`InitialCreate`クラスは、データ モデルのエンティティ セットに対応するデータベース テーブルを作成し、`Down`メソッドでは、それらが削除される次の例で示すようにします。

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

移行の呼び出し、`Up`移行のためのデータ モデルの変更を実装するメソッド。 更新プログラム、移行の呼び出しをロールバックするためのコマンドを入力すると、`Down`メソッドです。

このコードの入力時に作成された最初の移行は、`migrations add InitialCreate`コマンド。 移行の name パラメーター (この例では"InitialCreate") は、ファイル名が使用され、希望することができます。 単語または語句、移行で行われている新機能をまとめたものを選択することをお勧めします。 たとえば、後で移行"AddDepartmentTable"の名前を付けて可能性があります。

データベースが既に存在する場合、最初の移行を作成した場合は、データベース作成コードが生成されますが、データベースに既にデータ モデルと一致するために実行する必要はありません。 ここで、データベースが存在しない、このコードはまだ実行されて、データベースを作成する別の環境にアプリを展開するときにこれは最初にテストすることをお勧めします。 理由です--前の接続文字列でデータベースの名前を変更した移行は、最初から新しい 1 つを作成できるようにします。

## <a name="examine-the-data-model-snapshot"></a>データ モデルのスナップショットを確認します。

また、移行を作成、*スナップショット*で現在のデータベース スキーマの*Migrations/SchoolContextModelSnapshot.cs*です。 そのコードは次のようになります。

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

現在のデータベース スキーマは、コードで表される、ための移行を作成するデータベースとやり取りする EF コアがありません。 移行を追加すると、EF は、スナップショット ファイルにデータ モデルを比較することによって変更内容を判断します。 EF は、データベースを更新する必要がある場合にのみ、データベースとやり取りします。 

スナップショット ファイルが作成して、という名前のファイルを削除するだけで、移行を削除することはできませんので、マイグレーションと同期して保持するが*\<タイムスタンプ > _\<migrationname > .cs*です。 そのファイルを削除すると、残りのマイグレーションは、データベース スナップショット ファイルと同期されます。 削除する追加した前回の移行を使用して、 [dotnet ef 移行削除](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove)コマンド。

## <a name="apply-the-migration-to-the-database"></a>移行をデータベースに適用します。

コマンド ウィンドウで、データベースとテーブルが作成するには、次のコマンドを入力します。

```console
dotnet ef database update
```

コマンドの出力がに似ていますが、`migrations add`を使用して、点を除いて、SQL コマンドのデータベースを設定するためにログを参照してください。 ログのほとんどは、次のサンプル出力で除外されます。 このレベルのログ メッセージで詳細を表示しないようにする場合は、ログ レベルを変更できます、 *appsettings です。Development.json*ファイル。 詳細については、次を参照してください。[ログ記録の概要](xref:fundamentals/logging/index)です。

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

使用して**SQL Server オブジェクト エクスプ ローラー**の最初のチュートリアルで行ったように、データベースを調査します。  移行の種類は、データベースに適用されているの追跡を __EFMigrationsHistory テーブルの追加がわかります。 そのテーブルにデータを表示し、1 行の最初の移行データが表示されます。 (前の CLI 出力例では、最後のログは、この行が作成される INSERT ステートメントを示しています)。

以前と同じを引き続き動作するすべてのものを確認するアプリケーションを実行します。

![インデックス ページの受講者](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>コマンド ライン インターフェイス (CLI) とします。Package Manager Console (PMC)

移行を管理することは .NET Core CLI コマンドとは、Visual Studio での PowerShell コマンドレットからの EF tooling **Package Manager Console** (PMC) ウィンドウです。 このチュートリアルは、CLI を使用する方法を示しますが、したい場合は、PMC を使用することができます。

PMC コマンドの EF コマンドが、 [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)パッケージです。 このパッケージが既に含まれている、 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage、ためをインストールする必要はありません。

**重要:**を編集して、CLI をインストールするものと同じパッケージはありません、 *.csproj*ファイル。 この種類の名前で終わる`Tools`、終わる CLI パッケージ名とは異なり`Tools.DotNet`です。

CLI コマンドの詳細については、次を参照してください。 [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)です。 

PMC コマンドの詳細については、次を参照してください。 [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)です。

## <a name="summary"></a>まとめ

このチュートリアルでは、作成して、最初の移行を適用する方法を説明しました。 次のチュートリアルでは、データ モデルを展開してより高度なトピックを見るが始めます。 その過程を作成し、追加の移行を適用します。

>[!div class="step-by-step"]
[前へ](sort-filter-page.md)
[次へ](complex-data-model.md)  
