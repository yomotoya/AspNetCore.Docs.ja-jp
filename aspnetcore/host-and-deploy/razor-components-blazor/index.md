---
title: Razor Components および Blazor のホストと展開
author: guardrex
description: Razor Components と Blazor アプリをホストおよびデプロイする方法を説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/index
ms.openlocfilehash: eeaa27bef584523b77d8b47000b310fe0cac7341
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2019
ms.locfileid: "59070309"
---
# <a name="host-and-deploy-razor-components-and-blazor"></a>Razor Components および Blazor のホストと展開

作成者: [Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com)、[Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>アプリの発行

アプリは [dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドによって、リリース構成での展開のために発行されます。 `dotnet publish` コマンドの実行は、統合開発環境 (IDE) により組み込みの発行機能を使用して自動的に行われることがあります。そのため、ご使用の展開ツールによっては、このコマンドを手動でコマンド プロンプトから実行する必要がない場合があります。

```console
dotnet publish -c Release
```

`dotnet publish` により、プロジェクトの依存関係の[復元](/dotnet/core/tools/dotnet-restore)がトリガーされ、展開に使用されるアセットの作成前にプロジェクトが[ビルド](/dotnet/core/tools/dotnet-build)されます。 ビルド プロセスの一環として、アプリのダウンロード サイズを縮小し読み込み時間を短縮するため、未使用のメソッドおよびアセンブリが削除されます。 展開は、*/bin/Release/{ターゲット フレームワーク}/publish* フォルダーに作成されます。

*publish* フォルダー内のアセットは、Web サーバーに展開されます。 展開のプロセスが手動であるか自動であるかは、ご使用の展開ツールによって異なります。

## <a name="deployment"></a>配置

展開のガイダンスについては、次のトピックを参照してください。

* [クライアント側 Blazor](xref:host-and-deploy/razor-components-blazor/blazor)
* [サーバー側 ASP.NET Core Razor Components](xref:host-and-deploy/razor-components-blazor/razor-components)
