---
uid: webhooks/diagnostics/debugging
title: ASP.NET Webhook のデバッグ |Microsoft Docs
author: rick-anderson
description: ASP.NET Webhook をデバッグする方法。
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 25fa47202dd31d6ebd7a0e179075333108515274
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801989"
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
