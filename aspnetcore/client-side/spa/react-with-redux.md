---
title: ASP.NET Core で React-with-Redux プロジェクト テンプレートを使用する
author: SteveSandersonMS
description: React with Redux と create-react-app 用の ASP.NET Core シングル ページ アプリケーション (SPA) プロジェクト テンプレートの使用を開始する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: dab3d20865250aae548bff4614e631dd7c73b46f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291485"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a>ASP.NET Core で React-with-Redux プロジェクト テンプレートを使用する

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> このドキュメントは、ASP.NET Core 2.0 に含まれている React-with-Redux プロジェクト テンプレートについて説明するものではありません。 手動で更新できる新しい React-with-Redux テンプレートについて説明するものです。 このテンプレートは、ASP.NET Core 2.1 に既定で含まれています。

::: moniker-end

更新された React-with-Redux プロジェクト テンプレートは、React、Redux、および [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) 規則を使用してリッチなクライアント側ユーザー インターフェイス (UI) を実装する ASP.NET Core アプリを開発する場合に、便利な開始点として利用できます。

プロジェクト作成コマンドを除き、React-with-Redux テンプレートに関するすべての情報は React テンプレートと同じです。 このプロジェクトの種類を作成するには、`dotnet new react` の代わりに `dotnet new reactredux` を実行します。 両方の React ベースのテンプレートに共通する機能の詳細については、[React テンプレートのドキュメント](xref:spa/react)を参照してください。
