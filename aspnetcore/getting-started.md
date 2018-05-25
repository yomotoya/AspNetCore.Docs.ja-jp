---
title: ASP.NET Core の概要
author: rick-anderson
description: ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a>ASP.NET Core の概要

::: moniker range=">= aspnetcore-2.0"

1. [!INCLUDE[](~/includes/net-core-sdk-download-link.md)] をインストールします。

2. 新しい .NET Core プロジェクトを作成します。

   macOS と Linux では、ターミナル ウィンドウを開きます。 Windows では、コマンド プロンプトを開きます。 次のコマンドを入力します。

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. 次のコマンドでアプリを実行します。

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. [http://localhost:5000](http://localhost:5000) を参照します。

5. *Pages/About.cshtml* を開き、"Hello, world! サーバー上の時刻は @DateTime.Now です" というメッセージが表示されるようにページを修正します。

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. [http://localhost:5000/About](http://localhost:5000/About) を参照して、変更を確認します。

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. [.NET の「All Downloads」](https://www.microsoft.com/net/download/all)から、SDK 1.0.4 の .NET Core **SDK インストーラー**をインストールします。

2. 新しい .NET Core プロジェクト用のフォルダーを作成します。

   macOS と Linux では、ターミナル ウィンドウを開きます。 Windows では、コマンド プロンプトを開きます。

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. コンピューターに新しい SDK バージョンがインストールされている場合は、1.0.4 SDK を選択する *global.json* ファイルを作成します。

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. 新しい .NET Core プロジェクトを作成します。

   ```terminal
   dotnet new web
   ```

5. パッケージを復元します。

    ```terminal
    dotnet restore
    ```

6. アプリを実行します。

   ```terminal
   dotnet run
   ```

   必要に応じて、まず [dotnet run](/dotnet/core/tools/dotnet-run) コマンドでアプリを構築します。

7. `http://localhost:5000` を参照します。

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end