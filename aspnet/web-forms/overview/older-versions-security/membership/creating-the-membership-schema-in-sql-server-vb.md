---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
title: SQL Server (VB) でのメンバーシップのスキーマの作成 |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルは、データベースに、SqlMembershipProvider を使用するために必要なスキーマを追加するための手法を調べることで開始します。 次に、お wi しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 112a674d-716f-41a6-99b8-4074d65a54c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 44458e8022f1f0d52cf136ad7fbaa5dd1f546632
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
---
<a name="creating-the-membership-schema-in-sql-server-vb"></a>SQL Server (VB) でのメンバーシップのスキーマの作成
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_vb.pdf)

> このチュートリアルは、データベースに、SqlMembershipProvider を使用するために必要なスキーマを追加するための手法を調べることで開始します。 次に、スキーマ内のキーのテーブルを確認およびその目的と重要性について説明されます。 このチュートリアルは、ASP.NET アプリケーション メンバーシップ フレームワークが使用するプロバイダーを確認する方法を見てで終了します。


## <a name="introduction"></a>はじめに

Web サイトの訪問者を識別するフォーム認証を使用して調べる 2 つの前のチュートリアルです。 認証チケットを使用してアクセスが簡単に、ユーザー web サイトにログインし、ページ間でそれらを記憶する開発者向けのフォーム認証フレームワークでできます。 `FormsAuthentication`クラスには、チケットを生成して、訪問者の cookie への追加のメソッドが含まれています。 `FormsAuthenticationModule`すべての着信要求を調べ、有効な認証チケットを使用して、それらを作成および関連付けます、`GenericPrincipal`と`FormsIdentity`現在の要求を持つオブジェクト。 フォーム認証は、ログインと、以降の要求でユーザーの id を確認するには、そのチケットを解析するときに、訪問者に認証チケットを付与するためのメカニズムだけを実行します。 ユーザー アカウントをサポートするために web アプリケーションでまだユーザー ストアを実装し、新しいユーザー、およびその他のユーザー アカウントに関連するタスクが多数の登録を資格情報を検証する機能を追加する必要があります。

ASP.NET 2.0 では、前に、開発者は、これらすべてのユーザー アカウントに関連するタスクを実装するためのフックでした。 幸運にも ASP.NET チームはこの欠点を認識し、メンバーシップ フレームワーク ASP.NET 2.0 で導入されました。 メンバーシップのフレームワークとは、中核となるユーザー アカウントに関連するタスクを実行するためのプログラム インターフェイスを提供する .NET Framework のクラスのセットです。 このフレームワークが上に構築される、[プロバイダー モデル](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)、カスタマイズされた実装を標準化された API に接続できます。

説明したように、 <a id="Tutorial1"> </a> [*セキュリティの基礎と ASP.NET サポート*](../introduction/security-basics-and-asp-net-support-vb.md)チュートリアルでは、次の 2 つの組み込みのメンバーシップ プロバイダーが付属して .NET Framework: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx)および[ `SqlMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx)です。 その名前からわかるように、`SqlMembershipProvider`ユーザー ストアとして Microsoft SQL Server データベースを使用します。 アプリケーションでこのプロバイダーを使用するためには、ストアとして使用するには、どのようなデータベース プロバイダーに指示する必要があります。 ご想像のとおり、`SqlMembershipProvider`して、特定のデータベース テーブル、ビュー、およびストアド プロシージャ、ユーザー ストアのデータベースが必要です。 選択したデータベースにこの予期されるスキーマを追加する必要があります。

このチュートリアルを使用するために必要なスキーマをデータベースに追加するための手法を調べることで開始、`SqlMembershipProvider`です。 次に、スキーマ内のキーのテーブルを確認およびその目的と重要性について説明されます。 このチュートリアルは、ASP.NET アプリケーション メンバーシップ フレームワークが使用するプロバイダーを確認する方法を見てで終了します。

開始しましょう!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>手順 1: ユーザーのストアを配置する場所を決定します。

ASP.NET アプリケーションのデータは通常、複数のデータベース内のテーブルに格納されます。 実装する場合、`SqlMembershipProvider`データベース スキーマおアプリケーション データと同じデータベースまたは別のデータベース メンバーシップ スキーマを配置するかどうかを決定する必要があります。

次の理由により、アプリケーション データと同じデータベースでメンバーシップ スキーマを検索することをお勧めします。

- **保守容易性**の 1 つのデータベースにカプセル化するデータを含むアプリケーションが理解しやすく、保守、2 つの独立したデータベースがあるアプリケーションよりも展開できます。
- **リレーショナル整合性**アプリケーションがそれをテーブルとして、同じデータベース内のメンバーシップに関連するテーブルの検索を確立することが[外部キー制約](http://en.wikipedia.org/wiki/Foreign_key)の主キーの間で、メンバーシップに関連するテーブルと関連するアプリケーションのテーブルです。

別のデータベースにユーザーのストアとアプリケーション データを分離することのみ合理的それぞれ別々 のデータベースを使用してが一般的なユーザー ストアを共有する必要がある複数のアプリケーションがある場合です。

### <a name="creating-a-database"></a>データベースを作成します。

2 番目のチュートリアルではデータベースがまだ必要ありませんのでを構築してきましたアプリケーションです。 必要がありますいずれかのようになりましたが、ただし、ユーザー ストアのです。 みましょういずれかを作成し、しを追加で必要なスキーマ、`SqlMembershipProvider`プロバイダー (手順 2. を参照してください)。

> [!NOTE]
> このチュートリアルの系列全体では、使用、 [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx)データベースに、アプリケーション テーブルを保存して、`SqlMembershipProvider`スキーマです。 この決定をした 2 つの理由: 最初に、そのコストのため無料の Express Edition は SQL Server 2005; の最も変わるようにアクセス可能なバージョンSQL Server 2005 Express Edition のデータベースを配置して、web アプリケーションの直接の 2 番目に、`App_Data`フォルダー、せずを再展開が任意の特殊なセットアップ手順と、データベースをパッケージ化し、web アプリケーションは同時に 1 つの ZIP ファイルを簡単になりますまたは、構成オプション。 進めるために非-Express Edition のバージョンの SQL Server を使用する場合は、自由にできます。 手順は、ほぼ同じです。 `SqlMembershipProvider`スキーマは Microsoft SQL Server 2000 のバージョンで動作し、セットアップします。

右クリックし、ソリューション エクスプ ローラーで、`App_Data`フォルダーに新しい項目の追加を選択します。 (表示されない場合、`App_Data`プロジェクトのフォルダーにソリューション エクスプ ローラーでプロジェクトを右クリックし、ASP.NET フォルダーの追加 を選択および pick `App_Data`)。新しい項目の追加 ダイアログ ボックスからという名前の新しい SQL データベースの追加を選択します。`SecurityTutorials.mdf`です。 このチュートリアルでは、追加、`SqlMembershipProvider`追加作成する後続のチュートリアルです。 このデータベースにスキーマを、アプリケーションのデータをキャプチャするテーブル。


[![App_Data フォルダーに SecurityTutorials.mdf データベースの名前を新しい SQL データベースを追加します。](creating-the-membership-schema-in-sql-server-vb/_static/image2.png)](creating-the-membership-schema-in-sql-server-vb/_static/image1.png)

**図 1**: 新しい SQL データベースの名前付きの追加`SecurityTutorials.mdf`データベースを`App_Data`フォルダー ([フルサイズのイメージを表示するをクリックして](creating-the-membership-schema-in-sql-server-vb/_static/image3.png))


データベースに追加する、`App_Data`フォルダーに自動的に含まれています、データベース エクスプ ローラーのビューです。 (非-Express Edition バージョンの Visual Studio では、データベース エクスプ ローラーといいますサーバー エクスプ ローラー。)データベース エクスプ ローラーに移動し、単に追加された順に展開`SecurityTutorials`データベース。 画面で、データベース エクスプ ローラーが表示されない場合は、表示 メニューに移動またはデータベース エクスプ ローラーを選択し Ctrl + Alt + S をヒットします。 図 2 に示す、`SecurityTutorials`データベースは空 - ないテーブル、ビュー、およびなしのストアド プロシージャが含まれています。


[![SecurityTutorials データベースは空です。](creating-the-membership-schema-in-sql-server-vb/_static/image5.png)](creating-the-membership-schema-in-sql-server-vb/_static/image4.png)

**図 2**:`SecurityTutorials`データベースは現在空 ([フルサイズのイメージを表示するをクリックして](creating-the-membership-schema-in-sql-server-vb/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>手順 2: 追加、`SqlMembershipProvider`データベースにスキーマ

`SqlMembershipProvider`テーブル、ビュー、およびストアド プロシージャのユーザー ストア データベースにインストールする特定のセットが必要です。 使用してこれらの必要なデータベース オブジェクトを追加することができます、 [ `aspnet_regsql.exe`ツール](https://msdn.microsoft.com/library/ms229862.aspx)です。 このファイルにあります、`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\`フォルダーです。

> [!NOTE]
> `aspnet_regsql.exe`ツールには、コマンド ライン機能と、グラフィカル ユーザー インターフェイスの両方が用意されています。 グラフィカル インターフェイスよりユーザー フレンドリなは、このチュートリアルで説明されます。 コマンド ライン インターフェイスが役に場合の追加、`SqlMembershipProvider`を自動化する必要があるスキーマ、スクリプトまたはテスト シナリオの自動ビルドのようです。

`aspnet_regsql.exe`ツールを使用して追加または削除する*ASP.NET アプリケーション サービス*指定した SQL Server データベースにします。 ASP.NET アプリケーション サービスのスキーマを含む、`SqlMembershipProvider`と`SqlRoleProvider`、他の ASP.NET 2.0 フレームワーク用の SQL ベースのプロバイダーのスキーマとします。 情報の 2 つのビットを提供する必要があります、`aspnet_regsql.exe`ツール。

- 追加またはアプリケーションのサービスを削除するか、および
- 追加またはアプリケーションのサービス スキーマを削除する元のデータベース

使用するには、データベースの入力を求めて、`aspnet_regsql.exe`ツールには、データベースに接続するためのセキュリティ資格情報に、データベースが存在するサーバーの名前とデータベース名を指定するよう求められます。 非 Express エディションの SQL Server を使用している場合を既に知っておく必要がこの情報は、ASP.NET web ページを使用してデータベースを使用する場合、接続文字列で指定する必要があります、同じ情報です。 SQL Server 2005 Express Edition のデータベースを使用する場合は、サーバーとデータベース名を決定、`App_Data`フォルダーはただし、少し複雑です。

次のセクションでは、内の SQL Server 2005 Express Edition データベースのサーバーとデータベース名を指定するための簡単な方法を調べ、`App_Data`フォルダーです。 SQL Server 2005 Express Edition お気軽にインストールをスキップし、使用していない場合、アプリケーションのサービス セクションです。

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>サーバーと SQL Server 2005 Express Edition データベースでのデータベース名を決定する、`App_Data`フォルダー

使用するために、`aspnet_regsql.exe`ツールがサーバーとデータベース名を把握する必要があります。 サーバー名が`localhost\InstanceName`です。 最も可能性の高い、 *InstanceName*は`SQLExpress`します。 ただし、手動で SQL Server 2005 Express Edition をインストールした場合 (つまり、インストールしなかった場合、自動的に Visual Studio のインストール中に)、別のインスタンス名が選択されている可能性があります。

データベース名は、決定を少し複雑になります。 内のデータベースの`App_Data`フォルダーでは、データベース名を含む通常がある、[グローバル一意識別子](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)と共にデータベース ファイルへのパス。 使用して、アプリケーション サービス スキーマを追加するためにこのデータベースの名前を決定する必要があります`aspnet_regsql.exe`です。

データベース名を確認するために最も簡単な方法では、SQL Server Management Studio を調べることです。 SQL Server Management Studio が SQL Server 2005 データベースを管理するためのグラフィカル インターフェイスを提供しますが、これは、Express エディションの SQL Server 2005 と共に含まれていません。 良いニュースは[ダウンロードできます](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)空き Express エディションの SQL Server Management Studio。

> [!NOTE]
> デスクトップにインストールされている SQL Server 2005 の非-Express Edition バージョンもあれば完全版の Management Studio がインストールされている可能性があります。 完全バージョンを使用すると、Express edition の場合、以下のように、同じ手順を次に、データベース名を決定します。

データベース ファイル上の Visual Studio によって課されるすべてのロックが閉じられていることを確認する Visual Studio を終了して開始します。 次に、SQL Server Management Studio を起動しへの接続、 `localhost\InstanceName` SQL Server 2005 Express Edition のデータベースです。 インスタンス名が前述のように、高く`SQLExpress`です。 認証オプションの場合は、Windows 認証を選択します。


[![SQL Server 2005 Express Edition のインスタンスに接続します。](creating-the-membership-schema-in-sql-server-vb/_static/image8.png)](creating-the-membership-schema-in-sql-server-vb/_static/image7.png)

**図 3**: SQL Server 2005 Express Edition インスタンスに接続 ([フルサイズのイメージを表示するをクリックして](creating-the-membership-schema-in-sql-server-vb/_static/image9.png))


SQL Server 2005 Express Edition のインスタンスに接続すると、Management Studio には、データベース、セキュリティ設定、サーバー オブジェクト、およびなどのフォルダーが表示されます。 データベース タブを展開する場合 \outputfromtestproviderdebugmode.txt、`SecurityTutorials.mdf`データベースが*いない*最初にデータベースをアタッチする必要があります - データベース インスタンスに登録します。

データベース フォルダーを右クリックし、コンテキスト メニューから接続を選択します。 これにより、データベースのアタッチ ダイアログ ボックスが表示されます。 ここでは、[追加] ボタンをクリックして、参照、`SecurityTutorials.mdf`データベース、および [ok] をクリックします。 図 4 は、データベースのアタッチ ダイアログ ボックスの後に、`SecurityTutorials.mdf`データベースが選択されています。 図 5 は、データベースが正常にアタッチされた後に、Management Studio のオブジェクト エクスプ ローラーを示します。


[![SecurityTutorials.mdf データベースをアタッチします。](creating-the-membership-schema-in-sql-server-vb/_static/image11.png)](creating-the-membership-schema-in-sql-server-vb/_static/image10.png)

**図 4**: アタッチ、`SecurityTutorials.mdf`データベース ([フルサイズのイメージを表示するをクリックして](creating-the-membership-schema-in-sql-server-vb/_static/image12.png))


[![データベース フォルダーに SecurityTutorials.mdf データベースが表示されます。](creating-the-membership-schema-in-sql-server-vb/_static/image14.png)](creating-the-membership-schema-in-sql-server-vb/_static/image13.png)

**図 5**:`SecurityTutorials.mdf`データベース フォルダーにデータベースが表示されます ([フルサイズのイメージを表示するをクリックして](creating-the-membership-schema-in-sql-server-vb/_static/image15.png))


図 5 に示す、`SecurityTutorials.mdf`データベースではなく abstruse 名前が指定されています。 変更してみましょう覚えやすい (と入力しやすい) の名前。 データベースを右クリックし、コンテキスト メニューでは、名前の変更を選択し、名前変更`SecurityTutorialsDatabase`です。 これは、ファイル名は変更されません、データベース名だけを使用して SQL Server を識別します。


[![SecurityTutorialsDatabase にデータベースの名前変更します。](creating-the-membership-schema-in-sql-server-vb/_static/image17.png)](creating-the-membership-schema-in-sql-server-vb/_static/image16.png)

**図 6**: データベースの名前を変更`SecurityTutorialsDatabase`([フルサイズのイメージを表示するをクリックして](creating-the-membership-schema-in-sql-server-vb/_static/image18.png))


この時点でのサーバーとデータベースの名前が分かって、`SecurityTutorials.mdf`データベース ファイル:`localhost\InstanceName`と`SecurityTutorialsDatabase`、それぞれします。 を通じてアプリケーション サービスをインストールする準備が整いました、`aspnet_regsql.exe`ツールです。

### <a name="installing-the-application-services"></a>アプリケーション サービスをインストールします。

起動する、`aspnet_regsql.exe`ツールを [スタート] メニューに移動し、実行を選択します。 入力`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` ボックスに ok をクリックします。 代わりに、Windows エクスプ ローラーを使用して、適切なフォルダーにドリルダウンし、ダブルクリックすることができます、`aspnet_regsql.exe`ファイル。 いずれかの方法では、同じ結果を net はします。

実行している、`aspnet_regsql.exe`コマンドライン引数を指定せずにツールが ASP.NET SQL Server セットアップ ウィザードのグラフィカル ユーザー インターフェイスを起動します。 ウィザードでは、簡単に追加したり、指定したデータベースでの ASP.NET アプリケーション サービスを削除します。 図 7 に示すように、ウィザードの最初の画面では、ツールの目的について説明します。


[![ASP.NET SQL Server セットアップ ウィザードによりを使用してメンバーシップ スキーマに追加するには](creating-the-membership-schema-in-sql-server-vb/_static/image20.png)](creating-the-membership-schema-in-sql-server-vb/_static/image19.png)

**図 7**: を使用して、ASP.NET SQL Server セットアップ ウィザードを使用する追加メンバーシップ スキーマ ([フルサイズのイメージを表示するをクリックして](creating-the-membership-schema-in-sql-server-vb/_static/image21.png))


ウィザードの 2 番目の手順要求するアプリケーション サービスを追加または削除するかどうか。 テーブル、ビュー、およびストアド プロシージャに必要なを追加するため、 `SqlMembershipProvider`、SQL Server の構成アプリケーション サービスのオプションを選択します。 後で、このスキーマをデータベースから削除する場合は、このウィザードを再実行が、代わりに既存のデータベース オプションから、情報を削除するアプリケーション サービスを選択します。


[![選択して、アプリケーション サービスのオプションの SQL Server の構成](creating-the-membership-schema-in-sql-server-vb/_static/image23.png)](creating-the-membership-schema-in-sql-server-vb/_static/image22.png)

**図 8**: アプリケーションのサービス オプションの SQL Server の構成を選択 ([フルサイズのイメージを表示するをクリックして](creating-the-membership-schema-in-sql-server-vb/_static/image24.png))


3 番目の手順は、データベースについては、メッセージが表示されます。 サーバー名、認証情報、およびデータベース名。 このチュートリアルに従ってされてし、追加されたかどうか、`SecurityTutorials.mdf`データベースを`App_Data`に接続されている`localhost\InstanceName`に、名前を変更`SecurityTutorialsDatabase`、以下の値を使用します。

- サーバー: `localhost\InstanceName`
- Windows 認証
- データベース: `SecurityTutorialsDatabase`


[![データベース情報を入力します。](creating-the-membership-schema-in-sql-server-vb/_static/image26.png)](creating-the-membership-schema-in-sql-server-vb/_static/image25.png)

**図 9**: データベースの情報を入力 ([フルサイズのイメージを表示するをクリックして](creating-the-membership-schema-in-sql-server-vb/_static/image27.png))


データベースの情報を入力すると、[次へ] をクリックします。 最後の手順では、実施する手順について説明します。 次にアプリケーション サービスをインストールし、ウィザードを完了し、[完了] をクリックします。

> [!NOTE]
> Management Studio を使用してデータベースをアタッチし、データベース ファイルの名前を変更した場合は、データベースをデタッチし、Visual Studio を再オープンする前に、Management Studio を終了することを確認します。 デタッチする、`SecurityTutorialsDatabase`データベース、データベース名を右クリックし、[タスク] メニューからデタッチします。

完了すると、ウィザードの Visual Studio に戻り、およびデータベース エクスプ ローラーに移動します。 [テーブル] フォルダーを展開します。 一連のテーブルのプレフィックスで始まる名前が表示されます`aspnet_`です。 同様に、さまざまなビューとストアド プロシージャは、ビューとストアド プロシージャのフォルダーの下で確認できます。 これらのデータベース オブジェクトは、アプリケーション サービスのスキーマを構成します。 手順 3 でのメンバーシップと役割に固有のデータベース オブジェクトを見ていきます。


[![さまざまなテーブル、ビュー、およびストアド プロシージャがデータベースに追加されました](creating-the-membership-schema-in-sql-server-vb/_static/image29.png)](creating-the-membership-schema-in-sql-server-vb/_static/image28.png)

**図 10**: A 各種のテーブル、ビュー、およびストアド プロシージャがデータベースに追加されました ([フルサイズのイメージを表示するをクリックして](creating-the-membership-schema-in-sql-server-vb/_static/image30.png))


> [!NOTE]
> `aspnet_regsql.exe`ツールのグラフィカル ユーザー インターフェイスは、アプリケーション全体のサービス スキーマをインストールします。 実行する際にも`aspnet_regsql.exe`コマンドラインからどのような特定のアプリケーション サービスをインストール (または削除) のコンポーネントを指定できます。 そのため、テーブルだけを追加する場合は、ビュー、およびストアド プロシージャに必要な`SqlMembershipProvider`と`SqlRoleProvider`実行プロバイダー`aspnet_regsql.exe`コマンドラインからです。 また、手動で実行できます、適切な T-SQL のサブセットで使用されるスクリプトを作成する`aspnet_regsql.exe`です。 あるこれらのスクリプト、`WINDIR%\Microsoft.Net\Framework\v2.0.50727\`のような名前を持つフォルダー `InstallCommon.sql`、 `InstallMembership.sql`、 `InstallRoles.sql`、 `InstallProfile.sql`、`InstallSqlState.sql`のようにします。

この時点で必要なデータベース オブジェクトを作成したが、`SqlMembershipProvider`です。 ただし、必要がありますにそれを使用することをメンバーシップ フレームワークに指示する、 `SqlMembershipProvider` (と言い、 `ActiveDirectoryMembershipProvider`) ことと、`SqlMembershipProvider`を使用する必要があります、`SecurityTutorials`データベース。 手順 4. で選択したプロバイダーの設定をカスタマイズする方法と使用するプロバイダーを指定する方法に紹介します。 まずは、作成されたデータベース オブジェクトを掘り下げるを見てみましょう。

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>手順 3: を見て、スキーマの主要なテーブル

ASP.NET アプリケーションでのメンバーシップとロールのフレームワークを使用する場合、実装の詳細は、プロバイダーによってカプセル化されます。 .NET Framework のを介してこれらのフレームワークとインターフェースはチュートリアル将来的に`Membership`と`Roles`クラスです。 これらの大まかな Api を使用する場合にどのようなクエリの実行によってどのテーブルが変更などの低レベルの詳細関係自分自身お必要はありません、`SqlMembershipProvider`と`SqlRoleProvider`です。

そのため、おでした自信を持ってフレームワークを使用して、メンバーシップとロール手順 2 で作成されるデータベース スキーマを持つため、探索なし。 ただし、アプリケーション データを格納するテーブルを作成するときにユーザーまたはロールに関連するエンティティを作成することがあります。 熟知していること、`SqlMembershipProvider`と`SqlRoleProvider`スキーマの外部の確立時にアプリケーションのデータ テーブルと手順 2. で作成、それらのテーブル間の制約のキーします。 さらに、まれに、ユーザーと対話する必要があります、ロールがデータベース レベルで直接格納 (使用せずに、`Membership`または`Roles`クラス)。

### <a name="partitioning-the-user-store-into-applications"></a>アプリケーションへのユーザー ストアのパーティション分割

メンバーシップとロールのフレームワークは、1 つのユーザーおよびロール ストアは、多数の異なるアプリケーション間で共有することができますになるように設計されています。 ロールのメンバーシップまたはフレームワークを使用する ASP.NET アプリケーションには、どのようなアプリケーション パーティションを使用する必要がありますを指定します。 要するに、複数の web アプリケーションは、同じユーザーおよびロール ストアを使用することができます。 図 11 は、3 つのアプリケーションにパーティション分割は、ユーザーおよびロールのストアを示しています。 HRSite、CustomerSite、および SalesSite です。 これら 3 つの web アプリケーションごとのある独自の一意のユーザーとロール、まだすべてに物理的に格納、ユーザー アカウントとロールについて、同じデータベース テーブルです。


[![ユーザー アカウントは、複数のアプリケーション間でパーティション分割することがあります。](creating-the-membership-schema-in-sql-server-vb/_static/image32.png)](creating-the-membership-schema-in-sql-server-vb/_static/image31.png)

**図 11**: ユーザー アカウントがするパーティション分割されているアプリケーション間で複数 ([フルサイズのイメージを表示するをクリックして](creating-the-membership-schema-in-sql-server-vb/_static/image33.png))


`aspnet_Applications`テーブルは、これらのパーティションの定義内容。 ユーザー アカウント情報を格納するデータベースを使用する各アプリケーションは、このテーブルの行で表されます。 `aspnet_Applications`テーブルには 4 つの列: `ApplicationId`、 `ApplicationName`、 `LoweredApplicationName`、および`Description`です。`ApplicationId` 型は[ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx)であり、テーブルの主キーです。`ApplicationName`アプリケーションごとに一意のわかりやすい名前を提供します。

他のメンバーシップおよびロールに関連するテーブルがリンクに戻す、`ApplicationId`フィールドに`aspnet_Applications`です。 たとえば、`aspnet_Users`ユーザー アカウントごとにレコードを含むテーブルには、`ApplicationId`外部キー フィールド以外の ditto、`aspnet_Roles`テーブル。 `ApplicationId`ユーザー アカウントに、アプリケーション パーティションを指定するこれらのテーブル内のフィールドまたはロールが所属します。

### <a name="storing-user-account-information"></a>ユーザー アカウント情報を格納します。

ユーザー アカウント情報が 2 つのテーブルに格納されています:`aspnet_Users`と`aspnet_Membership`です。 `aspnet_Users`テーブルには、基本的なユーザー アカウント情報を保持するフィールドが含まれています。 3 つの最も関連する列は次のとおりです。

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` 主キーである (型`uniqueidentifier`)。 `UserName` 型は`nvarchar(256)`になり、パスワード、と共に、ユーザーの資格情報をします。 (ユーザーのパスワードに保存される、`aspnet_Membership`テーブルです)。`ApplicationId`で特定のアプリケーションにユーザー アカウントをリンク`aspnet_Applications`です。 ある複合[`UNIQUE`制約](https://msdn.microsoft.com/library/ms191166.aspx)上、`UserName`と`ApplicationId`列です。 こうことをアプリケーションで指定された各ユーザー名は一意でまだこれにより、同じを`UserName`別のアプリケーションに使用します。

`aspnet_Membership`テーブルには、ユーザーのパスワード、電子メール アドレス、最後のログイン日と時間、およびようなど、追加のユーザー アカウント情報が含まれています。 内のレコードは一対一で対応、`aspnet_Users`と`aspnet_Membership`テーブル。 このリレーションシップを保証、`UserId`フィールドに`aspnet_Membership`テーブルの主キーとして機能します。 同様に、`aspnet_Users`テーブル、`aspnet_Membership`が含まれています、`ApplicationId`特定のアプリケーション パーティションにこの情報に結合するフィールドです。

### <a name="securing-passwords"></a>パスワードのセキュリティ保護

パスワードの情報が格納されている、`aspnet_Membership`テーブル。 `SqlMembershipProvider`では、次の 3 つの手法のいずれかを使用してデータベースに格納されるパスワード。

- **クリア**-パスワードがプレーン テキストとしてデータベースに格納されています。 このオプションを使用はお勧めします。 データベースが侵害された場合に、ハッカーがバックドアまたは - データベースのアクセス権を持つ悪意のある従業員を検索するは、すべて単一のユーザーの資格情報はれることがありますは。
- **ハッシュ**-パスワードは、一方向のハッシュ アルゴリズムおよびランダムに生成された salt 値を使用してハッシュされます。 このハッシュ値 (と共に salt) は、データベースに格納されます。
- **暗号化**-暗号化されたパスワードは、データベースに格納します。

使用されるパスワード ストレージ方法によって異なります、`SqlMembershipProvider`で指定された設定`Web.config`です。 カスタマイズを見ていきましょう、`SqlMembershipProvider`手順 4. で設定します。 既定の動作では、パスワードのハッシュを格納します。

パスワードを格納する列は`Password`、 `PasswordFormat`、および`PasswordSalt`です。 `PasswordFormat` 型のフィールドである`int`値を持つが、パスワードを格納するために使用される手法を示します。 オフの場合は 0 以外の場合は Hashed を 1 以外の場合は暗号化の 2 です。 `PasswordSalt` 使用されていません。 パスワードの記憶域手法に関係なく、ランダムに生成された文字列が割り当てられています。値`PasswordSalt`はパスワードのハッシュを計算するときにのみ使用します。 最後に、`Password`列には、実際のパスワード データが含まれています。 を使用して、プレーン テキスト パスワード、パスワード、または暗号化されたパスワードのハッシュ。

表 1 は、どのようなこれら 3 つの列のようになります、さまざまなストレージ技術の MySecret パスワードを格納する場合を示しています! である必要があります。

| **記憶域手法&lt;\_o3a\_p/&gt;** | **パスワード&lt;\_o3a\_p/&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Clear | MySecret! | 0 | tTnkPlesqissc2y2SMEygA== |
| ハッシュ | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| 暗号化 | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**表 1**: パスワード MySecret を格納するときに、パスワードに関連するフィールドの値の例です。

> [!NOTE]
> 特定の暗号化またはによって使用されるハッシュ アルゴリズム、`SqlMembershipProvider`の設定によって決まりますが、`<machineKey>`要素。 この構成要素の手順 3 で説明した、 <a id="Tutorial3"> </a> [*フォーム認証の構成と高度なトピック*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)チュートリアルです。

### <a name="storing-roles-and-role-associations"></a>ロールとロールの関連付けを格納します。

ロール フレームワークにより、開発者は、一連のロールを定義し、どのようなユーザー ロールに属しているを指定します。 この情報は 2 つのテーブルを使用してデータベースにキャプチャ:`aspnet_Roles`と`aspnet_UsersInRoles`です。 内の各レコード、`aspnet_Roles`テーブルは、特定のアプリケーション ロールを表します。 ほぼ同様に、 `aspnet_Users` 、テーブル、`aspnet_Roles`ここに関連する次の 3 つの列がテーブルにします。

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` 主キーである (型`uniqueidentifier`)。 `RoleName` は `nvarchar(256)` 型です。 および`ApplicationId`で特定のアプリケーションにユーザー アカウントをリンク`aspnet_Applications`です。 ある複合`UNIQUE`の制約を`RoleName`と`ApplicationId`列、特定のアプリケーションで各ロール名が一意であることを確認します。

`aspnet_UsersInRoles`テーブルは、ユーザーおよびロールの間のマッピングとして機能します。 -2 つの列がある`UserId`と`RoleId`- し、一緒に複合主キーを構成します。

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>手順 4: プロバイダーを指定して、その設定のカスタマイズ

すべてのメンバーシップとロールのフレームワークのなどのプロバイダー モデルをサポートするフレームワークの自体の実装の詳細が不足しているし、プロバイダー クラスには、その責任を委任する代わりにします。 メンバーシップ framework の場合、`Membership`クラスが、ユーザー アカウントを管理するための API を定義しますが、任意のユーザー ストアと直接やり取りしません。 代わりに、`Membership`クラスのメソッドに渡す要求の構成済みのプロバイダーを使用する、`SqlMembershipProvider`です。 内のメソッドのいずれかを呼び出しすると、`Membership`クラス、方法は、メンバーシップ framework 認識への呼び出しを委任する、`SqlMembershipProvider`しますか?

`Membership`クラスには、 [ `Providers`プロパティ](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx)メンバーシップ、フレームワークによってすべての使用可能な登録済みのプロバイダー クラスへの参照を格納しています。 各登録されているプロバイダーでは、関連付けられている名前と種類があります。 名前で特定のプロバイダーを参照するわかりやすい手段が提供、`Providers`コレクション型は、プロバイダー クラスを識別します。 さらに、各登録済みのプロバイダーには、構成設定が含まれます。 メンバーシップ フレームワークの構成設定を含める`PasswordFormat`と`requiresUniqueEmail`、その他の多くの間でします。 によって使用される構成設定の完全な一覧については、表 2 を参照してください、`SqlMembershipProvider`です。

`Providers`プロパティの内容は、web アプリケーションの構成の設定で指定します。 既定では、すべての web アプリケーションがあるという名前のプロバイダー`AspNetSqlMembershipProvider`型の`SqlMembershipProvider`します。 この既定のメンバーシップ プロバイダーが登録されている`machine.config`(にある`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`)。

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample1.xml)]

上に示すマークアップとして、 [ `<membership>`要素](https://msdn.microsoft.com/library/1b9hw62f.aspx)中に、メンバーシップ フレームワークの構成設定を定義、 [ `<providers>`子要素](https://msdn.microsoft.com/library/6d4936ht.aspx)登録されているを指定しますプロバイダー。 プロバイダーが追加することも、またはを使用して削除、 [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx)または[ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx)要素; 使用して、 [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx)現在すべてを削除する要素登録済みカスタム プロバイダー。 上に示すマークアップとして`machine.config`という名前のプロバイダーを追加`AspNetSqlMembershipProvider`型の`SqlMembershipProvider`します。

加え、`name`と`type`、属性、`<add>`要素には、さまざまな構成設定の値を定義する属性が含まれています。 表 2 は、使用可能な一覧`SqlMembershipProvider`-ごとの説明と共に、特定の構成設定。

> [!NOTE]
> 表 2 に記載されている任意の既定値で定義された既定値を参照してください、`SqlMembershipProvider`クラスです。 その not に注意してくださいすべての構成設定で`AspNetSqlMembershipProvider`の既定値に対応している、`SqlMembershipProvider`クラスです。 たとえば、メンバーシップ プロバイダーで指定されていない場合、`requiresUniqueEmail`の既定値を true に設定します。 ただし、`AspNetSqlMembershipProvider`の値を明示的に指定してこの既定値をオーバーライド`false`です。

| **設定&lt;\_o3a\_p/&gt;** | **説明&lt;\_o3a\_p/&gt;** |
| --- | --- |
| `ApplicationName` | メンバーシップ フレームワークを複数のアプリケーションにわたって分割する 1 人のユーザー ストアを利用することに注意してください。 この設定は、メンバーシップ プロバイダーによって使用されているアプリケーション パーティションの名前を示します。 かどうかはこの値が明示的に指定されていない、設定すると、アプリケーションの仮想ルート パスの値に、実行時にします。 |
| `commandTimeout` | SQL コマンドのタイムアウト値 (秒) を指定します。 既定値は 30 です。 |
| `connectionStringName` | 内の接続文字列の名前、`<connectionStrings>`ユーザー ストア データベースへの接続に使用する要素。 この値が必要です。 |
| `description` | 登録済みのプロバイダーのわかりやすい説明を提供します。 |
| `enablePasswordRetrieval` | ユーザーが自分のパスワードを取得可能性があるかどうかを指定します。 既定値は `false` です。 |
| `enablePasswordReset` | パスワードのリセットを許可されているかどうかを示します。 既定値は `true` です。 |
| `maxInvalidPasswordAttempts` | 指定した特定のユーザーに対して発生する可能性がありますの失敗したログイン試行の最大数`passwordAttemptWindow`ユーザーがロックアウトされるまでです。既定値は 5 です。 |
| `minRequiredNonalphanumericCharacters` | ユーザーのパスワードに表示することで、英数字以外の文字の最小数。 この値は 0 ~ 128; でなければなりません既定値は 1 です。 |
| `minRequiredPasswordLength` | パスワードに必要な文字の最小数。 この値は 0 ~ 128; でなければなりません既定値は 7 です。 |
| `name` | 登録済みのプロバイダーの名前。 この値が必要です。 |
| `passwordAttemptWindow` | ある時間を分単位では、ログイン試行を追跡できませんでした。 場合は、ユーザーが無効なログインの資格情報を提示`maxInvalidPasswordAttempts`内で時間枠を指定する、ロックアウトされます。既定値は 10 です。 |
| `PasswordFormat` | パスワードの記憶域形式: `Clear`、 `Hashed`、または`Encrypted`です。 既定値は、`Hashed` です。 |
| `passwordStrengthRegularExpression` | 指定した場合、この正規表現は自分のパスワードを変更する場合、または新しいアカウントを作成するときに、ユーザーの選択したパスワードの強度を評価する使用されます。 既定値は空の文字列です。 |
| `requiresQuestionAndAnswer` | ユーザーが自分または自分のパスワードをリセットするを取得する際は、セキュリティの質問に答える必要があるかどうかを指定します。 既定値は `true` です。 |
| `requiresUniqueEmail` | 特定のアプリケーション パーティション内のすべてのユーザー アカウントが一意の電子メール アドレスが必要かどうかを示します。 既定値は `true` です。 |
| `type` | プロバイダーの種類を指定します。 この値が必要です。 |

**表 2**: メンバーシップと`SqlMembershipProvider`構成設定

加え`AspNetSqlMembershipProvider`、その他のメンバーシップ プロバイダーは登録をアプリケーションごとのようなマークアップを追加することによって、`Web.config`ファイル。

> [!NOTE]
> ロール フレームワークで同じ方法: で既定の登録済みのロール プロバイダーがある`machine.config`と登録されているプロバイダーでのアプリケーション間ごとにカスタマイズできます。`Web.config`です。 今後のチュートリアルでは、ロール フレームワークとその構成マークアップの詳細を考察はします。

### <a name="customizing-thesqlmembershipprovidersettings"></a>カスタマイズ、`SqlMembershipProvider`設定

既定値`SqlMembershipProvider`(`AspNetSqlMembershipProvider`) がその`connectionStringName`属性に設定`LocalSqlServer`です。 同様に、`AspNetSqlMembershipProvider`プロバイダー、接続文字列名`LocalSqlServer`で定義された`machine.config`です。

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample2.xml)]

この接続文字列がデータベースにある SQL 2005 Express Edition を定義することがわかります |DataDirectory|aspnetdb.mdf です。 文字列 |DataDirectory |実行時 をポイントするのには変換、`~/App_Data/`ディレクトリ、ため、データベースのパス |変換される DataDirectory|aspnetdb.mdf `~/App_Data` /`aspnet.mdf`です。

アプリケーションのでは、メンバーシップ プロバイダー情報お指定しなかったかどうか`Web.config`ファイル、アプリケーションが登録されている既定のメンバーシップ プロバイダーを使用して`AspNetSqlMembershipProvider`です。 場合、`~/App_Data/aspnet.mdf`データベースが存在しないか、ASP.NET ランタイムが自動的に作成し、アプリケーションのサービス スキーマを追加します。 ただし、ここを使用しない、`aspnet.mdf`データベース; を使用する代わりに、`SecurityTutorials.mdf`手順 2. で作成したデータベースです。 この変更は、2 つの方法のいずれかで実行できます。

- <strong>値を指定、</strong><strong>`LocalSqlServer`</strong><strong>での接続文字列名</strong><strong>`Web.config`</strong><strong>です。</strong> 上書きすることによって、`LocalSqlServer`の接続文字列名値`Web.config`は登録されている既定のメンバーシップ プロバイダーを使用することができます (`AspNetSqlMembershipProvider`) で正しく動作させることが、`SecurityTutorials.mdf`データベース。 この方法は、コンテンツで指定された構成設定を使用する場合は問題ありません。`AspNetSqlMembershipProvider`です。 この方法の詳細については、次を参照してください。 [Scott Guthrie](https://weblogs.asp.net/scottgu/)のブログ投稿「[を使用して SQL Server 2000 または SQL Server 2005 に ASP.NET 2.0 アプリケーション サービスを構成する](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)です。
- <strong>型の場合は、新しい登録済みプロバイダーの追加</strong><strong>`SqlMembershipProvider`</strong><strong>構成とその</strong><strong>`connectionStringName`</strong><strong>をポイントする設定</strong><strong>`SecurityTutorials.mdf`</strong><strong>データベース。</strong> この方法は、データベース接続文字列だけでなく他の構成プロパティをカスタマイズする場合に便利です。 自分のプロジェクトでは常に、その柔軟性と読みやすさのためこの方法を使用します。

参照する新しい登録済みのプロバイダーを追加する、`SecurityTutorials.mdf`データベース、まず必要がありますに適切な接続文字列の値を追加する、 `<connectionStrings>` 」の「`Web.config`です。 次のマークアップがという名前の新しい接続文字列を追加`SecurityTutorialsConnectionString`を参照する SQL Server 2005 Express Edition`SecurityTutorials.mdf`データベースに格納されて、`App_Data`フォルダーです。

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample3.xml)]

> [!NOTE]
> 別のデータベース ファイルを使用している場合は、必要に応じて、接続文字列を更新します。 正しい接続文字列を形成する詳細についてを参照してください[ConnectionStrings.com](http://www.connectionstrings.com/)です。

次のメンバーシップ構成マークアップを次に、追加、`Web.config`ファイル。 このマークアップという名前の新しいプロバイダーを登録する`SecurityTutorialsSqlMembershipProvider`です。

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample4.xml)]

登録するだけでなく、`SecurityTutorialsSqlMembershipProvider`プロバイダー、上記のマークアップで定義、`SecurityTutorialsSqlMembershipProvider`既定のプロバイダーとして (を使用して、`defaultProvider`属性、`<membership>`要素)。 メンバーシップ フレームワークが複数の登録済みカスタム プロバイダーを持てることに注意してください。 `AspNetSqlMembershipProvider`の最初のプロバイダーとして登録されて`machine.config`、それ以外の場合に指定しない限り、既定のプロバイダーとして機能します。

アプリケーションが現時点では、次の 2 つの登録済みカスタム プロバイダーを持つ:`AspNetSqlMembershipProvider`と`SecurityTutorialsSqlMembershipProvider`です。 登録する前に、ただし、`SecurityTutorialsSqlMembershipProvider`おでしたがクリアすべて以前プロバイダーの登録済みプロバイダーを追加して、 [ `<clear />`要素](https://msdn.microsoft.com/library/t062y6yc.aspx)する直前に、`<add>`要素。 これオフには、 `AspNetSqlMembershipProvider` 、登録されているプロバイダーの一覧からことを意味、`SecurityTutorialsSqlMembershipProvider`のみに登録済みのメンバーシップ プロバイダーになります。 このアプローチを使用したかどうかは、ここはマークする必要はありません、`SecurityTutorialsSqlMembershipProvider`既定のプロバイダーとしてのでことが唯一の登録済みのメンバーシップ プロバイダー。 使用の詳細について`<clear />`を参照してください[Using`<clear />`と追加のプロバイダー](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)です。

注意してください、`SecurityTutorialsSqlMembershipProvider`の`connectionStringName`、単に追加された参照の設定`SecurityTutorialsConnectionString`接続文字列名、およびその`applicationName`SecurityTutorials の値に設定が設定されています。 さらに、`requiresUniqueEmail`設定に設定されている`true`です。 その他のすべての構成オプションは内の値と同じ`AspNetSqlMembershipProvider`です。 自由にここで構成変更を加える場合。 たとえば、1 つでなく 2 つの英数字以外の文字を要求することによって、または 7 の代わりに 8 文字に、パスワードの長さを増やすことで、パスワードの強度を強化可能性があります。

> [!NOTE]
> メンバーシップ フレームワークを複数のアプリケーションにわたって分割する 1 人のユーザー ストアを利用することに注意してください。 メンバーシップ プロバイダーの`applicationName`設定は、ユーザーのストアを使用する場合、プロバイダーを使用してアプリケーションを示します。 値を明示的に設定することが重要である、`applicationName`構成設定のため場合、`applicationName`が実行時に web アプリケーションの仮想ルートのパスに割り当てられている、明示的に設定されていません。 これは問題なく動作別のパスにアプリケーションを移動する場合は、アプリケーションの仮想ルート パスが変更されない限り、`applicationName`設定が変更されます。 この場合、メンバーシップ プロバイダーは使用されていたよりも、別のアプリケーション パーティションの操作を開始します。 別のアプリケーション パーティションで、移行する前に作成されたユーザー アカウントが存在して、それらのユーザーは、サイトにログインできなきます。 この問題の詳細については、次を参照してください。[は常に設定、`applicationName`プロパティと構成 ASP.NET 2.0 のメンバーシップとその他のプロバイダー](https://weblogs.asp.net/scottgu/443634)です。

## <a name="summary"></a>まとめ

この時点で構成されているアプリケーションのサービスとデータベースがある (`SecurityTutorials.mdf`) メンバーシップ フレームワークで使用できるように、web アプリケーションを構成し、`SecurityTutorialsSqlMembershipProvider`登録プロバイダー。 この登録されているプロバイダーの種類が`SqlMembershipProvider`あり、その`connectionStringName`を適切な接続文字列に設定 (`SecurityTutorialsConnectionString`) とその`applicationName`値が明示的に設定します。

これで、アプリケーションからメンバーシップ フレームワークを使用する準備が整いました。 次のチュートリアルでは、新しいユーザー アカウントを作成する方法を検討します。 次のことは、ユーザー ベースの承認を実行して、データベースに追加のユーザーに関連する情報を格納する、ユーザーの認証をについて学びます。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [常に設定、`applicationName`プロパティと構成 ASP.NET 2.0 のメンバーシップとその他のプロバイダー](https://weblogs.asp.net/scottgu/443634)
- [アプリケーション サービスを使用して SQL Server 2000 または SQL Server 2005 に ASP.NET 2.0 を構成します。](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [SQL Server Management Studio の Express Edition をダウンロードします。](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [ASP.NET 2.0 を調べて s メンバーシップ、ロール、およびプロファイル](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [`<add>`メンバーシップ プロバイダーの要素](https://msdn.microsoft.com/library/whae3t94.aspx)
- [`<membership>`要素](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [`<providers>`メンバーシップ要素](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [使用する`<clear />`と追加のプロバイダー](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [直接操作します `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>このチュートリアルに含まれるトピックに関するビデオ トレーニング

- [ASP.NET メンバーシップについて理解する](../../../videos/authentication/understanding-aspnet-memberships.md)
- [メンバーシップ スキーマと連動するように SQL を構成する](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [既定のメンバーシップ スキーマのメンバーシップ設定を変更する](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>作成者について

Scott Mitchell、複数の受け取りますブックの作成者と 4GuysFromRolla.com の創設者は、Microsoft の Web テクノロジと 1998 年取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書 *[Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*です。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログでを介して[ http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Alicja Maziarz しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)です。

> [!div class="step-by-step"]
> [前へ](storing-additional-user-information-cs.md)
> [次へ](creating-user-accounts-vb.md)
