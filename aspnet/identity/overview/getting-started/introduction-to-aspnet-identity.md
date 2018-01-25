---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: "ASP.NET Id の概要 |Microsoft ドキュメント"
author: jongalloway
description: "ASP.NET メンバーシップ システムで導入された ASP.NET 2.0 背面 2005年では、および変更があった多くの方法の web アプリケーション違ってにし、."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 7c7dcb7903b0d0772acc560161ff39c6869c599a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-aspnet-identity"></a>ASP.NET Id の概要
====================
によって[Jon Galloway](https://github.com/jongalloway)、 [Pranav Rastogi](https://github.com/rustd)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

> ASP.NET メンバーシップ システムで導入された ASP.NET 2.0 背面 2005年では、および変更があった多くの web アプリケーションは、認証と承認に通常処理の方法で、します。 ASP.NET Identity は、メンバーシップ システムべき web、電話またはタブレットの最新のアプリケーションを作成するときに改めて注目します。
> 
> この記事は Pranav Rastogi によって書き込まれました ([@rustd](https://twitter.com/rustd))、Jon Galloway ([@jongalloway](https://twitter.com/jongalloway))、Tom Dykstra と Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。


## <a name="background-membership-in-aspnet"></a>ASP.NET メンバーシップの背景知識:

### <a name="aspnet-membership"></a>ASP.NET メンバーシップ

[ASP.NET メンバーシップ](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx)が 2005年では、フォーム認証、およびユーザー名、パスワード、およびプロファイル データの SQL Server データベースに関連する一般的なサイトのメンバーシップの要件を解決するように設計されました。 現在、web アプリケーションのデータ記憶域オプションの多くに広範な配列があるし、ほとんどの開発者が認証と承認の機能のためのソーシャル id プロバイダーを使用するには、そのサイトを有効にします。 ASP.NET メンバーシップのデザインの制限を行います。 この移行困難

- SQL Server 用に設計された、データベース スキーマとそれを変更することはできません。 プロファイル情報を追加することができますが、追加のデータが困難にアクセスする以外で、プロファイル プロバイダーの API を介して別のテーブルにパックされました。
- プロバイダーのシステムでは、バックアップ データ ストアを変更することができますが、システムが、リレーショナル データベースの適切な前提条件の周囲に設計されています。 Azure ストレージ テーブルなどの非リレーショナル ストレージ メカニズムでメンバーシップ情報を格納するプロバイダーを作成することができますが、多くのコードとの多くを記述してリレーショナル設計を回避する必要がある、`System.NotImplementedException`しないメソッドに対して例外NoSQL データベースに適用されます。
- ログ/ログ アウト機能は、フォーム認証に基づいているため、メンバーシップ システムは使用できません[OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)です。 OWIN にはミドルウェア コンポーネント (Microsoft アカウント、Facebook、Google、Twitter) などの外部の id プロバイダーを使用してログのサポートを含む、認証が含まれていますから組織のアカウントを使用してログを内部設置型 Active Directory またはAzure Active Directory。 OWIN には、OAuth 2.0 JWT、CORS のサポートも含まれています。

### <a name="aspnet-simple-membership"></a>単純な ASP.NET メンバーシップ

[ASP.NET の簡易なメンバーシップ](../../../web-pages/overview/security/16-adding-security-and-membership.md)ASP.NET Web Pages のメンバーシップ システムとして開発されました。 WebMatrix や Visual Studio 2010 SP1 でリリースされました。 簡易なメンバーシップの目標は、Web Pages アプリケーションにメンバーシップ機能を追加しやすいことでした。

簡易なメンバーシップがしやすく、ユーザー プロファイル情報をカスタマイズするが、まだ ASP.NET メンバーシップと、他の問題を共有してあり、いくつかの制限。

- 非リレーショナル ストアにメンバーシップ システムのデータを保持することでした。
- OWIN の使用することはできません。
- 既存の ASP.NET メンバーシップ プロバイダーでは機能しないしは拡張できません。

### <a name="aspnet-universal-providers"></a>ASP.NET ユニバーサル プロバイダー

[ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx)と Azure SQL データベース、SQL Server Compact の操作も Microsoft でメンバーシップ情報を永続化に使用するために開発されました。 ユニバーサル プロバイダーは、Entity Framework Code First、EF でサポートされている任意のストア内のデータを永続化する汎用プロバイダーを使用できることを意味で構築されました。 ユニバーサルのプロバイダーで、データベース スキーマは、非常に多くも駆除されました。

ユニバーサル プロバイダーは、まだ SqlMembership プロバイダーと同じ制限を実行するため、ASP.NET メンバーシップ インフラストラクチャに基づいて構築されます。 つまり、リレーショナル データベース用に設計されたとしているユーザー プロファイルの情報をカスタマイズすることは困難です。 これらのプロバイダーは、ログインとログアウト機能のフォーム認証を使用します。

## <a name="aspnet-identity"></a>ASP.NET Identity

メンバーシップとして ASP.NET のストーリーの進化に伴い、長年にわたって ASP.NET チームがお客様からのフィードバックから多くについて学習しました。

想定されるユーザー名と、独自のアプリケーションに登録されている、パスワードを入力してユーザーがログインが無効になっています。 Web をよりソーシャルになっています。 ユーザーは、Facebook、Twitter、およびその他のソーシャル web サイトなどのソーシャル チャネルを通じて、リアルタイムで互いと対話します。 開発者は、ユーザーが web サイトでリッチ エクスペリエンスを持てるようにそれらのソーシャル id でログインできるをします。 最新メンバーシップ システムには、リダイレクトに基づくログインなど、Facebook、Twitter、およびその他のユーザーの認証プロバイダーが有効にする必要があります。

Web 開発の進化に伴いと web 開発のパターンをくれました。 単位アプリケーション開発者向けの中核となる問題になるアプリケーション コードのテストになりました。 2008 では、ASP.NET は、単体テストが容易な ASP.NET アプリケーションをビルドする開発者を支援する一部のモデル ビュー コント ローラー (MVC) パターンに基づいて新しいフレームワークを追加します。 単位に抑えようとした開発者は、メンバーシップ システムで行うことができる必要も、アプリケーション ロジックをテストします。

Web アプリケーションの開発でのこれらの変更を考えると ASP.NET Identity は、次の目的に開発されました。

- **1 つの ASP.NET Id システム**

    - ASP.NET Identity は、ASP.NET MVC、Web フォーム、Web ページ、Web API、および SignalR など、ASP.NET フレームワークのすべてで使用できます。
    - ASP.NET Identity は、web、電話、ストア、またはハイブリッド アプリケーションを作成するときに使用できます。
- **ユーザー プロファイル データの接続性**

    - ユーザーおよびプロファイル情報のスキーマに制御があります。 たとえば、アプリケーションでアカウントを登録するときに、ユーザーが入力した生年月日を格納するシステムを簡単に有効にできます。

- **永続化コントロール**

    - 既定では、ASP.NET Identity システムは、データベースのすべてのユーザー情報を格納します。 ASP.NET Identity Entity Framework Code First を使用してすべての永続化メカニズムを実装します。
    - データベース スキーマ、テーブル名の変更や変更などの一般的なタスクを制御するための主キーのデータ型は簡単です。
    - スローせずなど、SharePoint、Azure ストレージ テーブル サービス、NoSQL データベースなどの別のストレージ メカニズムを接続するは簡単`System.NotImplementedExceptions`例外。
- **単体テストの容易性**

    - ASP.NET Id 検証を実現 web アプリケーション ユニットです。 ASP.NET の Id を使用するアプリケーションの部分の単体テストを記述することができます。
- **ロール プロバイダー**

    - ロールによって、アプリケーションの部分へのアクセスを制限するロール プロバイダーがあります。 簡単に"Admin"などの役割を作成して、ユーザー ロールを追加できます。
- **クレーム ベース**

    - ASP.NET Identity は、ユーザーの id が、クレームのセットとして表されるクレーム ベース認証をサポートします。 信頼性情報には、ロールを使用するよりも、ユーザーの id を記述するときよりもずっと表現力が豊かする開発者ができるようにします。 ロールのメンバーシップだけブール値 (または非メンバー) に所属しますが、クレームは、ユーザーの id およびメンバーシップに関する豊富な情報を含めることができます。
- **ソーシャル ログイン プロバイダー**

    - 簡単に、アプリケーションにソーシャル ログインなどの Microsoft アカウント、Facebook、Twitter、Google、および他のユーザーを追加して、アプリケーションでユーザーに固有のデータを格納します。
- **Azure Active Directory**

    - また、Azure Active Directory を使用してログの機能を追加でき、アプリケーションでユーザーに固有のデータを格納できます。 詳細については、次を参照してください[組織アカウント](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth)で Visual Studio 2013 で ASP.NET Web プロジェクトの作成。
- **OWIN の統合**

    - ASP.NET 認証は、任意の OWIN ベースのホストで使用できますの OWIN ミドルウェアに基づいています。 ASP.NET の Id には、System.Web のすべての依存関係がありません。 完全に準拠して OWIN フレームワークと、任意のホストされている OWIN アプリケーションで使用できます。
    - ASP.NET Identity は、ログ/ログ-アウトの web サイト内のユーザーのための OWIN 認証を使用します。 これは、cookie を生成する FormsAuthentication を使用する代わりに、アプリケーションが使用する OWIN CookieAuthentication を行うにはことを意味します。
- **NuGet パッケージ**

    - ASP.NET Identity は、Visual Studio 2013 に付属している ASP.NET MVC、Web フォームおよび Web API テンプレートにインストールされている NuGet パッケージとして再配分されます。 NuGet ギャラリーからこの NuGet パッケージをダウンロードすることができます。
    - NuGet として ASP.NET Identity を解放するパッケージやすくの新機能とバグの修正、繰り返しを簡便な方法でこれらの開発者に提供するための ASP.NET チームです。

## <a name="getting-started-with-aspnet-identity"></a>ASP.NET Identity の概要

ASP.NET Identity は、ASP.NET MVC、Web フォーム、Web API および SPA を Visual Studio 2013 プロジェクト テンプレートで使用されます。 このチュートリアルでは、プロジェクト テンプレートが ASP.NET Identity を使用してログインを登録する機能を追加する方法を示していて、ユーザーをログアウトおします。

ASP.NET Identity は、次の手順を使用して実装されます。 この記事の目的は、ASP.NET Identity; の高レベルな概要を提供するには次の手順に従って、詳細を読み取るだけでしたりできます。 新しい API を使用して、ユーザー、ロール、およびプロファイル情報を追加するなど、ASP.NET の Id を使用してアプリを作成する方法の詳細な手順については、この記事の最後に次の手順を参照してください。

1. 個々 のアカウントで、ASP.NET MVC アプリケーションを作成します。 ASP.NET MVC、Web フォーム、Web API SignalR などで ASP.NET の Id を使用することができます。この記事で、ASP.NET MVC アプリケーションを開始します。  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. 作成したプロジェクトには、ASP.NET Identity の次の 3 つのパッケージが含まれています。

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
 このパッケージには、ASP.NET Identity データと SQL Server へのスキーマに保持される ASP.NET Identity の Entity Framework 実装があります。
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
 このパッケージには、ASP.NET Identity のコア インターフェイスがあります。 このパッケージを使用して、データベースなどの Azure テーブル ストレージ、NoSQL などさまざまな永続化の対象が格納 ASP.NET Identity の実装を記述します。
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
 このパッケージには、ASP.NET アプリケーションで ASP.NET Identity の OWIN 認証で接続に使用される機能が含まれています。 これは、機能で、アプリケーションと cookie を生成する OWIN の Cookie 認証ミドルウェアに呼び出しにログを追加する場合に使用されます。
3. ユーザーを作成します。  
 アプリケーションを起動し、をクリックして、**登録**ユーザーを作成するリンクです。 次の図は、ユーザー名とパスワードを収集する登録ページを示します。  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
 ユーザーがクリックしたとき、**登録**ボタン、`Register`アカウント コント ローラーのアクションは、以下に示すように、ASP.NET Identity API を呼び出すことによって、ユーザーを作成します。

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. ログイン。  
 ユーザーが正常に作成された場合、彼女に記録によって、`SignInAsync`メソッドです。  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

 上での強調表示されたコード、`SignInAsync`メソッドを生成、 [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx)です。 ASP.NET Identity と OWIN の Cookie 認証は要求ベースのシステムであるため、フレームワークには、ユーザーの ClaimsIdentity を生成するアプリが必要です。 ClaimsIdentity には、ユーザーが属するロールなどのユーザーのすべての要求に関する情報があります。 この段階で、ユーザーのクレームを追加することもできます。  
  
 強調表示されたコードの下で、 `SignInAsync` OWIN と呼び出し元からの AuthenticationManager を使用して、ユーザーがサインイン メソッド`SignIn`ClaimsIdentity を渡しています。  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. ログオフします。  
 クリックすると、**ログオフ**リンクは、アカウント コント ローラーでログオフ アクションを呼び出します。 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

 強調表示されたコードの表示上、OWIN`AuthenticationManager.SignOut`メソッドです。 これは、メソッドは[FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx)で使用する方法、 [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) Web フォーム内のモジュール。

## <a name="components-of-aspnet-identity"></a>ASP.NET Identity のコンポーネント

次の図は、ASP.NET Identity のシステムのコンポーネントを示しています。 ([] をクリック[この](introduction-to-aspnet-identity/_static/image3.png)または拡大するダイアグラム上)。 緑に含まれるパッケージは、ASP.NET Identity システムを構成します。 その他のすべてのパッケージには、ASP.NET アプリケーションで ASP.NET Id システムを使用する必要は依存があります。

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

説明したようにない NuGet パッケージの簡単な説明を次に示します。

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 により、アプリケーションはクッキーを使用するミドルウェア ベースの認証、ASP に似ています。NET のフォーム認証です。
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework は、リレーショナル データベース用の Microsoft の推奨データ アクセス テクノロジです。

## <a name="migrating-from-membership-to-aspnet-identity"></a>メンバーシップから ASP.NET の Id への移行

すぐに新しい ASP.NET Identity システムへの ASP.NET メンバーシップまたは簡易なメンバーシップを使用して、既存のアプリの移行のガイダンスを提供する予定です。

## <a name="next-steps"></a>次の手順

- [Facebook と Google OAuth2 および OpenID サインオンと ASP.NET MVC 5 アプリを作成します。](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 チュートリアルでは、ASP.NET Identity API を使用して、プロファイル情報をユーザー データベース、および Google、Facebook の認証方法を追加します。
- [Azure App Service に認証と SQL DB の ASP.NET MVC アプリの作成し、展開](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 このチュートリアルでは、Id API を使用して、ユーザーとロールを追加する方法を示します。
- [個々 のユーザー アカウント](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth)Visual Studio 2013 で ASP.NET Web プロジェクトを作成します。
- [組織アカウント](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth)Visual Studio 2013 で ASP.NET Web プロジェクトを作成します。
- [VS 2013 テンプレートの ASP.NET Identity のプロファイル情報をカスタマイズします。](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [VS 2013 プロジェクト テンプレートで使用されるソーシャル プロバイダーから情報を入手します。](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 基本的な役割とユーザーのサポートを追加する方法およびロールとユーザー管理を行う方法を示すサンプル アプリケーション。
