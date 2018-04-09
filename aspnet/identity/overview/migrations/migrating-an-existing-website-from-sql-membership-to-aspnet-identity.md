---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: SQL メンバーシップから ASP.NET の Id への既存の web サイトの移行 |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、ユーザーとロールのデータを新しい ASP.NET Id の SQL メンバーシップを使用して作成された既存の web アプリケーションを移行する手順を示します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/19/2014
ms.topic: article
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 2790f32bc74cecf450f5a258fc1ff5b280a63923
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>SQL メンバーシップから ASP.NET Identity に既存の web サイトを移行します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)、 [Suhas Joshi](https://github.com/suhasj)

> このチュートリアルでは、ユーザーとロールのデータが、新しい ASP.NET Identity システムに SQL メンバーシップを使用して作成された既存の web アプリケーションを移行する手順を示します。 この方法では、ASP.NET Identity と古い]/[新しいクラス用のフック関数に必要な 1 つに、既存のデータベース スキーマを変更します。 データベースを移行したら、このアプローチを採用する後 Id への今後の更新を簡単に処理されます。


このチュートリアルでは、ユーザーおよびロールのデータを作成する Visual Studio 2010 を使用して作成された web アプリケーション テンプレート (Web フォーム) が取得されます。 Id システムで必要になったテーブルに既存のデータベースを移行するのには SQL スクリプトを使用し、します。 次に、必要な NuGet パッケージとメンバーシップの管理の Id システムを使用して新しいアカウントの管理ページを追加します。 移行のテストとして SQL メンバーシップを使用して作成できる必要がありますにログインして新しいユーザーを登録することにする必要があります。 完全なサンプルを見つけることができます[ここ](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/)です。 関連項目[ASP.NET メンバーシップから ASP.NET の Id への移行](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html)です。

## <a name="getting-started"></a>作業の開始

### <a name="creating-an-application-with-sql-membership"></a>SQL メンバーシップを持つアプリケーションを作成します。

1. SQL メンバーシップを使用し、ユーザーおよびロールのデータが既存のアプリケーションを起動する必要があります。 この資料には、Visual Studio 2010 で web アプリケーションを作成してみましょう。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. ASP.NET の構成ツールを使用して、2 人のユーザーを作成します。 **oldAdminUser**と**oldUser です。**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. 管理者をという名前のロールを作成し、そのロールのユーザーとして 'oldAdminUser' を追加します。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Default.aspx で、サイトの管理セクションを作成します。 管理者ロールのユーザーにのみアクセスを有効にするために web.config ファイルで、承認のタグを設定します。 詳細についてはこちらにあります。 [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. サーバー エクスプ ローラーで SQL メンバーシップ システムによって作成されたテーブルを理解するには、データベースを表示します。 ユーザー ログインのデータは、aspnet で\_ユーザーと"aspnet"\_メンバーシップ テーブルに、aspnet でロールのデータが格納されている\_Roles テーブル。 ユーザーに関する情報がどのロールに属するが、aspnet に格納されている\_UsersInRoles テーブル。 基本メンバーシップの管理用に移植する ASP.NET Identity システムに、上記のテーブル内の情報で十分です。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Visual Studio 2013 に移行します。

1. Web またはと共に Visual Studio 2013 用の Visual Studio Express 2013 のインストール、[最新の更新プログラム](https://www.microsoft.com/download/details.aspx?id=44921)です。
2. インストールされているバージョンの Visual Studio で上記のプロジェクトを開きます。 コンピューターに SQL Server Express がインストールされていない場合は、接続文字列は、SQL Express を使用するために、プロジェクトを開いたときに、プロンプトが表示されます。 SQL Express をインストールまたはとして回避変更 LocalDb への接続文字列にするか選択できます。 この記事には、LocalDb へ変更おします。
3. Web.config を開きからの接続文字列を変更します。(LocalDb) v11.0 を SQLExpess です。 削除 ' ユーザー インスタンスは true を =' 接続文字列からです。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. サーバー エクスプ ローラーを開き、テーブルのスキーマとデータが発生する可能性があることを確認します。
5. ASP.NET Identity システムは、バージョン 4.5 以降のフレームワークで動作します。 4.5 またはそれ以上のアプリケーションを再ターゲットします。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    プロジェクトをビルドしてエラーがないことを確認します。

### <a name="installing-the-nuget-packages"></a>Nuget パッケージをインストールします。

1. ソリューション エクスプ ローラーでプロジェクトを右クリックして&gt; **NuGet パッケージの管理**です。 [検索] ボックスで、"Asp.net Identity"を入力します。 結果の一覧で、パッケージを選択し、[インストール] をクリックします。 "I Accept"ボタンをクリックしてライセンス条項に同意します。 このパッケージの依存関係パッケージがインストールされる注: EntityFramework と Microsoft ASP.NET Identity Core です。 同様に、次のパッケージ (ログで OAuth が有効にしたくない場合は、最後の 4 つの OWIN パッケージを省略します) をインストールします。

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>データベースを新しい Id システムに移行します。

次の手順では、ASP.NET Identity システムで必要なスキーマに既存のデータベースを移行します。 実現するために新しいテーブルを作成し、新しいテーブルに既存のユーザー情報を移行するためのコマンドのセットを持つ SQL を実行してこのスクリプトを作成します。 スクリプト ファイルが見つからないことができます[ここ](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql)です。

このスクリプト ファイルは、このサンプルに固有です。 テーブルのスキーマが作成された場合は SQL メンバーシップを使用してをカスタマイズまたはそれに応じて変更するスクリプトの必要性を変更します。

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>スキーマの移行用の SQL スクリプトを生成する方法

ASP.NET Identity クラス、ボックスに、データの既存のユーザーの外で作業を ASP.NET Identity で必要な 1 つに、データベース スキーマを移行する必要があります。 新しいテーブルを追加し、それらのテーブルに既存の情報をコピーしてこの作業を行うことができます。 既定では、ASP.NET Identity は、Id モデル クラスを情報を格納および取得するのにデータベースにマップするのに EntityFramework を使用します。 これらのモデル クラスは、ユーザーとロール オブジェクトを定義するコア Identity インターフェイスを実装します。 テーブルと、データベース内の列は、これらのモデル クラスに基づいています。 Identity うち、v2.1.0 よりとそのプロパティである EntityFramework モデル クラス定義を次には

| **IdentityUser** | **Type** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| ID | string | ID | RoleId | ProviderKey | ID |
| ユーザー名 | string | 名前 | UserId | UserId | ClaimType |
| PasswordHash | string |  |  | LoginProvider | ClaimValue |
| SecurityStamp | string |  |  |  | ユーザー\_Id |
| 電子メール | string |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| PhoneNumber | string |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

これらの各モデルのプロパティに対応する列を含むテーブルが存在する必要があります。 クラスとテーブル間のマッピングが定義されている、`OnModelCreating`のメソッド、`IdentityDBContext`です。 これは構成の fluent API メソッドとして知られ、詳細を参照して[ここ](https://msdn.microsoft.com/data/jj591617.aspx)です。 クラスの構成は、後述のように

| **クラス** | **テーブル** | **主キー** | **外部キー** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | ID |  |
| IdentityRole | AspnetRoles | ID |  |
| IdentityUserRole | AspnetUserRole | UserId + RoleId | ユーザー\_Id -&gt;AspnetUsers RoleId-&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey+UserId + LoginProvider | UserId-&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | ID | User\_Id-&gt;AspnetUsers |

この情報を含む新しいテーブルを作成する SQL ステートメントを作成できます。 各ステートメントを個別に入力おか、全体を必要に応じてし編集する EntityFramework PowerShell コマンドを使用してスクリプトを生成します。 これを行う、VS オープン、 **Package Manager Console**から、**ビュー**または**ツール**メニュー

- EntityFramework 移行を有効にする"Enable-migrations"のコマンドを実行します。
- コマンド「add-migration 初期」を作成する C# の場合、データベースを作成する初期セットアップ コードを実行/vb です。
- 最後の手順が実行するには"データベースの更新 – スクリプト"モデル クラスに基づいて SQL スクリプトを生成するコマンド。

このデータベースの生成のスクリプトは、開始場所お加えて追加の変更を新しい列を追加し、データをコピーとして使用できます。 これの利点は、生成する、`_MigrationHistory`モデル クラスの Id のリリースの将来のバージョンの変更時に、データベースのスキーマを変更する EntityFramework によって使用されるテーブル。 

SQL メンバーシップ ユーザーの情報は、その他のプロパティ Id ユーザー モデル クラスつまり電子メール内だけでなく、パスワードの試行回数、最後のログイン日、最後のロックアウト日などがありました。これは、有用な情報したいし、思いますに Id システムに引き継がれます。 これによりユーザー モデルに追加のプロパティを追加して、データベースのテーブル列にマッピングすることによってでしょう。 そのため、クラスを追加することによってをサブクラス化する、`IdentityUser`モデル。 このカスタム クラスにプロパティを追加したり、テーブルを作成するときに、対応する列を追加する SQL スクリプトを編集できます。 このクラスのコードは、アーティクルさらに説明されています。 SQL スクリプトを作成するため、`AspnetUsers`表の後、新しいプロパティを追加することになります

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

次に、Id の既存の情報を SQL メンバーシップ データベースから新しく追加されたテーブルにコピーする必要があります。 これは、使用できる SQL を別の 1 つのテーブルから直接データをコピーしています。 使用してテーブルの行にデータを追加する、`INSERT INTO [Table]`を構築します。 使用して別のテーブルからコピーする、`INSERT INTO`ステートメントと共に、`SELECT`ステートメントです。 クエリを実行する必要がありますすべてのユーザー情報を取得する、 *aspnet\_ユーザー*と*aspnet\_メンバーシップ*テーブルし、データのコピー、 *AspNetUsers*テーブル。 使用して、`INSERT INTO`と`SELECT`と共に`JOIN`と`LEFT OUTER JOIN`ステートメントです。 クエリを実行して、テーブル間のデータのコピーの詳細についてを参照してください[この](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx)リンクします。 さらに、AspnetUserLogins および AspnetUserClaims テーブルが空まず始めに既定では、これにマップされる SQL メンバーシップ情報が存在しないためです。 コピーのみの情報は、ユーザーおよびロールです。 前の手順で作成されたプロジェクト、ユーザー テーブルに情報をコピーする SQL クエリになります

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

上記の SQL ステートメントからは、各ユーザーに関する情報で、 *aspnet\_ユーザー*と*aspnet\_メンバーシップ*の列にテーブルをコピー、 *AspnetUsers*テーブル。 ここで唯一の変更は、パスワードをコピーするとします。 SQL メンバーシップ内のパスワードの暗号化アルゴリズムには、'PasswordSalt' および 'PasswordFormat' が使用されるため、コピーをすぎるハッシュされたパスワードと Id を使用して、パスワードを復号化に使用できるようにします。 カスタム パスワード hasher に接続するときに、アーティクルでさらに詳細についてはします。 

このスクリプト ファイルは、このサンプルに固有です。 、追加のテーブルである必要がアプリケーションの開発者はユーザー モデル クラスに追加のプロパティを追加し、AspnetUsers テーブル内の列にマップする同様のアプローチに従うことができます。 スクリプトを実行するには

1. サーバー エクスプ ローラーを開きます。 テーブルを表示する 'ApplicationServices' 接続を展開します。 テーブル ノードを右クリックし、新規クエリ ' オプションを選択

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. クエリ ウィンドウにコピーし、Migrations.sql ファイルから SQL スクリプト全体を貼り付けます。 [実行] の矢印ボタンをクリックしてスクリプト ファイルを実行します。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    サーバー エクスプ ローラー ウィンドウを更新します。 5 つの新しいテーブルは、データベースに作成されます。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    以下には、新しい Id システムを SQL メンバーシップ テーブル内の情報をマップする方法を示します。

    aspnet\_Roles --&gt; AspNetRoles

    asp\_netUsers および asp\_netMembership--&gt; AspNetUsers

    aspnet\_UserInRoles --&gt; AspNetUserRoles

    上記のセクションで説明、AspNetUserClaims と AspNetUserLogins テーブルが空です。 AspNetUser テーブルの '識別子' フィールドには、次の手順として定義されているモデルのクラス名と一致する必要があります。 PasswordHash 列がフォームでも '暗号化されたパスワード | パスワードの salt | パスワードの形式' です。 これにより、特別な SQL メンバーシップ暗号ロジックを使用して、古いパスワードを再利用できるようにすることができます。 操作は、記事の後半で説明します。

### <a name="creating-models-and-membership-pages"></a>モデルおよびメンバーシップ ページの作成

前述のように、Id 機能は既定ではアカウント情報を格納するため、データベースと対話する Entity Framework を使用します。 テーブル内の既存のデータを操作するには、テーブルにマップされ、Id システムにフック モデル クラスを作成する必要があります。 Identity コントラクトの一部として、モデル クラスは Identity.Core dll で定義されているインターフェイスを実装する必要がありますか、または Microsoft.AspNet.Identity.EntityFramework で利用できるこれらのインターフェイスの既存の実装を拡張することができます。

この例では、AspNetRoles、AspNetUserClaims、AspNetLogins および AspNetUserRole テーブルは、Id システムの既存の実装に類似する列を持ちます。 そのためにこれらのテーブルにマップする既存のクラスに再利用可能です。 AspNetUser テーブルには、SQL メンバーシップ テーブルから追加情報の格納に使用されるいくつかの追加の列があります。 これは、'IdentityUser' の既存の実装を拡張し、追加のプロパティを追加するモデル クラスを作成することでマップできます。

1. プロジェクトのフォルダーを作成するモデル クラスのユーザーを追加します。 クラスの名前は、'AspnetUsers' テーブルの '識別子' 列に追加されたデータと一致する必要があります。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    ユーザー クラスで見つかった IdentityUser クラスを拡張する必要があります、 *Microsoft.AspNet.Identity.EntityFramework* dll です。 AspNetUser 列にマッピングされるクラスのプロパティを宣言します。 ID、ユーザー名、PasswordHash、SecurityStamp プロパティは、IdentityUser で定義され、ため省略しています。 すべてのプロパティを持つユーザー クラスのコードを次に示します

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Entity Framework DbContext クラスは、テーブルにモデル内のデータを永続化し、モデルを作成するテーブルからデータを取得するために必要です。 *Microsoft.AspNet.Identity.EntityFramework* dll defines the IdentityDbContext class which interacts with the Identity tables to retrieve and store information. IdentityDbContext&lt;tuser&gt; IdentityUser クラスを拡張するクラスであることができます 'TUser' クラスを取得します。

    ApplicationDBContext 'モデル' フォルダーで、手順 1. で作成された 'User' クラスを渡して IdentityDbContext を拡張する新しいクラスを作成します。

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. 新しい Id システムでのユーザー管理を行う UserManager を使用して&lt;tuser&gt;クラスで定義されている、 *Microsoft.AspNet.Identity.EntityFramework* dll です。 手順 1. で作成された 'User' クラスを渡して UserManager を拡張するカスタム クラスを作成する必要があります。

    UserManager UserManager を拡張する新しいクラスを作成、Models フォルダーに&lt;ユーザー&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. アプリケーションのユーザーのパスワードは暗号化され、データベースに格納されています。 SQL メンバーシップで使用される暗号アルゴリズムは、新しい Id システムと異なるです。 古いパスワードを再利用するには、選択的に新しいユーザーの Id で暗号アルゴリズムを使用しているときに SQL メンバーシップ アルゴリズムを使用して古いユーザー ログインとパスワードの暗号化を解除する必要があります。

    UserManager クラスには、'PasswordHasher'、'IPasswordHasher' インターフェイスを実装するクラスのインスタンスを格納するプロパティがあります。 これは、ユーザー認証のトランザクション処理中にパスワードを暗号化/暗号解除に使用されます。 手順 3 で定義されている UserManager クラスに、作成、新しいクラス SQLPasswordHasher とコピー、下記のコード。

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    コンパイル エラーを解決するには、System.Text および System.Security.Cryptography 名前空間をインポートします。

    EncodePassword メソッドは、既定の SQL メンバーシップ暗号実装に従ってパスワードを暗号化します。 これは、System.Web dll から取得されます。 古いアプリは、カスタム実装を使用する場合、その必要がありますが渡されます。 その他の 2 つのメソッドを定義する必要があります*HashPassword*と*VerifyHashedPassword*を使用する、 *EncodePassword*メソッドを指定したパスワードのハッシュまたはプレーン テキストを確認します。データベース内の 1 つの既存のパスワードです。

    SQL メンバーシップ システムは、PasswordHash、PasswordSalt および PasswordFormat を登録またはそのパスワードを変更したときに、ユーザーが入力したパスワードのハッシュに使用されます。 移行中に次の 3 つすべてのフィールド列に格納されて、PasswordHash で区切られた AspNetUser の表に、' |' 文字です。 ユーザーがログインし、パスワードがこれらのフィールド、おを使用して SQL メンバーシップ crypto 確認パスワードです。それ以外の場合に、パスワードを確認する Id システムの既定の暗号化を使用します。 このように古いユーザーは、アプリを移行したら、自分のパスワードを変更する必要はありません。
5. UserManager クラスのコンス トラクターを宣言し、コンス トラクターでプロパティを SQLPasswordHasher として渡すことです。

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>新しいアカウントの管理ページを作成します。

移行するには、次の手順では、により、ユーザーを登録し、ログイン アカウントの管理ページを追加します。 SQL メンバーシップから古いアカウント ページは、新しい Id システムとでは動作しないコントロールを使用します。 管理ページを新しいユーザーを追加するには、次のチュートリアルでは、このリンク[ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md)手順 'を追加する Web フォーム アプリケーションにユーザーを登録する' から既にプロジェクトを作成し、NuGet を追加したのでパッケージ。

ここにあるプロジェクトを使用するサンプルの変更を加える必要があります。

- クラスを使用して分離 Register.aspx.cs と Login.aspx.cs コード、`UserManager`ユーザーを作成する Identity パッケージからです。 この例では、前述の手順に従って、Models フォルダーに追加された UserManager を使用します。
- クラスの背後にある Register.aspx.cs と Login.aspx.cs のコードで IdentityUser の代わりに作成したユーザー クラスを使用します。 これは、Id システムに、カスタム ユーザー クラスにフックします。
- データベースを作成するパートをスキップすることができます。
- 開発者は、現在のアプリケーション ID に一致するように、新しいユーザーの application Id を設定する必要があります。 これにより Register.aspx.cs クラスで、ユーザー オブジェクトが作成される前にこのアプリケーションの ApplicationId のクエリを実行し、ユーザーを作成する前に設定されます。 

    例:

    Aspnet を照会する Register.aspx.cs ページで、メソッドを定義\_アプリケーションは、テーブルし、アプリケーションの名前に従って、アプリケーション Id を取得

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    ここでユーザー オブジェクトの設定を取得

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

古いユーザー名とパスワードを使用して、既存のユーザーをログインします。 登録ページを使用して、新しいユーザーを作成します。 また、ユーザーがロールで正しいことを確認します。

Id システムへの移植にオープン認証 (OAuth)、アプリケーションを追加するユーザーに役立ちます。 このサンプルを参照してください[ここ](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/)OAuth が有効になっています。

## <a name="next-steps"></a>次の手順

このチュートリアルでは ASP.NET Identity SQL メンバーシップからユーザーを移植する方法を紹介しましたが、プロファイル データをポートおしませんでした。 次のチュートリアルでは、新しい Id システムに SQL メンバーシップからプロファイル データの移植に紹介します。

この記事の下部にあるフィードバックを送信することができます。

*Tom Dykstra および Rick Anderson、記事のレビューいただきありがとうございます。*
