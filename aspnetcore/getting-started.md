---
title: "ASP.NET Core 2.0 の概要"
author: rick-anderson
description: "ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。"
keywords: "ASP.NET Core,チュートリアル,概要"
ms.author: riande
manager: wpickett
ms.date: 08/30/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: f7852f0dddb0585089f5ccd8f4c865f5b87b049b
ms.sourcegitcommit: fb518f856f31fe53c09196a13309eacb85b37a22
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/08/2017
---
# <a name="getting-started-with-aspnet-core"></a>ASP.NET Core の概要

> [!NOTE]
> ここで説明する手順は最新バージョンの ASP.NET Core 用です。 以前のバージョンの概要については、 [このチュートリアルの 1.1 バージョン](xref:getting-started-1.1)を参照してください。

1. [.NET Core](https://microsoft.com/net/core/) をインストールします。

2. 新しい .NET Core プロジェクトを作成します。

   macOS と Linux では、ターミナル ウィンドウを開きます。 Windows では、コマンド プロンプトを開きます。

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

### <a name="next-steps"></a>次のステップ

入門のチュートリアルについては、「[ASP.NET Core Tutorials](tutorials/index.md)」(ASP.NET Core のチュートリアル) を参照してください。

ASP.NET Core の概念とアーキテクチャの概要については、「[ASP.NET Core Introduction](index.md)」(ASP.NET Core の概要) と「[ASP.NET Core Fundamentals](fundamentals/index.md)」(ASP.NET Core の基礎) を参照してください。

ASP.NET Core アプリは、.NET Core または .NET Framework 基本クラス ライブラリおよびランタイムを使用できます。 詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。
