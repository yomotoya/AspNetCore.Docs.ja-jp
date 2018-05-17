---
title: ASP.NET Core の Razor ページの概要
author: rick-anderson
description: ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。 Razor ページは、ASP.NET Core の Web ワークロードで推奨されています。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 12/22/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: caf4376c0a02931eeec85e5067a082b37ef9da68
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>ASP.NET Core の Razor ページの概要

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。 ASP.NET Core で Web アプリ用の UI を構築する際に Razor ページを使用することをお勧めします。

このチュートリアルには 3 つのバージョンがあります。

* Windows 向け: 本チュートリアル
* MacOS 向け: [Visual Studio for Mac 向けの Razor ページの概要 ](xref:tutorials/razor-pages-mac/razor-pages-start)
* macOS、Linux、Windows 向け: [Visual Studio Code を使用する ASP.NET Core の Razor ページの概要](xref:tutorials/razor-pages-vsc/razor-pages-start)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Razor Web アプリの作成

* Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。
* 新しい ASP.NET Core Web アプリケーションを作成します。 プロジェクトに **RazorPagesMovie** という名前を付けます。 コードのコピー/貼り付けを行う際に名前空間が一致するように、プロジェクトに *RazorPagesMovie* という名前を付けることが重要です。
  ![新しい ASP.NET Core Web アプリケーション](../../mvc/razor-pages/index/_static/np.png)
* ドロップダウン リストで **[ASP.NET Core 2.0]** を選択してから、**[Web アプリケーション]** を選択します。

  [!INCLUDE [install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

以下のように、Visual Studio のテンプレートでスタート プロジェクトを作成します。

![ソリューション エクスプローラー](razor-pages-start/_static/se.png)

**F5** キーを押して、デバッグ モードでアプリを実行するか、**Ctrl + F5** キーを押して、デバッガーをアタッチせずに実行します。

![ホームまたはインデックス ページ](razor-pages-start/_static/home.png)

* Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。 アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。 これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。 localhost では、ローカル コンピューターからの Web 要求のみが処理されます。 Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。 上の図では、ポート番号は 5000 です。 アプリを実行する際には、別のポート番号が表示されます。
* **Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。 多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

> [!div class="step-by-step"]
> [次: モデルの追加](xref:tutorials/razor-pages/model)
