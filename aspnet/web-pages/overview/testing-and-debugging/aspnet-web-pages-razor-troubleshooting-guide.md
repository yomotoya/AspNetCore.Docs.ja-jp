---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages (Razor) トラブルシューティング ガイド |Microsoft Docs
author: tfitzmac
description: この記事では、ASP.NET Web Pages (Razor) と一部の推奨されるソリューションを使用する場合がある問題について説明します。 ASP.NET Web ページのソフトウェア バージョン.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: c27139a720decd34a4ab89e6f93e71c97d123b45
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831270"
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a>ASP.NET Web Pages (Razor) トラブルシューティング ガイド
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) と一部の推奨されるソリューションを使用する場合がある問題について説明します。
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 および ASP.NET Web Pages 1.0 により連携します。


このトピックは、次のセクションで構成されています。

- [ページの実行に関する問題](#Issues_Running_.cshtml_Pages)
- [Razor コードの問題](#IssuesWithRazorCode)
- [セキュリティとメンバーシップに関する問題](#membership)
- [電子メールの送信に関する問題](#email)
- [その他のリソース](#AdditionalResources)

一般的な質問については、次を参照してください。 [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000)します。

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>ページの実行に関する問題

さまざまな問題を防ぐことができます *.cshtml*と *.vbhtml*ページを正しく実行されているからです。 このセクションでは、一般的なエラー メッセージを一覧表示し、考えられる原因します。

### <a name="http-error-403---forbidden-access-is-denied"></a>HTTP エラー 403 - アクセス不可: アクセスが拒否されました

*このディレクトリまたは指定した資格情報を使用してページを表示するアクセス許可がありません。*

このエラーは、サーバーに .NET Framework の正しいバージョンが実行されていない場合に発生することができます。 (ローカルまたはリモート) に、サーバーを実行しているコンピューターが以上もインストールされている .NET Framework 4 を持っていることを確認します。 また、適切なバージョンを実行する、アプリケーション自体が構成されていることを確認します。

WebMatrix での作業中にこの問題をローカルで発生する場合にクリックして、**サイト** ワークスペースをクリックしますクリックし、ツリー ビューで**設定**します。 **.NET Framework のバージョンの選択**一覧で、選択 **.NET 4 (Integrated)** します。 このバージョンは既に設定されている場合は、WebMatrix を管理者として実行してみてください。

Web サイトのルートが少なくとも 1 つであること確認 *.cshtml*ファイル。

Web サーバーがリモート サーバー上のときにこのエラーが発生した場合は、サーバーの管理者に問い合わせてください。 サーバーに .NET Framework 4 があることを確認または以降がインストールされていること。 また、アプリケーションがそのバージョンの.net Framework を使用するように構成されるアプリケーション プールで実行されていることを確認します。

サーバーを制御する場合は、.NET Framework の正しいバージョンが実行されていることを確認します。 インストールの修復を実行しても試すことがあります、`aspnet_regiis -iru`コマンド。 (たとえば、.NET Framework をインストールした後に IIS をインストールする場合 IIS がない正しく構成する ASP.NET ページを実行する。)詳細については、次を参照してください。 [ASP.NET IIS 登録ツール (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx)します。

### <a name="http-error-40314---forbidden"></a>HTTP エラー 403.14 - 許可されていません

*Web サーバーは、このディレクトリの内容が表示されないに構成されます。*

このエラーは、保護されているリソースを要求する場合に発生することができます (のように、 *Web.config*ファイル) または保護されているフォルダーには (のように*アプリ\_データ*または*アプリ\_コード*)。

### <a name="http-error-40417---not-found"></a>HTTP Error 404.17 - が見つかりませんでした。

*要求されたコンテンツにはスクリプトが表示され、静的ファイル ハンドラーは発生しません。*

サーバーは、.NET Framework 4 を使用して正しく構成されていないか、後で、そのためにコードを認識しません、このエラーが発生する可能性が`@{ }`ブロックします。 前の説明を参照して*HTTP エラー 403 - アクセス不可: アクセスが拒否された*します。

### <a name="http-error-4047---not-found"></a>HTTP エラー 404.7 - 見つかりません

*ファイル拡張子の拒否するように構成要求のフィルタ リング モジュール*

場合、このエラーが発生する可能性が *.cshtml*または *.vbhtml*拡張機能は、サーバーで明示的にブロックされています。 拡張機能が追加した Url が含まれないときに、この問題の現象がその Url 作業 *.cshtml*または *.vbhtml*機能しません。 解決策は、サイトの拡張機能を再度有効にする*Web.config*ファイル。 次の例では、有効にする方法を示しています、 *.cshtml*拡張機能。

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>HTTP エラー 404.8 - 見つかりません

*要求のフィルタ リング モジュールを構成するには hiddenSegment セクションを含む URL にパスを拒否します。*

このエラーは、保護されているリソースを要求する場合に発生することができます (のように、 *Web.config*ファイル) または保護されているフォルダーには (のように*アプリ\_データ*または*アプリ\_コード*)。

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>この種類のページ (サーバー エラーは '/' アプリケーションで) は処理できません。

HTTP エラー 404.17 については、前の説明を参照してください。

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Razor コードの問題

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>名前 '*クラス*'、現在のコンテキストに存在しません

多くの場合、このエラーが発生する理由は`class`参照、ヘルパーは、ヘルパーがインストールされていません。 たとえば、ヘルパーを使用しようとする場合、NuGet からパッケージをインストールしていない場合は、このエラーが表示されます。 WebMatrix でギャラリーを使用して、検索し、ヘルパーをインストールします。

かどうかは、ヘルパーがインストールされているが、ページが認識しないことを追加してみてくださいの追加、`using`ステートメントのコードにします。 `using`ステートメントでは、ヘルパーを含む名前空間の参照。 たとえば、ASP.NET Web Helpers パッケージに含まれる基本的なヘルパーはでは、`System.Web.Helpers`名前空間。 ヘルパーを使用するページの上部にあるこの行を追加します。

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>セキュリティとメンバーシップに関する問題

ASP.NET Web Pages (Razor) で、組み込みのセキュリティ (メンバーシップ) システムを使用している場合、は、次の問題が発生する可能性があります。

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>このメソッドを呼び出すには、"Membership.Provider"プロパティが"ExtendedMembershipProvider"のインスタンスをある必要があります。

このエラーはことを示すことがない`AspNetSqlMembershipProvider`クラスが構成されています。 (症状は、サイトがローカルで正常に動作が、ホスティング プロバイダーのサーバーに公開するときに、このエラーをスローします)。この問題の 1 つの修正をサイトの次を追加することで、簡易なメンバーシップを明示的に有効にするには*Web.config*ファイル。

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>電子メールの送信に関する問題

電子メールの送信に関する問題は、デバッグする困難になります。 最初の問題は、SMTP サーバーに接続できないことができます。 接続が成功した場合、ASP.NET では、オフがメッセージを SMTP サーバーに渡します。 ただし、SMTP サーバーが送信するを防ぐメッセージ自体に問題があります。

アプリケーションが正常に電子メールを送信しない場合は、次の操作を再試行してください。

- SMTP サーバー名はのような多くの場合、`smtp.provider.com`または`smtp.provider.net`します。 ただし、サイトをホスティング プロバイダーにパブリッシュする場合、SMTP サーバー名その時点であります`localhost`します。 このような状況では、公開した後、プロバイダーのサーバーで、サイトが実行されている場合でも、SMTP サーバーは、アプリケーションの観点からローカル可能性があるために発生します。 サーバー名には、この変更は、発行のプロセスの一部として、SMTP サーバーの名前を変更する必要がある可能性があります。
- ポート番号は、通常は 25 です。 ただし、一部のプロバイダー使用する必要がポート 587 またはいくつかその他のポート。 ポート番号と期待を使用する SMTP サーバーの所有者を確認します。
- 適切な資格情報を使用することを確認します。 ホスティング プロバイダーには、サイトを公開した場合は、プロバイダーは、電子メールが個別に指定された資格情報を使用します。 これらの資格情報は、パブリッシュに使用する資格情報とは異なる場合があります。
- 場合によって資格情報をまったく必要はありません。 個人の ISP を使用して電子メールを送信する場合は、メール プロバイダーには、資格情報既にわかっているかもしれません。 パブリッシュした後は、ローカル コンピューターにテストするときに異なる資格情報を使用する必要があります。
- メール プロバイダーには、暗号化を使用している場合は、設定`WebMail.EnableSsl`に`true`します。

電子メールの送信中にエラーがある場合は、次のような標準の ASP.NET エラー メッセージを参照してください可能性があります。

![電子メールの問題がある場合に、ASP.NET エラー メッセージ](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

使用して電子メールの送信に関する問題をデバッグすることも、`try-catch`次の例のように、ブロックします。 使用すると、`try-catch`ブロック、ASP.NET では、標準的なエラー メッセージを表示しません。 代わりに、エラーをキャプチャすることができます、`catch`ブロックの部分。

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

適切な値に置き換えてください`your-SMTP-server-name`など。 この方法が表示されるエラー メッセージの一部を以下に示します。

- *電子メールを送信できません。*

    - または -

    *接続されているパーティが一定の時間、または確立された接続は接続されているホストが応答しなかったために失敗しました正しく応答しなかったために、接続試行が失敗しました*

    このエラーは通常、アプリケーションが SMTP サーバーに接続できなかったことを意味します。 サーバー名を確認して、ポート番号。
- <em>メールボックスが使用できません。サーバーの応答が: 5.1.0 &lt; someuser@invaliddomain &gt;拒否送信者: 無効な送信者のドメイン</em>

    このメッセージは、ことを示すことができます、`From`アドレスが間違っているかがありません。
- *指定の文字列は、電子メール アドレスに必要な形式ではありません。*

    このエラーになっているための値、`To`または`From`プロパティは、電子メール アドレスとして認識されていません。 (電子メール アドレスが有効などの形式が正しくことのみである ASP.NET をチェックできません*name@domain.com*)。

> [!NOTE]
> エラーが表示されるマークアップを削除 (`@errorMessage`) ページからライブ サイトに発行する前にします。 いないユーザーがサーバーから取得するエラー メッセージが表示できるようにすることをお勧めします。


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>その他のリソース

[ASP.NET Web ページ (Razor) のよくあるご質問](https://go.microsoft.com/fwlink/?LinkId=253000)

[WebMatrix と ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) ASP.NET web サイトのフォーラム
