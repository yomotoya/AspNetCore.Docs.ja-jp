---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages (Razor) トラブルシューティング ガイド |Microsoft ドキュメント
author: tfitzmac
description: この記事では、ASP.NET Web Pages (Razor) と一部の推奨されるソリューションを使用するときにする必要がありますのある問題について説明します。 ASP.NET Web ページのソフトウェア バージョンしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: ec51169ccea0016712de3fdb28a16a174150a8bd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a>ASP.NET Web Pages (Razor) トラブルシューティング ガイド
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) と一部の推奨されるソリューションを使用するときにする必要がありますのある問題について説明します。
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 と ASP.NET Web Pages 1.0 でも動作します。


このトピックは、次のセクションで構成されています。

- [ページの実行に関する問題](#Issues_Running_.cshtml_Pages)
- [Razor コードの問題](#IssuesWithRazorCode)
- [セキュリティとメンバーシップに関する問題](#membership)
- [電子メールの送信に関する問題](#email)
- [その他のリソース](#AdditionalResources)

一般的な質問については、次を参照してください。 [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000)です。

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>ページの実行に関する問題

さまざまな問題を防ぐことができます*.cshtml*と*.vbhtml*ページから正常に実行します。 このセクションでは、一般的なエラー メッセージを一覧表示し、可能性のある原因です。

### <a name="http-error-403---forbidden-access-is-denied"></a>HTTP エラー 403 - アクセス不可: アクセスが拒否されました

*このディレクトリまたは入力した資格情報を使用してページを表示するアクセス許可がありません。*

このエラーは、サーバーに .NET Framework の正しいバージョンが実行されていない場合に発生することができます。 (ローカルまたはリモート)、サーバーを実行しているコンピューターに少なくとも .NET Framework 4 をインストールすることを確認します。 また、適切なバージョンを実行する、アプリケーション自体が構成されていることを確認します。

WebMatrix で作業中にこの問題をローカルで発生する場合にクリックして、**サイト** ワークスペースで、し、ツリー ビューのをクリックして**設定**です。 **.NET Framework のバージョンの選択**一覧で、選択**.NET 4 (Integrated)**です。 このバージョンは既に設定されている場合は、管理者として WebMatrix を実行してください。

Web サイトのルートが少なくとも 1 つであること確認*.cshtml*ファイル。

Web サーバーがリモート サーバーにこのエラーが発生する場合は、サーバー管理者に問い合わせてください。 サーバーに .NET Framework 4 があることを確認または後でインストールされているようにします。 また、アプリケーションが.net Framework のバージョンを使用するように構成されるアプリケーション プールで実行されていることを確認します。

サーバーを制御する場合は、.NET Framework の正しいバージョンが実行されていることを確認します。 インストールの修復を実行してもみて、`aspnet_regiis -iru`コマンド。 (たとえば、.NET Framework をインストールした後に IIS をインストールする場合 IIS がない正しく構成する ASP.NET ページを実行する。)詳細については、次を参照してください。 [ASP.NET IIS 登録ツール (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx)です。

### <a name="http-error-40314---forbidden"></a>HTTP エラー 403.14 - 許可されていません

*Web サーバーは、このディレクトリの内容が表示されないするように構成します。*

このエラーは、保護されているリソースを要求する場合に発生することができます (と同様に、 *Web.config*ファイル) か、保護されているフォルダーにある (と同様に*アプリ\_データ*または*アプリ\_コード*)。

### <a name="http-error-40417---not-found"></a>HTTP エラー 404.17 - が見つかりません。

*要求されたコンテンツでは、スクリプトである可能性があり、静的ファイル ハンドラーでは提供されません。*

このエラーは、サーバーは、.NET Framework 4 を使用する正しく構成されていないか、後で、およびでのコードは認識されない場合に発生する可能性が`@{ }`ブロックします。 前の説明を参照して*HTTP エラー 403 - アクセス不可: アクセスが拒否された*です。

### <a name="http-error-4047---not-found"></a>HTTP エラー 404.7 - が見つかりません。

*要求フィルター モジュールはファイル拡張子の拒否するように構成します。*

場合、このエラーが発生する可能性が*.cshtml*または*.vbhtml*サーバーで、拡張機能を明示的にブロックされています。 拡張機能を含む Url が含まれないときに、この問題の症状がその Url 作業*.cshtml*または*.vbhtml*機能しません。 考えられる解決方法は、サイトの拡張機能を再度有効にする*Web.config*ファイル。 次の例は、有効にする方法を示しています、 *.cshtml*拡張機能です。

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>HTTP エラー 404.8 - が見つかりません。

*要求フィルター モジュールが、hiddenSegment セクションを含む URL のパスを拒否するように構成します。*

このエラーは、保護されているリソースを要求する場合に発生することができます (と同様に、 *Web.config*ファイル) か、保護されているフォルダーにある (と同様に*アプリ\_データ*または*アプリ\_コード*)。

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>この種類のページは (サーバー エラーは '/' アプリケーションで) を処理できません。

HTTP エラー 404.17 については、前の説明を参照してください。

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Razor コードの問題

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>名前 '*クラス*'、現在のコンテキストに存在しません

多くの場合、このエラーが発生する理由は`class`参照、ヘルパーが、ヘルパーはインストールされません。 たとえば、ヘルパーに渡しを使用しようとする場合 NuGet からパッケージをインストールしていない場合は、このエラーが表示されます。 探して支援者をインストールするには、WebMatrix で、ギャラリーを使用します。

かどうか、ヘルパーがインストールされているが、ページも認識されません、try を追加、`using`ステートメントのコードにします。 `using`ステートメントでは、ヘルパーが含まれる名前空間の参照。 たとえば、ASP.NET Web Helpers パッケージ内にある、基本的なヘルパーはでは、`System.Web.Helpers`名前空間。 ヘルパーを使用するページの上部には、この行を追加します。

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>セキュリティとメンバーシップに関する問題

組み込みのセキュリティ (メンバーシップ) システムは、ASP.NET Web Pages (Razor) で使用する場合、は、次の問題が発生する可能性があります。

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>このメソッドを呼び出すには、"Membership.Provider"プロパティが"ExtendedMembershipProvider"のインスタンスを指定する必要があります。

このエラーは、ことを示すことがない`AspNetSqlMembershipProvider`クラスが構成されています。 (症状には、サイトがローカルで正常に動作が、ホスティング プロバイダーのサーバーにパブリッシュするときにこのエラーがスローされます)。この問題の 1 つの解決策をサイトの次を追加することで、簡易なメンバーシップを明示的に有効にする*Web.config*ファイル。

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>電子メールの送信に関する問題

電子メールの送信に関する問題をデバッグする困難なことができます。 最初の問題は、SMTP サーバーに接続できないことを指定できます。 接続が成功した場合、ASP.NET では、オフがメッセージを SMTP サーバーに渡します。 ただし、SMTP サーバーが送信することを防止するメッセージ自体に問題があります。

アプリケーションが正常に電子メールを送信しない場合は、次の操作を再試行してください。

- SMTP サーバー名は、のようなものでは多くの場合、`smtp.provider.com`または`smtp.provider.net`です。 ただし、ホスティング プロバイダーにサイトを発行する場合、SMTP サーバー名の点にあります`localhost`です。 このような状況では、公開したされ、プロバイダーのサーバーに、サイトが実行中、SMTP サーバーは、アプリケーションの観点からローカル可能性があるために発生します。 このサーバー名の変更は、発行プロセスの一部として SMTP サーバー名を変更する必要がある可能性があります。
- ポート番号は、通常は 25 です。 ただし、一部のプロバイダーを使用する必要ポート 587 またはいくつか他のポートです。 ポート番号と期待どおりに使用する SMTP サーバーの所有者を確認します。
- 適切な資格情報を使用することを確認します。 ホスティング プロバイダーには、サイトを公開した場合は、プロバイダーが具体的には示されている電子メールには、資格情報を使用します。 これらの資格情報は、発行に使用する資格情報と異なる可能性があります。
- 場合によって資格情報をまったく必要ありません。 個人の ISP を使用して電子メールを送信する場合は、電子メール プロバイダーが、資格情報を知っている可能性があります。 パブリッシュした後は、ローカル コンピューターでテストするとよりも別の資格情報を使用する必要があります。
- 場合は、電子メール プロバイダーは、暗号化を使用して、設定`WebMail.EnableSsl`に`true`です。

電子メールを送信中にエラーがある場合は、次のような標準の ASP.NET エラー メッセージを参照してください可能性があります。

![電子メールの問題がある場合に、ASP.NET エラー メッセージ](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

使用して電子メールの送信に関する問題をデバッグすることも、`try-catch`次の例のように、ブロックします。 使用すると、`try-catch`ブロック、ASP.NET は、標準的なエラー メッセージを表示しません。 エラーをキャプチャする代わりに、`catch`ブロックの部分です。

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

適切な値に置き換えてください`your-SMTP-server-name`のようにします。 この方法が表示されるエラー メッセージの一部を以下に示します。

- *メールの送信に失敗します。*

    - または -

    *接続されているパーティが一定の時間、または確立された接続が接続されているホストが応答に失敗しましたが失敗しました。 正しく応答しなかったために、接続試行が失敗しました*

    このエラーは通常、アプリケーションが、SMTP サーバーに接続できなかったことを意味します。 サーバー名を確認して、ポート番号。
- <em>メールボックスが使用できません。サーバーの応答: 5.1.0 &lt; someuser@invaliddomain &gt;送信者を拒否しました: 無効な送信者のドメイン</em>

    このメッセージは、ことを示すことができます、`From`アドレスが間違っているかがありません。
- *指定した文字列は、電子メール アドレスのために必要な形式ではありません。*

    このエラーが発生することの値、`To`または`From`プロパティは、電子メール アドレスとして認識されません。 (ASP.NET が電子メール アドレスが有効などの形式が正しくのみになることを確認することはできません*name@domain.com*)。

> [!NOTE]
> エラーが表示されるマークアップを削除 (`@errorMessage`) 実際のサイトにページを公開する前にします。 いないユーザーがサーバーから取得するエラー メッセージが表示できるようにすることをお勧めします。


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>その他のリソース

[ASP.NET Web ページ (Razor) のよくあるご質問](https://go.microsoft.com/fwlink/?LinkId=253000)

[WebMatrix と ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) ASP.NET web サイトのフォーラム
