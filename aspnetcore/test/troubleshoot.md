---
title: ASP.NET Core プロジェクトをトラブルシューティングします。
author: Rick-Anderson
description: ASP.NET Core プロジェクトでの警告とエラーについて説明し、トラブルシューティングを行います。
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: ae4e6f191d8f856de60ecf21cb882b5ee9b02064
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274594"
---
# <a name="troubleshoot-aspnet-core-projects"></a>ASP.NET Core プロジェクトをトラブルシューティングします。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

次のリンクは、トラブルシューティングのガイダンスを提供します。

* [Azure App Service での ASP.NET Core のトラブルシューティング](xref:host-and-deploy/azure-apps/troubleshoot)
* [IIS での ASP.NET Core のトラブルシューティング](xref:host-and-deploy/iis/troubleshoot)
* [Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)
* [ASP.NET Core アプリケーションでの問題の診断 NDC 会議 (ロンドン、2018)。](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET ブログ: ASP.NET Core パフォーマンス問題のトラブルシューティング](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>.NET core SDK 警告

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>32 ビットと 64 ビット バージョンの .NET Core SDK の両方がインストールされています。

**新しいプロジェクト**ダイアログ ASP.NET Core、表示される、次の警告。

> 両方 32 バージョンと 64 ビット バージョンの .NET Core SDK がインストールされます。 インストールされている 64 ビット版からのみテンプレート 'c:\\Program Files\\dotnet\\sdk\\' が表示されます。

![警告メッセージを示す OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/both32and64bit.png)

この警告を表示するに 32 ビット (x86) と 64 ビット (x64) バージョンの両方、 [.NET Core SDK](https://www.microsoft.com/net/download/all)がインストールされています。 両方のバージョンはインストールされている一般的な理由は次のとおりです。

* もともと 32 ビット コンピューターを使用して、.NET Core SDK インストーラーのダウンロードが間でそれをコピーし、64 ビット コンピューターにインストールします。
* 別のアプリケーションによって、32 ビットの .NET Core SDK がインストールされました。
* 間違ったバージョンがダウンロードされ、インストールします。

この警告を回避するのには、32 ビット .NET Core SDK をアンインストールします。 アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。 警告が発生した理由とその影響を理解している場合は、警告を無視できます。

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>複数の場所では、.NET Core SDK がインストールされています。

**新しいプロジェクト**ダイアログ ASP.NET Core、表示される、次の警告。

> .NET Core SDK は、複数の場所にインストールされます。 インストールされている SDK(s) からテンプレートのみ 'c:\\Program Files\\dotnet\\sdk\\' が表示されます。

![警告メッセージを示す OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/multiplelocations.png)

.NET Core SDK の少なくとも 1 つのインストール ディレクトリの外部にある場合にこのメッセージを参照してください*c:\\Program Files\\dotnet\\sdk\\*です。 通常は、.NET Core SDK は、MSI インストーラーではなくコピー/貼り付けを使用してコンピューターに配置されている場合に発生します。

この警告を回避するのには、32 ビット .NET Core SDK をアンインストールします。 アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。 警告が発生した理由とその影響を理解している場合は、警告を無視できます。

### <a name="no-net-core-sdks-were-detected"></a>.NET Core Sdk は検出されませんでした。

**新しいプロジェクト**ダイアログ ASP.NET Core、表示される、次の警告。

> .NET Core Sdk は検出されませんでした、環境変数 'PATH' に含まれていることを確認します。

![警告メッセージを示す OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/NoNetCore.png)

この警告を表示するには環境変数`PATH`コンピューターで、.NET Core の Sdk を指していません。 この問題を解決するには。

* インストールまたは .NET Core SDK がインストールされていることを確認します。
* 確認、`PATH`環境変数が SDK のインストール場所をポイントします。 インストーラーが通常の設定、`PATH`です。

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a>IHtmlHelper.Partial の使い方はアプリケーション デッドロックになる可能性があります。

ASP.NET Core 2.1 以降では、呼び出す`Html.Partial`デッドロックの可能性があるのため、アナライザー警告が表示されます。 警告メッセージは次のとおりです。

> IHtmlHelper.Partial を使用すると、アプリケーションにデッドロックの可能性があります。 使用を検討して`<partial>`タグ ヘルパーまたは`IHtmlHelper.PartialAsync`です。

呼び出す`@Html.Partial`で置き換える必要があります`@await Html.PartialAsync`または部分的なタグ ヘルパー`<partial name="_Partial" />`です。

::: moniker-end
