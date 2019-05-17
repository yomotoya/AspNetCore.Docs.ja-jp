---
title: ASP.NET Core 用の認証のサンプル
author: rick-anderson
description: ASP.NET Core リポジトリでの認証のサンプルへのリンクを提供します。
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 7b3c911d60ad4737ebd12ce6f7628ad624b11658
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897159"
---
# <a name="authentication-samples-for-aspnet-core"></a>ASP.NET Core 用の認証のサンプル

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

[ASP.NET Core リポジトリ](https://github.com/aspnet/AspNetCore)の*AspNetCore/src/Security/samples*フォルダーには次の通り認証についてのサンプルが含まれています 。

* [クレームの変換](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Cookie 認証](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [カスタム ポリシー プロバイダー - IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [動的な認証スキームとオプション](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [外部クレーム](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [要求に基づくCookieと別の認証スキームの選択](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [静的ファイルへのアクセス制限](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>サンプルの実行

* [ブランチ](https://github.com/aspnet/AspNetCore)を選択します。 例、`release/2.2`
* [ASP.NET Core リポジトリ](https://github.com/aspnet/AspNetCore)を複製またはダウンロードします。
* 複製したASP.NET Core リポジトリと一致するバージョンの[.NET Core SDK](https://www.microsoft.com/net/download/all)がインストールされていることを確認します。
* *AspNetCore/src/Security/samples*以下のサンプルに移動し、`dotnet run`でサンプルを実行します。
