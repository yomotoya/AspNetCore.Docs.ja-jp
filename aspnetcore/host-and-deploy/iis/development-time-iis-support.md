---
title: "Visual Studio for ASP.NET Core の開発時 IIS サポート"
author: shirhatti
description: "ASP.NET Core アプリケーションが Windows Server の IIS の背後で実行されている場合に、そのデバッグのサポートを検出します。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 264be49e8aff72f913c22508150e424541d49fa0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Visual Studio for ASP.NET Core の開発時 IIS サポート

[Sourabh Shirhatti](https://twitter.com/sshirhatti) による投稿

この記事では、 Windows Server の IIS の背後で実行されている ASP.NET Core アプリケーションをデバッグするための、[Visual Studio](https://www.visualstudio.com/vs/) のサポートについて説明します。 このトピックでは、この機能を有効にして、プロジェクトの設定を処理します。

## <a name="prerequisites"></a>必須コンポーネント

* Visual Studio (2017/バージョン 15.3 以降)
* ASP.NET および Web 開発ワークロード*または* .NET Core クロスプラットフォームの開発ワークロード

## <a name="enable-iis"></a>IIS を有効にする

IIS を有効にします。 **[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。 **[インターネット インフォメーション サービス]**チェック ボックスをオンにします。

![[インターネット インフォメーション サービス] チェックボックスが黒い四角 (チェックマークではない) で選択され、一部の IIS 機能が有効であることが表示されている [Windows の機能]](development-time-iis-support/_static/enable_iis.png)

IIS のインストールには、再起動が必要とする場合は、システムを再起動します。

## <a name="enable-development-time-iis-support"></a>開発時 IIS サポートを有効にする

IIS がインストールされると、既存の Visual Studio インストールを変更する Visual Studio インストーラーを起動します。 インストーラーで、**[開発時 IIS サポート]** コンポーネントを選択します。 **[ASP.NET と Web 開発]** ワークロードの **[概要]** パネルに、このコンポーネントが省略可能なコンポーネントとして表示されます。 これにより、ASP.NET Core アプリケーションの実行に必要なネイティブの IIS モジュールである [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がインストールされます。

![Visual Studio の機能の変更: [ワークロード] タブが選択されています。 [Web & Cloud]\(Web とクラウド\) のセクションでは、[ASP.NET と Web 開発] パネルが選択されています。 概要パネルの省略可能な領域の右側には、チェック ボックスに関しては IIS をサポートする開発にかかる時間です。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>プロジェクトを構成する

新しい起動プロファイルを作成して、開発時 IIS サポートを追加します。 Visual Studio の**ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[プロパティ]** をクリックします。 **[デバッグ]** タブを選択します。**[起動]** ドロップダウンから **[IIS]**を選択します。 **[Launch browser]\(ブラウザーの起動\)** 機能が正しい URL で有効になっていることを確認します。

![[デバッグ] タブが選択された、プロジェクト プロパティのウィンドウ。 [プロファイル] と [起動] の設定は [IIS] に設定されます。 [Launch browser]\(ブラウザーの起動\) 機能は http://localhost/WebApplication2 のアドレスで有効になっています。 同じアドレスが、[匿名認証を有効にする] が有効な状態で [Web サーバー設定] の [アプリ URL] フィールドにも入力されます。](development-time-iis-support/_static/project_properties.png)

また、起動プロファイルを手動で追加、 [launchSettings.json](http://json.schemastore.org/launchsettings)アプリ内のファイル。

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

Visual Studio は、管理者として実行されていない場合、再起動を要求があります。 その場合は、Visual Studio を再起動します。

おめでとうございます!  この時点では、プロジェクトでは、開発時の IIS のサポートのためには構成されます。 

## <a name="additional-resources"></a>その他の技術情報

* [IIS を使用した Windows での ASP.NET Core のホスト](xref:host-and-deploy/iis/index)
* [ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)
