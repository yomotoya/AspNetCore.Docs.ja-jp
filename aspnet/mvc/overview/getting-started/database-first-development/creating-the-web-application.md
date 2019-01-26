---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'チュートリアル: 作成、ASP.NET MVC での最初のデータベース、Web アプリケーションと ef データ モデル'
description: この記事では、web アプリケーションを作成して、データベース テーブルに基づくデータ モデルの生成について説明します。
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 095d355866c9ab8fba3759f3e05e2a521992f3d6
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889770"
---
# <a name="tutorial-create-the-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a>チュートリアル: 作成、ASP.NET MVC での最初のデータベース、Web アプリケーションと ef データ モデル

 MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブル内の列に対応します。

この記事では、web アプリケーションを作成して、データベース テーブルに基づくデータ モデルの生成について説明します。

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * ASP.NET Web アプリを作成する
> * モデルを生成します。

## <a name="prerequisites"></a>必須コンポーネント

* [Entity Framework 6 Database First と MVC 5 の使用の概要](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a>ASP.NET Web アプリを作成する

新しいソリューションまたはデータベース プロジェクトと同じソリューションで、Visual Studio で新しいプロジェクトを作成し、 **ASP.NET Web アプリケーション**テンプレート。 プロジェクトに名前を**ContosoSite**します。

![プロジェクトを作成します。](creating-the-web-application/_static/image1.png)

**[OK]** をクリックします。

新しい ASP.NET プロジェクト ウィンドウで、選択、 **MVC**テンプレート。 オフにすることができます、**クラウドでホスト**は後で、クラウドにアプリケーションを展開するために、ここではオプションです。 クリックして**OK**アプリケーションを作成します。

既定のファイルとフォルダー、プロジェクトが作成されます。

このチュートリアルでは、Entity Framework 6 を使用します。 NuGet パッケージの管理 ウィンドウを使用してプロジェクトで Entity Framework のバージョンを再確認することができます。 必要に応じて、Entity Framework のバージョンを更新します。

![バージョンを表示します。](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>モデルを生成します。

Entity Framework モデルをデータベース テーブルから作成されます。 これらのモデルは、データを操作に使用するクラスです。 各モデルでは、ミラー データベースのテーブルとテーブルの列に対応するプロパティが含まれています。

右クリックし、**モデル**フォルダー、および選択**追加**と**新しい項目の**します。

新しい項目の追加 ウィンドウで、次のように選択します。**データ**左側のウィンドウで、 **ADO.NET Entity Data Model**から中央のウィンドウのオプション。 新しいモデル ファイルに名前**ContosoModel**します。

**[追加]** をクリックします。

Entity Data Model ウィザード、選択**データベースの EF デザイナー**します。

**[次へ]** をクリックします。

開発環境内で定義されているデータベースの接続があれば、事前選択されているこれらの接続のいずれかを表示可能性があります。 ただし、このチュートリアルの最初の部分で作成したデータベースへの新しい接続を作成します。 をクリックして、**新しい接続**ボタンをクリックします。

接続のプロパティ ウィンドウで、作成されたデータベースのローカル サーバーの名前を指定します (ここで **(localdb) \Projects13**)。 サーバーの名前を指定するには、利用可能なデータベースから、ContosoUniversityData を選択します。

![接続プロパティの設定](creating-the-web-application/_static/image8.png)

**[OK]** をクリックします。

適切な接続プロパティが表示されます。 Web.Config ファイルでは、接続の既定の名前を使用できます。

**[次へ]** をクリックします。

Entity Framework の最新バージョンを選択します。

**[次へ]** をクリックします。

選択**テーブル**の 3 つすべてのテーブル モデルを生成します。

**[完了]** をクリックします。

でセキュリティ警告が発生する場合は、選択**OK**テンプレートの実行を続行します。

モデルは、データベースのテーブルから生成され、プロパティと、テーブル間のリレーションシップを示すダイアグラムが表示されます。

![モデルのダイアグラム](creating-the-web-application/_static/image11.png)

今すぐ、Models フォルダーには、データベースから生成されたモデルに関連する多くの新しいファイルが含まれます。

**ContosoModel.Context.cs**ファイルにはから派生したクラスが含まれています、 **DbContext**クラス、および各データベース テーブルに対応するモデル クラスのプロパティを提供します。 **Course.cs**、 **Enrollment.cs**、および**Student.cs**ファイルがデータベース テーブルを表すモデル クラスが含まれます。 スキャフォールディングを使用する場合は、モデル クラスとコンテキスト クラスの両方を使用します。

このチュートリアルで前に、プロジェクトをビルドします。 セクションでは、プロジェクトがビルドされていない場合は機能しませんが、次のセクションで、データ モデルに基づくコードが生成されます。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * ASP.NET web アプリの作成
> * モデルの生成

事前作成する方法については、次の記事には、データ モデルに基づくコードを生成します。
> [!div class="nextstepaction"]
> [ビューの生成](generating-views.md)