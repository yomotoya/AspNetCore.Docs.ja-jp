---
title: ASP.NET Core の概要
author: rick-anderson
description: ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 4a5a0cc5a5dab2171ab8ef43818185a4ee91af0e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912567"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>チュートリアル: ASP.NET Core の概要

このチュートリアルでは、.NET Core コマンド ライン インターフェイスを使用して ASP.NET Core Web アプリを作成する方法について説明します。 以下の方法について説明します。

> [!div class="checklist"]
> * Web アプリ プロジェクトを作成する。
> * ローカル HTTPS を有効にする。
> * アプリを実行します。
> * Razor ページを編集する。

最後に、作業用の Web アプリがご利用のローカル コンピューター上で実行されるようにします。

![Web アプリのホーム ページ](_static/home-page.png)


## <a name="prerequisites"></a>必須コンポーネント

* [!INCLUDE [](~/includes/2.1-SDK.md)] をインストールします。

## <a name="create-a-web-app-project"></a>Web アプリ プロジェクトを作成する

* コマンド シェルを開き、次のコマンドを入力します。

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

## <a name="enable-local-https"></a>ローカル HTTPS を有効にする

* HTTPS 開発証明書を信頼します。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  上記のコマンドでは、次のダイアログが表示されます。

  ![セキュリティ警告のダイアログ](_static/cert.png)

  開発証明書を信頼することに同意する場合は、**[はい]** を選択します。

# <a name="macostabmacos"></a>[macOS](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  上記のコマンドでは、次のメッセージが表示されます。

  *HTTPS 開発証明書の信頼が要求されました。証明書がまだ信頼されていない場合は、次のコマンドを実行します。* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`。  
  * このコマンドでは、システム キーチェーン上に証明書をインストールするためにご自分のパスワードを入力するよう求められる場合があります。
  
  パスワード:*

  開発証明書を信頼することに同意する場合は、パスワードを入力します。

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

  HTTPS 開発証明書を信頼する方法については、Linux ディストリビューションのドキュメントを参照してください。
   
---

## <a name="run-the-app"></a>アプリを実行する

* 次のコマンドを実行します。

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

* [https://localhost:5001](https://localhost:5001) を参照します。 **[Accept]\(承認\)** をクリックして、プライバシーとクッキーのポリシーを受け入れます。 このアプリで個人情報が保持されることはありません。

## <a name="edit-a-razor-page"></a>Razor ページを編集する

* *Pages/About.cshtml* を開き、次の強調表示されたマークアップでページを変更します。

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

* [https://localhost:5001/About](https://localhost:5001/About) を参照して、変更が表示されていることを確認します。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行う方法を学びました。

> [!div class="checklist"]
> * Web アプリ プロジェクトを作成する。
> * ローカル HTTPS を有効にする。
> * プロジェクトを実行します。
> * 変更を加えます。

ASP.NET Core の詳細については、次の概要を参照してください。

> [!div class="nextstepaction"]
> <xref:index>
