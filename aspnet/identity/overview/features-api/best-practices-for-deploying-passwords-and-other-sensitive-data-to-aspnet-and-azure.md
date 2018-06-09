---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: ASP.NET と Azure App Service へのパスワードなどの機密データの展開のベスト プラクティス |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、どのようにコードに安全に格納してセキュリティで保護された情報にアクセスします。 最も重要な点は、パスワードやその他の送信を保存しないでください.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2015
ms.topic: article
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 995d9a088e3095f36a01d2adb19ec08e6a6d1b3e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "28033022"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>ASP.NET と Azure App Service にパスワードや他の機密データを展開するためのベスト プラクティス
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、どのようにコードに安全に格納してセキュリティで保護された情報にアクセスします。 最も重要な点は、ソース コードで、パスワードや他の機密データを格納する必要がありますしないと、開発およびテスト モードで運用環境の機密情報を使用しないでください。
> 
> サンプル コードは、単純な web ジョブ コンソール アプリと、データベースの接続文字列パスワード、Twilio、Google、SendGrid セキュリティで保護されたキーへのアクセスを必要がある ASP.NET MVC アプリです。
> 
> 内部設置型の設定と PHP も含まれています。


- [開発環境でパスワードの操作](#pwd)
- [開発環境での接続文字列の使用](#con)
- [コンソール アプリを WebJobs](#wj)
- [機密データを Azure に展開します。](#da)
- [内部設置型および PHP のノート](#not)
- [その他のリソース](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>開発環境でパスワードの操作

チュートリアルは、ソース コード内の機密データを格納することはありませんように注意を願っていますのソース コードで頻繁に機密データを表示します。 たとえば、my [SMS や電子メールの 2 fa で ASP.NET MVC 5 アプリ](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)チュートリアルでは、次に示します、 *web.config*ファイル。

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

*Web.config*ファイルには、ソース コードがあるため、これらのシークレットは、そのファイルに保存することはありませんか。 さいわい、`<appSettings>`要素には、`file`属性を機密性の高いアプリ構成設定を含む外部ファイルを指定することができます。 外部ファイルに、すべてのシークレットを移動するには、としてソース ツリーには、外部のファイルはチェックされません。 たとえば、次のマークアップ ファイルで*AppSettingsSecrets.config*アプリ シークレットには、すべてが含まれています。

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

外部ファイル内のマークアップ (*AppSettingsSecrets.config*このサンプルでは) が同じマークアップ内にある、 *web.config*ファイル。

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

ASP.NET ランタイム内のマークアップを含む外部ファイルの内容と結合&lt;appSettings&gt;要素。 指定したファイルが見つからない場合、ランタイムは、ファイル属性を無視します。

> [!WARNING]
> セキュリティ - 追加しないでください、*シークレット .config*をプロジェクトにファイルまたはソース管理にチェックインします。 既定では、Visual Studio の設定、`Build Action`に`Content`、つまり、ファイルを展開します。 詳細については、次を参照してください[理由しないすべてのプロジェクト フォルダーにファイルのデプロイしますか?。](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) 拡張機能を使用できますが、*シークレット .config*を保持するをお勧めファイル、 *.config*構成ファイルは IIS では処理されないように、します。 また、 *AppSettingsSecrets.config*ファイルが 2 つのディレクトリ レベルをから、 *web.config*ファイルを完全にソリューション ディレクトリ外であるためです。 ソリューションのディレクトリからファイルを移動することによって&quot;git 追加\*&quot;リポジトリに追加されません。


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>開発環境での接続文字列の使用

Visual Studio を使用する新しい ASP.NET プロジェクトを作成する[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)です。 LocalDB は、開発環境用に作成されました。 パスワードをする必要がない、機密情報が、ソース コードにチェックインされていることを防ぐために何もする必要はないためです。 一部の開発チームは、パスワードが必要な SQL Server (またはその他の DBMS) の完全なバージョンを使用します。

使用することができます、`configSource`全体を置き換える`<connectionStrings>`マークアップ。 異なり、 `<appSettings>` `file` 、マークアップをマージする属性、`configSource`属性が、マークアップに置き換えられます。 次のマークアップの表示、`configSource`属性、 *web.config*ファイル。

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> 使用する場合、`configSource`属性の上記のように、接続文字列を外部のファイルに移動する、Visual studio で新しい web サイトを作成、データベースを使用して、データベースの構成オプションが表示されなくを検出することはできませんとする puVisual Studio から Azure に blish です。 使用している場合、`configSource`属性、PowerShell を使用するには、web サイトとデータベースを作成して展開を作成することもことができます、web サイトとデータベース ポータルで発行する前にします。 [新規 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)スクリプトは、新しい web サイトとデータベースを作成します。


> [!WARNING]
> セキュリティ - とは異なり、 *AppSettingsSecrets.config*ファイル、外部接続文字列ファイルは、ルートと同じディレクトリにする必要があります*web.config*ファイル、対策を講じることを確認する必要がありますしないソース リポジトリにチェックインします。


> [!NOTE]
> **機密データ ファイルにセキュリティ警告:** 開発、テストや実稼働の機密情報を使用しないようにお勧めします。 これらの機密情報をリークをテストまたは開発で実稼働のパスワードを使用します。


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>コンソール アプリを WebJobs

*App.config* 、相対パスをサポートしていませんコンソール アプリによって使用されるファイルが絶対パスをサポートしています。 絶対パスを使用して機密情報をプロジェクト ディレクトリから移動するのにことができます。 次のマークアップで機密情報を示しています、 *C:\secrets\AppSettingsSecrets.config*ファイル、および内の非機密データ、 *app.config*ファイル。

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>機密データを Azure に展開します。

Azure に web アプリを展開するとき、 *AppSettingsSecrets.config* (つまり目的) ファイルを展開されません。 移動する可能性があります、 [Azure Management Portal](https://azure.microsoft.com/services/management-portal/)を行うには、手動で設定するとします。

1. 移動して[ https://portal.azure.com ](https://portal.azure.com)、し、Azure の資格情報でサインインします。
2. をクリックして**参照&gt;Web Apps**、web アプリの名前をクリックします。
3. をクリックして**すべて設定&gt;アプリケーション設定**です。

**アプリ設定**と**接続文字列**値が同じ設定を上書き、 *web.config*ファイル。 この例ではお配置しなかったこれらの設定、azure でこれらのキーがいた場合は、 *web.config*ファイル、ポータルに表示される設定が優先されます。

従うベスト プラクティスは、 [DevOps ワークフロー](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md)して[Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (またはなどの他のフレームワーク[Chef](http://www.opscode.com/chef/)または[Puppet](http://puppetlabs.com/puppet/what-is-puppet)) にAzure でのこれらの値の設定を自動化します。 次の PowerShell スクリプトを使用して[Export-clixml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk)暗号化された機密データをディスクにエクスポートします。

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

'上記のスクリプトで Name' は、秘密キーの名前など '&quot;FB\_AppSecret&quot;または"TwitterSecret"です。 お使いのブラウザーでスクリプトによって作成された".credential"ファイルを表示することができます。 次のスニペットは、これらの資格情報ファイルをテストし、名前付きの web アプリのシークレットを設定します。

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> セキュリティ: 機密性の高いデータを展開する PowerShell スクリプトを使用する目的は損なわを行って、PowerShell スクリプトにパスワードまたはその他の機密情報を含めないでください。 [Get-credential](https://technet.microsoft.com/library/hh849815.aspx)コマンドレットは、パスワードを取得するセキュリティで保護されたメカニズムを提供します。 UI のプロンプトを使用して、パスワードの漏洩を防止できます。


### <a name="deploying-db-connection-strings"></a>DB 接続文字列を展開します。

DB 接続文字列は、アプリの設定を同様に処理されます。 Visual Studio から web アプリを配置する場合、接続文字列が構成します。 これは、ポータルで確認できます。 PowerShell では、接続文字列を設定することをお勧めです。 PowerShell スクリプトの例については、web サイトとデータベースを作成し、接続文字列を設定、web サイトでダウンロード[新規 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)から、 [Azure スクリプト ライブラリ](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure)です。

<a id="not"></a>
## <a name="notes-for-php"></a>PHP のノート

両方のキー/値ペアから**アプリ設定**と**接続文字列**は、web アプリのフレームワーク (PHP) などを簡単に使用する開発者で Azure App Service 環境変数に格納これらの値を取得します。 Stefan Schackow を参照してください[Windows Azure Web サイト: どのアプリケーション文字列と接続文字列の作業](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)ブログの投稿をアプリの設定と接続文字列を読み取る PHP スニペットを表示します。

## <a name="notes-for-on-premises-servers"></a>内部設置型サーバーのノート

場合は、内部設置型の web サーバーに配置することができますをセキュリティで保護された機密情報[、構成ファイルの構成セクションを暗号化](https://msdn.microsoft.com/library/ff647398.aspx)です。 代わりに、Azure web サイトの推奨されるのと同じアプローチを使用することができます。 構成ファイルでの開発設定を保持し、実稼働設定の環境変数の値を使用します。 Azure Websites で自動化する機能をアプリケーション コードを記述する必要があるただし、ここでは、: 環境変数から設定を取得し、構成ファイルの設定の代わりにそれらの値を使用してまたは構成ファイルの設定を使用する場合環境変数が見つかりませんでした。

<a id="addRes"></a>
## <a name="additional-resources"></a>その他のリソース

接続文字列 + アプリの設定、ダウンロード、PowerShell の例については、web アプリ + データベースを作成するスクリプトを設定[新規 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)から、 [Azure スクリプト ライブラリ](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure)です。 

Stefan Schackow を参照してください[Windows Azure Web サイト: アプリケーション文字列と接続文字列の動作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Barry Dorrans 頂いた ( [ @blowdart ](https://twitter.com/blowdart) ) と Carlos Farre を確認します。
