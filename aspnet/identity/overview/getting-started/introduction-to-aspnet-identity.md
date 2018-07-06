---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: ASP.NET Identity の概要 |Microsoft Docs
author: jongalloway
description: ASP.NET メンバーシップ システムで導入された ASP.NET 2.0 のバックしているので 2005年では、方法は web アプリケーションの場合に多くの変更されているし、.
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 3cefefc85857c3e3e295789dfa9d9f4789de4602
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811582"
---
<a name="introduction-to-aspnet-identity"></a>ASP.NET Identity の概要
====================
によって[Jon Galloway](https://github.com/jongalloway)、 [Pranav Rastogi](https://github.com/rustd)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

> ASP.NET メンバーシップ システムで導入された ASP.NET 2.0 のバックしているので 2005年では、web アプリケーションは、通常は認証と承認に処理の方法で多くの変更されているし。 ASP.NET Identity では、メンバーシップ システムべきは、web、電話、またはタブレット向けのモダン アプリケーションを作成するときに改めて注目です。
> 
> この記事の執筆者は、Pranav Rastogi ([@rustd](https://twitter.com/rustd))、Jon Galloway ([@jongalloway](https://twitter.com/jongalloway))、Tom Dykstra と Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。


## <a name="background-membership-in-aspnet"></a>ASP.NET メンバーシップの背景知識:

### <a name="aspnet-membership"></a>ASP.NET メンバーシップ

[ASP.NET メンバーシップ](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx)が 2005 で、フォーム認証、およびユーザー名、パスワード、およびプロファイル データの SQL Server データベースに関連する一般的なサイト メンバーシップの要件を解決するために設計されました。 現在、web アプリケーションのデータ ストレージ オプションのはるかに広範な配列があるし、ほとんどの開発者が認証と承認の機能のソーシャル id プロバイダーを使用するには、そのサイトを有効にします。 ASP.NET メンバーシップの設計の制限事項は、困難な移行を行います。

- データベース スキーマは、SQL Server 用に設計された、変更することはできません。 プロファイル情報を追加することができますが、追加のデータが困難にアクセスする以外で、プロファイル プロバイダーの API を介して、別のテーブルにまとめられます。
- プロバイダーのシステムでは、バックアップ データ ストアを変更することができますが、システムが、リレーショナル データベースの適切な前提条件の周囲に設計されています。 Azure Storage のテーブルなどの非リレーショナル ストレージ メカニズムにメンバーシップ情報を格納するプロバイダーを作成することができますが、多くのコードとの多くを記述することでリレーショナル設計を回避する必要がある、`System.NotImplementedException`そうでないメソッドの例外NoSQL データベースに適用されます。
- ログ/ログ アウト機能は、フォーム認証に基づいているため、メンバーシップ システムは使用できません[OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)します。 OWIN にはミドルウェア コンポーネント (Microsoft アカウント、Facebook、Google、Twitter) などの外部 id プロバイダーを使用してログインのサポートを含む、認証が含まれていますから組織のアカウントを使用してログをオンプレミス Active Directory またはAzure Active Directory。 OWIN には、OAuth 2.0 JWT、CORS のサポートも含まれています。

### <a name="aspnet-simple-membership"></a>単純な ASP.NET メンバーシップ

[ASP.NET の簡易なメンバーシップ](../../../web-pages/overview/security/16-adding-security-and-membership.md)ASP.NET Web Pages のメンバーシップ システムとして開発されました。 WebMatrix と Visual Studio 2010 SP1 がリリースされました。 簡易なメンバーシップの目標は、Web Pages アプリケーションにメンバーシップ機能を追加するが簡単でした。

簡易なメンバーシップが容易にできるように、ユーザー プロファイル情報をカスタマイズして、ASP.NET のメンバーシップを持つその他の問題をまだ共有しますが、いくつかの制限があります。

- 非リレーショナル ストアでのメンバーシップ システムのデータを保持する難しかったです。
- これは、OWIN を使用できません。
- 既存の ASP.NET メンバーシップ プロバイダーともうまくいかないし、は拡張できません。

### <a name="aspnet-universal-providers"></a>ASP.NET ユニバーサル プロバイダー

[ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) 、Azure SQL Database と SQL Server Compact の操作も Microsoft でのメンバーシップ情報を保持できるようにして開発されました。 ユニバーサル プロバイダーは、Entity Framework Code First、EF でサポートされている任意のストアにデータを保持するユニバーサル プロバイダーを使用できることを意味するでビルドされていました。 ユニバーサルのプロバイダーを使用したデータベースのスキーマはも非常に多くをクリーンアップされました。

ユニバーサル プロバイダーは、まだ SqlMembership プロバイダーと同じ制限を実行するために、ASP.NET メンバーシップ インフラストラクチャに基づいて構築されます。 つまり、リレーショナル データベース用に設計された、およびプロファイルとユーザーの情報をカスタマイズすることは困難です。 これらのプロバイダーは、ログインとログアウト機能もフォーム認証を使用します。

## <a name="aspnet-identity"></a>ASP.NET Identity

メンバーシップと ASP.NET でのストーリーは進化しました、長年にわたって ASP.NET チームがお客様からのフィードバックから多くについて説明しました。

ユーザー名と、独自のアプリケーションに登録されている、パスワードを入力してユーザーがログインであるという前提が有効ではなくなりました。 Web は、その他のソーシャルになっています。 ユーザーは、Facebook、Twitter、およびその他のソーシャル web サイトなどのソーシャル チャンネルにリアルタイムで相互作用します。 開発者は、ユーザーが web サイトで豊富なエクスペリエンスができるように、ソーシャル id でログインできるようにします。 最新のメンバーシップ システムには、リダイレクトに基づくログインなど、Facebook、Twitter、およびその他のユーザーの認証プロバイダーが有効にする必要があります。

Web 開発の進化と web 開発のパターンをくれました。 ユニット アプリケーション開発者向けの大きな懸念テスト アプリケーション コードのようになりました。 2008 では、ASP.NET は、単体テストが容易な ASP.NET アプリケーションをビルドする開発者のために、モデル-ビュー-コント ローラー (MVC) パターンに基づいて新しいフレームワークを追加します。 単位たい開発者にとっては、することができるメンバーシップ システムにする必要も、アプリケーション ロジックをテストします。

Web アプリケーションの開発でこれらの変更では、考慮すると、ASP.NET Identity は、次の目的で開発されました。

- **1 つの ASP.NET の Id システム**

    - ASP.NET MVC、Web フォーム、Web ページ、Web API や SignalR など、ASP.NET フレームワークのすべてでは、ASP.NET Identity を使用できます。
    - ASP.NET Identity は、web、phone、ストア、またはハイブリッド アプリケーションを作成するときに使用できます。
- **ユーザーに関するプロファイル データに接続性**

    - ユーザーおよびプロファイル情報のスキーマに制御があります。 たとえば、アプリケーションでアカウントを登録するときに、ユーザーが入力した生年月日を格納するシステムを簡単に実現できます。

- **永続化の制御**

    - 既定では、ASP.NET Identity システムは、データベースのすべてのユーザー情報を格納します。 ASP.NET Identity は、すべての永続化メカニズムを実装するために、Entity Framework Code First を使用します。
    - データベース スキーマ、テーブル名の変更や変更などの一般的なタスクを制御するための主キーのデータ型は簡単に実行します。
    - スローすることがなくなど、SharePoint や Azure Storage Table Service、NoSQL データベースなどの別のストレージ メカニズムを接続するは簡単`System.NotImplementedExceptions`例外。
- **単体テストの容易性**

    - ASP.NET Identity によって、web アプリケーションのさらに単体テスト可能です。 ASP.NET Identity を使用するアプリケーションの部品用の単体テストを記述することができます。
- **ロール プロバイダー**

    - ロールによって、アプリケーションの部分へのアクセスを制限できるロール プロバイダーがあります。 "Admin"などのロールを作成し、ロールにユーザーを追加することができます簡単にします。
- **クレーム ベース**

    - ASP.NET Identity では、ユーザーの id が要求のセットとして表される、クレーム ベースの認証をサポートします。 クレームは、開発者がのロールを使用するよりも、ユーザーの id を記述するよりずっと表現力豊かなを許可します。 ロールのメンバーシップが (メンバーまたはメンバーではない)、ブールだけの場合は、クレームは、ユーザーの id とメンバーシップに関する豊富な情報を含めることができます。
- **ソーシャル ログイン プロバイダー**

    - 簡単にアプリケーションには、ソーシャル ログインなどの Microsoft アカウント、Facebook、Twitter、Google、および他のユーザーを追加し、アプリケーションでユーザーに固有のデータを格納できます。
- **Azure Active Directory**

    - Azure Active Directory を使用してログの機能を追加し、アプリケーションでユーザーに固有のデータを格納することもできます。 詳細については、次を参照してください[組織アカウント](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth)で Visual Studio 2013 で ASP.NET Web プロジェクトの作成。
- **OWIN の統合**

    - ASP.NET 認証は、任意の OWIN ベースのホストで使用できる、OWIN ミドルウェアに基づいています。 ASP.NET Identity には、System.Web のすべての依存関係がありません。 OWIN フレームワークを完全に準拠し、OWIN ホストされているアプリケーションで使用できます。
    - ASP.NET Identity は、ログ/ログ出し、web サイト内のユーザーのための OWIN 認証を使用します。 つまり、クッキーを生成する FormsAuthentication を使用する代わりに、アプリケーションが使用する OWIN CookieAuthentication そのためには。
- **NuGet パッケージ**

    - ASP.NET Identity は、Visual Studio 2013 に付属する ASP.NET MVC、Web フォームおよび Web API テンプレートにインストールされている NuGet パッケージとして再配分されます。 この NuGet パッケージは、NuGet ギャラリーからダウンロードできます。
    - NuGet として ASP.NET Identity を解放するパッケージを簡単で新機能とバグ修正、反復処理をアジャイルの方法で開発者に提供するための ASP.NET チーム。

## <a name="getting-started-with-aspnet-identity"></a>ASP.NET Identity の概要

ASP.NET Identity は、ASP.NET MVC、Web フォーム、Web API、SPA の Visual Studio 2013 のプロジェクト テンプレートに使用されます。 このチュートリアルでログインを登録する機能を追加するプロジェクト テンプレートが ASP.NET Identity を使用する方法について説明がされ、ユーザーをログアウトします。

ASP.NET Identity は、次の手順を使用して実装されます。 この記事の目的は、ASP.NET Identity; の高レベルな概要を表示するには次の手順に従ってまたは詳細を読み取るだけできます。 新しい API を使用して、ユーザー、ロールおよびプロファイル情報を追加するなど、ASP.NET Identity を使用してアプリを作成する方法の詳細な手順については、この記事の最後に次の手順を参照してください。

1. 個々 のアカウントを使用して、ASP.NET MVC アプリケーションを作成します。 ASP.NET MVC、Web フォーム、Web API、SignalR などの ASP.NET Identity を使用することができます。この記事では、ASP.NET MVC アプリケーションで開始します。  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. 作成したプロジェクトには、ASP.NET Identity の次の 3 つのパッケージが含まれています。

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   このパッケージには、ASP.NET Identity のデータと SQL Server へのスキーマに保持される ASP.NET Identity の Entity Framework の実装があります。
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   このパッケージには、ASP.NET Identity に core インターフェイスがあります。 このパッケージは、データベースなどの Azure Table Storage、NoSQL など別の永続化のターゲットが格納 ASP.NET Identity の実装を記述する使用できます。
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   このパッケージには、ASP.NET アプリケーションで ASP.NET Identity で OWIN 認証で接続するために使用する機能が含まれています。 これは、機能で cookie を生成する OWIN クッキー認証ミドルウェアを呼び出すとアプリケーション ログを追加するときに使用されます。
3. ユーザーを作成します。  
   アプリケーションを起動し、をクリックして、**登録**ユーザーを作成するリンク。 次の図は、ユーザー名とパスワードを収集する登録ページを示します。  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   ユーザーがクリックすると、**登録**ボタン、`Register`アカウント コント ローラーのアクションは、以下の強調表示されている、ASP.NET Identity API を呼び出すことによって、ユーザーを作成します。

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. ログイン。  
   ユーザーが正常に作成すると、彼女が記録によって、`SignInAsync`メソッド。  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

   上での強調表示されたコード、`SignInAsync`メソッドを生成、 [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx)します。 ASP.NET Identity と OWIN クッキー認証は要求ベースのシステムであるため、フレームワークには、ユーザーの ClaimsIdentity を生成するアプリが必要です。 ClaimsIdentity には、ユーザーが属するロールなど、ユーザーのすべての要求についての情報があります。 この段階でユーザーの複数のクレームを追加することもできます。  
  
   次の強調表示されたコード、 `SignInAsync` OWIN と呼び出し元から AuthenticationManager を使用して、メソッドが、ユーザーでサインイン`SignIn`ClaimsIdentity を渡すとします。  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. ログオフします。  
   クリックすると、**ログオフ**リンクは、account コント ローラー ログオフ アクションを呼び出します。 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   OWIN に示す、強調表示されたコード`AuthenticationManager.SignOut`メソッド。 これは、メソッドは[FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx)メソッドで使用される、 [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) Web フォームでのモジュール。

## <a name="components-of-aspnet-identity"></a>ASP.NET Identity のコンポーネント

次の図は、ASP.NET Identity のシステムのコンポーネントを示しています。 ([] をクリック[この](introduction-to-aspnet-identity/_static/image3.png)または拡大するダイアグラムで)。 緑色のパッケージは、ASP.NET の Id システムを構成します。 その他のすべてのパッケージは、ASP.NET アプリケーションで ASP.NET の Id システムを使用するために必要な依存関係です。

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

いない前に説明した NuGet パッケージの簡単な説明を次に示します。

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Cookie を使用するアプリケーションをできるようにするミドルウェア ベースの認証、ASP に似ています。NET のフォーム認証です。
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework は、リレーショナル データベース用の Microsoft の推奨されるデータ アクセス テクノロジです。

## <a name="migrating-from-membership-to-aspnet-identity"></a>メンバーシップから ASP.NET Identity に移行します。

すぐに新しい ASP.NET Identity システムへの ASP.NET メンバーシップまたは簡易なメンバーシップを使用する既存のアプリの移行に関するガイダンスを提供することと思います。

## <a name="next-steps"></a>次の手順

- [Facebook と Google の OAuth2 や OpenID サインオンで ASP.NET MVC 5 アプリを作成します。](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 このチュートリアルでは、ASP.NET Identity API を使用して、プロファイル情報をユーザー データベース、および Google と Facebook を認証する方法を追加します。
- [Auth と SQL DB を使って、ASP.NET MVC アプリを作成し、Azure App Service にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 このチュートリアルでは、Id API を使用して、ユーザーとロールを追加する方法を示します。
- [個々 のユーザー アカウント](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth)Visual Studio 2013 で ASP.NET Web プロジェクトを作成します。
- [組織アカウント](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth)Visual Studio 2013 で ASP.NET Web プロジェクトを作成します。
- [テンプレートの VS 2013 で ASP.NET Identity でのプロファイル情報のカスタマイズ](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [VS 2013 のプロジェクト テンプレートで使用するソーシャル プロバイダーの詳細情報を取得します。](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 基本的なロールとユーザーのサポートを追加する方法、およびロールとユーザー管理を行う方法を示すサンプル アプリケーション。
