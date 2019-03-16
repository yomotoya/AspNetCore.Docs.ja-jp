---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: ASP.NET と Azure App Service にパスワードやその他の機密データを展開するためのベスト プラクティス |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、どのようにコードに安全に格納してセキュリティで保護された情報にアクセスします。 最も重要な点は、パスワードやその他の送信を保存しないでください.
ms.author: riande
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 8b5d6bf9fad72218341e4e0b90144da01abea3aa
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577542"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>ASP.NET と Azure App Service にパスワードやその他の機密データを展開するためのベスト プラクティス
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

> このチュートリアルでは、どのようにコードに安全に格納してセキュリティで保護された情報にアクセスします。 最も重要な点は、ソース コードで、パスワードや他の機密データを格納する必要がありますしないと、開発およびテスト モードでは運用シークレットを使用しないでください。
> 
> サンプル コードは、単純な web ジョブ コンソール アプリと、データベース接続文字列パスワード、Twilio、Google と SendGrid セキュリティで保護されたキーへのアクセスが必要な ASP.NET MVC アプリです。
> 
> オンプレミスでの設定と PHP も説明します。


- [開発環境でパスワードを使用](#pwd)
- [開発環境での接続文字列の使用](#con)
- [Web ジョブのコンソール アプリ](#wj)
- [シークレットを Azure に展開します。](#da)
- [オンプレミスおよび PHP 向けのノート](#not)
- [その他のリソース](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>開発環境でパスワードを使用

チュートリアルは、ソース コードで機密データを格納する必要がありますしないことに注意をうまくいけば、ソース コードで頻繁に機密データを説明します。 たとえば、マイ[SMS と電子メール 2 fa での ASP.NET MVC 5 アプリ](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)チュートリアルでは、次に示します、 *web.config*ファイル。

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

*Web.config*ファイルには、ソース コードがあるため、これらのシークレットは、そのファイルに保存することはありませんか。 さいわい、`<appSettings>`要素には、`file`属性を機密性の高いアプリの構成設定を含む外部ファイルを指定することができます。 外部ファイルは、ソース ツリーにチェックしない限り、外部ファイルに、すべてのシークレットを移動できます。 たとえば、次のマークアップ ファイルで*AppSettingsSecrets.config*すべてのアプリ シークレットが含まれています。

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

外部ファイル内のマークアップ (*AppSettingsSecrets.config*このサンプルでは)、共通のマークアップで見つかった、 *web.config*ファイル。

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

ASP.NET ランタイム内のマークアップを含む外部ファイルの内容と結合&lt;appSettings&gt;要素。 ランタイムは、指定したファイルが見つからない場合に、ファイル属性を無視します。

> [!WARNING]
> セキュリティ - 追加しないでください、*シークレット .config*をプロジェクトにファイルまたはソース管理にチェックインします。 既定では、Visual Studio の設定、`Build Action`に`Content`、つまり、ファイルが展開されています。 詳細については、次を参照してください[理由はありませんすべてのプロジェクト フォルダーにファイルをデプロイしますか?。](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) 任意の拡張機能を使用することができますが、*シークレット .config*ようにすることをお勧めファイル、 *.config*構成ファイルが IIS によって提供されないように、します。 また、 *AppSettingsSecrets.config*ファイルは上からの 2 つのディレクトリ レベルでは、 *web.config*ファイル、ソリューションのディレクトリからは完全にします。 ソリューションのディレクトリからファイルを移動することによって&quot;git 追加\*&quot;リポジトリに追加されません。


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>開発環境での接続文字列の使用

Visual Studio を使用する新しい ASP.NET プロジェクトを作成する[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)します。 LocalDB は、具体的には、開発環境用に作成されました。 パスワードは必要ありません、ため、シークレットが、ソース コードにチェックインされていることを防ぐために何も実行する必要はありません。 一部の開発チームは、パスワードが必要な SQL Server (またはその他の DBMS) の完全なバージョンを使用します。

使用することができます、`configSource`全体を置き換える`<connectionStrings>`マークアップ。 異なり、 `<appSettings>` `file` 、マークアップをマージする属性、`configSource`属性のマークアップに置き換えられます。 次のマークアップに示す、`configSource`属性、 *web.config*ファイル。

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> 使用する場合、`configSource`接続文字列を外部ファイルに移動する前に示した属性し Visual studio で新しい web サイトを作成、データベースを使用しているし、データベースを構成するオプションが表示されなくを検出するために行うことはできませんとする puVisual Studio から Azure への blish します。 使用する場合、`configSource`属性、PowerShell を使用するには、web サイトとデータベースを作成して展開するまたはを作成、web サイトと、データベース、ポータルで発行する前にします。 [新規 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)スクリプトが新しい web サイトおよびデータベースに作成されます。


> [!WARNING]
> セキュリティ - とは異なり、 *AppSettingsSecrets.config*ファイル、外部接続文字列のファイルは、ルートと同じディレクトリである必要があります*web.config*ファイル、対策を講じることを確認する必要がありますソース リポジトリにチェックインしません。


> [!NOTE]
> **シークレット ファイルのセキュリティ警告:** テストと開発における運用シークレットを使用しないようにお勧めします。 テストまたは開発での運用環境のパスワードを使用すると、これらの機密情報がリークします。


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Web ジョブのコンソール アプリ

*App.config* 、相対パスをサポートしていませんコンソール アプリで使用されるファイルが絶対パスをサポートしています。 絶対パスを使用して、プロジェクト ディレクトリから、シークレットを移動することができます。 次のマークアップ内のシークレットを示しています、 *C:\secrets\AppSettingsSecrets.config*ファイル、および非機密データを*app.config*ファイル。

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Azure へシークレットを展開

Web アプリを Azure にデプロイするときに、 *AppSettingsSecrets.config* (つまり対象) ファイルはデプロイされません。 移動する可能性があります、 [Azure 管理ポータル](https://azure.microsoft.com/services/management-portal/)し、そのためには、それらを手動で設定します。

1. 移動して[ https://portal.azure.com ](https://portal.azure.com)、し、Azure 資格情報でサインインします。
2. をクリックして**参照&gt;Web Apps**、web アプリの名前をクリックします。
3. クリックして**すべて設定&gt;アプリケーション設定**します。

**アプリ設定**と**接続文字列**値で同じ設定が上書き、 *web.config*ファイル。 この例ではでしたいないこれらの設定を Azure にデプロイでこれらのキーがいた場合は、 *web.config*ファイル、設定をポータルに表示される優先順位。

従うベスト プラクティスを[DevOps ワークフロー](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md)して[Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (または別のフレームワークなど[Chef](http://www.opscode.com/chef/)または[Puppet](http://puppetlabs.com/puppet/what-is-puppet)) にAzure でのこれらの値の設定を自動化します。 次の PowerShell スクリプトを使用して[Export-clixml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk)暗号化した機密情報をディスクにエクスポートします。

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

'上記のスクリプトで Name' は、秘密キーの名前など '&quot;FB\_AppSecret&quot;または「[twittersecret]」。 お使いのブラウザーでスクリプトによって作成された".credential"ファイルを表示できます。 次のスニペットでは、各資格情報ファイルをテストし、名前付きの web アプリのシークレットを設定します。

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> セキュリティ - パスワードやその他のシークレットをそのララェホの PowerShell スクリプトを使用して機密データを展開する目的の実行、PowerShell スクリプトに含めないでください。 [Get-credential](https://technet.microsoft.com/library/hh849815.aspx)コマンドレットは、パスワードを取得するセキュリティで保護されたメカニズムを提供します。 UI プロンプトを使用して、パスワードのリークを防止できます。


### <a name="deploying-db-connection-strings"></a>DB 接続文字列の展開

DB の接続文字列は、アプリの設定を同様に処理されます。 Visual Studio から web アプリをデプロイする場合、接続文字列が構成します。 これは、ポータルで確認できます。 接続文字列を設定することをお勧めの方法は、PowerShell を使用したです。 PowerShell スクリプトの例については、web サイトとデータベースを作成し、接続文字列を設定、web サイトでダウンロード[新規 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)から、 [Azure スクリプト ライブラリ](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure)します。

<a id="not"></a>
## <a name="notes-for-php"></a>PHP における考慮事項

両方のキーと値のペアから**アプリ設定**と**接続文字列**任意の web アプリ (PHP) などのフレームワークを簡単に使用する開発者で、Azure App Service 環境変数に格納されますこれらの値を取得します。 Stefan Schackow を参照してください。 [Windows Azure Web サイト: アプリケーション文字列と接続文字列の動作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)ブログの投稿がアプリの設定と接続文字列を読み取るための PHP スニペットが表示されます。

## <a name="notes-for-on-premises-servers"></a>オンプレミス サーバーにおける考慮事項

オンプレミスの web サーバーを展開する場合は、によってセキュリティで保護された機密情報が役立つことができます[構成ファイルの構成セクションを暗号化](https://msdn.microsoft.com/library/ff647398.aspx)します。 別の方法として、同じ方法で Azure Websites の推奨を使用することができます。 構成ファイルでの開発設定を保持し、運用環境の設定の環境変数の値を使用します。 機能には、Azure Websites では、自動のアプリケーション コードを記述する必要がただし、ここでは、: 環境変数から設定を取得し、構成ファイルの設定の代わりにそれらの値を使用して、または構成ファイルの設定を使用して、ときに環境変数が見つかりませんでした。

<a id="addRes"></a>
## <a name="additional-resources"></a>その他のリソース

接続文字列 + アプリの設定、ダウンロード、PowerShell の例については、web アプリとデータベースを作成するスクリプトを設定[新規 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)から、 [Azure スクリプト ライブラリ](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure)します。 

Stefan Schackow を参照してください[Windows Azure Web サイト: アプリケーション文字列と接続文字列の動作。](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Barry Dorrans 頂いた ( [ @blowdart ](https://twitter.com/blowdart) ) と Carlos Farre レビューします。
