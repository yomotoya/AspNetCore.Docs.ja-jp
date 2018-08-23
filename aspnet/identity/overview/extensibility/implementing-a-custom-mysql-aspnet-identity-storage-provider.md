---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: カスタム MySQL ASP.NET Identity ストレージ プロバイダーを実装する |Microsoft Docs
author: raquelsa
description: ASP.NET Identity は、拡張可能なシステムを独自の記憶域プロバイダーを作成して、アプリケーションを再動作せず、アプリケーションに組み込むことができます.
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 358b1a3b7277f21c63a1d395f2a5fce79bbe0d56
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831967"
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>カスタム MySQL ASP.NET Identity ストレージ プロバイダーを実装します。
====================
によって[Raquel Soares De Almeida](https://github.com/raquelsa)、 [Suhas Joshi](https://github.com/suhasj)、 [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity は、拡張可能なシステムを独自の記憶域プロバイダーを作成して、アプリケーションを再動作せず、アプリケーションに組み込むことができます。 このトピックでは、ASP.NET Identity の MySQL の記憶域プロバイダーを作成する方法について説明します。 カスタム ストレージ プロバイダーの作成の概要については、次を参照してください。[概要カスタム ストレージ プロバイダーの ASP.NET Identity の](overview-of-custom-storage-providers-for-aspnet-identity.md)します。
> 
> このチュートリアルを完了するには、Visual Studio 2013 with Update 2 が必要です。
> 
> このチュートリアルを行います。
> 
> - Azure で MySQL データベース インスタンスを作成する方法を示します。
> - MySQL クライアント ツール (MySQL Workbench) を使用してテーブルを作成し、Azure でデータベースをリモート管理する方法を示します。
> - MVC アプリケーション プロジェクトにカスタム実装を既定の ASP.NET Identity ストレージの実装を置き換える方法を示します。
> 
> このチュートリアルは Raquel Soares De Almeida と Rick Anderson によって書き込まれた最初 ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。 サンプル プロジェクトは、Identity 2.0 Suhas Joshi によって更新されました。 トピックは、Tom FitzMacken によって Identity 2.0 に更新されました。


## <a name="download-completed-project"></a>プロジェクトのダウンロードが完了しました

このチュートリアルの終わりには、MVC アプリケーション プロジェクトを Azure でホストされている MySQL database を使用する ASP.NET Id を持つがあります。

完了した MySQL ストレージ プロバイダーをダウンロードする[AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/)します。

## <a name="the-steps-you-will-perform"></a>実行する手順

このチュートリアルでは。

1. Azure で MySQL データベースを作成します。
2. ASP.NET Identity に MySQL でのテーブルを作成します。
3. MVC アプリケーションを作成し、MySQL プロバイダーを使用するように構成
4. アプリを実行する

このトピックでは、ASP.NET Identity と顧客の記憶域プロバイダーを実装する際に、意思決定のアーキテクチャは含まれません。 詳細については、次を参照してください。[概要カスタム ストレージ プロバイダーの ASP.NET Identity の](overview-of-custom-storage-providers-for-aspnet-identity.md)します。

## <a name="review-mysql-storage-provider-classes"></a>MySQL ストレージ プロバイダー クラスを確認してください。

MySQL の記憶域プロバイダーを作成する手順に進むには、前に、記憶域プロバイダーを構成するクラスを見てみましょう。 データベース操作を管理するクラスおよびクラスのユーザーとロールを管理するアプリケーションから呼び出される必要があります。

### <a name="storage-classes"></a>ストレージ クラス

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -ユーザーのプロパティが含まれています。
- [UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -追加、更新、またはユーザーを取得するための操作が含まれています。
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -ロールのプロパティが含まれています。
- [RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -追加、削除、更新、およびロールを取得するための操作が含まれています。

### <a name="data-access-layer-classes"></a>データ アクセス層のクラス

この例では、データ アクセス層のクラスは、テーブルを操作するための SQL ステートメントを含めることがただし、コードは、Entity Framework や NHibernate などのオブジェクト リレーショナル マッピング (ORM) を使用する必要があります。 具体的には、アプリケーションは、ORM 遅延読み込みとキャッシュのオブジェクトが含まれることがなくパフォーマンスの低下を発生することがあります。 詳細については、次を参照してください。[せず、Entity Framework、ASP.NET Identity 2.0 でしょうか。](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -MySQL データベースの接続とデータベースの操作を実行するためのメソッドが含まれています。 UserStore と RoleStore このクラスのインスタンスとインスタンスします。
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -ロールを格納するテーブルのデータベース操作が含まれています。
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -ユーザーの信頼性情報を格納するテーブルのデータベース操作が含まれています。
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -ユーザーのログイン情報を格納するテーブルのデータベース操作が含まれています。
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -どのロールに割り当てられているユーザーを格納するテーブルのデータベース操作が含まれています。
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -ユーザーを格納するテーブルのデータベース操作が含まれています。

## <a name="create-a-mysql-database-instance-on-azure"></a>Azure で MySQL データベース インスタンスを作成します。

1. [Azure Portal](https://manage.windowsazure.com/) にログインします。
2. クリックして **+ 新規**クリックして、ページの下部にある**ストア**します。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. **選択してアドオン**ウィザードで、 **ClearDB MySQL Database**  ダイアログ ボックスの右下にある次へ進む矢印をクリックします。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. 既定値を保持**Free**の計画し、変更、**名前**に**IdentityMySQLDatabase**します。 お近くのリージョンを選択し、[次へ] 矢印をクリックします。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. データベースの作成を完了するチェック マークをクリックします。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. データベースが作成されたら後からを管理できます、**アドオン** タブで、管理ポータル。   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. クリックして、データベース接続情報を取得できます**接続情報**ページの下部にあります。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. コピー ボタンをクリックして、接続文字列をコピーし、後で、MVC アプリケーションで使用できるように保存します。   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>ASP.NET Identity に MySQL データベース内のテーブルを作成します。

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>接続し、MySQL データベースを管理するツールを MySQL Workbench をインストールします。

1. インストール、 **MySQL Workbench**ツールから、 [MySQL のダウンロード ページ](http://dev.mysql.com/downloads/windows/installer/)
2. アプリを起動し、[追加] をクリックして、 **MySQLConnections +** ボタンをクリックして新しい接続を追加します。 このチュートリアルで先ほど作成した Azure MySQL データベースからコピーした接続文字列のデータを使用します。
3. 接続を確立した後、新しく開きます**クエリ**タブ; からのコマンドを貼り付けます[MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql)をクエリにして、データベース テーブルを作成するために実行します。
4. 次に示すように、Azure でホストされている MySQL データベースに作成されたすべての ASP.NET Identity 必要なテーブルがあるようになりました。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>テンプレートから MVC アプリケーション プロジェクトを作成し、MySQL プロバイダーを使用するように構成します。

必要な場合、いずれかをインストール[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) with Update 2。

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>CodePlex から ASP.NET.Identity.MySQL プロジェクトをダウンロードします。

1. リポジトリの URL を参照[AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/)します。
2. ソース コードをダウンロードします。
3. ローカル フォルダーに .zip ファイルを抽出します。
4. 開く AspNet.Identity.MySQL ソリューションをビルドします。

### <a name="create-a-new-mvc-application-project-from-template"></a>テンプレートから新しい MVC アプリケーション プロジェクトを作成します。

1. 右クリックして、 **AspNet.Identity.MySQL**ソリューションと**追加**、**新しいプロジェクト**
2. **新しいプロジェクトの追加**ダイアログの  **Visual c#** し、左側の**Web**選び**ASP.NET Web アプリケーション**します。 プロジェクトに名前を**IdentityMySQLDemo**; [ok] をクリックします。  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. **新しい ASP.NET プロジェクト**ダイアログ ボックスで、既定のオプションで MVC テンプレートを選択します (を含む**個々 のユーザー アカウント**認証方法として) をクリック**OK**.![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. ソリューション エクスプ ローラーで IdentityMySQLDemo プロジェクトを右クリックし、選択**NuGet パッケージの管理**します。 検索テキスト ボックス ダイアログ ボックスで次のように入力します。 **Identity.EntityFramework**します。 結果の一覧でこのパッケージを選択し、をクリックして**アンインストール**します。 Entity Framework の依存関係パッケージをアンインストールするように促されます。 このアプリケーションでこのパッケージは不要になったように [はい] をクリックします。
5. IdentityMySQLDemo プロジェクトを右クリックして、選択**追加**、**の参照をソリューション、プロジェクト**AspNet.Identity.MySQL プロジェクトを選択し、をクリックして**OK**します。
6. IdentityMySQLDemo プロジェクトですべての参照を置き換える  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   代入  
     `using AspNet.Identity.MySQL;`
7. IdentityModels.cs、設定**ApplicationDbContext**から派生する**MySqlDatabase**接続名に 1 つのパラメーターを受け取るコンス トラクターを含めるとします。  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. IdentityConfig.cs ファイルを開きます。 **ApplicationUserManager.Create**メソッド、UserManager をインスタンス化、次のコードで置き換えます。  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Web.config ファイルを開き、DefaultConnection 文字列を強調表示されている値を前の手順で作成した MySQL データベースの接続文字列に置き換えて、このエントリに置き換えます。  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>アプリを実行し、MySQL DB に接続します。

1. 右クリックして、 **IdentityMySQLDemo**順に選択して**スタートアップ プロジェクトとして設定**
2. キーを押して**ctrl キーを押しながら f5 キーを押して**アプリをビルドして実行します。
3. をクリックして**登録**ページ上部にあるタブ。
4. 新しいユーザー名とパスワードを入力し、をクリックして**登録**します。  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. 今すぐ新しいユーザーが登録されログに記録します。  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. MySQL Workbench ツールに戻るし、検査、 **IdentityMySQLDatabase**テーブルの内容。 新しいユーザーを登録すると、ユーザー テーブルのエントリを確認します。  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>次の手順

このアプリで他の認証方法を有効にする方法の詳細についてを参照してください[Facebook と Google の OAuth2 や OpenID サインオンでの ASP.NET MVC 5 アプリケーションの作成](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)です。

ユーザー、アプリへのアクセスを制限するロールを設定して、DB OAuth と統合する方法についてを参照してください。[メンバーシップ、OAuth、SQL Database を使用したセキュリティで保護された ASP.NET MVC 5 アプリを Azure にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。
