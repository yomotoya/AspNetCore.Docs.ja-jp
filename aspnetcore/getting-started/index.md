---
title: ASP.NET Core の概要
author: rick-anderson
description: ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/18/2018
ms.locfileid: "38216214"
---
# <a name="get-started-with-aspnet-core"></a>ASP.NET Core の概要

::: moniker range=">= aspnetcore-2.1"

1. [!INCLUDE [](~/includes/2.1-SDK.md)] をインストールします。

2. ASP.NET Core プロジェクトを作成します。 コマンド シェルを開き、次のコマンドを入力します。

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]

3. HTTPS 開発証明書を信頼します。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. 次のようにアプリを実行します。

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. [http://localhost:5001](http://localhost:5001) を参照します。  **[Accept]\(承認\)** をクリックして、プライバシーとクッキーのポリシーを受け入れます。 このアプリで個人情報が保持されることはありません。

6. *Pages/About.cshtml* を開き、次の強調表示されたマークアップでページを変更します。

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. [http://localhost:5001/About](http://localhost:5001/About) を参照して、変更が表示されていることを確認します。

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. [!INCLUDE [](~/includes/net-core-sdk-download-link.md)] をインストールします。

2. 新しい ASP.NET Core プロジェクトを作成します。

   コマンド シェルを開きます。 次のコマンドを入力します。

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. 次のコマンドでアプリを実行します。

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. [http://localhost:5000](http://localhost:5000) を参照します。

5. *Pages/About.cshtml* を開き、"Hello, world! サーバー上の時刻は @DateTime.Now です" というメッセージが表示されるようにページを修正します。

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. [http://localhost:5000/About](http://localhost:5000/About) を参照して、変更を確認します。

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. [.NET の「All Downloads」](https://www.microsoft.com/net/download/all)から、SDK 1.0.4 の .NET Core **SDK インストーラー**をインストールします。

2. 新しい ASP.NET Core プロジェクト用のフォルダーを作成します。

   コマンド シェルを開きます。 次のコマンドを入力します。

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. コンピューターに新しい SDK バージョンがインストールされている場合は、1.0.4 SDK を選択する *global.json* ファイルを作成します。

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. 新しい ASP.NET Core プロジェクトを作成します。

   ```console
   dotnet new web
   ```

5. パッケージを復元します。

    ```console
    dotnet restore
    ```

6. アプリを実行します。

   ```console
   dotnet run
   ```

   必要に応じて、まず [dotnet run](/dotnet/core/tools/dotnet-run) コマンドでアプリを構築します。

7. `http://localhost:5000` を参照します。

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
