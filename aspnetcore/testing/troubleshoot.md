---
title: ASP.NET Core をトラブルシューティングします。
author: Rick-Anderson
description: 理解し、警告および ASP.NET Core プロジェクトによるエラーのトラブルシューティングを行います。
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: f2c785bfe27ddd67db0313b8ee1c077a8cc06e05
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a>ASP.NET Core プロジェクトをトラブルシューティングします。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

次のリンクは、トラブルシューティングのガイダンスを提供します。

* [Azure App Service での ASP.NET Core のトラブルシューティング](xref:host-and-deploy/azure-apps/troubleshoot)
* [IIS での ASP.NET Core のトラブルシューティング](xref:host-and-deploy/iis/troubleshoot)
* [Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)
* [YouTube: ASP.NET Core アプリケーションでの問題を診断する](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a>.NET core SDK 警告

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>32 ビットと 64 ビット バージョンの .NET Core SDK の両方がインストールされています。
**新しいプロジェクト**ダイアログ ASP.NET Core、表示される、次の警告。 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![警告メッセージを示す OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/both32and64bit.png)

この警告を表示するに 32 ビット (x86) と 64 ビット (x64) バージョンの両方、 [.NET Core SDK](https://www.microsoft.com/net/download/all)がインストールされています。 両方のバージョンをインストールする一般的な理由は次のとおりです。

* 最初、32 ビット コンピューターを使用して、.NET Core SDK インストーラーのダウンロードが間でそれをコピーし、64 ビット コンピューターにインストールします。 
* 別のアプリケーションによって、32 ビットの .NET Core SDK がインストールされました。
* 間違ったバージョンがダウンロードされ、インストールします。

この警告を回避するのには、32 ビット .NET Core SDK をアンインストールします。 アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。 警告が発生した理由とその影響を理解している場合は、警告を無視できます。

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>複数の場所では、.NET Core SDK がインストールされています。
**新しいプロジェクト**ダイアログ ASP.NET Core の次の警告が表示。 

 .NET Core SDK は、複数の場所にインストールされます。 インストールされている SDK(s) からのみテンプレート ' C:\Program Files\dotnet\sdk\'が表示されます。

![警告メッセージを示す OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/multiplelocations.png)

.NET Core SDK の少なくとも 1 つのインストール ディレクトリの外部にある場合にこのメッセージを参照してください * C:\Program Files\dotnet\sdk\*です。 通常を .NET Core SDK は、MSI インストーラーではなくコピー/貼り付けを使用してコンピューターに配置されている場合に発生します。

この警告を回避するのには、32 ビット .NET Core SDK をアンインストールします。 アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。 警告が発生した理由とその影響を理解している場合は、警告を無視できます。

### <a name="no-net-core-sdks-were-detected"></a>.NET Core Sdk は検出されませんでした。
**新しいプロジェクト**ダイアログ ASP.NET Core の次の警告が表示。 

**.NET Core Sdk は検出されませんでした、環境変数 'PATH' に含まれていることを確認**

![警告メッセージを示す OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/NoNetCore.png)

この警告を表示するには環境変数`PATH`コンピューターで、.NET Core の Sdk を指していません。 この問題を解決するには。

* インストールまたは .NET Core SDK がインストールされていることを確認します。
* 確認、`PATH`環境変数が SDK のインストール場所をポイントします。 インストーラーが通常の設定、`PATH`です。