---
title: ASP.NET Core 1.1 の概要
author: rick-anderson
description: ASP.NET Core 1.1 を使用して単純な Hello World アプリを作成し、実行するこの簡単なチュートリアルに従います。
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: c61a9a918e51bbd6c1f1142a04473393c8fc54ca
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="get-started-with-aspnet-core-11"></a>ASP.NET Core 1.1 の概要

> [!NOTE]
> ここで説明する手順は ASP.NET Core 1.1 用です。 最新バージョンについては、 [このチュートリアルの最新バージョン](xref:getting-started)を参照してください。

1. [.NET の「All Downloads」](https://www.microsoft.com/net/download/all)から、SDK 1.0.4 の .NET Core **SDK インストーラー**をインストールします。

2. 新しい .NET Core プロジェクト用のフォルダーを作成します。

   macOS と Linux では、ターミナル ウィンドウを開きます。 Windows では、コマンド プロンプトを開きます。

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. コンピューターに新しい SDK バージョンがインストールされている場合は、1.0.4 SDK を選択する *global.json* ファイルを作成します。

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. 新しい .NET Core プロジェクトを作成します。

   ```terminal
   dotnet new web
   ```
   
3.  パッケージを復元します。

    ```terminal
    dotnet restore
    ```

4. アプリを実行します。

   必要に応じて、まず [dotnet run](/dotnet/core/tools/dotnet-run) コマンドでアプリを構築します。

   ```terminal
   dotnet run
   ```

5. `http://localhost:5000` を参照します

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>次の手順

入門のチュートリアルについては、「[ASP.NET Core Tutorials](tutorials/index.md)」(ASP.NET Core のチュートリアル) を参照してください。

ASP.NET Core の概念とアーキテクチャの概要については、「[ASP.NET Core Introduction](index.md)」(ASP.NET Core の概要) と「[ASP.NET Core Fundamentals](fundamentals/index.md)」(ASP.NET Core の基礎) を参照してください。

ASP.NET Core アプリは、.NET Core または .NET Framework 基本クラス ライブラリおよびランタイムを使用できます。 詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。
