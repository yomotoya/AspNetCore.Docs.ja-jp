---
title: ASP.NET Core の概要
author: rick-anderson
description: ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 4c899ff3c087f1b569521c6e2e8458fca01d6061
ms.sourcegitcommit: bdfba5e7575b2a786ef27c0edf688c7dbd09ee95
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2018
ms.locfileid: "52288643"
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



> [!NOTE]
> ASP.NET Core の目次の提案された新しい構造の有用性をテストしています。  現在または提案された目次で 7 つのトピックを探す演習をする時間がある場合は、[ここをクリックして、調査に参加してください](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82)。