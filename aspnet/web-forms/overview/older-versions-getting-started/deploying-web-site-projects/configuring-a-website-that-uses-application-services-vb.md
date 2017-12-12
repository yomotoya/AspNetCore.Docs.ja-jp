---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: "アプリケーション サービス (VB) を使用する web サイトの構成 |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET version 2.0 では、一連の .NET Framework の一部であるし、構築ブロックのスイート サービス yo として機能するアプリケーション サービスを導入しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: 7fb212638765589b998c4eca8265dfeb2910082f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="configuring-a-website-that-uses-application-services-vb"></a>アプリケーション サービス (VB) を使用する web サイトを構成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> ASP.NET version 2.0 には、一連の .NET Framework の一部であるし、一連の機能を追加する機能豊富な web アプリケーションに使用できるビルド ブロック サービスとして使用するアプリケーション サービスが導入されました。 このチュートリアルでは、アプリケーション サービスを使用する実稼働環境で web サイトを構成する方法について説明をユーザー アカウントと実稼働環境でロールの管理の一般的な問題に対処します。


## <a name="introduction"></a>はじめに

ASP.NET version 2.0 に導入された、一連の*アプリケーション サービス*、.NET Framework の一部であるおよびビルディング ブロックのスイート サービスとして機能するが、web アプリケーションに豊富な機能を追加に使用します。 アプリケーション サービスは次のとおりです。

- **メンバーシップ**- を作成して、ユーザー アカウントを管理するための API です。
- **ロール**- ユーザーをグループに分類するための API です。
- **プロファイル**- カスタムのユーザーに固有のコンテンツを格納するための API です。
- **サイト マップ**- メニューやパンくずリストなどのナビゲーション コントロールを使用して表示されることができますし、階層構造の形式でサイトの論理構造を定義するための API です。
- **パーソナル化**- で最もよく使用される、カスタマイズ設定を維持するための API [ *WebParts*](https://msdn.microsoft.com/en-us/library/e0s9t4ck.aspx)です。
- **正常性の監視**- パフォーマンス、セキュリティ、エラー、および実行中の web アプリケーションの他のシステム正常性メトリックを監視するための API です。
  

アプリケーション サービスの Api は、特定の実装に関連付けられていません。 アプリケーションのサービスを使用して、特定するように指示する代わりに、*プロバイダー*、し、そのプロバイダーが特定のテクノロジを使用して、サービスを実装します。 Web ポータルのホストでホストされているインターネット ベースの web アプリケーションに最もよく使用されるプロバイダーは、SQL Server データベースの実装を使用してこれらのプロバイダーです。 たとえば、 `SqlMembershipProvider` Microsoft SQL Server データベースにユーザー アカウント情報を格納するメンバーシップ API 用のプロバイダーです。

アプリケーションを展開するときに、いくつかの課題を追加するアプリケーション サービスと SQL Server プロバイダーを使用します。 まず、アプリケーション サービスのデータベース オブジェクトを開発および運用の両方のデータベースで適切に作成し、適切に初期化する必要があります。 重要な構成設定ができるようにする必要があります。

> [!NOTE]
> Api は、使用するように設計されていますが、アプリケーション サービス、 [*プロバイダー モデル*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)s の API の実行時に提供する実装の詳細をできるようにするデザイン パターンです。 .NET Framework が使用できるなど、アプリケーション サービス プロバイダーの数が付属しています、`SqlMembershipProvider`と`SqlRoleProvider`メンバーシップは、プロバイダーは、および SQL Server を使用してロール Api のデータベースの実装です。 作成することもでき、プラグイン、カスタム プロバイダー。 書評 web アプリケーションが既にが実際には、サイト マップ API 用のカスタム プロバイダーを含む (`ReviewSiteMapProvider`)、内のデータからのサイト マップを構築する、`Genres`と`Books`データベース内のテーブルです。


このチュートリアルは、メンバーシップとロールの Api を使用する書評 web アプリケーションを拡張した方法を見てを開始します。 これは、SQL Server データベースの実装でアプリケーション サービスを使用し、最後のユーザー アカウントと実稼働環境でロールを管理する一般的な問題を解決している web アプリケーションを配置する方法をについて説明します。

## <a name="updates-to-the-book-reviews-application"></a>Book レビュー アプリケーションへの更新

過去いくつかを Book レビュー web アプリケーションが、データ ドリブン動的な web アプリケーションに静的な web サイトから更新されたチュートリアルはジャンルとレビューを管理するための管理 ページのセットで完了します。 ただし、この管理セクションで現在保護されていません - すべてのユーザーの管理ページの URL を知っている (または推測) ことができますワルツのようにし、作成、編集、またはについては、サイトのレビューを削除します。 Web サイトの特定の部分を保護する一般的な方法では、ユーザー アカウントを実装し、URL 承認規則を使用して、特定のユーザーまたはロールのアクセスを制限します。 このチュートリアルをダウンロードできる書評 web アプリケーションでは、ユーザー アカウントとロールをサポートします。 1 つが定義されているロールに管理者がという名前し、このロールのユーザーのみが管理ページにアクセスできます。

> [!NOTE]
> I ve 書評 web アプリケーションで次の 3 つのユーザー アカウントを作成する: Scott、Jisun、および Alice です。 次の 3 つのすべてのユーザーに同じパスワードがあります:**パスワードです。** Scott および Jisun 管理者の役割では、Alice はありません。 サイト %s の非管理ページは匿名ユーザーにアクセスできます。 つまり、する必要はありません、管理するのでない限り、サイトにサインインする場合は、管理者の役割でのユーザーとしてにサインインする必要があります。


認証および匿名ユーザーの別のユーザー インターフェイスを含める書評アプリケーション s のマスター ページが更新されました。 匿名ユーザーとして、サイトにアクセスする場合は、右上隅でログイン リンクが表示されます。 認証されたユーザーに、メッセージが表示される"こそ*username*!" およびログアウトへのリンク。S ログイン ページもあります (`~/Login.aspx`)、訪問者を認証するためのロジックとユーザー インターフェイスを提供するログイン Web コントロールが含まれています。 管理者のみには、新しいアカウントを作成できます。 (ページの作成および管理でのユーザー アカウントは、`~/Admin`フォルダーです)。

### <a name="configuring-the-membership-and-roles-apis"></a>メンバーシップとロール Api の構成

書評 web アプリケーションでは、ユーザー アカウントをサポートするために (つまり、Admin ロール) の役割にそれらのユーザーをグループ化して、メンバーシップとロール Api を使用します。 `SqlMembershipProvider`と`SqlRoleProvider`プロバイダー クラスは、SQL Server データベースにアカウントとロールの情報を格納する必要があるために使用します。

> [!NOTE]
> このチュートリアルは、メンバーシップとロールの Api をサポートするために web アプリケーションを構成する方法の詳細についてでものではありません。 これらの Api とそれらを使用する web サイトを構成するために必要な手順を詳しく見るには、「マイ[ *web サイトのセキュリティ チュートリアル*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)です。


SQL Server データベースとアプリケーション サービスを使用するには、ユーザー アカウントとなるデータベースにこれらのプロバイダーによって使用されるデータベース オブジェクトおよび格納されているロール情報を追加する必要があります。 これらの必要なデータベース オブジェクトには、さまざまなテーブル、ビュー、およびストアド プロシージャが含まれます。 それ以外の場合、指定されていない限り、`SqlMembershipProvider`と`SqlRoleProvider`プロバイダー クラスは、という名前の SQL Server Express Edition データベースを使用して`ASPNETDB`アプリケーション s 内にある`App_Data`フォルダーですこのようなデータベースが存在しない場合、は自動的に作成。実行時にこれらのプロバイダーで必要なデータベース オブジェクトです。

アプリケーション サービスの web サイトのアプリケーション固有のデータが格納されている同じデータベース内のデータベース オブジェクトを通常最適で、作成して、可能なです。 という名前のツールが付属しています、.NET Framework`aspnet_regsql.exe`指定したデータベースでデータベース オブジェクトをインストールします。 続けてがあり、このツールを使用してこれらのオブジェクトを追加、`Reviews.mdf`データベースに格納されて、`App_Data`フォルダー (開発用データベース)。 実稼働データベースにこれらのオブジェクトを追加する際に、このチュートリアルで後ほどこのツールを使用する方法を会いしましょう。

アプリケーションがデータベースにデータベース オブジェクトを以外のサービスを追加する場合`ASPNETDB`をカスタマイズする必要があります、`SqlMembershipProvider`と`SqlRoleProvider`プロバイダーは、適切なデータベースを使用するように構成をクラスです。 メンバーシップ プロバイダーの追加をカスタマイズする、 [ *&lt;メンバーシップ&gt;要素*](https://msdn.microsoft.com/en-us/library/1b9hw62f.aspx)内で、 `<system.web>` 」の「 `Web.config`; を使用して、 [ *&lt;roleManager&gt;要素*](https://msdn.microsoft.com/en-us/library/ms164660.aspx)ロール プロバイダーを構成します。 次のスニペットは書評アプリケーション s から取得`Web.config`し、メンバーシップとロール Api の設定の構成を示しています。 注 - 新しいプロバイダーを登録両方`ReviewMembership`と`ReviewRole`-を使用する、`SqlMembershipProvider`と`SqlRoleProvider`プロバイダー、それぞれします。

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

`Web.config`ファイルの`<authentication>`フォーム ベース認証をサポートする要素が構成されてもします。
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>管理ページへのアクセスを制限します。

ASP.NET では、簡単に特定のファイルまたはフォルダーのユーザーまたはロールを介してアクセス許可または拒否にその*URL 承認*機能します。 (で URL の承認に簡単に説明した、*コア違いの間で IIS と ASP.NET 開発サーバー*チュートリアルでは IIS と ASP.NET 開発サーバーが静的に異なる URL 承認規則を適用する方法を示しましたと動的なコンテンツです。) との比較アクセスを禁止する必要があるため、`~/Admin`管理者の役割でそれらのユーザーを除くフォルダーで、このフォルダーに URL 承認規則を追加する必要があります。 具体的には、URL 承認規則は、管理者の役割でユーザーを許可およびその他のすべてのユーザーを拒否する必要があります。 追加することでこれを行う、`Web.config`ファイルの名前を`~/Admin`フォルダーで、次の内容。

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

ASP.NET の URL の承認機能とユーザー ロールの場合は、承認規則を綴るの使用方法の詳細については必ずを読み取る、 [*ユーザー ベースの承認*](../../older-versions-security/membership/user-based-authorization-vb.md)と[*ロールベース承認*](../../older-versions-security/roles/role-based-authorization-vb.md)からチュートリアル my [ *web サイトのセキュリティ チュートリアル*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)です。

## <a name="deploying-a-web-application-that-uses-application-services"></a>アプリケーション サービスを使用する Web アプリケーションを展開します。

アプリケーション サービスとアプリケーション サービス情報を格納するデータベースのプロバイダーを使用する web サイトを展開するときに、実稼働データベースで、アプリケーション サービスで必要なデータベース オブジェクトを作成することが不可欠です。 最初に、実稼働データベースにが含まれていないこれらのオブジェクト、そのため、アプリケーションが最初に展開されたときに (またはアプリケーション サービスを追加した後は、最初に配置されるとき)、これらの必要なデータベース オブジェクトを取得する追加の手順を行う必要があります、実稼働データベースです。

課題の 1 つは、運用環境に開発環境で作成されたユーザー アカウントをレプリケートする場合は、アプリケーション サービスを使用する web サイトを展開するときに発生します。 メンバーシップとロールの構成に応じて、可能であれば、実稼働データベースを開発環境で作成されたユーザー アカウントを正常にコピーする場合でも、これらのユーザーが実稼働環境で web アプリケーションにサインインできないことです。 この問題の原因を拝見し、防ぐ方法について説明します。、

ASP.NET 付属、nice [ *Web サイト管理ツール (WSAT)* ](https://msdn.microsoft.com/en-us/library/yy40ytx0.aspx)を Visual Studio から起動できるでき、ユーザー アカウント、ロール、および承認の規則を web ベースで管理します。インターフェイスです。 残念ながら、WSAT だけが、ローカルの web サイト、ユーザー アカウント、ロール、および実稼働環境で web アプリケーションの承認規則をリモート管理を使用できないことを意味します。 実稼働 web サイトから WSAT のような動作を実装する方法に紹介します。

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>データベース オブジェクトを使用して aspnet を追加する\_regsql.exe

*データベースの配置*チュートリアルでは、開発用データベースからテーブルとデータを実稼働データベースにコピーする方法を示しましたし、これらの手法は、アプリケーション サービス データベース オブジェクトのコピーを確実に使用できます、実稼働データベースです。 別のオプションは、`aspnet_regsql.exe`ツールを追加またはデータベースからアプリケーション サービスのデータベース オブジェクトを削除します。

> [!NOTE]
> `aspnet_regsql.exe`ツールは、指定したデータベースでデータベース オブジェクトを作成します。 これは移行されませんでは、データベース オブジェクトのデータ、開発用データベースから、実稼働データベースに。 開発用データベースのユーザー アカウントとロール情報を実稼働データベースにコピーすることを意味する場合で説明する方法を使用して、*データベースの配置*チュートリアルです。


使用して実稼働データベースにデータベース オブジェクトを追加する方法を見て s をできるように、`aspnet_regsql.exe`ツールです。 Windows エクスプ ローラーを開き、%WINDIR%\ コンピューターに .NET Framework version 2.0 のディレクトリに移動して開始します。Microsoft.NET\Framework\v2.0.50727 です。 存在する必要がありますが見つかったら、`aspnet_regsql.exe`ツールです。 コマンドラインからこのツールを使用することができますが、グラフィカル ユーザー インターフェイスも含まれています。ダブルクリックして、`aspnet_regsql.exe`グラフィカルなコンポーネントを起動するファイル。

その目的を説明するスプラッシュ画面を表示することによって、ツールを開始します。 「セットアップ オプションを選択して」画面で、図 1 に示す前払い金の横にあるをクリックします。 ここでは、データベース オブジェクトか、データベースから削除して、アプリケーション サービスを追加できます。 ため、実稼働データベースにこれらのオブジェクトを追加する、"アプリケーション サービスの SQL Server を構成する"オプションを選択し、[次へ] をクリックします。


[![アプリケーション サービスの SQL Server を構成します。](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**図 1**: アプリケーション サービスの SQL Server の構成を選択 ([フルサイズのイメージを表示するをクリックして](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))


「サーバーとデータベースの選択」で画面は、については、データベースに接続します。 データベース サーバー、セキュリティ資格情報と、web ホスティング会社によって提供されたデータベース名を入力し、[次へ] をクリックします。

> [!NOTE]
> データベース サーバーと資格情報を入力した後にデータベースのボックスの一覧を展開するときにエラーが表示することがあります。 `aspnet_regsql.exe`ツールのクエリ、`sysdatabases`システム テーブルに、サーバーがこの情報が公開しないように、データベース サーバーを企業ロックダウンをホストしている一部の web 上のデータベースの一覧を取得します。 このエラーが発生した場合は、ドロップダウン リストに直接、データベース名を入力できます。


[![データベースの接続情報とツールを提供します。](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**図 2**: ツールでデータベースの接続情報を指定 ([フルサイズのイメージを表示するをクリックして](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))


後続の画面は、アプリケーション サービスのデータベース オブジェクトは、指定されたデータベースに追加することを行う、つまりされるアクションにまとめます。 この操作の完了の横にある をクリックします。 しばらくすると、最後の画面が表示されます、(図 3 を参照してください)、データベース オブジェクトが追加されたことに注意してください。


[![正常にされました。アプリケーション サービスのデータベース オブジェクトが、実稼働データベースに追加されました。](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**図 3**: 成功! アプリケーション サービス データベース オブジェクトに追加された、実稼働データベース ([フルサイズのイメージを表示するをクリックして](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))


アプリケーション サービスのデータベース オブジェクトが、実稼働データベースに正常に追加されたことを確認するため、SQL Server Management Studio を開き、実稼働データベースに接続します。 図 4 に示すようになりましたアプリケーション サービス データベース テーブルに表示、データベース`aspnet_Applications`、 `aspnet_Membership`、`aspnet_Users`などのようにします。


[![データベース オブジェクトが、実稼働データベースに追加されたことを確認します。](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**図 4**: データベース オブジェクトが、実稼働データベースに追加されたことを確認 ([フルサイズのイメージを表示するをクリックして](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))


使用する必要がありますのみ、`aspnet_regsql.exe`ツールを初めて、または最初にアプリケーション サービスの使用を開始した後、web アプリケーションを展開するときにします。 後のこれらのデータベース オブジェクトが、実稼働データベースでは、再追加または変更する必要が勝利しました。

### <a name="copying-user-accounts-from-development-to-production"></a>開発環境から実稼働環境にユーザー アカウントのコピー

使用する場合、`SqlMembershipProvider`と`SqlRoleProvider`アプリケーション サービス情報、SQL Server データベースに格納するプロバイダー クラスを含むデータベース テーブルのさまざまな種類でユーザー アカウントとロールの情報が格納されている`aspnet_Users`、 `aspnet_Membership`、`aspnet_Roles`、および`aspnet_UsersInRoles`、その他。 開発時に、開発環境でユーザー アカウントを作成する場合は、該当するデータベース テーブルから対応するレコードをコピーして実稼働環境でそれらのユーザー アカウントをレプリケートできます。 データベース パブリッシュ ウィザードを使用するアプリケーション サービスのデータベース オブジェクトを展開する場合可能性がありますもことを選択したレコードをコピーします、開発も上にある実稼働環境で作成したユーザー アカウントになります。 ただし、構成の設定に応じてそれらのユーザー アカウントを持つため開発で作成した、実稼働環境にコピーが、実稼働 web サイトからのログインことができないことを見つける可能性があります。 なぜでしょう。

`SqlMembershipProvider`と`SqlRoleProvider`プロバイダー クラスは、1 つのデータベースとして使用ユーザーのストアの複数のアプリケーションに、理論上、各アプリケーションがでした場所がある重複しているユーザー名を持つユーザーと同じ名前を持つロールになるように設計されました。 このような柔軟性を許容するデータベースでのアプリケーションの一覧を保持、`aspnet_Applications`テーブル、および各ユーザーは、これらのアプリケーションに関連付けられています。 具体的には、`aspnet_Users`テーブルには、`ApplicationId`内のレコードには、各ユーザーに結合する列、`aspnet_Applications`テーブル。

加え、`ApplicationId`列で、`aspnet_Applications`テーブルも含まれています、`ApplicationName`列で、アプリケーションのよりわかりやすい名前を提供します。 Web サイトが区別する必要がありますログイン ページで、ユーザーの資格情報の検証などのユーザー アカウントを使用しようとしたときに、`SqlMembershipProvider`クラスを使用するには、どのようなアプリケーションです。 通常は、アプリケーション名を指定することによってこれと、この値は内のプロバイダーの構成から取得`Web.config`経由で具体的には、`applicationName`属性。

どうすれば、`applicationName`で属性が指定されていない`Web.config`しますか? このような場合は、メンバーシップ システム パスを使用して、アプリケーション ルートとして、`applicationName`値。 場合、`applicationName`属性が明示的に設定されていない`Web.config`、次に、開発環境と運用環境は別のアプリケーション ルートを使用し、そのため、別のアプリケーションに関連付けられる可能性があります。アプリケーション サービスの名前です。 開発環境で作成されたユーザーの場合は、このような不一致が発生した場合、`ApplicationId`と一致しない値、`ApplicationId`運用環境の値。 最終的な結果は、t が勝利したそれらのユーザーが、ログインできることです。

> [!NOTE]
> ならなくなった場合、この状況で、一致しないと実稼働環境にコピーするユーザー アカウントを使って`ApplicationId`値 - 正しくないこれらを更新するクエリを記述することも`ApplicationId`値を`ApplicationId`実稼働環境で使用します。 更新されると、開発環境で作成されたアカウントを持つユーザーは運用上の web アプリケーションにサインインできるはようになりました。


良いニュースは、同じ 2 つの環境を使用していることを確認するために行える、簡単な手順が`ApplicationId`- 明示的に設定されて、`applicationName`属性`Web.config`アプリケーション サービス プロバイダーのすべてのです。 明示的に設定すれば、`applicationName`で属性"BookReviews"を`<membership>`と`<roleManager>`要素からこのスニペットとして`Web.config`を示しています。

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

設定の詳細については、`applicationName`属性と、その背後にある論理的根拠を参照してください[ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) s ブログの投稿「 [*常 applicationName を設定します。ASP.NET メンバーシップとその他のプロバイダーを構成するときにプロパティ*](https://weblogs.asp.net/scottgu/443634)です。

### <a name="managing-user-accounts-in-the-production-environment"></a>実稼働環境でユーザー アカウントの管理

ASP.NET Web サイト管理ツール (WSAT) を容易に作成し、ユーザー アカウントの管理、定義しロールを適用および略さずにユーザーおよびロール ベースの承認規則にします。 Visual Studio から WSAT を起動するには、ソリューション エクスプ ローラーに移動し、ASP.NET の構成のアイコンをクリックして、または web サイトまたはプロジェクトのメニューに移動し、ASP.NET の構成のメニュー項目を選択します。 残念ながら、WSAT はローカルの web サイトでのみ機能できます。 したがって、実稼働環境で web サイトを管理するのに、ワークステーションから、WSAT を使用できません。

良いニュースは、公開される機能の WSAT によって提供されるすべてが、メンバーシップとロール Api にプログラムを使用できることさらに、WSAT 画面の多くは、標準の ASP.NET ログインに関連するコントロールを使用します。 つまり、必要な管理機能を提供する web サイトに、ASP.NET ページを追加できます。

以前のチュートリアルを含めるように書評 web アプリケーションを更新することに注意してください、`~/Admin`フォルダー、およびこのフォルダーは、ユーザーを管理者の役割でのみ許可に構成されています。 そのフォルダーに、ページを追加した`CreateAccount.aspx`からを管理者が新しいユーザー アカウントを作成します。 このページでは、CreateUserWizard コントロールを使用して、新しいユーザー アカウントを作成するためのユーザー インターフェイスとバックエンド ロジックを表示します。 含める新しいユーザーは、管理者ロールにも追加する必要があるかどうかを要求する チェック ボックス コントロールをカスタマイズする、どのような s (図 5 を参照してください)。 少しの作業では、ユーザーおよびロール管理に関連するタスクは、それ以外の場合、WSAT によって提供されることを実装するページのカスタム セットを構築できます。

> [!NOTE]
> 詳細については、ログインに関連する ASP.NET Web コントロールと共に、メンバーシップとロールの Api を使用して上を必ずお読みのマイ[ *web サイトのセキュリティのチュートリアル*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)です。 CreateUserWizard コントロールをカスタマイズする方法の詳細を参照してください、 [*ユーザー アカウントを作成する*](../../older-versions-security/membership/creating-user-accounts-vb.md)と[*追加のユーザー情報を格納する*](../../older-versions-security/membership/storing-additional-user-information-vb.md)チュートリアルについては、またはチェック アウト[ *Erich peterson と一緒にお届け*](http://www.erichpeterson.com/) s 記事[ *CreateUserWizard コントロールのカスタマイズ*](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![管理者は、新しいユーザー アカウントを作成できます。](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**図 5**: 管理者が新しいユーザー アカウントを作成 ([フルサイズのイメージを表示するをクリックして](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))


WSAT チェック アウトのすべての機能が必要なかどうかは[*ローリング、独自の Web サイト管理ツール*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)著者の Dan Clem 順を追ってカスタム WSAT のようなツールを構築するプロセスです。 Dan は彼アプリケーションのソース コード (C# の場合) を共有し、ホストされる web サイトに追加するための手順を提供します。

## <a name="summary"></a>概要

アプリケーション サービス データベースの実装を使用する web アプリケーションを展開するときにまず、実稼働データベースが必要なデータベース オブジェクトを持つことを確認する必要があります。 説明した手法を使用してこれらのオブジェクトを追加することができます、*データベースの配置*チュートリアルです。 また、使用することができます、`aspnet_regsql.exe`ツール、このチュートリアルで示したようにします。 (これは重要なは、実稼働環境では有効にするには、ユーザーと、開発環境で作成されたロールの場合)、開発および運用環境との手法で使用されるアプリケーション名の同期を中心に説明した他の課題ユーザーと、実稼働環境でロールを管理します。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [*ASP.NET SQL Server の登録ツール (aspnet_regsql.exe)*](https://msdn.microsoft.com/en-us/library/ms229862.aspx)
- [*SQL Server 用のアプリケーション サービス データベースを作成します。*](https://msdn.microsoft.com/en-us/library/x28wfk74.aspx)
- [*SQL Server でのメンバーシップのスキーマの作成*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*ASP.NET のメンバーシップ、ロール、およびプロファイルを確認します。*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*独自の Web サイト管理ツールのローリング*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Web サイトのセキュリティのチュートリアル*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Web サイト管理ツールの概要*](https://msdn.microsoft.com/en-us/library/yy40ytx0.aspx)

>[!div class="step-by-step"]
[前へ](configuring-the-production-web-application-to-use-the-production-database-vb.md)
[次へ](strategies-for-database-development-and-deployment-vb.md)
