---
title: ASP.NET Core プロジェクトをトラブルシューティングします。
author: Rick-Anderson
description: ASP.NET Core プロジェクトでの警告とエラーについて説明し、トラブルシューティングを行います。
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: c72dd93f6ba705d7f03ade556c7a037dadeb6295
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938395"
---
# <a name="troubleshoot-aspnet-core-projects"></a>ASP.NET Core プロジェクトをトラブルシューティングします。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

次のリンクは、トラブルシューティングの指針を提供します。

* [Azure App Service での ASP.NET Core のトラブルシューティング](xref:host-and-deploy/azure-apps/troubleshoot)
* [IIS での ASP.NET Core のトラブルシューティング](xref:host-and-deploy/iis/troubleshoot)
* [Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)
* [NDC カンファレンス (ロンドン、2018): ASP.NET Core アプリケーションで問題の診断](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET ブログ: ASP.NET Core のパフォーマンスの問題のトラブルシューティング](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>.NET core SDK の警告

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>32 ビットと 64 ビット バージョンの .NET Core SDK の両方がインストールされています。

**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。

> 32 と 64 ビット版の両方、.NET Core SDK がインストールされます。 インストールされている 64 ビット バージョンからのテンプレートのみ 'c:\\Program Files\\dotnet\\sdk\\' が表示されます。

![警告メッセージを表示する OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/both32and64bit.png)

この警告が表示されるときに 32 ビット (x86) と 64 ビット (x64) 版の両方、 [.NET Core SDK](https://www.microsoft.com/net/download/all)がインストールされています。 両方のバージョンがインストールされている可能性がありますが、一般的な理由は次のとおりです。

* 最初、32 ビット コンピューターを使用して、.NET Core SDK インストーラーをダウンロードが間でコピーし、64 ビット コンピューターにインストールされていること。
* 別のアプリケーションで 32 ビットの .NET Core SDK がインストールされました。
* 間違ったバージョンがダウンロードされ、インストールされています。

この警告を回避するには、32 ビット .NET Core SDK をアンインストールします。 アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。 警告が発生した理由とその影響を理解している場合は、警告を無視できます。

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>複数の場所に、.NET Core SDK がインストールされています。

**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。

> .NET Core SDK は、複数の場所にインストールされます。 インストールされた SDK からのテンプレートのみ 'c:\\Program Files\\dotnet\\sdk\\' が表示されます。

![警告メッセージを表示する OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/multiplelocations.png)

.NET Core SDK の少なくとも 1 つのインストール ディレクトリの外部にある場合、このメッセージが表示*c:\\Program Files\\dotnet\\sdk\\*します。 通常は、.NET Core SDK が、MSI インストーラーではなくコピー/貼り付けを使用して、マシンにデプロイされている場合に発生します。

この警告を回避するには、32 ビット .NET Core SDK をアンインストールします。 アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。 警告が発生した理由とその影響を理解している場合は、警告を無視できます。

### <a name="no-net-core-sdks-were-detected"></a>.NET Core Sdk は検出されませんでした。

**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。

> .NET Core Sdk は検出されませんでした、環境変数 'PATH' に含まれていることを確認します。

![警告メッセージを表示する OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/NoNetCore.png)

この警告が表示されるときに、環境変数`PATH`コンピューターの任意の .NET Core 用 Sdk を指していません。 この問題を解決するには。

* インストールまたは .NET Core SDK がインストールされていることを確認します。
* いることを確認、`PATH`環境変数が、SDK がインストールされている場所を指します。 インストーラーが通常の設定、`PATH`します。
