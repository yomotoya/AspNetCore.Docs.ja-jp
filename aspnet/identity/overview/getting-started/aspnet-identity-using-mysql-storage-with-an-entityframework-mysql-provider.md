---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: ': ASP.NET Identity EntityFramework MySQL プロバイダーでは (c#) での MySQL ストレージの使用 |Microsoft ドキュメント'
author: maumar
description: このチュートリアルでは、MySQL または EntityFramework (SQL クライアント プロバイダー) と ASP.NET Identity の既定のデータ ストレージ機構を置き換える方法を表示しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 6018b4f62f95f9abffece536f345d7a16d052aac
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873392"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>(C#) EntityFramework MySQL プロバイダーと MySQL の記憶域を使用して ASP.NET Id:
====================
によって[Maurycy Markowski](https://github.com/maumar)、 [Raquel Soares De Almeida](https://github.com/raquelsa)、 [Robert McMurray](https://github.com/rmcmurray)

> このチュートリアルの既定のデータ ストレージ機構を置換する方法を示します[ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) MySQL プロバイダーである EntityFramework (SQL クライアント プロバイダー) とします。


次のトピックは、このチュートリアルで説明されます。

- Azure で MySQL データベースの作成
- Visual Studio 2013 の MVC テンプレートを使用して MVC アプリケーションを作成します。
- MySQL データベース プロバイダーと連携する EntityFramework を構成します。
- 結果を確認するアプリケーションを実行します。

このチュートリアルの最後は格納 ASP.NET Id を持つ MVC アプリケーションが Azure でホストされている MySQL データベースを使用しています。

## <a name="creating-a-mysql-database-instance-on-azure"></a>Azure の MySQL データベース インスタンスを作成します。

1. ログインに、 [Azure ポータル](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409)です。
2. をクリックして**新規**クリックし、ページの下部にある**ストア**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. **アドオンと選択**ウィザードで、 **ClearDB MySQL データベース**、をクリックし、**次**フレームの下部にある矢印。  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. 既定値を保持**空き**の計画、および変更、**名前**に**IdentityMySQLDatabase**地域は、一番近いレベルを選択し、クリックして、 **次へ**フレームの下部にある矢印。  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. クリックして、**購買**データベースの作成を完了するチェック マークします。  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. データベースが作成された後をから管理できる、**アドオン** タブで、管理ポータル。 クリックして、データベースの接続情報を取得する**接続情報**ページの下部にあります。  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. 接続文字列をコピーして [コピー] ボタンをクリックすると、 **CONNECTIONSTRING**フィールドし、保存以外の場合は、MVC アプリケーションのこのチュートリアルの後半では、この情報を使用するは。  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>MVC アプリケーション プロジェクトを作成します。

チュートリアルのこのセクションの手順を完了するを最初にインストールする必要は[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)です。 Visual Studio がインストールされると、新しい MVC アプリケーション プロジェクトを作成するのに、次の手順を使用します。

1. Visual Studio 2103 を開きます。
2. をクリックして**新しいプロジェクト**から、**開始** ページで、またはする をクリックして、**ファイル**メニューし**新しいプロジェクト**:  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、展開**Visual c#** 、テンプレートの一覧でクリックして**Web**を選択し、 **のASP.NETWebアプリケーション**. プロジェクトの名前を付けます**IdentityMySQLDemo**  をクリックし、 **OK**:  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. **新しい ASP.NET プロジェクト**ダイアログで、選択、 **MVC**され、既定のオプションです。 この構成には**個々 のユーザー アカウント**の認証方法として。 をクリックして**OK**:  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>MySQL データベースを使用する EntityFramework を構成します。

### <a name="update-the-entity-framework-assembly-for-your-project"></a>プロジェクトの Entity Framework アセンブリを更新します。

Visual Studio 2013 のテンプレートから作成された MVC アプリケーションにはへの参照が含まれています、 [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework)をパッケージ化がありますが含まれている更新プログラムのリリース以降には、そのアセンブリへの重要なされてパフォーマンスの向上。 アプリケーションでこれらの最新の更新プログラムを使用するために、次の手順を使用します。

1. Visual Studio 2013 では、MVC プロジェクトを開きます。
2. をクリックして**ツール**をクリックし、**ライブラリ パッケージ マネージャー**、順にクリック**パッケージ マネージャー コンソール**:  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. **Package Manager Console** Visual Studio の下部に表示されます。 型&quot;**更新プログラム パッケージ EntityFramework** &quot; Enter キーを押します。  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>EntityFramework の MySQL プロバイダーをインストールします。

EntityFramework MySQL のデータベースに接続するためには、MySQL プロバイダーをインストールする必要があります。 ためには、開く、**パッケージ マネージャー コンソール**と型&quot; **Install-package MySql.Data.Entity Pre**&quot;、Enter キーを押します。

> [!NOTE]
> これは、アセンブリのプレリリース版であり、ようバグが含まれること。 実稼働環境でプレリリース バージョンのプロバイダーを使用する必要があります。


[展開の次のイメージをクリックします。]  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>アプリケーションの Web.config ファイルにプロジェクト構成の変更を加える

Entity Framework をインストールした MySQL プロバイダーを使用して構成するこのセクションでは、MySQL プロバイダー ファクトリを登録し、Azure から、接続文字列を追加します。

> [!NOTE]
> 次の例には、MySql.Data.dll の特定のアセンブリのバージョンが含まれます。 アセンブリのバージョンが変更された場合は、正しいバージョンで適切な構成設定を変更する必要があります。


1. Visual Studio 2013 で、プロジェクトの Web.config ファイルを開きます。
2. 次の構成の設定は、Entity Framework 用のデータベースの既定のプロバイダーおよびファクトリの定義を見つけます。

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. MySQL プロバイダーを使用する Entity Framework を構成するように、次の構成設定を置き換えます。 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. 検索、 &lt;connectionStrings&gt;セクションし、Azure でホストされている MySQL データベースの接続文字列を定義する次のコードに置き換えます (providerName 値がから変更されてもことに注意してください、元の)。

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>カスタムの MigrationHistory コンテキストを追加します。

Entity Framework Code First を使用して、 **MigrationHistory**テーブル モデルの変更を追跡して、データベース スキーマと概念スキーマの間の一貫性を確保します。 ただし、このテーブルは機能しません for MySQL 既定では、主キーが大きすぎるため。 このような状況を解決するには、そのテーブルのキーのサイズを縮小する必要があります。 これを行うには、次の手順に従います。

1. このテーブルのスキーマ情報をキャプチャ、 **HistoryContext**、その他の変更があります**DbContext**です。 という新しいクラス ファイルを追加するには、 **MySqlHistoryContext.cs**をプロジェクトにその内容を次のコードに置き換えます。

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. 次に、Entity Framework を使用して、変更を構成する必要は**HistoryContext**、いずれかの既定ではなくです。 これは、コード ベースの構成機能を活用することで実行できます。 という名前の新しいクラス ファイルを追加するには、 **MySqlConfiguration.cs**をプロジェクトとその内容を置き換えます。

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>ApplicationDbContext のカスタム EntityFramework 初期化子を作成します。

このチュートリアルで取り上げるは MySQL プロバイダーは、Entity Framework での移行がデータベースに接続するためにモデルの初期化子を使用する必要がありますので現在サポートしていません。 このチュートリアルは、MySQL インスタンスを使用して、Azure では、ため、カスタムの Entity Framework の初期化子を作成する必要があります。

> [!NOTE]
> Azure またはオンプレミスでホストされているデータベースを使用しているかどうかに SQL Server インスタンスに接続している場合、この手順は必要ありません。


MySQL のカスタム Entity Framework の初期化子を作成するには、次の手順を使用します。

1. という新しいクラス ファイルを追加**MySqlInitializer.cs**プロジェクト、および置換には次のコードの内容。 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. 開く、 **IdentityModels.cs**にある、プロジェクト ファイルを**モデル**ディレクトリ、し、次のファイルの内容に置き換えます。 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>アプリケーションを実行して、データベースの検証

前のセクションの手順を完了すると、データベースをテストする必要があります。 これを行うには、次の手順に従います。

1. キーを押して**ctrl キーを押しながら f5 キーを押して**をビルドして、web アプリケーションを実行します。
2. クリックして、**登録**ページの上部タブ。  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. 新しいユーザー名とパスワードを入力し、クリックして**登録**:  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. この時点で、ASP.NET Identity テーブルは、MySQL データベースにされ、ユーザーが登録されているし、アプリケーションにログインしています。  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>データを検証する MySQL Workbench ツールのインストール

1. インストール、 **MySQL Workbench**ツールから、 [MySQL がダウンロード ページ](http://dev.mysql.com/downloads/windows/installer/)
2. インストール ウィザード:**機能の選択**  タブで、 **MySQL Workbench** **アプリケーション**セクションです。
3. アプリを起動し、このチュートリアルの求めたで作成した Azure の MySQL データベース接続文字列データを使用して新しい接続を追加します。
4. 接続を確立すた後には、検査、 **ASP.NET Identity**で作成されたテーブル、 **IdentityMySQLDatabase です。**
5. すべての ASP.NET Identity のテーブルには、次の図に示すように、作成に必要なことがわかります。  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. 検査、 **aspnetusers**テーブルのインスタンスの新しいユーザーを登録すると、エントリを確認します。  
  
   [展開の次のイメージをクリックします。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
