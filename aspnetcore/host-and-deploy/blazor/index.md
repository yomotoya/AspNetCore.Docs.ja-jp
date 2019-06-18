---
title: ASP.NET Core Blazor のホストと展開
author: guardrex
description: Blazor アプリをホストおよびデプロイする方法を説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 8a5ac5c58e7ceab07e55da8b61ebb01f7ac984bc
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153196"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a>ASP.NET Core Blazor のホストと展開

作成者: [Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com)、[Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>アプリの発行

アプリは、リリース構成での展開のために発行されます。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. **[ビルド]**  >  **[Publish {APPLICATION}]\({アプリケーション} を発行する\)** を選択します。
1. *[publish target]\(発行先\)* を選択します。 ローカルに発行するには、 **[フォルダー]** を選択します。
1. **[フォルダーの選択]** フィールド内で既定の場所を受け入れるか、または別の場所を指定します。 **[発行]** ボタンを選びます。

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code / .NET Core CLI](#tab/visual-studio-code+netcore-cli)

[dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドを使用して、リリース構成によってアプリを発行します。

```console
dotnet publish -c Release
```

---

アプリを発行すると、プロジェクトの依存関係の[復元](/dotnet/core/tools/dotnet-restore)がトリガーされ、展開されるアセットを作成する前にプロジェクトが[ビルド](/dotnet/core/tools/dotnet-build)されます。 ビルド プロセスの一環として、アプリのダウンロード サイズを縮小し読み込み時間を短縮するため、未使用のメソッドおよびアセンブリが削除されます。

Blazor クライアント側アプリは、" */bin/Release/{ターゲット フレームワーク}/publish/{アセンブリ名}/dist*" フォルダーに発行されます。 Blazor サーバー側アプリは、 */bin/Release/{TARGET FRAMEWORK}/publish* フォルダーに発行されます。

フォルダー内のアセットは、Web サーバーに展開されます。 展開のプロセスが手動であるか自動であるかは、ご使用の展開ツールによって異なります。

## <a name="deployment"></a>配置

展開のガイダンスについては、次のトピックを参照してください。

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a>Azure Storage を使った Blazor サーバーレス ホスティング

クライアント側 Blazor アプリは、ストレージ コンテナーから直接静的コンテンツとして、[Azure Storage](https://azure.microsoft.com/services/storage/) からサービスを提供できます。

詳細については、[クライアント側 ASP.NET Core Blazor のホストと展開 (スタンドアロン展開):Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage) に関するページをご覧ください。
