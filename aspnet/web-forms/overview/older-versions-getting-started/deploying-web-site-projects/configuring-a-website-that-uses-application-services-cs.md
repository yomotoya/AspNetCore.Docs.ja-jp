---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
title: アプリケーション サービス (c#) を使用する web サイトの構成 |Microsoft Docs
author: rick-anderson
description: ASP.NET version 2.0 では、一連の .NET Framework の一部であり、一連のビルディング ブロックされるサービスは、yo として機能するアプリケーション サービスを導入しています.
ms.author: aspnetcontent
ms.date: 04/23/2009
ms.assetid: 1e33d1c6-3f9f-4c26-81e2-2a8f8907bb05
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
msc.type: authoredcontent
ms.openlocfilehash: 9bbf6d84c3ca25a3476901ec3d7996d5ca197446
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837653"
---
<a name="configuring-a-website-that-uses-application-services-c"></a>アプリケーション サービス (c#) を使用する web サイトを構成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_cs.pdf)

> ASP.NET version 2.0 では、一連の .NET Framework の一部であり、一連の web アプリケーションに豊富な機能を追加するために使用できるビルド ブロックのサービスとして使用するアプリケーション サービスが導入されました。 このチュートリアルでは、アプリケーション サービスを使用する実稼働環境で web サイトを構成する方法について説明し、ユーザー アカウントと実稼働環境でロールの管理に関する一般的な問題に対処します。


## <a name="introduction"></a>はじめに

ASP.NET version 2.0 に導入された、一連の*アプリケーション サービス*、web アプリケーションに豊富な機能を追加して、.NET Framework の一部である一連の文書パーツのサービスとして機能を使用できます。 アプリケーション サービスは次のとおりです。

- **メンバーシップ**- 作成して、ユーザー アカウントを管理するための api。
- **ロール**- ユーザーをグループに分類するための api。
- **プロファイル**- カスタムのユーザーに固有のコンテンツを格納するための api。
- **サイト マップ**- 階層リンク メニューなどのナビゲーション コントロールを使用して表示する階層構造の形式でサイトの論理構造を定義するための api。
- **パーソナル化**と共に最もよく使用、カスタマイズの基本設定を維持するための api - [ *WebParts*](https://msdn.microsoft.com/library/e0s9t4ck.aspx)します。
- **正常性の監視**- パフォーマンス、セキュリティ、エラー、および実行中の web アプリケーションの他のシステム正常性メトリックを監視するための api。
  

アプリケーション サービスの Api は特定の実装に関連付けられていません。 使用して、特定のアプリケーション サービスに指示する代わりに、*プロバイダー*、し、そのプロバイダーが特定のテクノロジを使用してサービスを実装します。 Web ホスト会社でホストされているインターネット ベースの web アプリケーションの最もよく使用されるプロバイダーとは、SQL Server データベースの実装を使用して、それらのプロバイダーです。 たとえば、`SqlMembershipProvider`は Microsoft SQL Server データベースのユーザー アカウント情報を格納するメンバーシップ API 用のプロバイダーです。

アプリケーション サービスと SQL Server プロバイダーを使用して、アプリケーションをデプロイするときにいくつかの課題を追加します。 まず、アプリケーション サービスのデータベース オブジェクトを開発と運用環境の両方のデータベースに正常に作成し、適切に初期化する必要があります。 する必要がある重要な構成設定もあります。

> [!NOTE]
> Api を使用して設計されたアプリケーション サービス、 [*プロバイダー モデル*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)s、API の実行時に提供する実装の詳細を許可する設計パターン。 .NET Framework を使用できるなど、アプリケーション サービス プロバイダーが付属しています、`SqlMembershipProvider`と`SqlRoleProvider`Membership の providers は、SQL Server を使用してロール Api データベースの実装。 作成することもでき、プラグイン、カスタム プロバイダー。 実際には、書籍レビューの web アプリケーションに既にがサイト マップ API 用カスタム プロバイダーが含まれます (`ReviewSiteMapProvider`)、データからサイト マップを構築する、`Genres`と`Books`データベース内のテーブル。


このチュートリアルでは、メンバーシップとロール Api を使用する、書籍レビューの web アプリケーションを拡張する方法を参照してください。 SQL Server データベースの実装でアプリケーション サービスを使用し、最後に、ユーザー アカウントと実稼働環境でロールの管理に関する一般的な問題に対処する web アプリケーションを配置する方法をについて説明します。

## <a name="updates-to-the-book-reviews-application"></a>書籍レビューのアプリケーションを更新する

ここ数は、書籍レビューの web アプリケーションが、データに基づく動的な web アプリケーションに静的な web サイトから更新されたチュートリアルは、一連のジャンルとレビューを管理するための管理 ページの完了します。 ただし、この [管理] セクションは現在保護されていません - 管理ページの URL を知っている (または推測) ユーザーできますワルツのように、作成、編集、またはサイトでレビューを削除します。 Web サイトの特定の部分を保護する一般的な方法では、ユーザー アカウントを実装し、URL 承認規則を使用して、特定のユーザーまたはロールのアクセスを制限します。 このチュートリアルでダウンロード可能な書籍レビューの web アプリケーションでは、ユーザー アカウントとロールをサポートします。 1 つが定義されているロールに管理者がという名前し、このロールのユーザーのみが管理ページにアクセスできます。

> [!NOTE]
> 書籍レビューの web アプリケーションで 3 つのユーザー アカウントを作成したら: Scott、Jisun、および Alice です。 次の 3 つのすべてのユーザー パスワードは、同じである:**パスワード。** Scott と Jisun が管理者の役割では、Alice はありません。 サイト %s の非管理ページは、匿名ユーザーに引き続きアクセスできます。 それを管理する場合を除き、サイトにサインインする必要がないは、その場合は、管理者ロールのユーザーとしてにサインインする必要があります。


認証および匿名ユーザーの別のユーザー インターフェイスを含める、書籍レビューのアプリケーションのマスター ページが更新されました。 匿名ユーザーのサイトにアクセスする場合は、右上隅でログイン リンクが表示されます。 認証されたユーザーが、メッセージが表示されます"こそ*username*!" ログアウトへのリンク。S ログイン ページもあります (`~/Login.aspx`)、訪問者を認証するため、ユーザー インターフェイスとロジックを提供する、ログイン Web コントロールを含むです。 管理者のみには、新しいアカウントを作成できます。 (作成と管理のユーザー アカウントのページは、`~/Admin`フォルダー)。

### <a name="configuring-the-membership-and-roles-apis"></a>メンバーシップとロール Api を構成します。

書籍レビューの web アプリケーションでは、ユーザー アカウントをサポートし、それらのユーザーをロール (つまり、管理者ロール) にグループ化する、メンバーシップとロール Api を使用します。 `SqlMembershipProvider`と`SqlRoleProvider`プロバイダー クラスは、SQL Server データベースのアカウントとロールの情報を格納するために使用します。

> [!NOTE]
> このチュートリアルは、メンバーシップとロール Api をサポートするために web アプリケーションを構成する方法の詳細についてはではありません。 これらの Api とそれらを使用する web サイトを構成するために必要な手順を詳しく見るには、「マイ[ *web サイトのセキュリティのチュートリアル*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)します。


SQL Server データベースとアプリケーション サービスを使用するロール情報が格納されているとするユーザー アカウント データベースにこれらのプロバイダーで使用されるデータベース オブジェクトを追加する必要があります。 これらの必要なデータベース オブジェクトには、さまざまなテーブル、ビュー、およびストアド プロシージャが含まれます。 それ以外の場合、指定されていない限り、`SqlMembershipProvider`と`SqlRoleProvider`という名前の SQL Server Express Edition データベースを使用して、プロバイダー クラス`ASPNETDB`アプリケーション s 内にある`App_Data`フォルダーは自動的に作成されたそのようなデータベースが存在しない場合実行時にこれらのプロバイダーで必要なデータベース オブジェクト。

可能であればと、アプリケーション サービスの web サイトのアプリケーションに固有のデータが格納されている同じデータベース内のデータベース オブジェクトを作成する通常最適です。 という名前のツールが付属しています、.NET Framework`aspnet_regsql.exe`指定したデータベースでデータベース オブジェクトをインストールします。 私だったら、続けてこのツールを使用して、これらのオブジェクトを追加、`Reviews.mdf`データベースに、`App_Data`フォルダー (開発用データベース)。 運用データベースにこれらのオブジェクトを追加する際に、このチュートリアルの後半でこのツールを使用する方法を見ていきます。

アプリケーションがデータベースにデータベース オブジェクトを以外のサービスを追加する場合`ASPNETDB`をカスタマイズする必要があります、`SqlMembershipProvider`と`SqlRoleProvider`プロバイダーは、適切なデータベースを使用するように構成をクラスします。 メンバーシップ プロバイダーの追加をカスタマイズする、 [ *&lt;メンバーシップ&gt;要素*](https://msdn.microsoft.com/library/1b9hw62f.aspx)内、`<system.web>`セクション`Web.config`; を使用して、 [ *&lt;roleManager&gt;要素*](https://msdn.microsoft.com/library/ms164660.aspx)ロール プロバイダーを構成します。 次のスニペットは、書籍レビューのアプリケーション s から取得`Web.config`し、メンバーシップとロール Api の構成設定を示しています。 注 - 新しいプロバイダーを登録する両方`ReviewMembership`と`ReviewRole`-を使用して、`SqlMembershipProvider`と`SqlRoleProvider`プロバイダー、それぞれします。

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample1.xml)]

`Web.config`ファイルの`<authentication>`要素がフォーム ベース認証をサポートするために構成されていることもできます。
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>管理ページへのアクセスを制限します。

ASP.NET では、簡単に特定のファイルまたはフォルダーのユーザーまたはロールを使用してアクセス許可または拒否にその*URL 承認*機能します。 (で URL 承認を簡単に説明した、 *Core の相違点の間で IIS と ASP.NET 開発サーバー*チュートリアルと、IIS と ASP.NET 開発サーバーが静的に異なる URL 承認規則を適用する方法を説明しました。動的なコンテンツです。) との比較アクセスを禁止するため、`~/Admin`管理者ロールのユーザーを除くフォルダー、URL 承認規則をこのフォルダーに追加する必要があります。 具体的には、URL の承認規則は、管理者ロールにユーザーを許可および他のすべてのユーザーを拒否する必要があります。 これは、追加することで実現を`Web.config`ファイルを`~/Admin`内容を次のフォルダー。

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample3.xml)]

ASP.NET の URL 認証機能とユーザーとロールについては、承認規則を使用する方法の詳細についてお読みくださいの[*ユーザー ベースの承認*](../../older-versions-security/membership/user-based-authorization-cs.md)と[*ロールベースの承認*](../../older-versions-security/roles/role-based-authorization-cs.md)チュートリアルからマイ[ *web サイトのセキュリティのチュートリアル*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)します。

## <a name="deploying-a-web-application-that-uses-application-services"></a>アプリケーション サービスを使用する Web アプリケーションを展開します。

アプリケーション サービス、およびアプリケーションのサービス情報をデータベースに格納するプロバイダーを使用する web サイトを展開する場合は、実稼働データベースで、アプリケーション サービスで必要なデータベース オブジェクトを作成することが重要です。 最初に、実稼働データベースは存在しないこれらのオブジェクト、そのため、アプリケーションが最初にデプロイされた (またはアプリケーション サービスを追加した後は、最初に配置されたとき)、これらの必要なデータベース オブジェクトを取得する追加の手順を行う必要があります、実稼働データベース。

別の課題は、開発環境を運用環境で作成されたユーザー アカウントをレプリケートする場合は、アプリケーション サービスを使用する web サイトを展開するときに発生します。 メンバーシップとロールの構成によっては、実稼働データベースを開発環境で作成されたユーザー アカウントを正常にコピーする場合でも、これらのユーザーが運用環境で web アプリケーションにサインインできないことができます。 この問題の原因を見てがされことを防ぐ方法について説明します。

ASP.NET は、優れたが付属しています[ *Web サイト管理ツール (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx)を Visual Studio から起動できるでき、ユーザー アカウント、ロール、および承認の規則を web ベースで管理します。インターフェイスです。 残念ながら、WSAT だけが、ローカルの web サイト、リモートで管理ユーザー アカウント、ロール、および運用環境で web アプリケーションの承認規則を使用できないことを意味します。 実稼働 web サイトから WSAT のような動作を実装する方法に注目します。

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>データベース オブジェクトを使用して aspnet を追加する\_regsql.exe

*データベースを配置する*チュートリアルは、実稼働データベースに、開発用データベースからテーブルとデータをコピーする方法と、これらの手法は、アプリケーション サービス データベース オブジェクトのコピーを確実に使用できます、実稼働データベース。 もう 1 つのオプションは、`aspnet_regsql.exe`ツールでは、追加またはデータベースからアプリケーション サービスのデータベース オブジェクトを削除します。

> [!NOTE]
> `aspnet_regsql.exe`ツールは、指定したデータベースでデータベース オブジェクトを作成します。 これは、データを移行しませんでは、データベース オブジェクト開発用データベースから運用データベースにします。 開発用データベースの実稼働データベースにユーザー アカウントとロール情報をコピーすることを意味する場合で説明する方法を使用して、*データベースを配置する*チュートリアル。


運用環境のデータベースを使用するデータベース オブジェクトを追加する方法を見て s をできるように、`aspnet_regsql.exe`ツール。 Windows エクスプ ローラーを開き %WINDIR%\ コンピューターに .NET Framework version 2.0 のディレクトリに移動して開始します。Microsoft.NET\Framework\v2.0.50727 します。 表示されます、`aspnet_regsql.exe`ツール。 このツールは、コマンドラインで使用できますが、グラフィカル ユーザー インターフェイスも含まれています。ダブルクリックして、`aspnet_regsql.exe`のグラフィカルなコンポーネントを起動するファイル。

その目的を説明するスプラッシュ画面を表示することで、ツールを開始します。 図 1 に示されている「セットアップ オプションを選択」画面に進みの横にあるをクリックします。 ここからは、追加のアプリケーション サービス、データベース オブジェクトまたはデータベースから削除できます。 運用データベースにこれらのオブジェクトを追加するため、"アプリケーション サービスの SQL Server を構成する"オプションを選択し、[次へ] をクリックします。


[![SQL Server アプリケーション サービスを構成します。](configuring-a-website-that-uses-application-services-cs/_static/image2.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image1.jpg)

**図 1**: SQL Server を構成するアプリケーション サービスの選択 ([フルサイズの画像を表示する をクリックします](configuring-a-website-that-uses-application-services-cs/_static/image3.jpg))。


「、サーバーとデータベースの選択」画面については、データベースに接続するように要求されます。 データベース サーバー、セキュリティ資格情報と、web ホスト会社によって提供されたデータベース名を入力し、[次へ] をクリックします。

> [!NOTE]
> データベース サーバーと資格情報を入力した後にデータベースのドロップダウン リストを展開するときにエラーが発生する可能性があります。 `aspnet_regsql.exe`クエリ ツール、`sysdatabases`システム テーブルに、サーバーがこの情報は公開されているように、データベース サーバーを企業のロックをホストしている一部の web 上のデータベースの一覧を取得します。 このエラーが発生した場合は、ドロップダウン リストに直接、データベース名を入力できます。


[![データベースの接続に関する情報、ツールを提供します。](configuring-a-website-that-uses-application-services-cs/_static/image5.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image4.jpg)

**図 2**: データベースのツールの接続情報を指定 ([フルサイズの画像を表示する をクリックします](configuring-a-website-that-uses-application-services-cs/_static/image6.jpg))。


後続の画面は、アプリケーション サービスのデータベース オブジェクトは、指定されたデータベースに追加する予定の実行、つまりされるアクションを要約します。 この操作の完了の横にある をクリックします。 しばらくすると、最後の画面が表示されます、(図 3 参照)、データベース オブジェクトが追加されたことを注意してください。


[![Success!アプリケーション サービスのデータベース オブジェクトが、実稼働データベースに追加されました。](configuring-a-website-that-uses-application-services-cs/_static/image8.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image7.jpg)

**図 3**: 成功! アプリケーション サービス データベース オブジェクトに追加された運用データベース ([フルサイズの画像を表示する をクリックします](configuring-a-website-that-uses-application-services-cs/_static/image9.jpg))。


アプリケーション サービスのデータベース オブジェクトが、実稼働データベースに正常に追加されたことを確認するには、SQL Server Management Studio を開き、実稼働データベースに接続します。 データベースで、アプリケーション サービス データベースのテーブルがわかります図 4 に示すよう`aspnet_Applications`、 `aspnet_Membership`、`aspnet_Users`となります。


[![データベース オブジェクトが、実稼働データベースに追加されたことを確認します。](configuring-a-website-that-uses-application-services-cs/_static/image11.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image10.jpg)

**図 4**: データベース オブジェクトが、実稼働データベースに追加されたことを確認します ([フルサイズの画像を表示する をクリックします](configuring-a-website-that-uses-application-services-cs/_static/image12.jpg))。


使用する必要がありますのみ、`aspnet_regsql.exe`ツールまたはアプリケーション サービスの使用を開始した後は、最初に、最初に、web アプリケーションをデプロイするときにします。 これらのデータベース オブジェクトが、実稼働データベースでは、再追加または変更する必要はありませんが勝利しました。

### <a name="copying-user-accounts-from-development-to-production"></a>開発から運用環境にユーザー アカウントのコピー

使用する場合、`SqlMembershipProvider`と`SqlRoleProvider`、SQL Server データベースにアプリケーションのサービス情報を格納するプロバイダー クラスなど、データベース テーブルのさまざまなユーザー アカウントとロールの情報が格納されている`aspnet_Users`、 `aspnet_Membership`、`aspnet_Roles`、および`aspnet_UsersInRoles`、他のユーザーの間で。 開発中に、開発環境でユーザー アカウントを作成する場合は、該当するデータベース テーブルから、対応するレコードをコピーして運用環境でこれらのユーザー アカウントをレプリケートできます。 開発をさらに、運用環境で作成されたユーザー アカウントになりますレコードをコピーすることも選択した可能性があります、アプリケーション サービス データベース オブジェクトを展開するには、データベース パブリッシュ ウィザードを使用します。 場合、 ただし、構成の設定によってそれらのユーザー アカウントを持つため開発で作成した、実稼働環境にコピーが、実稼働 web サイトからのログインをできないことを見つけることがあります。 何のためでしょう。

`SqlMembershipProvider`と`SqlRoleProvider`プロバイダー クラスが 1 つのデータベースは、重複するユーザー名を持つユーザーおよびロールと同じ名前を指定することで各アプリケーションできますが、理論上は、複数のアプリケーション ユーザー ストアとして使用できますできるように設計されました。 この柔軟性は、データベースでのアプリケーションの一覧を保持、`aspnet_Applications`テーブル、および各ユーザーは、これらのアプリケーションに関連付けられています。 具体的には、`aspnet_Users`テーブルには、`ApplicationId`列内のレコードには、各ユーザーを結び付ける、`aspnet_Applications`テーブル。

加え、`ApplicationId`列、`aspnet_Applications`テーブルも含まれています、`ApplicationName`列は、アプリケーションの複数の人が読みやすい名前を提供します。 Web サイトが指示する必要がありますが、ログイン ページからユーザーの資格情報の検証などのユーザー アカウントを使用しようとしたときに、`SqlMembershipProvider`クラスを使用するには、どのようなアプリケーションです。 これによって、アプリケーション名を指定し、これは通常、値は s のプロバイダーの構成から`Web.config`経由で具体的には、`applicationName`属性。

どうすれば、`applicationName`で属性が指定されていない`Web.config`でしょうか。 このような場合、メンバーシップ システムとしてアプリケーション ルートのパスを使用して、`applicationName`値。 場合、`applicationName`属性が明示的に設定されていない`Web.config`、その後、開発環境と運用環境別のアプリケーション ルートを使用し、そのため、別のアプリケーションに関連付けられる可能性があります。アプリケーション サービスの名前。 開発環境で作成したユーザーの場合は、このような不一致が発生した場合、`ApplicationId`と一致しない値、`ApplicationId`運用環境の値。 最終的な結果は、t が勝利したそれらのユーザーが、ログインできることです。

> [!NOTE]
> 場合、自分で、このような場合は、一致しないで実稼働環境にコピーするユーザー アカウントを使って検索`ApplicationId`値 - これらが正しくないを更新するクエリを記述する`ApplicationId`値を`ApplicationId`運用環境で使用します。 更新と、開発環境で作成されたアカウントを持つユーザーは運用上の web アプリケーションにサインインできるがようになりました。


良い知らせは、同じ 2 つの環境を使用していることを確認するための簡単な手順が`ApplicationId`- 明示的に設定されて、`applicationName`属性`Web.config`アプリケーション サービス プロバイダーのすべての。 明示的に設定されている、`applicationName`で属性"BookReviews"を`<membership>`と`<roleManager>`要素からこのスニペットとして`Web.config`を示しています。

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample4.xml)]

設定の詳細については、`applicationName`属性とその根拠を参照してください[ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) s ブログの投稿「 [*常 applicationName の設定ASP.NET メンバーシップとその他のプロバイダーを構成するときにプロパティ*](https://weblogs.asp.net/scottgu/443634)します。

### <a name="managing-user-accounts-in-the-production-environment"></a>運用環境でのユーザー アカウントの管理

ASP.NET Web サイト管理ツール (WSAT) 簡単に作成し、ユーザー アカウントの管理、定義し、ロールを適用およびユーザーとロール ベースの承認規則を説明します。 ソリューション エクスプ ローラーに移動し、ASP.NET の構成 アイコンをクリックして、または web サイトまたはプロジェクトのメニューに移動し、ASP.NET の構成のメニュー項目を選択して、Visual Studio から WSAT を起動できます。 残念ながら、WSAT はローカルの web サイトでのみ使用できます。 そのため、運用環境で web サイトを管理するのに、ワークステーションから、WSAT を使用できません。

良い知らせは、メンバーシップとロール Api を介してプログラムでは、WSAT によって提供される、すべての公開されている機能さらに、WSAT 画面の多くは、標準の ASP.NET ログイン関連コントロールを使用します。 つまり、必要な管理機能を提供する web サイトに ASP.NET ページを追加できます。

前のチュートリアルに含める書籍レビューの web アプリケーションが更新されたことを思い出してください、`~/Admin`フォルダー、およびこのフォルダーが管理者ロールのユーザーのみを許可する構成されています。 という名前のフォルダーにページを追加した`CreateAccount.aspx`からを管理者は新しいユーザー アカウントを作成します。 このページでは、CreateUserWizard コントロールを使用して、新しいユーザー アカウントを作成するためのユーザー インターフェイスとバックエンド ロジックを表示します。 含める新しいユーザーが管理者ロールに追加することもあるかどうかの入力を要求する チェック ボックス コントロールをカスタマイズした新機能、さらに多く (図 5 を参照してください)。 若干の開発タスクを実装すると、ユーザーとロール管理に関連するそれ以外の場合に、WSAT で指定するページのカスタム セットを構築できます。

> [!NOTE]
> 詳細については、メンバーシップとロール Api を使用して、ログインに関連する ASP.NET Web コントロールと共にお読みくださいのマイ[ *web サイトのセキュリティのチュートリアル*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)します。 CreateUserWizard コントロールをカスタマイズする方法の詳細を参照してください、 [*ユーザー アカウントを作成する*](../../older-versions-security/membership/creating-user-accounts-cs.md)と[*追加ユーザーの情報を格納する*](../../older-versions-security/membership/storing-additional-user-information-cs.md)チュートリアル、またはチェック アウト[ *Erich Peterson* ](http://www.erichpeterson.com/)記事[ *CreateUserWizard コントロールをカスタマイズする*](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![管理者は、新しいユーザー アカウントを作成できます。](configuring-a-website-that-uses-application-services-cs/_static/image14.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image13.jpg)

**図 5**: 管理者は新しいユーザー アカウントを作成 ([フルサイズの画像を表示する をクリックします](configuring-a-website-that-uses-application-services-cs/_static/image15.jpg))。


WSAT チェック アウトの完全な機能が必要なかどうかは[*ローリング、独自の Web サイト管理ツール*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)、Dan Clem を著者には、カスタム WSAT のようなツールを構築する処理手順について説明します。 Dan は、アプリケーションのソース コード (内の C# の場合) を共有し、ホストされる web サイトに追加するための手順を提供します。

## <a name="summary"></a>まとめ

アプリケーション サービス データベースの実装を使用する web アプリケーションをデプロイするときに実稼働データベースに必要なデータベース オブジェクトが含まれる最初ようにする必要があります。 説明した手法を使用して、これらのオブジェクトを追加することができます、*データベースを配置する*チュートリアルです。 また、使用することができます、`aspnet_regsql.exe`ツール、このチュートリアルで説明したようにします。 (これは重要なは、運用環境では有効にするには、ユーザーと、開発環境で作成されたロールの場合)、開発および運用環境との手法で使用されるアプリケーション名の同期が中心に説明したその他の課題ユーザーと運用環境でロールを管理します。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [*ASP.NET SQL Server 登録ツール (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*SQL Server のアプリケーション サービス データベースを作成します。*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*SQL Server でメンバーシップ スキーマの作成*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md)
- [*ASP.NET のメンバーシップ、ロール、およびプロファイルを調べる*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*独自の Web サイト管理ツールのローリング*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Web サイトのセキュリティのチュートリアル*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Web サイト管理ツールの概要*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [前へ](configuring-the-production-web-application-to-use-the-production-database-cs.md)
> [次へ](strategies-for-database-development-and-deployment-cs.md)
