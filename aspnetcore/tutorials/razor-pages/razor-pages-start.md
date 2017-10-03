---
title: "ASP.NET Core の Razor ページの概要"
author: rick-anderson
description: "ASP.NET Core の Razor ページの概要"
keywords: "ASP.NET Core,Razor ページ,Razor,MVC"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1d8d7805aafbf28fef044d09369a1dc76108b141
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a>ASP.NET Core の Razor ページの概要

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。 このチュートリアルを開始する前に、[Razor ページの概要](xref:mvc/razor-pages/index)に関するページを参照することをお勧めします。 ASP.NET Core で Web アプリケーション用の UI を構築する際に Razor ページを使用することをお勧めします。

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a>Razor Web アプリの作成

* Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。
* 新しい ASP.NET Core Web アプリケーションを作成します。 プロジェクトに **RazorPagesMovie** という名前を付けます。 コードのコピー/貼り付けを行う際に名前空間が一致するように、プロジェクトに *RazorPagesMovie* という名前を付けることが重要です。
  ![新しい ASP.NET Core Web アプリケーション](../../mvc/razor-pages/index/_static/np.png)
* ドロップダウン リストで [**ASP.NET Core 2.0**] を選択してから、**[Web アプリケーション]** を選択します。
  ![Web アプリケーション (Razor ページ)](../../mvc/razor-pages/index/_static/np2.png)

以下のように、Visual Studio のテンプレートでスタート プロジェクトを作成します。

![ソリューション エクスプローラー](razor-pages-start/_static/se.png)

**F5** キーを押して、デバッグ モードでアプリを実行するか、**Ctrl + F5** キーを押して、デバッガーをアタッチせずに実行します。

![ホームまたはインデックス ページ](razor-pages-start/_static/home.png)

* Visual Studio で [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。 アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。 これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。 localhost では、ローカル コンピューターからの Web 要求のみが処理されます。 Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。 上の図では、ポート番号は 5000 です。 アプリを実行する際には、別のポート番号が表示されます。
* **Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。 多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[次: モデルの追加](xref:tutorials/razor-pages/modelz)
