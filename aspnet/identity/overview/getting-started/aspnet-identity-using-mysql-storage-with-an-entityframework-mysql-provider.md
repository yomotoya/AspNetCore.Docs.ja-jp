---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: EntityFramework MySQL プロバイダーでは (c#) での MySQL の Storage の使用 |Microsoft Docs'
author: maumar
description: このチュートリアルでは、MySQL または EntityFramework (SQL クライアント プロバイダー) で ASP.NET Identity の既定のデータ ストレージ機構を置き換える方法を紹介しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: ''
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 6b1d57c65cb4197d1b20175415ee73b3e81cb53f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383040"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: EntityFramework MySQL プロバイダー (c#) で MySQL ストレージを使用します。
====================
によって[Maurycy Markowski](https://github.com/maumar)、 [Raquel Soares De Almeida](https://github.com/raquelsa)、 [Robert McMurray](https://github.com/rmcmurray)

> このチュートリアルでは、既定のデータ ストレージ メカニズムを置き換える方法[ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) (SQL クライアント プロバイダー) を EntityFramework MySQL プロバイダーを使用します。


このチュートリアルでは、次のトピックを取り上げます。

- Azure で MySQL データベースの作成
- Visual Studio 2013 の MVC テンプレートを使用して MVC アプリケーションを作成します。
- MySQL データベース プロバイダーを使用する entity Framework を構成します。
- 結果を確認するアプリケーションを実行します。

このチュートリアルの終わりには、必要があります格納 ASP.NET Identity と MVC アプリケーションが Azure でホストされている MySQL データベースを使用しています。

## <a name="creating-a-mysql-database-instance-on-azure"></a>Azure で MySQL データベース インスタンスを作成します。

1. [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409) にログインします。
2. クリックして**新規**クリックして、ページの下部にある**ストア**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. **アドオンと選択**ウィザードで、 **ClearDB MySQL Database**、順にクリックします、**次**フレームの下部にある矢印。  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. 既定値を保持**Free**計画、変更、**名前**に**IdentityMySQLDatabase**は、最も近いリージョンを選択し、クリックして、 **次へ** 、フレームの下部にある矢印。  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. をクリックして、**購入**チェック マークをデータベースの作成を完了します。  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. データベースが作成されたら後からを管理できます、**アドオン** タブで、管理ポータル。 データベースの接続情報を取得するには、次のようにクリックします。**接続情報**ページの下部にあります。  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. コピー ボタンをクリックして、接続文字列をコピー、 **CONNECTIONSTRING** ; フィールドを付けて保存、MVC アプリケーションにこのチュートリアルの後半では、この情報を使用します。  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>MVC アプリケーション プロジェクトを作成します。

このチュートリアルのこのセクションで手順を完了する必要がありますをインストールする[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)します。 Visual Studio がインストールされると、新しい MVC アプリケーション プロジェクトを作成するのに、次の手順を使用します。

1. Visual Studio 2103 を開きます。
2. クリックして**新しいプロジェクト**から、**開始** ページで、またはする をクリックして、**ファイル**メニューをクリックし**新しいプロジェクト**:  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、展開**Visual c#** テンプレートの一覧でをクリックし、 **Web**、選び**のASP.NETWebアプリケーション**. プロジェクトに名前を**IdentityMySQLDemo**し**OK**:  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. **新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **MVC**され、既定のオプションはこの構成では**個々 のユーザー アカウント**認証方法として。 クリックして**OK**:  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>MySQL データベースを使用する entity Framework を構成します。

### <a name="update-the-entity-framework-assembly-for-your-project"></a>プロジェクトの Entity Framework のアセンブリを更新します。

Visual Studio 2013 のテンプレートから作成された MVC アプリケーションにはへの参照が含まれています、 [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework)をパッケージ化がありますを含む更新プログラムのリリース以来、そのアセンブリへの大幅なされましたパフォーマンスの向上。 アプリケーションでこれらの最新の更新プログラムを使用するには、次の手順を使用します。

1. Visual Studio 2013 では、MVC プロジェクトを開きます。
2. をクリックして**ツール**、 をクリックし、**ライブラリ パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**:  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. **パッケージ マネージャー コンソール**Visual Studio の下部に表示されます。 型&quot; **Update-package EntityFramework** &quot; Enter キーを押します。  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>EntityFramework MySQL プロバイダーをインストールします。

EntityFramework MySQL database に接続するためには、MySQL プロバイダーをインストールする必要があります。 これを行うには、開く、**パッケージ マネージャー コンソール**と種類&quot; **Install-package MySql.Data.Entity Pre**&quot;、し、Enter キーを押します。

> [!NOTE]
> これは、アセンブリのプレリリース版であり、バグを含めることができますようです。 実稼働環境では、プレリリース バージョンのプロバイダーを使用する必要があります。


[展開の次の画像をクリックします。]  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>アプリケーションの Web.config ファイルをプロジェクト構成を変更

このセクションでは、インストールした MySQL プロバイダーを使用する Entity Framework を構成するでは、MySQL プロバイダー ファクトリを登録し、Azure から接続文字列を追加します。

> [!NOTE]
> 次の例には、MySql.Data.dll の特定のアセンブリのバージョンが含まれています。 アセンブリのバージョンが変更された場合は、適切なバージョンを使用して、適切な構成設定を変更する必要があります。


1. Visual Studio 2013 で、プロジェクトの Web.config ファイルを開きます。
2. 次の構成の設定は、Entity Framework の既定のデータベース プロバイダーとファクトリ定義を見つけます。

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. MySQL プロバイダーを使用する Entity Framework を構成するように、次の構成設定を置き換えます。 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. 検索、 &lt;connectionStrings&gt;セクションし、Azure でホストされている MySQL データベースの接続文字列を定義する次のコードに置き換えます (providerName 値がから変更されてもことに注意してください、元の)。

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>カスタムの MigrationHistory コンテキストを追加します。

Entity Framework Code First を使用して、 **MigrationHistory**テーブル モデルの変更を追跡して、データベース スキーマと概念スキーマの一貫性を確保します。 ただし、このテーブルは使えません MySQL 既定では、主キーが大きすぎるため。 このような状況を解決するにはそのテーブルのキーのサイズを縮小する必要があります。 これを行うには、次の手順を使用します。

1. このテーブルのスキーマ情報がキャプチャされた、 **HistoryContext**とその他の変更される**DbContext**します。 これを行うには、という名前の新しいクラス ファイルを追加**MySqlHistoryContext.cs**をプロジェクトにし、その内容を次のコードに置き換えます。

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. 次に、Entity Framework を使用して、変更された構成する必要が**HistoryContext**既定のものではなく。 これは、コード ベースの構成機能を活用することで実行できます。 これを行うには、という名前の新しいクラス ファイルを追加**MySqlConfiguration.cs**をプロジェクトに、内容に置き換えます。

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>ApplicationDbContext のカスタム EntityFramework の初期化子を作成します。

このチュートリアルでは機能を備えた MySQL プロバイダーは、Entity Framework の移行がデータベースに接続するためにモデルの初期化子を使用する必要があります現在サポートしていません。 このチュートリアルは Azure で MySQL インスタンスを使用しているため、カスタムの Entity Framework の初期化子を作成する必要があります。

> [!NOTE]
> Azure またはオンプレミスでホストされているデータベースを使用しているかどうかに SQL Server インスタンスに接続している場合、この手順は必要ありません。


Mysql カスタム Entity Framework の初期化子を作成するには、次の手順を使用します。

1. という名前の新しいクラス ファイルを追加**MySqlInitializer.cs**内容を次のコードは、プロジェクト、および置換します。 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. 開く、 **IdentityModels.cs**にあるプロジェクトのファイル、**モデル**ディレクトリに、次の内容に置き換えます。 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>アプリケーションを実行して、データベースの検証

前のセクションで手順を完了すると後、データベースをテストする必要があります。 これを行うには、次の手順を使用します。

1. キーを押して**ctrl キーを押しながら f5 キーを押して**をビルドして、web アプリケーションを実行します。
2. をクリックして、**登録**ページ上部にあるタブ。  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. 新しいユーザー名とパスワードを入力し、クリックして**登録**:  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. この時点で ASP.NET Identity のテーブルが、MySQL データベースを作成し、ユーザーが登録されているし、アプリケーションにログインしています。  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>データを確認するツールを MySQL Workbench のインストール

1. インストール、 **MySQL Workbench**ツールから、 [MySQL のダウンロード ページ](http://dev.mysql.com/downloads/windows/installer/)
2. インストール ウィザード:**機能の選択** タブで  **MySQL Workbench** **アプリケーション**セクション。
3. アプリを起動し、このチュートリアルの求めに作成した Azure MySQL データベースからの接続文字列データを使用して新しい接続を追加します。
4. 接続を確立した後は、検査、 **ASP.NET Identity**で作成されたテーブル、 **IdentityMySQLDatabase します。**
5. 次の図に示すように、テーブルが作成されたすべての ASP.NET Identity が必要なことが表示されます。  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. 検査、 **aspnetusers**インスタンスのテーブルに新しいユーザーを登録すると、エントリを確認します。  
  
   [展開の次の画像をクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
