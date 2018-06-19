---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: カスタムの MySQL ASP.NET Identity の記憶域プロバイダーを実装する |Microsoft ドキュメント
author: raquelsa
description: ASP.NET Identity は、独自の記憶域プロバイダーを作成し、アプリケーションを再動作せず、アプリケーションに接続することにより、拡張可能なシステムがしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: d843b31e011fe520aad6cfdab0beca2d12477f12
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872739"
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>カスタムの MySQL ASP.NET Identity の記憶域プロバイダーの実装
====================
によって[Raquel Soares De Almeida](https://github.com/raquelsa)、 [Suhas Joshi](https://github.com/suhasj)、 [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity は、拡張可能なシステムを独自の記憶域プロバイダーを作成し、アプリケーションを再動作せず、アプリケーションに接続することができます。 このトピックでは、ASP.NET Identity の MySQL 記憶域プロバイダーを作成する方法について説明します。 カスタムの記憶域プロバイダーの作成の概要については、次を参照してください。[概要のカスタムの記憶域プロバイダー ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)です。
> 
> このチュートリアルを完了するには、Visual Studio 2013 Update 2 が必要です。
> 
> このチュートリアルを行います。
> 
> - Azure の MySQL データベース インスタンスを作成する方法を示します。
> - MySQL クライアント ツール (MySQL ワークベンチ) を使用してテーブルを作成し、Azure でリモート データベースを管理する方法を示します。
> - MVC アプリケーション プロジェクトのカスタム実装を既定の ASP.NET Identity の記憶域の実装を置換する方法を示します。
> 
> このチュートリアルは Raquel Soares De Almeida および Rick Anderson によって書き込まれて ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。 サンプル プロジェクトは、Identity 2.0 Suhas Joshi によって更新されました。 トピックは、Tom FitzMacken によって Identity 2.0 用に更新されました。


## <a name="download-completed-project"></a>ダウンロードが完了したプロジェクト

このチュートリアルの最後に、MVC アプリケーション プロジェクトを Azure でホストされている MySQL データベースを使用する ASP.NET Id を持つがあります。

完了した MySQL の記憶域プロバイダーをダウンロードする[AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/)です。

## <a name="the-steps-you-will-perform"></a>実行する手順

このチュートリアルでは説明します。

1. Azure の MySQL データベースを作成します。
2. MySQL の ASP.NET Identity テーブルを作成します。
3. MVC アプリケーションを作成し、MySQL プロバイダーを使用するように構成
4. アプリを実行する

このトピックでは、ASP.NET の Id と顧客の記憶域プロバイダーを実装する際に、決定のアーキテクチャは説明しません。 その情報を参照してください。[概要のカスタムの記憶域プロバイダー ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)です。

## <a name="review-mysql-storage-provider-classes"></a>MySQL の記憶域プロバイダーのクラスを確認します。

MySQL の記憶域プロバイダーを作成するステップにジャンプ、前に、記憶域プロバイダーを構成するクラスを見てみましょう。 データベース操作を管理するクラスおよびクラスのユーザーおよびロールを管理するアプリケーションから呼び出される必要があります。

### <a name="storage-classes"></a>ストレージ クラス

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -ユーザーのプロパティが含まれています。
- [UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -追加、更新、またはユーザーを取得するための操作が含まれています。
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -ロールのプロパティが含まれています。
- [RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -追加、削除、更新、およびロールを取得するための操作が含まれています。

### <a name="data-access-layer-classes"></a>データ アクセス レイヤーのクラス

この例では、データ アクセス レイヤーのクラスが、テーブルを操作するための SQL ステートメントを含めるただし、コードは、Entity Framework や NHibernate などオブジェクト リレーショナル マッピング (ORM) を使用する必要があります。 具体的には、アプリケーションは、遅延読み込みとキャッシュのオブジェクトを含む ORM せずパフォーマンスの低下を発生する可能性があります。 詳細については、次を参照してください[Entity Framework せず ASP.NET Identity 2.0 しますか?。](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -MySQL データベースの接続とデータベースの操作を実行するためのメソッドが含まれています。 UserStore と RoleStore このクラスのインスタンスとインスタンスします。
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -ロールを格納するテーブルのデータベース操作が含まれています。
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -ユーザーの信頼性情報を格納するテーブルのデータベース操作が含まれています。
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -ユーザーのログイン情報を格納するテーブルのデータベース操作が含まれています。
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -ユーザーがどのロールに割り当てられたを格納するテーブルのデータベース操作が含まれています。
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -ユーザーを格納するテーブルのデータベース操作が含まれています。

## <a name="create-a-mysql-database-instance-on-azure"></a>Azure の MySQL データベース インスタンスを作成します。

1. ログインに、 [Azure ポータル](https://manage.windowsazure.com/)です。
2. をクリックして **+ 新規**クリックし、ページの下部にある**ストア**です。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. **選択してアドオン**ウィザードで、 **ClearDB MySQL データベース** ダイアログ ボックスの右下にある次へ進む矢印をクリックします。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. 既定値を保持**空き**計画し、変更、**名前**に**IdentityMySQLDatabase**です。 一番近い地域を選択し、[次へ] 矢印をクリックします。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. データベースの作成を完了するチェック マークをクリックします。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. データベースが作成された後をから管理できる、**アドオン** タブで、管理ポータル。   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. をクリックして、データベース接続情報を取得できます**接続情報**ページの下部にあります。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. [コピー] ボタンをクリックすると、接続文字列をコピーし、後で、MVC アプリケーションで使用できるように保存します。   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>ASP.NET Identity に MySQL データベース内のテーブルを作成します。

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>接続して MySQL データベースの管理の MySQL Workbench ツールをインストールします。

1. インストール、 **MySQL Workbench**ツールから、 [MySQL がダウンロード ページ](http://dev.mysql.com/downloads/windows/installer/)
2. アプリを起動し、をクリックを追加、 **MySQLConnections +** ボタンをクリックして新しい接続を追加します。 このチュートリアルで先ほど作成した Azure の MySQL データベースからコピーした接続文字列データを使用します。
3. 接続を確立した後、新しく開きます**クエリ**タブ; からのコマンドを貼り付ける[MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql)クエリにし、データベース テーブルを作成するために実行します。
4. 次に示すように、Azure でホストされている MySQL データベースで作成されるすべての ASP.NET Identity 必要なテーブルがあるようになりました。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>MVC アプリケーション プロジェクト テンプレートから作成し、MySQL プロバイダーを使用するように構成

必要な場合、インストールするか[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) Update 2 です。

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>CodePlex から ASP.NET.Identity.MySQL プロジェクトをダウンロードします。

1. リポジトリの URL を参照[AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/)です。
2. ソース コードをダウンロードします。
3. ローカル フォルダーに .zip ファイルを抽出します。
4. AspNet.Identity.MySQL ソリューションを開き、それをビルドします。

### <a name="create-a-new-mvc-application-project-from-template"></a>テンプレートから新しい MVC アプリケーション プロジェクトを作成します。

1. 右クリックして、 **AspNet.Identity.MySQL**ソリューションと**追加**、**新しいプロジェクト**
2. **新しいプロジェクトの追加**ダイアログの  **Visual c#** し、左側の**Web**し、 **ASP.NET Web アプリケーション**です。 プロジェクトの名前を付けます**IdentityMySQLDemo**; [ok] をクリックします。  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. **新しい ASP.NET プロジェクト**ダイアログ ボックスで、既定のオプションで MVC テンプレートを選択 (が含まれている**個々 のユーザー アカウント**認証方法として) をクリック**OK**.![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. ソリューション エクスプ ローラーで、IdentityMySQLDemo プロジェクトを右クリックして**NuGet パッケージの管理**です。 検索テキスト ボックス ダイアログ ボックスで次のように入力します。 **Identity.EntityFramework**です。 結果の一覧で、このパッケージを選択し、をクリックして**アンインストール**です。 依存関係パッケージ EntityFramework をアンインストールするように促されます。 このアプリケーションには、このパッケージでは不要になったように [はい] をクリックします。
5. IdentityMySQLDemo プロジェクトを右クリックしてで、選択**追加**、**参照、ソリューション、プロジェクト;** AspNet.Identity.MySQL プロジェクトを選択し、をクリックして**OK**です。
6. IdentityMySQLDemo プロジェクトに対するすべての参照を置き換える  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   代入  
     `using AspNet.Identity.MySQL;`
7. IdentityModels.cs で次のように設定します。 **ApplicationDbContext**から派生する**MySqlDatabase**接続名に 1 つのパラメーターを受け取るコンス トラクターが含まれています。  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. IdentityConfig.cs ファイルを開きます。 **ApplicationUserManager.Create**メソッドは、次のコードで UserManager をインスタンス化する置換します。  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Web.config ファイルを開き、このエントリを強調表示された値を置き換える前の手順で作成した MySQL データベースの接続文字列に DefaultConnection 文字列を置き換えます。  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>アプリを実行して、MySQL データベースへの接続

1. 右クリックして、 **IdentityMySQLDemo**プロジェクトし、選択**スタートアップ プロジェクトとして設定**
2. キーを押して**ctrl キーを押しながら f5 キーを押して**アプリをビルドして実行します。
3. をクリックして**登録**ページの上部のタブです。
4. 新しいユーザー名とパスワードを入力し、をクリックして**登録**です。  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. 今すぐ新しいユーザーが登録されログに記録します。  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. MySQL Workbench ツールに戻るし、検査、 **IdentityMySQLDatabase**テーブルの内容。 新しいユーザーを登録すると、ユーザー テーブルのエントリを検査します。  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>次の手順

このアプリで他の認証方法を有効にする方法の詳細についてを参照してください[Facebook、Google OAuth2 および OpenID サイン オンで、ASP.NET MVC 5 アプリを作成する](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)です。

OAuth と、データベースを統合して、アプリへのユーザー アクセスを制限するロールを設定する方法についてを参照してください。[メンバーシップ、OAuth、SQL データベースでの ASP.NET MVC 5 のセキュリティで保護されたアプリケーションを Azure にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。
