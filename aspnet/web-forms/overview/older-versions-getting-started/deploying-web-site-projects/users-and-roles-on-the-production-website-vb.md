---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
title: "ユーザーとロール、実稼働 web サイト (VB) |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET web サイト管理ツール (WSAT) は、メンバーシップとロールの設定を構成して、作成するために web ベースのユーザー インターフェイスを提供、編集、."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 491ed5ae-9be1-4191-87be-65e4e1c57690
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
msc.type: authoredcontent
ms.openlocfilehash: f3bfaa0e14e3e04a7faae1a78b566d7c2067785a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="users-and-roles-on-the-production-website-vb"></a>ユーザーと、実稼働 web サイト (VB) の役割
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF をダウンロードします。](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_vb.pdf)

> ASP.NET web サイト管理ツール (WSAT) は、作成、編集、およびユーザーとロールを削除すると、メンバーシップおよびロールの設定を構成するために web ベースのユーザー インターフェイスを提供します。 残念ながら、WSAT のみ機能、ローカル ホストからアクセスすると、ブラウザーから、実稼働 web サイトの管理ツールに到達できないことを意味します。 良いニュースは、ユーザーと実稼働環境での役割の管理を可能にするための回避策があることです。 このチュートリアルは、これらの回避策やその他のユーザーを見ます。


## <a name="introduction"></a>はじめに

ASP.NET 2.0 の数の導入*アプリケーション サービス*、web アプリケーションに追加できるビルディング ブロック サービスのスイートであります。 書評の web サイトにメンバーシップとロール サービスがバックアップに追加したは、 [*サービスの構成、web サイトを使用してアプリケーション*チュートリアル](configuring-a-website-that-uses-application-services-vb.md)です。 メンバーシップ サービスには、作成および管理するユーザー アカウントが容易になります。ロール サービスのグループにユーザーを分類するための API が提供します。 書評サイトには、次の 3 つのユーザー アカウント - Scott、Jisun、and Alice の 1 つのロール、Scott と管理者の役割で Jisun の管理があります。

ASP です。NET のアプリケーション サービスは、特定の実装に関連付けられていません。 アプリケーションのサービスを使用して、特定するように指示する代わりに、*プロバイダー*、し、そのプロバイダーが特定のテクノロジを使用して、サービスを実装します。 使用する書評 web アプリケーションを構成しました、`SqlMembershipProvider`と`SqlRoleProvider`のメンバーシップとロール サービスのプロバイダー。 これら 2 つのプロバイダーは、SQL Server データベースにユーザー アカウントとロール情報を格納、web ポータルのホストでホストされているインターネット ベースの web アプリケーションに最もよく使用されるプロバイダーです。

メンバーシップとロール サービスを使用する開発者向けの一般的な課題がユーザーと実稼働環境でロールを管理します。 実稼働 web サイトからユーザー アカウントを削除する、新しいロールを追加または既存のロールに、既存のユーザーを追加する方法 このチュートリアルでは、ユーザーと、実稼働 web サイト ロールを管理するためのさまざまな手法について説明します。

## <a name="using-the-aspnet-web-site-administration-tool"></a>ASP.NET Web サイト管理ツールを使用します。

ASP.NET には、 [Web サイト管理ツール](https://msdn.microsoft.com/library/yy40ytx0.aspx)(WSAT) を簡単に作成および管理ユーザー アカウントとロールとユーザーおよびロール ベースの承認規則を指定します。 使用して、WSAT、ソリューション エクスプ ローラーで [ASP.NET 構成] アイコンをクリックしてまたは web サイトまたはプロジェクト] メニューの [ASP.NET の構成オプションを選択します。 どちらの方法では、web ブラウザーを起動しのようなアドレスで WSAT を指します。`http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

WSAT は、3 つのセクションに分かれています。

- **セキュリティ**-ユーザー、ロール、および承認規則を管理します。
- **「アプリケーション構成」** -管理、 &lt;appSettings&gt;およびここから SMTP 設定します。 ことができますも、アプリケーションをオフラインのデバッグとトレースここでは、設定の管理し、既定のカスタム エラー ページを指定します。
- **ProviderConfiguration** -アプリケーション サービスで使用されるプロバイダーを構成します。

[セキュリティ] セクション (に示すように**図 1**) 新しいユーザーを作成する、ユーザーを管理するを作成して、ロールの管理の作成および管理のアクセス規則へのリンクが含まれています。 ここからするシステムに新しいロールを追加、既存のユーザーを削除または追加したり、特定のユーザー アカウントからロールを削除できます。

[![](users-and-roles-on-the-production-website-vb/_static/image2.png)](users-and-roles-on-the-production-website-vb/_static/image1.png)

**図 1**: WSAT セキュリティ セクションには、ユーザーおよびロールを管理するためのオプションが含まれています  
([フルサイズのイメージを表示するをクリックして](users-and-roles-on-the-production-website-vb/_static/image3.png))

残念ながら、WSAT のみがアクセスできるローカルです。 リモートの運用 web サイト以外で、WSAT にアクセスすることはできません。アクセスした場合`www.yoursite.com/asp.netwebadminfiles/default.aspx`404 Not Found 応答を取得します。 電源が、WSAT 使用するコード、`Membership`と`Roles`を作成するには、.NET Framework のクラスが編集、およびユーザーとロールを削除します。 これらのクラスは、; を使用するには、どのようなプロバイダーを決定する web アプリケーションの構成情報を参照してください。戻り、 [*サービスの構成、web サイトを使用してアプリケーション*チュートリアル](configuring-a-website-that-uses-application-services-vb.md)セットアップ書評の web サイトを使用、`SqlMembershipProvider`と`SqlRoleProvider`プロバイダー。 これに含まれる追加`<membership>`と`<roleManager>`をセクション`Web.config`です。

[!code-xml[Main](users-and-roles-on-the-production-website-vb/samples/sample1.xml)]

注意してください、`<membership>`と`<roleManager>`セクションでは、参照、`SqlMembershipProvider`と`SqlRoleProvider`内のプロバイダー、`type`属性をそれぞれします。 これらのプロバイダーは、指定した SQL Server データベースのユーザーとロール情報を格納します。 これらのプロバイダーによって使用されるデータベースがで指定された、`connectionStringName`属性、`ReviewsConnectionString`で定義されている、`~/ConfigSections/databaseConnectionStrings.config`ファイル。 注意してください、`databaseConnectionStrings.config`開発環境でのファイルが格納され、開発用データベースへの接続文字列、`databaseConnectionStrings.config`運用上のファイルには、実稼働データベースへの接続文字列が含まれています。

簡単に言うと、開発環境から、WSAT をローカルにアクセスする必要がありで指定されたデータベース内のユーザーおよびロール情報と連携して、`databaseConnectionStrings.config`ファイル。 その結果、内の接続文字列情報を変更する場合、`databaseConnectionStrings.config`ファイル開発環境では、WSAT ローカルで使用できます、実稼働環境のユーザーとロールを管理します。

この機能を示すためには、開く、`databaseConnectionStrings.config`開発環境で Visual Studio でファイルを開き、実稼働データベースの接続文字列で、開発データベース接続文字列を置き換えます。 WSAT を起動し、[セキュリティ] タブを移動およびでパスワードが"password"! Sam をという名前の新しいユーザーを追加 (以下、引用符)。 **図 2**このアカウントを作成するときに、WSAT 画面を示しています。

[![](users-and-roles-on-the-production-website-vb/_static/image5.png)](users-and-roles-on-the-production-website-vb/_static/image4.png)

**図 2**: 実稼働環境での Sam をという名前の新しいユーザーを作成します。  
([フルサイズのイメージを表示するをクリックして](users-and-roles-on-the-production-website-vb/_static/image6.png))

内の接続文字列を変更しましたので`databaseConnectionStrings.config`を実稼働データベース サーバーを指す Sam は実稼働環境でのユーザーとして追加されました。 これを確認するには、内の接続文字列を変更、`databaseConnectionStrings.config`ファイルの名前、開発用データベースをバックアップし、アクセス、`Login.aspx`開発環境でのページです。 Sam としてサインインしようとしています (を参照してください**図 3**)。

[![](users-and-roles-on-the-production-website-vb/_static/image8.png)](users-and-roles-on-the-production-website-vb/_static/image7.png)

**図 3**: 開発環境での Sam としてサインインすることはできません  
([フルサイズのイメージを表示するをクリックして](users-and-roles-on-the-production-website-vb/_static/image9.png))

サインインできない Sam として開発環境でユーザー アカウント情報がローカル データベースに存在しないためです。 代わりに、実稼働データベースに追加されました。 これを確認するには、内容を表示、`aspnet_Users`開発と実稼働環境の両方のデータベース内のテーブルです。 開発環境では、ユーザー Scott、Jisun、および Alice の 3 つだけのレコードが必要があります。 ただし、`aspnet_Users`実稼働データベース内のテーブルが 4 つのレコード: Scott、Jisun、Alice、および Sam です。 その結果、Sam は、開発環境ではなくが、実稼働環境での web サイトを経由署名できます。

[![](users-and-roles-on-the-production-website-vb/_static/image11.png)](users-and-roles-on-the-production-website-vb/_static/image10.png)

**図 4**: Sam は、実稼働 web サイトにサインインできます  
([フルサイズのイメージを表示するをクリックして](users-and-roles-on-the-production-website-vb/_static/image12.png))

> [!NOTE]
> 接続文字列を変更する注意してください、`databaseConnectionStrings.config`ファイルを再度、開発用データベース 's 接続文字列操作を終了すると、開発を使用してサイトをテストするときに実稼働データを操作する WSAT それ以外の場合の操作環境。 また、ここで説明した手法により、WSAT を使用してユーザーおよびロールをリモート管理をときに、その他の WSAT 構成オプション (アクセス規則、SMTP 設定、デバッグとトレースの設定、およびなど) のいずれかの変更が、を変更ことに注意してください。`Web.config`ファイル。 その結果、開発環境と運用環境にない設定に加えられた変更が適用されます。


## <a name="creating-custom-user-and-role-management-web-pages"></a>カスタム ユーザーおよびロール管理の Web ページの作成

WSAT 外ボックス システムのユーザーとロールを管理するためが、ローカルでのみ起動できますは、ユーザーと実稼働環境でロールを管理するために、接続文字列の情報に変更を加える必要があります。 ユーザー アカウントをサポートするほとんどの web サイトには、ユーザーおよび管理者は、サイト内のページからのユーザーとロールの管理を有効にするロールの管理 web ページの数にはもが含まれます。 このような web ベースの管理ページのユーザーとロールの管理をはるかに簡単にしてサイトに不可欠な部分が多くの管理者をまたは管理者へのアクセスまたは技術的な背景を Visual Studio を使用して、WSAT を起動する必要はありません。

ASP.NET には、組み込みのログインに関連する Web コントロールにドラッグ アンド ドロップ簡単これらの管理 web ページの多くを実装するための多くが含まれています。 たとえば、管理者がページ上に CreateUserWizard コントロールをドラッグして、いくつかのプロパティを設定し、新しいユーザー アカウントを作成するためのページを作成できます。 実際、ページに示すように WSAT でユーザーを作成するために**図 2**ページに追加できる同じ CreateUserWizard コントロールを使用します。 さらに、メンバーシップとロール サービスの機能はプログラムを利用可能な`Membership`と`Roles`.NET Framework クラスです。 これらのクラスとは、作成、編集、およびユーザーと役割を削除も追加またはロールにユーザーを削除するかを決定するユーザーにどの役割およびその他のユーザーおよびロールに関連するタスクを実行するコードを記述できます。

[*サービスの構成、web サイトを使用してアプリケーション*チュートリアル](configuring-a-website-that-uses-application-services-vb.md)はページを追加、`Admin`という名前のフォルダー`CreateAccount.aspx`です。 このページで、サイトに新しいユーザー アカウントを追加して、新しく作成したユーザーが管理者の役割でがかどうかを指定する管理者は、(を参照してください**図 5**)。

[![](users-and-roles-on-the-production-website-vb/_static/image14.png)](users-and-roles-on-the-production-website-vb/_static/image13.png)

**図 5**: 管理者は、新しいユーザー アカウントを作成できます  
([フルサイズのイメージを表示するをクリックして](users-and-roles-on-the-production-website-vb/_static/image15.png))

詳細な手順を使用すると、ユーザーおよびロールの管理ページのビルドについて、`Membership`と`Roles`クラスと、ログインに関連する ASP.NET Web コントロールを必ずお読み my [web サイトのセキュリティチュートリアル](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)です。 あるガイダンスが見つかります新しいアカウントを作成し、作成、およびの役割の管理用の web ページを構築する方法のユーザー ロール、およびその他の一般的な管理タスクを割り当てます。

実稼働 web サイトに WSAT のような機能を実装するには、常に、独自の一連の WSAT の機能を実装する web ページを構築できます。 作業を開始、フォルダーにある WSAT ソース コードをチェック アウト`%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`です。 別のオプションは、彼は共有の記事、Dan Clem WSAT 代替手段を使用する[ローリング、独自の Web サイト管理ツール](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)です。 Dan カスタム WSAT のようなツールを構築するプロセスを通じてリーダー、(で C# の場合)、ダウンロード、アプリケーションのソース コードが含まれていて、ホストされる web サイトにそのカスタム WSAT を追加する手順については、します。

## <a name="summary"></a>まとめ

ASP.NET Web サイト管理ツール (WSAT) は、web サイトのユーザーおよびロールの情報を管理するメンバーシップとロールのアプリケーション サービスと共に使用できます。 残念ながら、WSAT のみがローカルでアクセスできると、実稼働 web サイトからアクセスされることはできません。 ただし、開発に接続文字列を変更することにより、実稼働データベースをポイントするように環境を使用できます、WSAT ユーザーと、実稼働 web サイト ロールの管理します。

WSAT アプローチでは、迅速かつ簡単な方法をユーザーおよびロールを管理することによって、中に、接続文字列情報を一時的な変更だけでなく、Visual Studio から WSAT を起動することが必要です。 WSAT は、ユーザーと、運用上の役割を管理する簡単な方法を提供していますが煩雑あり、複数の管理者または管理者がないか、Visual Studio と、WSAT に慣れていないユーザーと web サイトに対して適切は動作しません。 これらの理由から、ユーザー アカウントをサポートするほとんどの web サイトには、管理用の web ページのセットが含まれます。 このような一連の web ページ、WSAT が不要し、任意のコンピューターからのさまざまな管理ユーザーによって使用されます。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP を検査中です。NET のメンバーシップ、ロール、およびプロファイル](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [独自の Web サイト管理ツールのローリング](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Web サイト管理ツールの概要](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Web サイトのセキュリティのチュートリアル](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

>[!div class="step-by-step"]
[前へ](precompiling-your-website-vb.md)
