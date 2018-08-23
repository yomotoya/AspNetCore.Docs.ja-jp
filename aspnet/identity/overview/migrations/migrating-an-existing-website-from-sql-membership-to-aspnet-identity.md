---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: 既存の web サイトを SQL メンバーシップから ASP.NET Identity に移行する |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、ユーザーとロールのデータが新しい ASP.NET identity s SQL メンバーシップを使用して作成された既存の web アプリケーションを移行する手順は説明しています.
ms.author: riande
ms.date: 12/19/2014
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 51b97ee413ea0304177d5963b5fd9d7253778d4f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836167"
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>既存の web サイトを SQL メンバーシップから ASP.NET Identity に移行します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)、 [Suhas Joshi](https://github.com/suhasj)

> このチュートリアルでは、ユーザーとロールのデータが新しい ASP.NET Identity のシステムに SQL メンバーシップを使用して作成された既存の web アプリケーションを移行する手順を説明します。 このアプローチでは、ASP.NET Identity と古いまたは新しいクラスでフックで必要な 1 つに、既存のデータベース スキーマを変更する必要があります。 後、データベースが移行した後に、このアプローチを採用する Id に今後の更新プログラムを簡単に処理されます。


このチュートリアルでは、ユーザーおよびロールのデータを作成する Visual Studio 2010 を使用して作成された web アプリケーション テンプレート (Web フォーム) ことになります。 Id システムで必要なテーブルに既存のデータベースを移行するのに SQL スクリプトを使用します。 次に、必要な NuGet パッケージのインストールがされメンバーシップの管理の Id システムを使用して新しいアカウントの管理ページを追加します。 移行のテストとして SQL メンバーシップを使用して作成されたユーザーにログインできるようにする必要があり、新しいユーザーが登録できる必要があります。 完全なサンプルが見つかります[ここ](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/)します。 参照してください[ASP.NET メンバーシップから ASP.NET Identity に移行する](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html)します。

## <a name="getting-started"></a>作業の開始

### <a name="creating-an-application-with-sql-membership"></a>SQL メンバーシップを持つアプリケーションを作成します。

1. SQL メンバーシップを使用し、ユーザーおよびロールのデータが含まれている既存のアプリケーションを開始する必要があります。 この記事では、Visual Studio 2010 で web アプリケーションを作成しましょう。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. 2 人のユーザーを作成する ASP.NET の構成ツールを使用して: **oldAdminUser**と**oldUser します。**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. 管理者をという名前のロールを作成し、そのロールのユーザーとして 'oldAdminUser' を追加します。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Default.aspx、サイトの管理セクションを作成します。 管理者ロールのユーザーにのみアクセスを有効にするために web.config ファイルで承認タグを設定します。 詳細についてはこちら [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. SQL メンバーシップ システムによって作成されたテーブルの概要をサーバー エクスプ ローラーでデータベースを表示します。 ユーザー ログインのデータは、aspnet で\_ユーザーと"aspnet"\_aspnet でロールのデータが格納されている間、メンバーシップ テーブル\_Roles テーブル。 ユーザーに関する情報がどのロールでは、aspnet に格納されている\_UsersInRoles テーブル。 基本メンバーシップの管理ポートを ASP.NET Id システム上の表に情報を十分です。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Visual Studio 2013 に移行します。

1. Web またはと共に Visual Studio 2013 の Visual Studio Express 2013 をインストール、[最新の更新プログラム](https://www.microsoft.com/download/details.aspx?id=44921)します。
2. インストールされているバージョンの Visual Studio には、上記のプロジェクトを開きます。 コンピューターの SQL Server Express がインストールされていない場合は、接続文字列が SQL Express を使用しているため、プロジェクトを開くと、プロンプトが表示されます。 か、SQL Express をインストールまたはとして回避変更 LocalDb への接続文字列を選択できます。 この記事で LocalDb に変更します。
3. Web.config を開きから接続文字列を変更します。(LocalDb) v11.0 を SQLExpess します。 削除 ' のユーザー インスタンス = true' の接続文字列。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. サーバー エクスプ ローラーを開き、テーブルのスキーマとデータが発生する可能性があることを確認します。
5. ASP.NET Identity システムは、バージョン 4.5 以降のフレームワークで動作します。 4.5 またはそれ以上のアプリケーションを再ターゲットします。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    プロジェクトをビルドしてエラーがないことを確認します。

### <a name="installing-the-nuget-packages"></a>Nuget パッケージをインストールします。

1. ソリューション エクスプ ローラーでプロジェクトを右クリックして&gt; **NuGet パッケージの管理**します。 検索ボックスに、"Asp.net Identity"を入力します。 結果の一覧で、パッケージを選択し、[インストール] をクリックします。 "I Accept"ボタンをクリックしてライセンス条項に同意します。 このパッケージが依存関係パッケージをインストールことに注意してください: EntityFramework と Microsoft ASP.NET Identity のコア。 同様に (OAuth でログを有効にしたくない場合は、最後の 4 つの OWIN パッケージを省略します)、次のパッケージをインストールします。

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>新しい Id システム データベースを移行します。

次の手順では、ASP.NET Identity のシステムで必要なスキーマに、既存のデータベースを移行します。 実現するためを新しいテーブルを作成し、新しいテーブルに既存のユーザー情報を移行するコマンドのセットを持つ、SQL 実行このスクリプトします。 スクリプト ファイルは、[ここ](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql)します。

このスクリプト ファイルは、このサンプルに固有です。 テーブルのスキーマが作成された場合は SQL メンバーシップを使用してをカスタマイズまたは変更を適切に変更するスクリプトが必要。

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>スキーマの移行用の SQL スクリプトを生成する方法

既存のユーザーのデータを事前に定義するクラスを ASP.NET Identity、ASP.NET Identity で必要な 1 つに、データベース スキーマを移行する必要があります。 この新しいテーブルを追加し、それらのテーブルに既存の情報をコピーして実行できます。 既定では、ASP.NET Identity は、Id モデルのクラスを保存/取得の情報をデータベースにマップするのに EntityFramework を使用します。 これらのモデル クラスは、ユーザーとロール オブジェクトを定義するコア Identity のインターフェイスを実装します。 テーブルと、データベース内の列は、これらのモデル クラスに基づいています。 Identity v2.1.0 とそのプロパティに EntityFramework モデル クラスには、以下に定義されています。

| **IdentityUser** | **Type** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| ID | string | ID | RoleId | ProviderKey | ID |
| [ユーザー名] | string | name | ユーザー Id | ユーザー Id | ClaimType |
| PasswordHash | string |  |  | LoginProvider | ClaimValue |
| SecurityStamp | string |  |  |  | ユーザー\_Id |
| 電子メール | string |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| PhoneNumber | string |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

これらのモデルの各プロパティに対応する列を含むテーブルが存在する必要があります。 クラスとテーブル間のマッピングが定義されている、`OnModelCreating`のメソッド、`IdentityDBContext`します。 これは構成の fluent API メソッドと呼ばれ、情報の詳細については[ここ](https://msdn.microsoft.com/data/jj591617.aspx)します。 クラスの構成が次のようには

| **クラス** | **テーブル** | **主キー** | **外部キー** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | ID |  |
| IdentityRole | AspnetRoles | ID |  |
| IdentityUserRole | AspnetUserRole | ユーザー Id + RoleId | ユーザー\_Id -&gt;AspnetUsers RoleId-&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserId + LoginProvider | UserId-&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | ID | ユーザー\_Id-&gt;AspnetUsers |

この情報を含む新しいテーブルを作成する SQL ステートメントを作成できます。 各ステートメントを個別に作成しましたかし編集する EntityFramework の PowerShell コマンドを使用して、必要に応じて、全体のスクリプトを生成します。 VS オープンで、**パッケージ マネージャー コンソール**から、**ビュー**または**ツール**メニュー

- EntityFramework migrations を有効にする"Enable-migrations"のコマンドを実行します。
- C# でデータベースを作成する最初のセットアップ コードを作成する"add-migration initial"のコマンドを実行/VB.
- 最後の手順が実行するには"データベースの更新-スクリプト"モデル クラスに基づいて SQL スクリプトを生成するコマンド。

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

このデータベースの生成スクリプトを加える新しい列を追加したり、データをコピーする追加の変更開始として使用できます。 これの利点は、生成ここ、 `_MigrationHistory` model クラスの将来のバージョンの Id のリリースの変更時にデータベース スキーマを変更する entity Framework で使用されるテーブル。 

SQL メンバーシップ ユーザーの情報は、その他の namely の電子メール Id ユーザー モデル クラスで使用されているだけでなく、プロパティ、パスワードの試行回数、前回のログイン日、最後のロックアウト日などがありました。これは、有用な情報と Id システムに持ち越すしたいです。 これは、ユーザー モデルに追加のプロパティを追加して、データベース内のテーブル列にマッピングすることによって実行できます。 そのため、クラスを追加することでをサブクラスとして持つ、`IdentityUser`モデル。 このカスタム クラスにプロパティを追加して、テーブルを作成するときに、対応する列を追加する SQL スクリプトを編集します。 このクラスのコードについては、情報の記事で詳しく説明します。 SQL スクリプトを作成するため、`AspnetUsers`テーブルは、新しいプロパティを追加した後

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

次に、Id の既存の情報を SQL メンバーシップ データベースから新しく追加されたテーブルにコピーする必要があります。 これは、1 つのテーブルから直接データをコピーして SQL を実行できます。 使用してテーブルの行にデータを追加する、`INSERT INTO [Table]`を構築します。 使用して別のテーブルからコピーする、`INSERT INTO`ステートメントと共に、`SELECT`ステートメント。 クエリを実行する必要がありますすべてのユーザー情報を取得する、 *aspnet\_ユーザー*と*aspnet\_メンバーシップ*テーブルと、データのコピー、 *AspNetUsers*。テーブル。 使用して、`INSERT INTO`と`SELECT`と共に`JOIN`と`LEFT OUTER JOIN`ステートメント。 クエリを実行して、テーブル間のデータをコピーする方法の詳細についてを参照してください[この](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx)リンク。 さらに、AspnetUserLogins、および AspnetUserClaims テーブルは空始めに既定では、これにマップされる SQL メンバーシップ情報がないためです。 コピーのみの情報はユーザーとロールです。 前の手順で作成されたプロジェクト、ユーザー テーブルに情報をコピーする SQL クエリになります。

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

上記の SQL ステートメントから各ユーザーについての情報で、 *aspnet\_ユーザー*と*aspnet\_メンバーシップ*の列にテーブルをコピー、 *AspnetUsers*テーブル。 ここで唯一の変更は、パスワードをコピーするとします。 SQL メンバーシップ内のパスワードの暗号化アルゴリズムには、'PasswordSalt' と 'PasswordFormat' が使用される、ためにコピーをすぎるハッシュされたパスワードと共に Id を使用して、パスワードを復号化する使用できるようにします。 カスタム パスワード ハッシャーをフックするときに、この記事ではさらにこれがについて説明します。 

このスクリプト ファイルは、このサンプルに固有です。 追加のテーブルが存在するアプリケーション、開発者がユーザー モデル クラスに追加のプロパティを追加し、AspnetUsers テーブル内の列にマップする同様のアプローチに従うことができます。 スクリプトを実行するには

1. サーバー エクスプ ローラーを開きます。 テーブルを表示する 'ApplicationServices' 接続を展開します。 [テーブル] ノードを右クリックし、[新規クエリ] オプションを選択します

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. クエリ ウィンドウでは、コピーして Migrations.sql ファイルから全体の SQL スクリプトを貼り付けます。 スクリプト ファイルを実行するには、[実行] の矢印ボタンを押します。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    サーバー エクスプ ローラー ウィンドウを更新します。 5 つの新しいテーブルは、データベースに作成されます。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    次には、新しい Id システムに SQL メンバーシップ テーブル内の情報をマップする方法を示します。

    aspnet\_ロール -&gt; AspNetRoles

    asp\_netUsers と asp\_netMembership--&gt; AspNetUsers

    aspnet\_UserInRoles--&gt; AspNetUserRoles

    上記のセクションで説明した、AspNetUserClaims と AspNetUserLogins テーブルが空です。 AspNetUser テーブルの '識別子' フィールドは、次の手順として定義されているモデルのクラス名と一致する必要があります。 また PasswordHash 列は、フォームでは ' パスワードの暗号化 | パスワードの salt | パスワードの形式 '。 これにより、古いパスワードを再利用できるように、特別な SQL メンバーシップ暗号化ロジックを使用することができます。 操作は、記事の後半で説明します。

### <a name="creating-models-and-membership-pages"></a>モデルおよびメンバーシップ ページの作成

前述のように、Id 機能は既定ではアカウント情報を格納するためのデータベースと通信する Entity Framework を使用します。 テーブル内の既存のデータを操作するには、テーブルにマップし、Id システムでそれらをフックするモデル クラスを作成する必要があります。 Identity コントラクトの一部として、モデル クラスは Identity.Core dll で定義されたインターフェイスを実装する必要がありますか、または Microsoft.AspNet.Identity.EntityFramework で利用できるこれらのインターフェイスの既存の実装を拡張することができます。

サンプルでは、AspNetRoles、AspNetUserClaims、AspNetLogins および AspNetUserRole テーブルに Id システムの既存の実装に類似した列があります。 そのためにこれらのテーブルにマップする既存のクラスを再利用できます。 AspNetUser テーブルには、SQL のメンバーシップ テーブルから追加情報の格納に使用されるいくつかの追加列があります。 これは、'IdentityUser' の既存の実装を拡張し、追加のプロパティを追加するモデル クラスを作成してマップできます。

1. プロジェクトのフォルダーを作成するモデル クラスのユーザーを追加します。 クラスの名前は、'AspnetUsers' テーブルの '識別子' 列に追加されたデータと一致する必要があります。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    ユーザー クラスがで見つかった IdentityUser クラスを拡張する必要があります、 *Microsoft.AspNet.Identity.EntityFramework* dll。 AspNetUser 列にマッピングされるクラスのプロパティを宣言します。 ID、ユーザー名、PasswordHash および SecurityStamp プロパティは、IdentityUser で定義され、ため省略しています。 すべてのプロパティを持つユーザー クラスのコードを次に示します

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. モデル テーブルにデータを永続化し、モデルを作成するテーブルからデータを取得するには、Entity Framework DbContext クラスが必要です。 *Microsoft.AspNet.Identity.EntityFramework* dll defines the IdentityDbContext class which interacts with the Identity tables to retrieve and store information. IdentityDbContext&lt;tuser&gt; IdentityUser クラスを拡張するクラスは、'TUser' クラスを受け取る。

    ApplicationDBContext 'モデル' フォルダーで、手順 1. で作成した 'User' クラスを渡す IdentityDbContext を拡張する新しいクラスを作成します。

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. 新しい Id システムでのユーザー管理を行う、UserManager を使用して&lt;tuser&gt;クラスで定義されている、 *Microsoft.AspNet.Identity.EntityFramework* dll。 手順 1. で作成した 'User' クラスを渡して UserManager を拡張するカスタム クラスを作成する必要があります。

    Models フォルダーで新しいクラスを UserManager を拡張する UserManager 作成&lt;ユーザー&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. アプリケーションのユーザーのパスワードは暗号化され、データベースに格納されます。 SQL メンバーシップで使用される暗号化アルゴリズムは、新しい Id システムと異なるです。 古いパスワードを再利用するには、古いユーザー Id で新しいユーザーを暗号アルゴリズムを使用しながら SQL メンバーシップ アルゴリズムを使用してログイン時に選択的にパスワードを復号化する必要があります。

    UserManager クラスには、'PasswordHasher'、'IPasswordHasher' インターフェイスを実装するクラスのインスタンスを格納するプロパティがあります。 これは、ユーザー認証トランザクション中にパスワードを暗号化/暗号化解除に使用されます。 手順 3 で定義されている、UserManager クラスの作成、新しいクラス SQLPasswordHasher とコピー、次のコード。

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    コンパイル エラーを解決するには、System.Text、System.Security.Cryptography 名前空間をインポートします。

    EncodePassword メソッドでは、既定の SQL メンバーシップ暗号実装に従ってパスワードを暗号化します。 これは、System.Web dll から取得されます。 場合は、古いアプリ、カスタム実装を反映する必要がありますし、ここで使用します。 その他の 2 つのメソッドを定義する必要があります*HashPassword*と*VerifyHashedPassword*を使用して、 *EncodePassword*メソッドを指定したパスワードのハッシュまたはプレーン テキストを確認します。データベースの 1 つの既存のパスワードです。

    SQL メンバーシップ システムは、登録、または自分のパスワードを変更したときに、ユーザーが入力したパスワードをハッシュするのに PasswordHash、PasswordSalt および PasswordFormat を使用します。 PasswordHash 列で区切られた AspNetUser テーブルに移行中に 3 つすべてのフィールドが格納されている、' |' 文字。 パスワードを確認する SQL メンバーシップ crypto を使用してユーザーがログインすると、パスワードがこれらのフィールド、それ以外の場合、Id システムの既定の暗号化を使用してパスワードを確認します。 このように古いユーザーは、アプリが移行した後にパスワードを変更する必要はありません。
5. UserManager クラスのコンス トラクターを宣言し、これをコンス トラクターでプロパティを SQLPasswordHasher として渡します。

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>新しいアカウントを作成するには、管理ページ

移行の次の手順では、ユーザーに登録し、ログイン アカウントの管理ページを追加します。 SQL メンバーシップから古いアカウント ページでは、新しい Id システムと連携しないコントロールを使用します。 管理ページを新しいユーザーを追加するには、次のリンクでチュートリアル[ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md)ステップをアプリケーションにユーザーを登録するための Web フォームの追加 ' から始まるため、既にプロジェクトを作成し、NuGet を追加しましたパッケージ。

ここにあるプロジェクトを使用するサンプルの変更を加える必要があります。

- クラスの使用の背後にある Register.aspx.cs と Login.aspx.cs コード、`UserManager`からユーザーを作成するパッケージの Id。 この例で前に説明した手順に従って、Models フォルダーに追加 UserManager を使用します。
- 作成、IdentityUser クラスの背後にある Register.aspx.cs および Login.aspx.cs のコードではなく、ユーザー クラスを使用します。 これは、Id システムに、カスタム ユーザー クラスにフックします。
- データベースを作成する部分をスキップできます。
- 開発者は、現在のアプリケーション ID と一致する新しいユーザーの ApplicationId を設定する必要があります。 これは、Register.aspx.cs クラスで、ユーザー オブジェクトが作成される前にこのアプリケーションの ApplicationId クエリを実行して、ユーザーを作成する前に設定することによって実行できます。 

    例:

    Aspnet を照会する Register.aspx.cs ページで、メソッドを定義\_アプリケーションは、テーブルし、アプリケーション名に従ってアプリケーション Id を取得します。

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    これで、ユーザー オブジェクトで設定を取得

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

前のユーザー名とパスワードを使用して、既存のユーザーのログインします。 新しいユーザーを作成するのにには、登録ページを使用します。 また、ユーザーは、期待どおりの役割では確認します。

オープン認証 (OAuth)、アプリケーションを追加するユーザー Id システムに移植する際に役立ちます。 サンプルを参照してください[ここ](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/)OAuth が有効になっています。

## <a name="next-steps"></a>次の手順

このチュートリアルでは ASP.NET identity では、SQL メンバーシップからユーザーを移植する方法を紹介しましたが、プロファイル データをポートありませんでした。 次のチュートリアルでは、新しい Id システムを SQL メンバーシップからプロファイル データの移植に紹介します。

この記事の下部にあるフィードバックを送信することができます。

*Tom Dykstra と Rick Anderson、記事のレビューします。*
