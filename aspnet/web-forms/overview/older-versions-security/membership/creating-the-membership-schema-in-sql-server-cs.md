---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
title: SQL Server (c#) でメンバーシップ スキーマの作成 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、を使用するには、データベースに必要なスキーマを追加するための手法を調べることで開始します。 次に、私たち wi.
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: b4ac129d-1b8e-41ca-a38f-9b19d7c7bb0e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
msc.type: authoredcontent
ms.openlocfilehash: c28e9735884586c43be4cf25fb2a3e5fa597832c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816653"
---
<a name="creating-the-membership-schema-in-sql-server-c"></a>SQL Server (c#) でメンバーシップ スキーマの作成
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_cs.pdf)

> このチュートリアルでは、を使用するには、データベースに必要なスキーマを追加するための手法を調べることで開始します。 次に、スキーマ内のキー テーブルの確認はされ、目的と重要性について説明します。 このチュートリアルでは、ASP.NET アプリケーションのメンバーシップ フレームワークが使用するプロバイダーを確認する方法を見てで終わります。


## <a name="introduction"></a>はじめに

Web サイトの訪問者を識別するためにフォーム認証を使用して 2 つの前のチュートリアル。 フォーム認証のフレームワークでは、ユーザー web サイトにログインして、認証チケットを使用して、ページの訪問者の間でそれらを記憶する開発者が簡単です。 `FormsAuthentication`クラスには、チケットを生成して、訪問者の cookie への追加のメソッドが含まれています。 `FormsAuthenticationModule`すべての着信要求を調べ、有効な認証チケットを使用して、それらを作成し、関連付けます、`GenericPrincipal`と`FormsIdentity`現在の要求を持つオブジェクト。 フォーム認証は、単にし、ユーザーの id を確認するには、そのチケットを解析、後続の要求にログインするときに、訪問者に認証チケットを付与するメカニズムです。 ユーザー アカウントをサポートするために web アプリケーションの場合も、ユーザー ストアを実装し、資格情報の検証、新しいユーザーは、および無数の他のユーザー アカウントに関連するタスクを登録する機能を追加する必要があります。

ASP.NET 2.0 では、前に、開発者は、これらすべてのユーザー アカウントに関連するタスクを実装するために巻き込までした。 さいわいなことに、ASP.NET チームはこの欠点を認識し、ASP.NET 2.0 メンバーシップ フレームワークを導入します。 メンバーシップ フレームワークとは、中核となるユーザー アカウントに関連するタスクを実行するためのプログラム インターフェイスを提供する .NET Framework のクラスのセットです。 このフレームワークが上に構築される、[プロバイダー モデル](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)、標準化された API にカスタマイズした実装をプラグインできます。

説明したように、 <a id="Tutorial1"> </a> [*セキュリティの基礎と ASP.NET のサポート*](../introduction/security-basics-and-asp-net-support-cs.md)チュートリアルでは、2 つの組み込みのメンバーシップ プロバイダーが .NET Framework が付属しています: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx)[ `SqlMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx)します。 その名のとおり、`SqlMembershipProvider`ユーザー ストアとして Microsoft SQL Server データベースを使用します。 アプリケーションでこのプロバイダーを使用するには、ストアとして使用するには、どのようなデータベース プロバイダーに指示する必要があります。 ご想像のとおり、`SqlMembershipProvider`が特定のデータベース テーブル、ビュー、およびストアド プロシージャ、ユーザー ストア データベースが必要です。 この予期されるスキーマを選択したデータベースに追加する必要があります。

このチュートリアルで使用するには、データベースに必要なスキーマを追加するための手法を調べることでは、`SqlMembershipProvider`します。 次に、スキーマ内のキー テーブルの確認はされ、目的と重要性について説明します。 このチュートリアルでは、ASP.NET アプリケーションのメンバーシップ フレームワークが使用するプロバイダーを確認する方法を見てで終わります。

それでは、始めましょう!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>手順 1: ユーザー ストアに配置する場所を決定します。

ASP.NET アプリケーションのデータは通常、データベース内のテーブルの数値で格納します。 実装する場合、`SqlMembershipProvider`データベース スキーマがアプリケーション データと同じデータベースまたは別のデータベース メンバーシップ スキーマを配置するかどうかを決める必要があります。

アプリケーション データと同じデータベースで次の理由でメンバーシップ スキーマを検索することをお勧めします。

- **保守容易性**データが 1 つのデータベースでカプセル化されたアプリケーションが簡単に理解、保守、および 2 つの独立したデータベースを持つアプリケーションよりもデプロイします。
- **リレーショナル整合性**アプリケーションがそれをテーブルとして、同じデータベース内のメンバーシップに関連するテーブルを配置することによりを確立できる[外部キー制約](http://en.wikipedia.org/wiki/Foreign_key)の主キーの間で、メンバーシップに関連するテーブルと関連するアプリケーションのテーブル。

個別のデータベースにユーザー ストアとアプリケーション データを分離することのみ合理的それぞれ別個のデータベースを使用して、、共通のユーザー ストアを共有する必要があります複数のアプリケーションがある場合です。

### <a name="creating-a-database"></a>データベースを作成します。

アプリケーションが 2 番目のチュートリアルでは、データベースがまだ必要ありませんのでを構築してきました。 必要がありますいずれかのようになりました、ただし、ユーザー ストアの。 それでは 1 つ作成し、によって必要なスキーマにそれを追加、`SqlMembershipProvider`プロバイダー (手順 2 を参照してください)。

> [!NOTE]
> このチュートリアル シリーズ全体では、使用、 [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) 、アプリケーション テーブルを格納するデータベースと`SqlMembershipProvider`スキーマ。 この決定が 2 つの理由: 最初に、により、コスト - 無料 - Express Edition は SQL Server 2005; の強みで最もアクセス可能なバージョンSQL Server 2005 Express Edition データベースを配置して、web アプリケーションの直接の第 2 に、`App_Data`フォルダー、特別なセットアップ指示せず、再デプロイして、データベースをパッケージ化し、web アプリケーションはまとめて 1 つの ZIP ファイルに簡単になりますまたは、構成オプション。 非-Express Edition のバージョンの SQL Server を使用して作業を進めるにする場合もかまいません。 手順はほぼ同じです。 `SqlMembershipProvider`スキーマは任意のバージョンの Microsoft SQL Server 2000 を使用し、セットアップします。


ソリューション エクスプ ローラーを右クリックし、`App_Data`フォルダーと新しい項目の追加を選択します。 (表示されない場合、`App_Data`プロジェクトのフォルダーにソリューション エクスプ ローラーでプロジェクトを右クリックし、ASP.NET フォルダーの追加 を選択および選択`App_Data`)。という名前の新しい SQL データベースを追加することも、新しい項目の追加 ダイアログ ボックスから`SecurityTutorials.mdf`します。 このチュートリアルでは追加、`SqlMembershipProvider`スキーマがこのデータベースには追加作成後のチュートリアルで、アプリケーションのデータをキャプチャするテーブル。


[![App_Data フォルダーに SecurityTutorials.mdf Database という名前の新しい SQL データベースを追加します。](creating-the-membership-schema-in-sql-server-cs/_static/image2.png)](creating-the-membership-schema-in-sql-server-cs/_static/image1.png)

**図 1**: 追加する新しい SQL データベースの名前付き`SecurityTutorials.mdf`データベースを`App_Data`フォルダー ([フルサイズの画像を表示する をクリックします](creating-the-membership-schema-in-sql-server-cs/_static/image3.png))。


データベースを追加する、`App_Data`フォルダーが自動的に追加、データベース エクスプ ローラー ビュー。 (非-Express Edition バージョンの Visual Studio では、データベース エクスプ ローラー呼びますサーバー エクスプ ローラー。)データベース エクスプ ローラーに移動し、単に追加された順に展開`SecurityTutorials`データベース。 画面で [データベース エクスプ ローラーが表示されない場合表示] メニューに移動し、データベース エクスプ ローラーを選択または Ctrl + Alt + S をヒットします。 図 2 に示すよう、`SecurityTutorials`データベースが空でないテーブル、ビューがありませんおよびなしのストアド プロシージャが含まれています。


[![SecurityTutorials データベースは空です。](creating-the-membership-schema-in-sql-server-cs/_static/image5.png)](creating-the-membership-schema-in-sql-server-cs/_static/image4.png)

**図 2**:`SecurityTutorials`データベースは現在空 ([フルサイズの画像を表示する をクリックします](creating-the-membership-schema-in-sql-server-cs/_static/image6.png))。


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>手順 2: 追加、`SqlMembershipProvider`データベースにスキーマ

`SqlMembershipProvider`一連のテーブル、ビュー、およびストアド プロシージャ、ユーザー ストア データベースにインストールする必要があります。 使用して、これらの必要なデータベース オブジェクトを追加することができます、 [ `aspnet_regsql.exe`ツール](https://msdn.microsoft.com/library/ms229862.aspx)します。 このファイルにある、`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\`フォルダー。

> [!NOTE]
> `aspnet_regsql.exe`ツールには、コマンドラインの機能と、グラフィカル ユーザー インターフェイスの両方が用意されています。 グラフィカル インターフェイスはよりユーザー フレンドリなとこのチュートリアルで説明されます。 コマンド ライン インターフェイスときに便利ですが追加、`SqlMembershipProvider`を自動化する必要があるスキーマ、またはスクリプトのテスト シナリオを自動化ビルドのようにします。


`aspnet_regsql.exe`ツールを使用して追加または削除*ASP.NET アプリケーション サービス*指定された SQL Server データベースにします。 ASP.NET アプリケーション サービスのスキーマを網羅する、`SqlMembershipProvider`と`SqlRoleProvider`、他の ASP.NET 2.0 フレームワークの SQL ベースのプロバイダーのスキーマとします。 情報の 2 つのビットを提供する必要があります、`aspnet_regsql.exe`ツール。

- 追加またはアプリケーションのサービスを削除するかどうかと
- データベースを追加またはアプリケーションのサービス スキーマを削除します。

使用するには、データベースの入力を求める、`aspnet_regsql.exe`ツールでは、セキュリティ資格情報、データベースに接続するために、データベースが存在するサーバーの名前とデータベース名を指定するよう求められます。 非 Express エディションの SQL Server を使用している場合は、同じ情報を ASP.NET web ページを使用してデータベースを使用する場合、接続文字列を指定する必要があります、この情報を既に知っておくべき。 SQL Server 2005 Express Edition データベースを使用する場合は、サーバーとデータベース名を決定する、`App_Data`フォルダーは、少し複雑です。

次のセクションでは、SQL Server 2005 Express Edition データベース サーバーとデータベース名を指定するための簡単な方法を調べ、`App_Data`フォルダー。 SQL Server 2005 Express Edition を自由に、インストールに進んでを使用していない場合、アプリケーションのサービス セクション。

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>サーバーとデータベースの SQL Server 2005 Express Edition データベースの名前を決定する、`App_Data`フォルダー

使用するには、`aspnet_regsql.exe`ツールがサーバーとデータベース名を把握する必要があります。 サーバー名が`localhost\InstanceName`します。 最も可能性の高い、 *InstanceName*は`SQLExpress`します。 ただし、手動で SQL Server 2005 Express Edition をインストールした場合 (つまり、インストールしていないことに自動的に Visual Studio のインストール中に)、別のインスタンス名が選択されている可能性があります。

データベース名が決定する少し複雑になります。 内のデータベース、`App_Data`フォルダーが含まれるデータベース名を通常がある、[グローバル一意識別子](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)と共に、データベース ファイルへのパス。 を通じてアプリケーションのサービス スキーマを追加するにはこのデータベース名を確認する必要があります`aspnet_regsql.exe`します。

データベース名を確認する最も簡単な方法では、SQL Server Management Studio を調査します。 SQL Server Management Studio は、SQL Server 2005 のデータベースを管理するためのグラフィカル インターフェイスを提供しますが、Express エディションの SQL Server 2005 では含まれません。 良い知らせは[ダウンロードできます](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)無料 Express エディションの SQL Server Management Studio。

> [!NOTE]
> デスクトップにインストールされている SQL Server 2005 の非-Express Edition のバージョンもいる場合、完全なバージョンの Management Studio がインストールされている可能性があります。 Express Edition を以下に示すとおり、同じ手順を次に、データベース名を判断するのに完全なバージョンを使用することができます。


データベース ファイル上の Visual Studio によって課されるすべてのロックが閉じられていることを確認する Visual Studio を閉じることで開始します。 次に、SQL Server Management Studio を起動しに接続、 `localhost\InstanceName` SQL Server 2005 Express Edition のデータベース。 インスタンス名は、前述のように、可能性がありますが、`SQLExpress`します。 認証オプションでは、Windows 認証を選択します。


[![SQL Server 2005 Express Edition のインスタンスに接続します。](creating-the-membership-schema-in-sql-server-cs/_static/image8.png)](creating-the-membership-schema-in-sql-server-cs/_static/image7.png)

**図 3**: SQL Server 2005 Express Edition インスタンスへの接続 ([フルサイズの画像を表示する をクリックします](creating-the-membership-schema-in-sql-server-cs/_static/image9.png))。


SQL Server 2005 Express Edition のインスタンスに接続したら、Management Studio には、データベース、セキュリティ設定、サーバー オブジェクト、およびなどのフォルダーが表示されます。 データベース タブを展開する場合が表示されますが、`SecurityTutorials.mdf`データベースが*いない*最初にデータベースをアタッチする必要があります - データベース インスタンスに登録します。

データベース フォルダーを右クリックし、コンテキスト メニューから接続を選択します。 これにより、データベースのアタッチ ダイアログ ボックスが表示されます。 ここでは、[追加] ボタンをクリックして、参照、`SecurityTutorials.mdf`データベース、および [ok] をクリックします。 図 4 の後、データベースのアタッチ ダイアログ ボックスを示しています、`SecurityTutorials.mdf`データベースが選択されています。 図 5 は、データベースが正常にアタッチされた後に、Management Studio のオブジェクト エクスプ ローラーを示します。


[![SecurityTutorials.mdf データベースをアタッチします。](creating-the-membership-schema-in-sql-server-cs/_static/image11.png)](creating-the-membership-schema-in-sql-server-cs/_static/image10.png)

**図 4**: アタッチ、`SecurityTutorials.mdf`データベース ([フルサイズの画像を表示する をクリックします](creating-the-membership-schema-in-sql-server-cs/_static/image12.png))。


[![SecurityTutorials.mdf データベースが、データベース フォルダーに表示します。](creating-the-membership-schema-in-sql-server-cs/_static/image14.png)](creating-the-membership-schema-in-sql-server-cs/_static/image13.png)

**図 5**:`SecurityTutorials.mdf`データベース フォルダーにデータベースが表示されます ([フルサイズの画像を表示する をクリックします](creating-the-membership-schema-in-sql-server-cs/_static/image15.png))。


図 5 に示すよう、`SecurityTutorials.mdf`データベースではなく abstruse 名前が付いています。 変更してみましょうを覚えやすい (および簡単な形式) の名前。 データベースを右クリックし、名前の変更をコンテキスト メニューから選択、および名前を変更`SecurityTutorialsDatabase`します。 ファイル名は変更されません、データベース名だけを使用して SQL Server を識別します。


[![SecurityTutorialsDatabase にデータベースの名前変更します。](creating-the-membership-schema-in-sql-server-cs/_static/image17.png)](creating-the-membership-schema-in-sql-server-cs/_static/image16.png)

**図 6**: データベースの名前を変更`SecurityTutorialsDatabase`([フルサイズの画像を表示する をクリックします](creating-the-membership-schema-in-sql-server-cs/_static/image18.png))。


この時点でのサーバーとデータベースの名前がわかって、`SecurityTutorials.mdf`データベース ファイル:`localhost\InstanceName`と`SecurityTutorialsDatabase`、それぞれします。 アプリケーション サービスをインストールする準備ができました、`aspnet_regsql.exe`ツール。

### <a name="installing-the-application-services"></a>アプリケーション サービスをインストールします。

起動する、`aspnet_regsql.exe`ツールで、[スタート] メニューに移動し、実行を選択します。 入力`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe`テキスト ボックスに [ok] をクリックします。 ダブルクリック、適切なフォルダーをドリルダウンして、Windows エクスプ ローラーを使用する代わりに、`aspnet_regsql.exe`ファイル。 どちらの方法では、同じ結果が net されます。

実行している、`aspnet_regsql.exe`コマンドライン引数を使用せずにツールが ASP.NET SQL Server セットアップ ウィザードのグラフィカル ユーザー インターフェイスを起動します。 ウィザードでは、簡単に追加または指定したデータベースでの ASP.NET アプリケーション サービスを削除できます。 図 7 に示すように、ウィザードの最初の画面では、ツールの目的について説明します。


[![ASP.NET SQL Server セットアップ ウィザードによりを使用してメンバーシップ スキーマを追加するには](creating-the-membership-schema-in-sql-server-cs/_static/image20.png)](creating-the-membership-schema-in-sql-server-cs/_static/image19.png)

**図 7**: ASP.NET SQL Server セットアップ ウィザードを使用してメンバーシップ スキーマを追加する ([フルサイズの画像を表示する をクリックします](creating-the-membership-schema-in-sql-server-cs/_static/image21.png))。


ウィザードの 2 番目の手順は、アプリケーション サービスを追加または削除するかどうかを私たちを要求します。 テーブル、ビュー、および必要なストアド プロシージャを追加するので、 `SqlMembershipProvider`、SQL Server の構成アプリケーション サービスのオプションを選択します。 後で、このスキーマをデータベースから削除する場合は、このウィザードを再実行が、代わりに既存のデータベース オプションのアプリケーション サービスの情報を削除を選択します。


[![選択、SQL Server アプリケーション サービスのオプションを構成](creating-the-membership-schema-in-sql-server-cs/_static/image23.png)](creating-the-membership-schema-in-sql-server-cs/_static/image22.png)

**図 8**: SQL Server の構成をアプリケーションのサービス オプションの選択 ([フルサイズの画像を表示する をクリックします](creating-the-membership-schema-in-sql-server-cs/_static/image24.png))。


3 番目の手順は、データベースについては、メッセージが表示されます。 サーバー名、認証情報、およびデータベース名。 このチュートリアルに従っている必要し、追加したかどうか、`SecurityTutorials.mdf`データベースを`App_Data`に接続されている`localhost\InstanceName`、名前を変更する`SecurityTutorialsDatabase`、次の値を使用します。

- サーバー: `localhost\InstanceName`
- Windows 認証
- データベース: `SecurityTutorialsDatabase`


[![データベースの情報を入力します。](creating-the-membership-schema-in-sql-server-cs/_static/image26.png)](creating-the-membership-schema-in-sql-server-cs/_static/image25.png)

**図 9**: データベースの情報を入力します ([フルサイズの画像を表示する をクリックします](creating-the-membership-schema-in-sql-server-cs/_static/image27.png))。


データベースの情報を入力した後、[次へ] をクリックします。 最後の手順では、実行される手順をまとめたものです。 次に、アプリケーション サービスをインストールし、ウィザードを完了し、[完了] をクリックします。

> [!NOTE]
> Management Studio を使用してデータベースをアタッチし、データベース ファイルの名前を変更した場合、データベースをデタッチおよび Management Studio を閉じて、Visual Studio をもう一度開くことを確認します。 デタッチするには、`SecurityTutorialsDatabase`データベース、データベース名を右クリックして、タスク メニューからデタッチします。


ウィザードの完了には、Visual Studio に戻り、データベース エクスプ ローラーに移動します。 [テーブル] フォルダーを展開します。 一連のプレフィックスで始まる名前のテーブルを表示する必要があります`aspnet_`します。 同様に、さまざまなビューとストアド プロシージャは、ビューとストアド プロシージャのフォルダーの下で確認できます。 これらのデータベース オブジェクトは、アプリケーションのサービス スキーマを構成します。 手順 3 でのメンバーシップと役割に固有のデータベース オブジェクトを見ていきます。


[![さまざまなテーブル、ビュー、およびストアド プロシージャがデータベースに追加されました](creating-the-membership-schema-in-sql-server-cs/_static/image29.png)](creating-the-membership-schema-in-sql-server-cs/_static/image28.png)

**図 10**: する各種のテーブル、ビュー、およびストアド プロシージャがデータベースに追加されました ([フルサイズの画像を表示する をクリックします](creating-the-membership-schema-in-sql-server-cs/_static/image30.png))。


> [!NOTE]
> `aspnet_regsql.exe`ツールのグラフィカル ユーザー インターフェイスは、アプリケーション全体のサービス スキーマをインストールします。 実行するときに、`aspnet_regsql.exe`コマンドラインからどのような特定のアプリケーション サービスをインストール (または削除) のコンポーネントを指定できます。 そのため、テーブルだけを追加する場合は、ビュー、およびストアド プロシージャのために必要な`SqlMembershipProvider`と`SqlRoleProvider`実行プロバイダー`aspnet_regsql.exe`コマンドラインから。 または、手動で実行できます、適切な T-SQL のサブセットで使用されるスクリプトを作成する`aspnet_regsql.exe`します。 これらのスクリプトにある、`WINDIR%\Microsoft.Net\Framework\v2.0.50727\`のような名前のフォルダー `InstallCommon.sql`、`InstallMembership.sql`、`InstallRoles.sql`、 `InstallProfile.sql`、`InstallSqlState.sql`など。


この時点で必要なデータベース オブジェクトを作成しましたが、`SqlMembershipProvider`します。 ただし、あとに使用することをメンバーシップ フレームワークに指示する、 `SqlMembershipProvider` (versus、たとえば、 `ActiveDirectoryMembershipProvider`) して、`SqlMembershipProvider`を使用する必要があります、`SecurityTutorials`データベース。 使用するには、どのようなプロバイダーを指定する方法と手順 4. で選択したプロバイダーの設定をカスタマイズする方法に注目します。 まずは、作成されたデータベース オブジェクトについて詳しく説明してみましょう。

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>手順 3: を見て、スキーマの中核となるテーブル

ASP.NET アプリケーションのメンバーシップとロールのフレームワークを使用する場合、実装の詳細は、プロバイダーによってカプセル化されます。 これらのフレームワークを使用して、.NET Framework のインターフェイスはチュートリアルで将来`Membership`と`Roles`クラス。 これらの高度な Api を使用する場合は、どのようなクエリの実行によってどのようなテーブルが変更などの低レベルの詳細自分たちについて懸念する必要はありません、`SqlMembershipProvider`と`SqlRoleProvider`します。

そのため、手順 2. で作成されたデータベース スキーマを調査することがなくメンバーシップとロールのフレームワークを使用しましたでした自信を持って。 ただし、アプリケーション データを格納するテーブルを作成するときにユーザーまたはロールに関連するエンティティを作成することがあります。 理解することが役立ちます、`SqlMembershipProvider`と`SqlRoleProvider`スキーマの外部の確立時にアプリケーション データのテーブルと手順 2. で作成されたこれらのテーブル間の制約のキーします。 さらに、特定のまれな状況で、ユーザーとやり取りする必要があります、ロールがデータベース レベルで直接格納されます (使用せず、`Membership`または`Roles`クラス)。

### <a name="partitioning-the-user-store-into-applications"></a>アプリケーションへのユーザー ストアのパーティション分割

メンバーシップとロールのフレームワークでは、1 つのユーザーおよびロール ストアは、多数の異なるアプリケーション間で共有できるように設計されています。 ロールのメンバーシップまたはフレームワークを使用する ASP.NET アプリケーションでは、使用するには、どのようなアプリケーション パーティションを指定する必要があります。 つまり、複数の web アプリケーションでは、同じユーザーおよびロール ストアを使用できます。 図 11 は 3 つのアプリケーションにパーティション分割すると、ユーザーとロールのストアを示しています。 HRSite、CustomerSite、および SalesSite します。 これら 3 つの web アプリケーションをそれぞれ独自の一意のユーザーとロールをまだ同じデータベース テーブルで、ユーザー アカウントとロール情報を物理的に格納すべて。


[![ユーザー アカウントは、複数のアプリケーションでパーティション分割することがあります。](creating-the-membership-schema-in-sql-server-cs/_static/image32.png)](creating-the-membership-schema-in-sql-server-cs/_static/image31.png)

**図 11**: ユーザー アカウントがするパーティション分割されて間で複数のアプリケーション ([フルサイズの画像を表示する をクリックします](creating-the-membership-schema-in-sql-server-cs/_static/image33.png))。


`aspnet_Applications`テーブルは、これらのパーティションを定義します。 ユーザー アカウント情報を格納するデータベースを使用する各アプリケーションは、このテーブル内の行によって表されます。 `aspnet_Applications`テーブルには 4 つの列: `ApplicationId`、 `ApplicationName`、 `LoweredApplicationName`、および`Description`します。 `ApplicationId` 種類は[ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx)テーブルの主キー`ApplicationName`アプリケーションごとに一意のわかりやすい名前を提供します。

メンバーシップと役割に関連するテーブルにリンクして、`ApplicationId`フィールドに`aspnet_Applications`します。 たとえば、`aspnet_Users`テーブルで、ユーザー アカウントごとのレコードが含まれているが、 `ApplicationId` 。 外部キー フィールド以外も同上、`aspnet_Roles`テーブル。 `ApplicationId`これらのテーブル内のフィールドは、アプリケーション パーティションのユーザー アカウントを指定しますまたは、ロールが所属します。

### <a name="storing-user-account-information"></a>ユーザー アカウント情報を格納します。

ユーザー アカウント情報が 2 つのテーブルに格納されています:`aspnet_Users`と`aspnet_Membership`します。 `aspnet_Users`の不可欠なユーザー アカウント情報を保持するフィールドがテーブルに含まれています。 最も関連する 3 つの列は次のとおりです。

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` 主キー (と型の`uniqueidentifier`)。 `UserName` 種類は`nvarchar(256)`になり、このパスワードをユーザーの資格情報をします。 (ユーザーのパスワードは保存される、`aspnet_Membership`テーブルです)。`ApplicationId`で特定のアプリケーションにユーザー アカウントをリンク`aspnet_Applications`します。 ある複合[`UNIQUE`制約](https://msdn.microsoft.com/library/ms191166.aspx)上、`UserName`と`ApplicationId`列。 こうことで、特定のアプリケーションは各ユーザー名が一意でまだこれは、同じ`UserName`さまざまなアプリケーションで使用します。

`aspnet_Membership`テーブルには、ユーザーのパスワード、電子メール アドレス、最後のログイン日と時間、およびなどのように、追加のユーザー アカウント情報が含まれています。 内のレコードは一対一で対応、`aspnet_Users`と`aspnet_Membership`テーブル。 このリレーションシップのことを確認して、`UserId`フィールドに`aspnet_Membership`テーブルの主キーとして機能します。 ように、`aspnet_Users`テーブル`aspnet_Membership`が含まれています、`ApplicationId`特定のアプリケーション パーティションにこの情報に結合するフィールド。

### <a name="securing-passwords"></a>パスワードをセキュリティで保護します。

パスワードの情報が格納されている、`aspnet_Membership`テーブル。 `SqlMembershipProvider`により、次の 3 つの手法の 1 つを使用してデータベースに格納するためのパスワード。

- **クリア**-パスワードがプレーン テキストとしてデータベースに格納されます。 このオプションを使用はお勧めします。 ハッカーがバック ドアまたは - データベースへのアクセスを持つ不満を抱いている従業員を検索することで、データベースが侵害された場合は、すべて 1 つのユーザーの資格情報はれることがありますは。
- **ハッシュ**-パスワードが一方向のハッシュ アルゴリズムおよびランダムに生成された salt 値を使用してハッシュされます。 (Salt) と共にこのハッシュ値は、データベースに格納されます。
- **暗号化された**-パスワードの暗号化バージョンは、データベースに格納されます。

使用されるパスワード ストレージ手法によって異なります、`SqlMembershipProvider`で指定された設定`Web.config`します。 カスタマイズに注目するは、`SqlMembershipProvider`手順 4. で設定します。 既定の動作では、パスワードのハッシュを格納します。

パスワードを格納する列は`Password`、 `PasswordFormat`、および`PasswordSalt`します。 `PasswordFormat` 型のフィールドである`int`値を持つが、パスワードを格納するために使用される手法を示します: オフの場合は 0; Hashed の 1; 2 を暗号化します。 `PasswordSalt` 使用されています。 パスワード ストレージ手法に関係なく、ランダムに生成された文字列が割り当てられています。値`PasswordSalt`パスワードのハッシュを計算する場合にのみ使用します。 最後に、`Password`実際のパスワード データを含む列、プレーン テキスト パスワード、パスワード、または暗号化されたパスワードのハッシュは、します。

表 1 は、何これら 3 つの列のようになります、さまざまなストレージ手法の MySecret パスワードを格納する場合を示しています。 .

| **ストレージ手法&lt;\_o3a\_p/&gt;** | **パスワード&lt;\_o3a\_p/&gt;** | **PasswordFormat&lt;\_o3a\_p/&gt;** | **PasswordSalt&lt;\_o3a\_p/&gt;** |
| --- | --- | --- | --- |
| Clear | MySecret! | 0 | tTnkPlesqissc2y2SMEygA== |
| ハッシュ | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| 暗号化 | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**表 1.**: パスワード MySecret を格納するときに、パスワードに関連するフィールドの値の例です。

> [!NOTE]
> 特定の暗号化またはハッシュのアルゴリズムで使用される、`SqlMembershipProvider`の設定によって決まりますが、`<machineKey>`要素。 手順 3 では、この構成要素を説明した、 <a id="Tutorial3"> </a> [*フォーム認証の構成と高度なトピック*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)チュートリアル。


### <a name="storing-roles-and-role-associations"></a>ロールとロールの関連付けを格納します。

ロール フレームワークでは、ロールのセットを定義し、どのようなユーザー ロールに属しているを指定できます。 この情報は 2 つのテーブルを使用して、データベースのキャプチャ:`aspnet_Roles`と`aspnet_UsersInRoles`します。 内の各レコード、`aspnet_Roles`テーブルは、特定のアプリケーション ロールを表します。 ほぼ同じように、 `aspnet_Users` 、テーブル、`aspnet_Roles`ディスカッションに関連する 3 つの列をテーブルには。

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` 主キー (と型の`uniqueidentifier`)。 `RoleName` は `nvarchar(256)` 型です。 `ApplicationId`で特定のアプリケーションにユーザー アカウントをリンク`aspnet_Applications`します。 複合`UNIQUE`の制約、`RoleName`と`ApplicationId`列の場合、特定のアプリケーションで各ロール名が一意であることを確認します。

`aspnet_UsersInRoles`テーブルは、ユーザーとロール間のマッピングとして機能します。 -2 つの列がある`UserId`と`RoleId`-複合主キーを構成しているとします。

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>手順 4: プロバイダーを指定して、その設定のカスタマイズ

すべてのメンバーシップとロールのフレームワークなどのプロバイダー モデルをサポートするフレームワークの自体の実装の詳細がないし、代わりにプロバイダー クラスには、その責任を委任します。 場合は、メンバーシップ フレームワーク、`Membership`クラスは、ユーザー アカウントを管理するための API を定義しますが、任意のユーザー ストアと直接やり取りしません。 代わりに、`Membership`クラスのメソッドの手に要求の構成済みのプロバイダーを使用する、`SqlMembershipProvider`します。 内のメソッドのいずれかを呼び出すときに、`Membership`クラス、メンバーシップ フレームワークを認識する方法への呼び出しの委任、`SqlMembershipProvider`でしょうか。

`Membership`クラスには、 [ `Providers`プロパティ](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx)メンバーシップ フレームワークによってすべての使用可能な登録済みのプロバイダー クラスへの参照を格納しています。 登録されている各プロバイダーは、関連付けられている名前と種類をが。 名前で特定のプロバイダーを参照する人が読みやすい方法を提供する、`Providers`型は、プロバイダー クラスを識別中に、コレクション。 さらに、各登録済みのプロバイダーは、構成設定を含めることができます。 メンバーシップ フレームワークの構成設定を含める`passwordFormat`と`requiresUniqueEmail`、多数あります。 によって使用される構成設定の完全な一覧については、テーブル 2 を参照してください、`SqlMembershipProvider`します。

`Providers`プロパティの内容は、web アプリケーションの構成の設定で指定します。 既定では、すべての web アプリケーションがあるという名前のプロバイダー`AspNetSqlMembershipProvider`型の`SqlMembershipProvider`します。 この既定のメンバーシップ プロバイダーが登録されている`machine.config`(ある`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG)`:

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample1.xml)]

上記に示すマークアップとして、 [ `<membership>`要素](https://msdn.microsoft.com/library/1b9hw62f.aspx)中にメンバーシップ フレームワークの構成設定を定義、 [ `<providers>`子要素](https://msdn.microsoft.com/library/6d4936ht.aspx)登録されているを指定しますプロバイダー。 追加することがありますまたはを使用して削除されたプロバイダー、 [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx)または[ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx)要素は使用して、 [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx)現在すべてを削除する要素登録済みのプロバイダー。 上記に示すマークアップとして`machine.config`という名前のプロバイダーを追加します。`AspNetSqlMembershipProvider`型の`SqlMembershipProvider`します。

加え、`name`と`type`、属性、`<add>`要素には、さまざまな構成設定の値を定義する属性が含まれています。 表 2 は、使用可能な一覧`SqlMembershipProvider`のそれぞれの説明と共に、特定の構成設定。

> [!NOTE]
> 表 2 に記載されているすべての既定値で定義された既定値を参照してください、`SqlMembershipProvider`クラス。 注意してくださいで構成設定はすべて`AspNetSqlMembershipProvider`の既定値に対応して、`SqlMembershipProvider`クラス。 たとえば、メンバーシップ プロバイダーに、指定されていない場合、`requiresUniqueEmail`の既定値を true に設定します。 ただし、`AspNetSqlMembershipProvider`の値を明示的に指定して、この既定値をオーバーライド`false`します。


| **設定&lt;\_o3a\_p/&gt;** | **説明&lt;\_o3a\_p/&gt;** |
| --- | --- |
| `ApplicationName` | メンバーシップ フレームワークにより、複数のアプリケーション パーティションに分割する 1 人のユーザー ストアのことを思い出してください。 この設定は、メンバーシップ プロバイダーによって使用されているアプリケーション パーティションの名前を示します。 かどうかはこの値が明示的に指定されていない、アプリケーションの仮想ルート パスの値に、実行時に、設定されています。 |
| `commandTimeout` | SQL コマンドのタイムアウト値を秒単位で指定します。 既定値は、30 です。 |
| `connectionStringName` | 内の接続文字列の名前、`<connectionStrings>`ユーザー ストア データベースへの接続に使用する要素。 この値が必要です。 |
| `description` | 登録済みのプロバイダーのわかりやすい説明を提供します。 |
| `enablePasswordRetrieval` | ユーザーが忘れたパスワードを取得可能性があるかどうかを指定します。 既定値は `false` です。 |
| `enablePasswordReset` | ユーザーが自分のパスワードをリセットできるかどうかを示します。 既定値は `true` です。 |
| `maxInvalidPasswordAttempts` | 中に、指定した特定のユーザーに対して発生する失敗したログイン試行の最大数`passwordAttemptWindow`ユーザーがロックアウトされるまでにします。既定値は 5 です。 |
| `minRequiredNonalphanumericCharacters` | ユーザーのパスワードで使用する必要があります英数字以外の文字の最小数。 この値は 0 と 128; で指定する必要があります。既定では 1 です。 |
| `minRequiredPasswordLength` | パスワードに必要な文字の最小数。 この値は 0 と 128; で指定する必要があります。既定値は、7 です。 |
| `name` | 登録済みのプロバイダーの名前。 この値が必要です。 |
| `passwordAttemptWindow` | 分単位では、ログイン試行を追跡できませんでした。 ユーザーが無効なログイン資格情報を提供している場合`maxInvalidPasswordAttempts`時刻では、これは、ウィンドウを指定すると、ロックアウトされます。既定値は 10 です。 |
| `PasswordFormat` | パスワードのストレージ形式: `Clear`、 `Hashed`、または`Encrypted`します。 既定値は `Hashed` です。 |
| `passwordStrengthRegularExpression` | 指定した場合、この正規表現が自分のパスワードを変更する場合、または新しいアカウントを作成するときに、ユーザーの選択したパスワードの強度を評価するために使用します。 既定値は空の文字列です。 |
| `requiresQuestionAndAnswer` | 彼のセキュリティの質問を取得または自分のパスワードをリセットする際にユーザーが回答する必要があるかどうかを指定します。 既定値は `true` です。 |
| `requiresUniqueEmail` | 特定のアプリケーション パーティション内のすべてのユーザー アカウントが一意の電子メール アドレスが必要かどうかを示します。 既定値は `true` です。 |
| `type` | プロバイダーの種類を指定します。 この値が必要です。 |

**表 2.**: メンバーシップと`SqlMembershipProvider`構成設定

ほかに`AspNetSqlMembershipProvider`のようなマークアップを追加して、アプリケーションごとにその他のメンバーシップ プロバイダーを登録することがあります、`Web.config`ファイル。

> [!NOTE]
> 同じ方法でロール フレームワークの動作: で、既定の登録済みのロール プロバイダーがある`machine.config`でのアプリケーションによるごとに登録されているプロバイダーをカスタマイズすることが、`Web.config`します。 今後のチュートリアルでは、ロールのフレームワークとその構成マークアップの詳細についてを究明します。


### <a name="customizing-thesqlmembershipprovidersettings"></a>カスタマイズ、`SqlMembershipProvider`設定

既定の`SqlMembershipProvider`(`AspNetSqlMembershipProvider`) がその`connectionStringName`属性に設定`LocalSqlServer`します。 ように、`AspNetSqlMembershipProvider`プロバイダー、接続文字列名`LocalSqlServer`で定義されている`machine.config`します。

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample2.xml)]

この接続文字列がデータベースにある SQL 2005 Express Edition を定義して、ご覧のとおり |DataDirectory|aspnetdb.mdf します。 文字列 |DataDirectory | は実行時 をポイントするように変換されます、`~/App_Data/`ディレクトリ、そのため、データベースのパス |変換されます DataDirectory|aspnetdb.mdf" `~/App_Data` /`aspnet.mdf`します。

アプリケーションのでは、メンバーシップ プロバイダー情報お指定しなかったかどうか`Web.config`ファイル、アプリケーションは、登録されている既定のメンバーシップ プロバイダーを使用して`AspNetSqlMembershipProvider`します。 場合、`~/App_Data/aspnet.mdf`データベースが存在しないか、ASP.NET ランタイムに自動的にそれを作成し、アプリケーションのサービス スキーマを追加します。 ただし、使用するたく、`aspnet.mdf`を使用する代わりに、データベースでは、`SecurityTutorials.mdf`手順 2. で作成したデータベース。 この変更は、2 つの方法のいずれかで実行できます。

- <strong>値を指定、</strong><strong>`LocalSqlServer`</strong><strong>接続文字列名を</strong><strong>`Web.config`</strong><strong>します。</strong> 上書きすることで、`LocalSqlServer`の接続文字列名値`Web.config`、登録されている既定のメンバーシップ プロバイダーを使用します (`AspNetSqlMembershipProvider`) で正しく動作させることが、`SecurityTutorials.mdf`データベース。 この方法は、コンテンツで指定された構成設定を使用する場合は問題ありません。`AspNetSqlMembershipProvider`します。 この手法の詳細については、次を参照してください。 [Scott Guthrie](https://weblogs.asp.net/scottgu/)のブログ投稿「[使用して SQL Server 2000 または SQL Server 2005 に ASP.NET 2.0 アプリケーション サービスを構成する](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)します。
- <strong>型の新しい登録済みプロバイダーの追加</strong><strong>`SqlMembershipProvider`</strong><strong>構成とその</strong><strong>`connectionStringName`</strong><strong>をポイントする設定</strong><strong>`SecurityTutorials.mdf`</strong><strong>データベース。</strong> この方法は、データベース接続文字列だけでなく他の構成プロパティをカスタマイズするシナリオで役立ちます。 自分のプロジェクトでは常に、その柔軟性と読みやすさのためこの方法を使用します。

参照する新しい登録済みのプロバイダーを追加するため、`SecurityTutorials.mdf`データベースでは、まず必要があります内の適切な接続文字列値を追加する、`<connectionStrings>`セクション`Web.config`します。 次のマークアップは、という名前の新しい接続文字列を追加します。`SecurityTutorialsConnectionString`を参照する SQL Server 2005 Express Edition`SecurityTutorials.mdf`データベースに、`App_Data`フォルダー。

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample3.xml)]

> [!NOTE]
> 別のデータベース ファイルを使用している場合は、必要に応じて、接続文字列を更新します。 正しい接続文字列を形成する詳細についてを参照してください[ConnectionStrings.com](http://www.connectionstrings.com/)します。

次のメンバーシップ構成マークアップを次に、追加、`Web.config`ファイル。 このマークアップは、という名前の新しいプロバイダーを登録します。`SecurityTutorialsSqlMembershipProvider`します。

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample4.xml)]

登録するだけでなく、`SecurityTutorialsSqlMembershipProvider`プロバイダーは、上記のマークアップを定義、`SecurityTutorialsSqlMembershipProvider`既定のプロバイダーとして (を使用して、`defaultProvider`属性、`<membership>`要素)。 メンバーシップ フレームワークに登録されている複数のプロバイダーを使用できることを思い出してください。 `AspNetSqlMembershipProvider`の最初のプロバイダーとして登録されて`machine.config`、それ以外の場合に指定しない限り、既定のプロバイダーとして機能します。

現在、アプリケーションには 2 つの登録済みプロバイダー:`AspNetSqlMembershipProvider`と`SecurityTutorialsSqlMembershipProvider`します。 登録する前に、ただし、`SecurityTutorialsSqlMembershipProvider`すべて以前クリアがでしたプロバイダーの登録済みプロバイダーを追加して、 [ `<clear />`要素](https://msdn.microsoft.com/library/t062y6yc.aspx)する直前に、`<add>`要素。 これはクリア、 `AspNetSqlMembershipProvider` 、登録されているプロバイダーの一覧からことを意味、`SecurityTutorialsSqlMembershipProvider`のみの登録済みのメンバーシップ プロバイダーになります。 このアプローチを使用したかどうかは、マークすることは必要はありません、`SecurityTutorialsSqlMembershipProvider`既定のプロバイダーとしてためことが唯一の登録済みのメンバーシップ プロバイダー。 使用しての詳細については`<clear />`を参照してください[Using`<clear />`と追加のプロバイダー](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)します。

なお、`SecurityTutorialsSqlMembershipProvider`の`connectionStringName`、単に追加された参照の設定`SecurityTutorialsConnectionString`接続文字列名、およびその`applicationName`SecurityTutorials の値に設定が設定されています。 さらに、`requiresUniqueEmail`設定に設定されている`true`します。 その他のすべての構成オプションの値と同じ`AspNetSqlMembershipProvider`します。 自由にする場合は、ここでは、構成変更を加えます。 たとえば、または 7 ではなく、8 文字に、パスワードの長さを増やすことで、1 つではなく 2 つの英数字以外の文字を要求することで、パスワードの強度を強化でした。

> [!NOTE]
> メンバーシップ フレームワークにより、複数のアプリケーション パーティションに分割する 1 人のユーザー ストアのことを思い出してください。 メンバーシップ プロバイダーの`applicationName`設定は、ユーザー ストアを使用する場合、プロバイダーを使用して、どのようなアプリケーションを示します。 値を明示的に設定することが重要では、`applicationName`構成設定のため場合、`applicationName`が実行時に、web アプリケーションの仮想ルートのパスに割り当てられていることを明示的に設定されていません。 正常に機能しますが、アプリケーションの仮想ルートのパスが変更されない限り、アプリケーションを別のパスに移動する場合、`applicationName`設定が変更されます。 この場合、別のアプリケーション パーティションの操作に使用されていたよりも、メンバーシップ プロバイダーが開始されます。 別のアプリケーション パーティションに移動する前に作成されたユーザー アカウントが存在し、それらのユーザーがサイトにログインできなくなります。 この問題の詳細な議論については、次を参照してください。[は常に設定、`applicationName`プロパティときに構成する ASP.NET 2.0 メンバーシップとその他のプロバイダー](https://weblogs.asp.net/scottgu/443634)します。


## <a name="summary"></a>まとめ

この時点で、構成されたアプリケーション サービスでのデータベースがある (`SecurityTutorials.mdf`) メンバーシップ フレームワークを使用するように、web アプリケーションを構成し、`SecurityTutorialsSqlMembershipProvider`プロバイダーを登録します。 この登録済みのプロバイダーが型`SqlMembershipProvider`ありその`connectionStringName`適切な接続文字列に設定 (`SecurityTutorialsConnectionString`) とその`applicationName`値が明示的に設定します。

これで、アプリケーションからメンバーシップ フレームワークを使用する準備が整いました。 次のチュートリアルでは、新しいユーザー アカウントを作成する方法を説明します。 次のことは、ユーザー ベースの承認を実行して、データベースに追加のユーザー関連情報を格納する、ユーザーの認証をについて学びます。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [常に設定、`applicationName`プロパティ ASP.NET 2.0 を構成するときにメンバーシップとその他のプロバイダー](https://weblogs.asp.net/scottgu/443634)
- [アプリケーション サービスを使用して SQL Server 2000 または SQL Server 2005 の ASP.NET 2.0 を構成します。](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [SQL Server Management Studio Express Edition をダウンロードします。](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [ASP.NET 2.0 を調べて s メンバーシップ、ロール、およびプロファイル](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [`<add>` Membership の Providers の要素](https://msdn.microsoft.com/library/whae3t94.aspx)
- [`<membership>`要素](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [`<providers>`メンバーシップ要素](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [使用して`<clear />`プロバイダーを追加する場合](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [直接操作、 `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>このチュートリアルに含まれるトピックのビデオ トレーニング

- [ASP.NET メンバーシップについて理解する](../../../videos/authentication/understanding-aspnet-memberships.md)
- [メンバーシップ スキーマと連動するように SQL を構成する](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [既定のメンバーシップ スキーマのメンバーシップ設定を変更する](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>執筆者紹介

Scott Mitchell、複数の受け取ります書籍の著者と、4GuysFromRolla.com の創設者では、1998 年から、Microsoft Web テクノロジに取り組んできました。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は *[Sams 教える自分で ASP.NET 2.0 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* します。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Alicja Maziarz でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[ mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com)します。

> [!div class="step-by-step"]
> [次へ](creating-user-accounts-cs.md)
