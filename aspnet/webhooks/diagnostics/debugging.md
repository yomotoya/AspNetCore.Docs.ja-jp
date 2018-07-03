---
uid: webhooks/diagnostics/debugging
title: ASP.NET Webhook のデバッグ |Microsoft Docs
author: rick-anderson
description: ASP.NET Webhook をデバッグする方法。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: ''
ms.openlocfilehash: 5c567fb51db008526f59e09fce5bb60b66f6e479
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389060"
---
# <a name="aspnet-webhooks-debugging"></a>ASP.NET Webhook のデバッグ  

## <a name="debugging-in-azure"></a>Azure でのデバッグ

Azure で実行中に、Web アプリケーションをデバッグするチュートリアルを参照してください[Visual Studio を使用して Azure App Service で web アプリのトラブルシューティング](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)します。

## <a name="debugging-with-source-and-symbols"></a>ソースとシンボルを使用したデバッグ

に加えて、独自のコードをデバッグするには、および実際には、Microsoft ASP.NET Webhook に直接デバッグすることはすべて .NET の。 これは、ローカルまたはリモートでデバッグするかどうかに関係なく動作します。 Visual Studio を選択して、ソースとシンボルの検索を最初に、構成**デバッグ**し**オプションと設定**します。 このようなオプションを設定します。

![オプションと設定](_static/SourceSymbols.png)

リンクを追加し、 [symbolsource.org](http://symbolsource.org)ソースとシンボルをダウンロードします。 移動して、**シンボル**上部のメニューのタブとシンボルの場所として、次を追加。

```
http://srv.symbolsource.org/pdb/Public
```

さらに、キャッシュ ディレクトリに短い名前であることを確認します。それ以外の場合、ファイル名は、シンボルを読み込むことができませんは長すぎます取得できます。 パスのサンプルに示します。

```
C:\SymCache
```

設定は、次のようになります。

![オプションのシンボル ファイルの場所の例](_static/SymSource.png)
