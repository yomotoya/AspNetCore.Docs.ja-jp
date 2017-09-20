---
title: "Visual Studio for ASP.NET Core の開発時 IIS サポート"
author: shirhatti
description: "ASP.NET Core アプリケーションが Windows Server の IIS の背後で実行されている場合に、そのデバッグのサポートを検出します。"
keywords: "ASP.NET Core,インターネット インフォメーション サービス,iis,windows server,asp.net core モジュール,デバッグ"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: f531d90646b9d261c5fbbffcecd6ded9185ae292
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Visual Studio for ASP.NET Core の開発時 IIS サポート

[Sourabh Shirhatti](https://twitter.com/sshirhatti) による投稿

この記事では、 Windows Server の IIS の背後で実行されている ASP.NET Core アプリケーションをデバッグするための、[Visual Studio](https://www.visualstudio.com/vs/) のサポートについて説明します。 このトピックでは、手順に従ってこの機能を有効にし、プロジェクトを設定します。

## <a name="prerequisites"></a>必須コンポーネント

* Visual Studio (2017/バージョン 15.3 以降)
* ASP.NET および Web 開発ワークロード*または* .NET Core クロスプラットフォームの開発ワークロード

## <a name="enable-iis"></a>IIS を有効にする

システムで IIS を有効にします。 **[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。 **[インターネット インフォメーション サービス]**チェック ボックスをオンにします。

![[インターネット インフォメーション サービス] チェックボックスが黒い四角 (チェックマークではない) で選択され、一部の IIS 機能が有効であることが表示されている [Windows の機能]](development-time-iis-support/_static/enable_iis.png)

IIS のインストールで再起動が必要な場合は、システムを再起動します。

## <a name="enable-development-time-iis-support"></a>開発時 IIS サポートを有効にする

IIS をインストールしたら、Visual Studio インストーラーを起動して既存の Visual Studio のインストールを変更します。 インストーラーで、**[開発時 IIS サポート]** コンポーネントを選択します。 **[ASP.NET と Web 開発]** ワークロードの **[概要]** パネルに、このコンポーネントが省略可能なコンポーネントとして表示されます。 これにより、ASP.NET Core アプリケーションの実行に必要なネイティブの IIS モジュールである [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がインストールされます。

![Visual Studio の機能の変更: [ワークロード] タブが選択されています。 [Web & Cloud]\(Web とクラウド\) のセクションでは、[ASP.NET と Web 開発] パネルが選択されています。 右側の [概要] パネルの [省略可能] 領域には、[開発時 IIS サポート] のチェックボックスがあります。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>プロジェクトを構成する

新しい起動プロファイルを作成して、開発時 IIS サポートを追加します。 Visual Studio の**ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[プロパティ]** をクリックします。 **[デバッグ]** タブを選択します。**[起動]** ドロップダウンから **[IIS]**を選択します。 **[Launch browser]\(ブラウザーの起動\)** 機能が正しい URL で有効になっていることを確認します。

![[デバッグ] タブが選択された、プロジェクト プロパティのウィンドウ。 [プロファイル] と [起動] の設定は [IIS] に設定されます。 [Launch browser]\(ブラウザーの起動\) 機能は http://localhost/WebApplication2 のアドレスで有効になっています。 同じアドレスが、[匿名認証を有効にする] が有効な状態で [Web サーバー設定] の [アプリ URL] フィールドにも入力されます。](development-time-iis-support/_static/project_properties.png)

または、起動プロファイルをアプリの [launchSettings.json](http://json.schemastore.org/launchsettings) ファイルに手動で追加することもできます。

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

管理者として実行していなかった場合は、Visual Studio の再起動を求められる場合があります。 その場合は、Visual Studio を再起動します。

おめでとうございます!  これで、開発時 IIS サポートのためのプロジェクトが構成されました。 

## <a name="additional-resources"></a>その他の技術情報

* [IIS を使用した Windows での ASP.NET Core のホスト](xref:publishing/iis)
* [ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core モジュール構成リファレンス](xref:hosting/aspnet-core-module)
