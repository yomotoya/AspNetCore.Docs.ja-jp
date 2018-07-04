---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
title: ユーザーとロール、運用 web サイト (VB) |Microsoft Docs
author: rick-anderson
description: ASP.NET web サイト管理ツール (WSAT) は、メンバーシップとロールの設定を構成して、作成するために web ベースのユーザー インターフェイスを提供、編集、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 491ed5ae-9be1-4191-87be-65e4e1c57690
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
msc.type: authoredcontent
ms.openlocfilehash: 5b2a5ea84c5c16ad5bf4e3041d31ad29660d965f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380377"
---
<a name="users-and-roles-on-the-production-website-vb"></a>ユーザーとロール、運用 web サイト (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF のダウンロード](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_vb.pdf)

> ASP.NET web サイト管理ツール (WSAT) は、メンバーシップとロールの設定を構成して作成、編集、およびユーザーとロールを削除するための web ベースのユーザー インターフェイスを提供します。 残念ながら、WSAT のみ機能、localhost からアクセスすると、ブラウザーから、実稼働 web サイトの管理ツールにアクセスできないことを意味します。 良い知らせは、ユーザーと運用上の役割を管理できるようにするための回避策があることです。 このチュートリアルでは、これらの回避策や他のユーザーで検索します。


## <a name="introduction"></a>はじめに

ASP.NET 2.0 の多くを導入する*アプリケーション サービス*、これは、一連の構成要素サービス、web アプリケーションに追加することができます。 書籍レビューの web サイトにメンバーシップとロール サービスのバックアップを追加しました、 [*サービスを構成する、web サイトを使用してアプリケーション*チュートリアル](configuring-a-website-that-uses-application-services-vb.md)します。 メンバーシップ サービス作成および管理するユーザー アカウントを使用します。ロール サービスは、ユーザーをグループに分類するための API を提供します。 ブック_レビュー サイトには、3 つのユーザー アカウント、および Scott、Jisun、および Alice - 管理者は、Scott と Jisun 管理者の役割で、1 つの役割があります。

ASP します。NET のアプリケーション サービスは特定の実装に関連付けられていません。 使用して、特定のアプリケーション サービスに指示する代わりに、*プロバイダー*、し、そのプロバイダーが特定のテクノロジを使用してサービスを実装します。 使用する、書籍レビューの web アプリケーションを構成した、`SqlMembershipProvider`と`SqlRoleProvider`メンバーシップとロール サービスのプロバイダー。 これら 2 つのプロバイダーは、SQL Server データベースにユーザー アカウントとロールの情報を格納、web ホスト会社でホストされているインターネット ベースの web アプリケーションの最もよく使用されるプロバイダーです。

メンバーシップとロール サービスを使用する開発者向けの一般的な課題は、ユーザーおよびロール、運用環境の管理です。 実稼働 web サイトからユーザー アカウントを削除、新しいロールを追加または既存のロールに、既存のユーザーを追加する方法 このチュートリアルでは、ユーザーと実稼働 web サイト ロールを管理するためのさまざまな手法について説明します。

## <a name="using-the-aspnet-web-site-administration-tool"></a>ASP.NET Web サイトの管理ツールを使用します。

ASP.NET には、 [Web サイト管理ツール](https://msdn.microsoft.com/library/yy40ytx0.aspx)(WSAT) を簡単に作成してユーザー アカウントとロールを管理し、ユーザーとロール ベースの承認規則を指定します。 WSAT を使用するには、ソリューション エクスプ ローラーで ASP.NET の構成アイコンをクリックしてまたは web サイトまたはプロジェクト メニューに移動し、ASP.NET 構成オプションを選択します。 どちらの方法では、web ブラウザーを起動し、ようなアドレスで WSAT を指します。 `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

WSAT は、3 つのセクションに分かれています。

- **セキュリティ**-ユーザー、ロール、および承認規則を管理します。
- **「アプリケーション構成」** -管理、 &lt;appSettings&gt;とここでは、SMTP 設定します。 ことができますも、アプリケーションをオフラインにしてデバッグ出力およびトレースここでは、設定の管理だけでなく既定のカスタム エラー ページを指定します。
- **ProviderConfiguration** -アプリケーションのサービスによって使用されるプロバイダを構成します。

[セキュリティ] セクション (に示すように**図 1**) へのリンクには新しいユーザーを作成する、ユーザーの管理、作成やの役割の管理し作成と管理のアクセス規則が含まれています。 ここからするシステムに新しいロールを追加、既存のユーザーを削除または追加したり、特定のユーザー アカウントからロールを削除できます。

[![](users-and-roles-on-the-production-website-vb/_static/image2.png)](users-and-roles-on-the-production-website-vb/_static/image1.png)

**図 1**: WSAT セキュリティ セクションには、ユーザーおよびロールを管理するためのオプションが含まれています  
([フルサイズの画像を表示する をクリックします](users-and-roles-on-the-production-website-vb/_static/image3.png))。

残念ながら、WSAT はのみアクセスできるローカルです。 リモート運用 web サイトで、WSAT をアクセスすることはできませんアクセスした場合`www.yoursite.com/asp.netwebadminfiles/default.aspx`404 Not Found 応答を取得します。 WSAT を実行するコードを`Membership`と`Roles`を作成する .NET Framework のクラスは、編集、およびユーザーとロールを削除します。 これらのクラスは、使用するには、どのようなプロバイダーを決定する web アプリケーションの構成情報を参照してください。戻り、 [*サービスを構成する、web サイトを使用してアプリケーション*チュートリアル](configuring-a-website-that-uses-application-services-vb.md)、書籍レビューの web サイトを使用する設定、`SqlMembershipProvider`と`SqlRoleProvider`プロバイダー。 これに含まれる追加`<membership>`と`<roleManager>`にセクション`Web.config`します。

[!code-xml[Main](users-and-roles-on-the-production-website-vb/samples/sample1.xml)]

なお、`<membership>`と`<roleManager>`セクションを参照、`SqlMembershipProvider`と`SqlRoleProvider`でプロバイダー、`type`属性は、それぞれします。 これらのプロバイダーは、指定した SQL Server データベースのユーザーとロール情報を格納します。 これらのプロバイダーによって使用されるデータベースがで指定された、`connectionStringName`属性、 `ReviewsConnectionString`、定義されている、`~/ConfigSections/databaseConnectionStrings.config`ファイル。 いることを思い出してください、`databaseConnectionStrings.config`一方、開発環境でのファイルが開発用データベースへの接続文字列を含む、`databaseConnectionStrings.config`運用上のファイルには、実稼働データベースへの接続文字列が含まれています。

簡単に言うと、開発環境を介して、WSAT をローカルにアクセスする必要がありで指定されたデータベースでユーザーおよびロールについては、連携、`databaseConnectionStrings.config`ファイル。 その結果、内の接続文字列情報を変更する場合、`databaseConnectionStrings.config`ファイル開発環境で使用できます、WSAT ローカル ユーザーと運用環境でロールを管理します。

この機能を示すためには、開く、`databaseConnectionStrings.config`開発環境で Visual Studio でファイルし、開発データベースの接続文字列を実稼働データベースの接続文字列に置き換えてください。 WSAT を起動し、[セキュリティ] タブを移動し、でパスワードが"password!"Sam をという名前の新しいユーザーを追加 (以下、引用符)。 **図 2**このアカウントを作成するときに、WSAT 画面を示しています。

[![](users-and-roles-on-the-production-website-vb/_static/image5.png)](users-and-roles-on-the-production-website-vb/_static/image4.png)

**図 2**: 運用環境での Sam をという名前の新しいユーザーの作成  
([フルサイズの画像を表示する をクリックします](users-and-roles-on-the-production-website-vb/_static/image6.png))。

内の接続文字列を変更しましたので`databaseConnectionStrings.config`を実稼働データベース サーバーを指す、Sam が運用環境でのユーザーとして追加されました。 これを確認するには、接続文字列を変更、`databaseConnectionStrings.config`ファイル開発用データベースをバックアップし、アクセス、`Login.aspx`開発環境でのページ。 Sam でサインインしようとしています (を参照してください**図 3**)。

[![](users-and-roles-on-the-production-website-vb/_static/image8.png)](users-and-roles-on-the-production-website-vb/_static/image7.png)

**図 3**: 開発環境での Sam としてサインインすることはできません  
([フルサイズの画像を表示する をクリックします](users-and-roles-on-the-production-website-vb/_static/image9.png))。

できませんサインインする Sam として開発環境でのユーザー アカウント情報がローカル データベースに存在しないためです。 代わりは、実稼働データベースに追加されました。 これを確認するには、内容を表示、`aspnet_Users`開発と運用環境の両方のデータベース内のテーブル。 開発環境では、Scott、Jisun、Alice のユーザーのみに 3 つのレコードが必要があります。 ただし、`aspnet_Users`実稼働データベース内のテーブルが 4 つのレコード: Scott、Jisun、Alice、および Sam です。 その結果、Sam は、運用環境で web サイトを経由する、開発環境ではなくにサインインできます。

[![](users-and-roles-on-the-production-website-vb/_static/image11.png)](users-and-roles-on-the-production-website-vb/_static/image10.png)

**図 4**: Sam が実稼働 web サイトにサインインできます  
([フルサイズの画像を表示する をクリックします](users-and-roles-on-the-production-website-vb/_static/image12.png))。

> [!NOTE]
> 接続文字列を変更することを忘れないでください、`databaseConnectionStrings.config`完了すると、開発用データベースにファイルから文字列 's 接続、開発を使用してサイトをテストするときに運用データで作業する WSAT それ以外の場合の操作環境。 ここで説明した技法により、WSAT を使用して、リモート ユーザーおよびロールを管理する、中に、その他の WSAT 構成オプション (アクセス規則、SMTP 設定、トレースおよびデバッグ設定、およびなど) のいずれかの変更が、を変更ことに留意してください。`Web.config`ファイル。 その結果、開発環境と運用環境にない設定に加えられた変更が適用されます。


## <a name="creating-custom-user-and-role-management-web-pages"></a>カスタム ユーザーおよびロール管理 Web ページの作成

WSAT は、ユーザーとロールを管理するためのボックス システムの不足を提供しますが、ローカルでのみ起動でき、ユーザーと運用環境でロールを管理するために、接続文字列情報に変更を加える必要があります。 ユーザー アカウントをサポートするほとんどの web サイトには、さまざまなユーザーと管理者は、サイト内のページからユーザーとロールの管理を有効にするロールの管理 web ページも含まれます。 このような web ベースの管理ページがそれがより簡単にユーザーとロールの管理し、サイトに不可欠ですが、多くの管理者または管理者へのアクセスや、WSAT を起動する Visual Studio を使用する技術的な背景を持たない可能性があります。

ASP.NET には、組み込みのログインに関連する Web コントロールにドラッグ アンド ドロップするだけこれらの管理 web ページの多くを実装するための数値が含まれています。 たとえば、ページ上に CreateUserWizard コントロールをドラッグし、いくつかのプロパティを設定して新しいユーザー アカウントを作成する管理者用のページを作成できます。 実際、示した WSAT でユーザーを作成するためのページ**図 2**をページに追加できる同じ CreateUserWizard コントロールを使用します。 さらに、メンバーシップとロール サービスの機能を利用プログラムで、`Membership`と`Roles`.NET Framework のクラス。 これらのクラスには、作成、編集、およびユーザーとロールをもユーザーのロールに追加または削除したり、ロールではユーザーを特定し、他のユーザーと役割に関連するタスクを実行するを削除するコードを記述できます。

[*サービスを構成する、web サイトを使用してアプリケーション*チュートリアル](configuring-a-website-that-uses-application-services-vb.md)ページを追加、`Admin`という名前のフォルダー`CreateAccount.aspx`します。 このページで、サイトに新しいユーザー アカウントを追加して、新しく作成したユーザーが管理者ロールがかどうかを指定する管理者は、(を参照してください**図 5**)。

[![](users-and-roles-on-the-production-website-vb/_static/image14.png)](users-and-roles-on-the-production-website-vb/_static/image13.png)

**図 5**: 管理者は、新しいユーザー アカウントを作成できます  
([フルサイズの画像を表示する をクリックします](users-and-roles-on-the-production-website-vb/_static/image15.png))。

使用に関する詳細な手順と共に、ユーザーおよびロールの管理ページの構築について詳しく説明、`Membership`と`Roles`クラスと、ログインに関連する ASP.NET Web コントロールを必ずお読み、 [web サイトのセキュリティチュートリアル](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)します。 ユーザー ロール、およびその他の一般的な管理タスクを割り当てる新しいアカウントを作成し、作成、およびの役割の管理用の web ページを構築する方法のガイダンスを検索します。

運用 web サイトに WSAT のような機能を実装するには、独自の一連の WSAT の機能を実装する web ページをいつでも構築できます。 最初に、フォルダーにある WSAT ソース コードをチェック アウトする`%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`します。 別のオプションは、彼は、彼の記事で共有、Dan Clem の WSAT 代替手段を使用する[ローリング、独自の Web サイト管理ツール](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)します。 Dan はまで、カスタム WSAT のようなツールを構築するプロセスについて説明します (c# で)、ダウンロード、アプリケーションのソース コードが含まれています、およびホストされる web サイトを自分のカスタム WSAT を追加するための手順について説明します。

## <a name="summary"></a>まとめ

ASP.NET Web サイト管理ツール (WSAT) は、web サイトのユーザーおよびロールの情報を管理するメンバーシップとロールのアプリケーション サービスと連携して使用できます。 残念ながら、WSAT のみがローカルでアクセスできると、実稼働 web サイトからアクセスされることはできません。 ただし、開発の接続文字列を変更することで、実稼働データベースをポイントするための環境を使用できます、WSAT ユーザーと、実稼働 web サイト ロールの管理します。

WSAT アプローチは、ユーザーとロールを管理する迅速かつ簡単な方法により、中に、接続文字列情報を一時的な変更と同様に Visual Studio から WSAT を起動する必要があります。 WSAT はユーザーと、運用環境でロールを管理する簡単な方法を提供しています、面倒ですが、複数の管理者または管理者がないか、Visual Studio と、WSAT に慣れていないユーザーともの web サイトは機能しません。 これらの理由から、ユーザー アカウントをサポートするほとんどの web サイトには、管理用の web ページのセットが含まれます。 このような一連の web ページは、WSAT が不要し、任意のコンピューターからのさまざまな管理ユーザーによって使用されます。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP を調べています。NET のメンバーシップ、ロール、およびプロファイル](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [独自の Web サイト管理ツールのローリング](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Web サイト管理ツールの概要](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Web サイトのセキュリティのチュートリアル](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [前へ](precompiling-your-website-vb.md)
