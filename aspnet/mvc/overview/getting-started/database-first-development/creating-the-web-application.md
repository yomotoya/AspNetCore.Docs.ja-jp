---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'EF Database First と ASP.NET MVC: Web アプリケーションとデータ モデルの作成 |Microsoft Docs'
author: tfitzmac
description: MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの化しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: a1c4e3365d320ee54d378de33a77666558a854d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398246"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>EF Database First と ASP.NET MVC: Web アプリケーションとデータ モデルを作成します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブル内の列に対応します。
> 
> シリーズのこの部分は、web アプリケーションを作成して、データベース テーブルに基づくデータ モデルの生成に焦点を当てます。


## <a name="create-a-new-aspnet-web-application"></a>新しい ASP.NET Web アプリケーションを作成します。

新しいソリューションまたはデータベース プロジェクトと同じソリューションで、Visual Studio で新しいプロジェクトを作成し、 **ASP.NET Web アプリケーション**テンプレート。 プロジェクトに名前を**ContosoSite**します。

![プロジェクトを作成します。](creating-the-web-application/_static/image1.png)

**[OK]** をクリックします。

新しい ASP.NET プロジェクト ウィンドウで、選択、 **MVC**テンプレート。 オフにすることができます、**クラウドでホスト**は後で、クラウドにアプリケーションを展開するために、ここではオプションです。 クリックして**OK**アプリケーションを作成します。

![mvc テンプレートを選択します。](creating-the-web-application/_static/image2.png)

既定のファイルとフォルダー、プロジェクトが作成されます。

このチュートリアルでは、Entity Framework 6 を使用します。 NuGet パッケージの管理 ウィンドウを使用してプロジェクトで Entity Framework のバージョンを再確認することができます。 必要に応じて、Entity Framework のバージョンを更新します。

![バージョンを表示します。](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>モデルを生成します。

Entity Framework モデルをデータベース テーブルから作成されます。 これらのモデルは、データを操作に使用するクラスです。 各モデルでは、ミラー データベースのテーブルとテーブルの列に対応するプロパティが含まれています。

右クリックし、**モデル**フォルダー、および選択**追加**と**新しい項目の**します。

![新しい項目を追加します。](creating-the-web-application/_static/image4.png)

新しい項目の追加 ウィンドウで、次のように選択します。**データ**左側のウィンドウで、 **ADO.NET Entity Data Model**から中央のウィンドウのオプション。 新しいモデル ファイルに名前**ContosoModel**します。

![モデルを作成します。](creating-the-web-application/_static/image5.png)

**[追加]** をクリックします。

Entity Data Model ウィザード、選択**データベースの EF デザイナー**します。

![データベースから生成します。](creating-the-web-application/_static/image6.png)

**[次へ]** をクリックします。

開発環境内で定義されているデータベースの接続があれば、事前選択されているこれらの接続のいずれかを表示可能性があります。 ただし、このチュートリアルの最初の部分で作成したデータベースへの新しい接続を作成します。 をクリックして、**新しい接続**ボタンをクリックします。

![データベースへの接続します。](creating-the-web-application/_static/image7.png)

接続のプロパティ ウィンドウで、作成されたデータベースのローカル サーバーの名前を指定します (ここで **(localdb) \ProjectsV12**)。 サーバーの名前を指定するには、利用可能なデータベースから、ContosoUniversityData を選択します。

![接続プロパティの設定](creating-the-web-application/_static/image8.png)

**[OK]** をクリックします。

適切な接続プロパティが表示されます。 Web.Config ファイルで接続の既定の名前を使用することができます。

![接続の設定](creating-the-web-application/_static/image9.png)

**[次へ]** をクリックします。

選択**テーブル**の 3 つすべてのテーブル モデルを生成します。

![テーブルを選択します。](creating-the-web-application/_static/image10.png)

**[完了]** をクリックします。

でセキュリティ警告が発生する場合は、選択**OK**テンプレートの実行を続行します。

モデルは、データベースのテーブルから生成され、プロパティと、テーブル間のリレーションシップを示すダイアグラムが表示されます。

![モデルのダイアグラム](creating-the-web-application/_static/image11.png)

今すぐ、Models フォルダーには、データベースから生成されたモデルに関連する多くの新しいファイルが含まれます。

![新しいモデル ファイルを表示します。](creating-the-web-application/_static/image12.png)

**ContosoModel.Context.cs**ファイルにはから派生したクラスが含まれています、 **DbContext**クラス、および各データベース テーブルに対応するモデル クラスのプロパティを提供します。 **Course.cs**、 **Enrollment.cs**、および**Student.cs**ファイルがデータベース テーブルを表すモデル クラスが含まれます。 スキャフォールディングを使用する場合は、モデル クラスとコンテキスト クラスの両方を使用します。

このチュートリアルで前に、プロジェクトをビルドします。 セクションでは、プロジェクトがビルドされていない場合は機能しませんが、次のセクションで、データ モデルに基づくコードが生成されます。

> [!div class="step-by-step"]
> [前へ](setting-up-database.md)
> [次へ](generating-views.md)
