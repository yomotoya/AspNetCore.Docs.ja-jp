---
title: "ASP.NET Core 2.0 の概要"
author: rick-anderson
description: "ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。"
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: b5f1fb0de2776177374b8b4d5ea6041b0fc653a9
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-aspnet-core"></a>ASP.NET Core の概要

> [!NOTE]
> ここで説明する手順は最新バージョンの ASP.NET Core 用です。 以前のバージョンの概要については、 [このチュートリアルの 1.1 バージョン](xref:getting-started-1.1)を参照してください。

1. [.NET Core](https://www.microsoft.com/net/core/) をインストールします。

2. 新しい .NET Core プロジェクトを作成します。

   macOS と Linux では、ターミナル ウィンドウを開きます。 Windows では、コマンド プロンプトを開きます。 次のコマンドを入力します。

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. アプリを実行します。

    次のコマンドを使用してアプリを実行します。

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. [http://localhost:5000](http://localhost:5000) に移動します。

6. *Pages/About.cshtml* を開き、"Hello, world! サーバー上の時刻は @DateTime.Now です" というメッセージが表示されるようにページを修正します。

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. [http://localhost:5000/About](http://localhost:5000/About) を表示し、変更内容を確認します。

### <a name="next-steps"></a>次の手順

入門のチュートリアルについては、「[ASP.NET Core Tutorials](tutorials/index.md)」(ASP.NET Core のチュートリアル) を参照してください。

ASP.NET Core の概念とアーキテクチャの概要については、「[ASP.NET Core Introduction](index.md)」(ASP.NET Core の概要) と「[ASP.NET Core Fundamentals](fundamentals/index.md)」(ASP.NET Core の基礎) を参照してください。

ASP.NET Core アプリは、.NET Core または .NET Framework 基本クラス ライブラリおよびランタイムを使用できます。 詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。
