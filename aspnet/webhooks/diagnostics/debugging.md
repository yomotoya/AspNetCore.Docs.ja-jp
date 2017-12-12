---
uid: webhooks/diagnostics/debugging
title: "デバッグ ASP.NET Webhook |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET Webhook をデバッグする方法です。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 566ee353f6a947e3ef0efdfd0af3a81dff2147c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-debugging"></a>ASP.NET の Webhook のデバッグ  

## <a name="debugging-in-azure"></a>Azure でのデバッグ

Azure で実行中に、Web アプリケーションをデバッグするには、このチュートリアルに参照してください[Visual Studio を使用して Azure App service web アプリのトラブルシューティングを行う](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)です。

## <a name="debugging-with-source-and-symbols"></a>ソースとシンボルでのデバッグ

に加えて、独自のコードをデバッグするには、Microsoft ASP.NET の Webhook に直接と実際にデバッグすることはすべて .NET のです。 これは、ローカルまたはリモートでデバッグするかどうかに関係なく動作します。 まず、構成に移動して、ソースとシンボルを検索する Visual Studio**デバッグ**し**オプションと設定**です。 次のようにオプションを設定します。

![オプションと設定](_static/SourceSymbols.png)

リンクを追加[symbolsource.org](http://symbolsource.org)ソースとシンボルをダウンロードするためです。 移動して、**シンボル**タブ上のメニューのシンボルの場所として、以下を追加。

```
http://srv.symbolsource.org/pdb/Public
```

さらに、キャッシュ ディレクトリに短い名前があることを確認します。それ以外の場合、ファイル名を取得できますが長すぎますをロードしないシンボルが発生します。 サンプルのパスは次のとおりです。

```
C:\SymCache
```

設定は、次のようになります。

![オプションのシンボル ファイルの場所の例](_static/SymSource.png)
