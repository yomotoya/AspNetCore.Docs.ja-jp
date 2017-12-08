---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: "ASP.NET MVC で最初の EF データベース: Web アプリケーションとデータ モデルの作成 |Microsoft ドキュメント"
author: tfitzmac
description: "MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの seri しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: f495bfa3aa5332e4ca3e44c2ffbfb760fbbeafc8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>ASP.NET MVC で最初の EF データベース: Web アプリケーションとデータ モデルを作成します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの系列では、自動的にユーザーを表示、編集、作成するにようにコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブルの列に対応します。
> 
> 系列のこの部分は、web アプリケーションを作成して、データベース テーブルに基づくデータ モデルの生成について説明します。


## <a name="create-a-new-aspnet-web-application"></a>新しい ASP.NET Web アプリケーションを作成します。

新しいソリューションまたはデータベース プロジェクトと同じソリューションで、Visual Studio で新しいプロジェクトを作成し、選択、 **ASP.NET Web アプリケーション**テンプレート。 プロジェクトに名前を**ContosoSite**です。

![プロジェクトを作成します。](creating-the-web-application/_static/image1.png)

**[OK]** をクリックします。

新しい ASP.NET プロジェクト ウィンドウで、選択、 **MVC**テンプレート。 オフにすることができます、**クラウド内のホスト**後でアプリケーションをクラウドに展開するために、ここではオプションです。 をクリックして**OK**アプリケーションを作成します。

![mvc テンプレートを選択します。](creating-the-web-application/_static/image2.png)

プロジェクトには、既定のファイルとフォルダーが作成されます。

このチュートリアルでは、Entity Framework 6 を使用します。 [NuGet パッケージの管理] ウィンドウから、プロジェクトの Entity Framework のバージョンを再確認してくださいことができます。 必要に応じて、Entity Framework のバージョンを更新します。

![バージョンを表示します。](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>モデルを生成します。

Entity Framework モデルをデータベース テーブルから作成されます。 これらのモデルは、データを操作に使用するクラスです。 各モデルでは、ミラー化データベースのテーブルと、テーブル内の列に対応するプロパティが含まれています。

右クリックし、**モデル**フォルダー、および選択**追加**と**新しい項目の**します。

![新しい項目を追加します。](creating-the-web-application/_static/image4.png)

[新しい項目の追加] ウィンドウで、次のように選択します。**データ**左側のウィンドウで、 **ADO.NET エンティティ データ モデル**中央のペインにあるオプションからです。 新しいモデルのファイルの名前を付けます**ContosoModel**です。

![モデルを作成します。](creating-the-web-application/_static/image5.png)

**[追加]**をクリックします。

エンティティ データ モデル ウィザードで、次のように選択します。**データベースから EF Designer**です。

![データベースから生成します。](creating-the-web-application/_static/image6.png)

**[次へ]**をクリックします。

場合は、開発環境内で定義されているデータベースの接続がある場合は、事前に選択したこれらの接続のいずれかを参照してください可能性があります。 ただし、このチュートリアルの最初の部分で作成したデータベースへの新しい接続を作成します。 クリックして、**新しい接続**ボタンをクリックします。

![データベースへの接続します。](creating-the-web-application/_static/image7.png)

接続のプロパティ ウィンドウで作成されたデータベースのローカル サーバーの名前を指定します (ここでは**(localdb) \ProjectsV12**)。 サーバー名を入力すた後には、使用可能なデータベースから、ContosoUniversityData を選択します。

![接続プロパティの設定](creating-the-web-application/_static/image8.png)

**[OK]** をクリックします。

正しい接続プロパティが表示されます。 Web.Config ファイルで接続の既定の名前を使用することができます。

![接続の設定](creating-the-web-application/_static/image9.png)

**[次へ]**をクリックします。

選択**テーブル**をすべて次の 3 つのテーブルのモデルを生成します。

![テーブルを選択します。](creating-the-web-application/_static/image10.png)

**[完了]**をクリックします。

セキュリティの警告を受信する場合は、選択**OK**テンプレートの実行を続行します。

モデルは、データベースのテーブルから生成され、プロパティと、テーブル間のリレーションシップを表示する図が表示されます。

![モデルのダイアグラム](creating-the-web-application/_static/image11.png)

Models フォルダーには、データベースから生成されたモデルに関連する多くの新しいファイルが含まれています。

![新しいモデル ファイルを表示します。](creating-the-web-application/_static/image12.png)

**ContosoModel.Context.cs**ファイルにはから派生するクラスが含まれています、 **DbContext**クラス、し、データベース テーブルに対応する各モデル クラスのプロパティを提供します。 **Course.cs**、 **Enrollment.cs**、および**Student.cs**ファイルには、データベースのテーブルを表すモデル クラスが含まれています。 スキャフォールディングを使用する場合は、モデルのクラスとコンテキスト クラスの両方を使用します。

このチュートリアルの前に、プロジェクトをビルドします。 次のセクションでは、プロジェクトがビルドされていない場合、セクションでは機能しませんが、そのデータ モデルに基づいたコードが生成されます。

>[!div class="step-by-step"]
[前へ](setting-up-database.md)
[次へ](generating-views.md)
