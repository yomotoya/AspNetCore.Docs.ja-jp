---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: ASP.NET MVC と Web ページの XSRF/CSRF 防止 |Microsoft Docs
author: Rick-Anderson
description: 悪意のある web サイトを使用して、interacti に影響する、web でホストされるアプリケーションに対する攻撃をクロスサイト リクエスト フォージェリ (XSRF または CSRF とも呼ばれます) には.
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: cd1b8de51c180471ab273c4541959368ffbd48a3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836764"
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>ASP.NET MVC と Web ページの XSRF/CSRF 防止
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> サイト間の要求が偽造 (XSRF または CSRF) は、悪意のある web サイトがクライアント ブラウザーとそのブラウザーが信頼する web サイト間のやり取りに影響するという、web でホストされるアプリケーションに対する攻撃です。 Web ブラウザーがすべての要求で自動的に認証トークンを web サイトに送信するため、これらの攻撃が可能になります。 標準的な例は、ASP などの認証クッキーです。NET のフォーム認証チケット。 ただし、(Windows 認証、Basic、およびなど) などの任意の永続的な認証メカニズムを使用する web サイトの場合は、これらの攻撃対象となることができます。
> 
> XSRF 攻撃はフィッシング攻撃とは異なります。 フィッシング攻撃には、対象とのやり取りが必要です。 フィッシング攻撃では、悪意のある web サイトがターゲットの web サイト、し、対象は、攻撃者に機密情報を提供することにようにだまされます。 XSRF 攻撃では、ある必要はありません多くの場合、相互作用に攻撃対象です。 代わりに、自動的にすべての関連クッキーを模擬 web サイトに送信するブラウザーで、攻撃者が証明書利用者です。
> 
> 詳細については、次を参照してください。、 [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))します。


## <a name="anatomy-of-an-attack"></a>攻撃の分析

XSRF 攻撃を説明するには、いくつかのオンライン銀行取引トランザクションを実行したいユーザーを検討してください。 このユーザーに最初にこの時点で、応答ヘッダーが認証 cookie を含む WoodgroveBank.com と、ログをアクセスします。

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

認証 cookie は、セッションの cookie であるためは自動的にオフにする、ブラウザーでブラウザーのプロセスの終了時にします。 ただし、それまでは、ブラウザーが自動的に cookie が含まれます WoodgroveBank.com 要求ごとにします。 ユーザーは、彼女、銀行サイト上のフォームに入力し、ブラウザーがサーバーにこの要求を行うために、別のアカウントに $1000 を転送するようになりましたが。

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

この操作には、(金銭トランザクションを開始) 副作用があるため、銀行サイトがこの操作を開始するには、HTTP POST を要求するように選択します。 サーバーは、要求から認証トークンを読み取ります、現在のユーザーのアカウントの数を調べ、十分な資金が存在することを確認し、コピー先アカウントに、トランザクションを開始します。

オンライン バンキング完了させます、ユーザーが銀行サイトから離れたと、web で他の場所にアクセスします。 次のマークアップ内に埋め込まれてページ – fabrikam.com – これらのサイトのいずれかが含まれています、 &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

この要求を行い、ブラウザーが発生しています。

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

攻撃者は、ユーザーは、ターゲット web サイトの有効な認証トークンを必要があり、彼女が、ターゲット サイトに HTTP POST のことを自動的にブラウザーに Javascript の小さなスニペットを使用してください。 いるという事実を悪用してください。 認証トークンがまだ有効の場合は、銀行サイトの攻撃者のアカウントに 250 ドルの転送が開始されます。

### <a name="ineffective-mitigations"></a>無効な軽減策

あったことを示す前述のシナリオで WoodgroveBank.com は SSL 経由でアクセスされていたと、SSL 専用の認証クッキーが攻撃を阻止するために十分な興味深いは。 攻撃者を指定すること、 [URI スキーム](http://en.wikipedia.org/wiki/URI_scheme)(https) で彼女&lt;フォーム&gt;要素、およびブラウザーは引き続きこれらの cookie は、URI で一貫性のある限り、対象のサイトに期限が切れていない cookie を送信目的のターゲットのスキーム。

ユーザーによってとして信頼済みサイトが安全なオンラインを維持するだけにアクセスして、信頼されていないサイト必要がありますいないアクセスだけように 1 つと言えるでしょう。 これは、一部は本当しますが、残念ながらこのアドバイスは常に実用的です。 おそらく、ユーザー「信頼」ローカル ニュース サイト ConsolidatedMessenger します。 ConsolidatedMessenger.com およびが代わりに、サイトのアクセスがそのサイトには、攻撃者が fabrikam.com 上で実行されるコードの同じスニペットを挿入することができる、XSS 脆弱性を持ちます。

受信要求があることを確認することができます、 [Referer ヘッダー](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14)ドメインを参照します。 これにより、無意識にサード パーティのドメインから送信された要求は停止します。 ただし、一部の人には、プライバシー上の理由から、ブラウザーの Referer ヘッダーが無効にして、攻撃者できる場合は被害者があるインストールされている特定のセキュリティ保護されていないソフトウェアそのヘッダーなりすます場合があります。 検証、 [Referer ヘッダー](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) XSRF 攻撃を防ぐための安全なアプローチとは見なされません。

## <a name="web-stack-runtime-xsrf-mitigations"></a>Web スタック ランタイム XSRF の軽減策

ASP.NET Web スタックのランタイムのバリアントを使用して、[シンクロナイザー トークン パターン](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern)XSRF 攻撃から保護します。 シンクロナイザー トークン パターンの一般的な形式は、2 つの ANTI-XSRF トークンは、(認証トークン) だけでなく各 HTTP POST を使用してサーバーに送信されます: として、cookie、フォームの値としてもう 1 つのトークン。 ASP.NET ランタイムによって生成されたトークンの値は、決定的または攻撃者が予測可能ではありません。 トークンが送信されたとき、サーバーが要求に両方のトークンは、比較のチェックを渡す場合にのみ続行を許可します。

XSRF 要求検証*セッション トークン*は HTTP cookie として格納され、現在そのペイロードでは、次の情報が含まれています。

- 128 ビットのランダムな識別子で構成されるセキュリティ トークンです。   
 次の図は、Internet Explorer F12 開発者ツールで表示される XSRF 要求検証のセッション トークン (注これは、現在の実装であり、件名、は、変化する可能性にもします。)。

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

*フィールド トークン*として格納されて、`<input type="hidden" />`そのペイロードでは、次の情報が含まれています。

- ログインしているユーザーのユーザー名 (認証済み) 場合。
- その他のデータによって提供される、 [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)します。

ANTI-XSRF トークンのペイロードが暗号化され、署名済み、ツールを使用して、トークンを確認する場合は、ユーザー名を表示できないようにします。 暗号化サービスがによって提供される web アプリケーションで ASP.NET 4.0 が対象とするときに、 [MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx)ルーチン。 Web アプリケーションで ASP.NET 4.5 を対象とするまたはで提供される、暗号化サービスと、 [MachineKey.Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110))ルーチンより優れたパフォーマンス、拡張性、およびセキュリティを提供します。 詳細については、次のブログの投稿を参照してください。

- [ASP.NET 4.5 での暗号化の機能強化、pt です。1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [ASP.NET 4.5 での暗号化の機能強化、pt です。2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [ASP.NET 4.5 での暗号化の機能強化、pt です。3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>トークンを生成します。

ANTI-XSRF トークンを生成するには、呼び出し、 [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) MVC ビューからのメソッドまたは@AntiForgery.GetHtmlRazor ページから ()。 ランタイムは、次の手順を実行し、されます。

1. 現在の HTTP 要求には既に ANTI-XSRF のセッション トークンが含まれている場合 (ANTI-XSRF cookie \_ \_RequestVerificationToken)、セキュリティ トークンの抽出元。 ANTI-XSRF のセッション トークンが HTTP 要求に含まれていない場合、またはセキュリティ トークンの抽出に失敗した場合は、新しいランダムな ANTI-XSRF トークンが生成されます。
2. 上記の手順 (1) と、現在ログインしているユーザーの id からセキュリティ トークンを使用して、ANTI-XSRF フィールドのトークンが生成されます。 (ユーザー id を確認する方法については、次を参照してください、 **[特別なサポートのシナリオ](#_Scenarios_with_special)** 以下のセクションです。)。さらに場合、 [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx)はランタイムを呼び出すが、構成されているその[GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx)メソッドとフィールドのトークンで返される文字列が含まれます。 (を参照してください、 **[構成と拡張機能](#_Configuration_and_extensibility)** 詳細についてはします)。
3. 新しい ANTI-XSRF トークンは、(1) の手順で生成された場合、新しいセッション トークンはそれを含めるためが作成され、送信 HTTP クッキーのコレクションに追加されます。 手順 (2) のフィールドのトークンにラップされます、`<input type="hidden" />`要素とこの HTML マークアップはの戻り値に`Html.AntiForgeryToken()`または`AntiForgery.GetHtml()`します。

## <a name="validating-the-tokens"></a>トークンを検証

入力方向の ANTI-XSRF トークンを検証するには、開発者が含まれます、 [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) 、MVC アクションまたはコント ローラー、または彼女の呼び出しに属性`@AntiForgery.Validate()`自分の Razor ページから。 ランタイムは、次の手順が実行されます。

1. 受信セッション トークンとフィールドのトークンが読み取られ、それぞれから ANTI-XSRF トークンを抽出します。 ANTI-XSRF トークンは、各世代ルーチンで、手順 (2) と同じである必要があります。
2. 現在のユーザーが認証されている場合、自分のユーザー名は、フィールドのトークンに格納されているユーザー名と比較されます。 ユーザー名が一致する必要があります。
3. 場合、 [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)が構成されているランタイムの呼び出し、 *ValidateAdditionalData*メソッド。 メソッドはブール値を返す必要があります*true*します。

検証が成功すると、続行する、要求が許可されます。 検証に失敗した場合、framework がスローされます、 *HttpAntiForgeryException*します。

## <a name="failure-conditions"></a>エラー条件

以降、ASP.NET Web スタックのランタイムの v2 では、 *HttpAntiForgeryException*中にスローされる検証には問題点の詳細についてにが含まれます。 現在定義されているエラー条件は次のとおりです。

- セッション トークンまたはフォームのトークンでは、要求に存在しません。
- セッション トークンは、フォーム トークンは、読み取ることができません。 これの最も可能性の高い原因は、ASP.NET Web スタックのランタイムまたはファームのバージョンの不一致を実行するファームで、 &lt;machineKey&gt; Web.config 内の要素は、マシン間で異なります。 Fiddler などのツールを使用して、強制的にこの例外をいずれかの ANTI-XSRF トークンを改ざんすることができます。
- セッション トークンとフィールドのトークンがスワップされました。
- セッション トークンとトークンをフィールドには、一致しないセキュリティ トークンが含まれます。
- フィールドのトークンに埋め込まれたユーザー名は、現在ログインしているユーザーのユーザー名と一致しません。
- *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* メソッドが返される*false*します。

ANTI-XSRF 機能は、トークンの生成または検証中に追加のチェックも実行でき、例外がスローされているこれらのチェック中にエラーがあります。 参照してください、 [WIF/ACS]、[クレーム ベース認証](#_WIF_ACS)と**[構成と拡張機能](#_Configuration_and_extensibility)** 詳細については、セクション。

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>特別なサポートのシナリオ

### <a name="anonymous-authentication"></a>匿名認証

ANTI-XSRF システムには"anonymous"が定義されているユーザーとして、匿名ユーザーの特殊なサポートが含まれる場所、 *IIdentity.IsAuthenticated*プロパティが返す*false*。 シナリオには、ログイン ページ (前に、ユーザーが認証される) と、メカニズムを使用アプリケーション以外のカスタム認証スキームに XSRF 保護を提供することが含まれます*IIdentity*ユーザーを識別します。

これらのシナリオをサポートするには、セキュリティ トークンには、128 ビット ランダムに生成された非透過識別子によって、セッションとフィールドのトークンが参加していることを思い出してください。 このセキュリティ トークンは、彼女匿名の識別子の目的を効果的に機能するために、サイトが移動したときに、個々 のユーザーのセッションを追跡するために使用されます。 空の文字列は、上記の生成と検証ルーチンのユーザー名の代わりに使用されます。

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF/ACS]、[クレーム ベース認証

通常、 *IIdentity* 、.NET Framework に組み込まれているクラス プロパティが設定されている*IIdentity.Name*は特定のアプリケーション内の特定のユーザーを一意に識別するための十分な。 たとえば、 *FormsIdentity.Name* (これによって、そのデータベースのすべてのアプリケーションに対して一意では)、メンバーシップ データベースに格納されているユーザー名を返します*WindowsIdentity.Name*を返します、ユーザーのドメイン修飾 id です。 これらのシステムだけでなく認証を提供します。これらも*識別*アプリケーションに対するユーザー。

クレーム ベースの認証では、その一方は必ずしも必要ありません、特定のユーザーを識別します。 代わりに、 *ClaimsPrincipal*と*ClaimsIdentity*種類が一連の関連*要求*インスタンス、個々 の要求が「18 + 年齢の年が」をする可能性がありますか"管理者"するものです。 ユーザーを識別されていない必ずしもため、ランタイムは使用できません、 *ClaimsIdentity.Name*プロパティとして特定のユーザーの一意の識別子。 チームが実際の例を表示、 *ClaimsIdentity.Name*返します*null*フレンドリ (表示) 名を返しますまたは、それ以外の場合は、一意の識別子として使用するための適切な文字列を返しますユーザー。

クレーム ベース認証を使用する展開の多くを使用している[Azure Access Control Service](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) 具体的には。 ACS では、個々 の開発者は構成がによって*id プロバイダー* (など、Microsoft アカウント プロバイダーを ADFS OpenID プロバイダーなどの yahoo! など)、id プロバイダーを返すと*識別子の名前*. これらの名前識別子は、電子メール アドレスのように Personally Identifiable Information (PII) を含めることができますか、匿名化された Private Personal Identifier (PPID) のような可能性があります。 関係なく、(id プロバイダー、名前識別子) タプル十分に役割を果たします、特定のユーザー用の適切な追跡トークンを生成するときに、ASP.NET Web スタックのランタイムは、ユーザー名の代わりに、タプルを使用できるように、サイトを参照彼女はときに、ANTI-XSRF フィールドのトークンを検証しています。 Id プロバイダーおよび名前識別子の特定の Uri は次のとおりです。

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(この[ACS ドキュメント ページ](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx)の詳細)。

生成するか、トークンを検証、ASP.NET Web スタックのランタイム実行時に試みます型にバインド。

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (用、WIF SDK があります。)
- `System.Security.Claims.ClaimsIdentity` (For .NET 4.5)。

これらの型が存在しない場合、現在のユーザーの*IIIIdentity*実装またはサブクラスのいずれかの種類、ANTI-XSRF 機能は、(id プロバイダー、名前識別子) を使用を生成する場合はユーザー名の代わりにタプルとトークンを検証します。 このような組が存在しない場合、要求は、開発者に、使用中、特定のクレーム ベースの認証メカニズムを理解する ANTI-XSRF システムを構成する方法を説明するエラーで失敗します。 参照してください、 **[構成と拡張機能](#_Configuration_and_extensibility)** 詳細についてはします。

### <a name="oauth--openid-authentication"></a>OAuth と OpenID 認証

最後に、ANTI-XSRF 施設 OAuth または OpenID 認証を使用しているアプリケーション用の特別なサポートしています。 このサポートは、ヒューリスティックに基づく: 場合、現在*IIdentity.Name*はユーザー名の比較を実行し、http:// または https:// で始まる既定 OrdinalIgnoreCase の比較子ではなく序数の比較子を使用します。

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>構成と拡張機能

場合によっては、開発者は、ANTI-XSRF 生成と検証の動作に厳密な制御を必要があります。 たとえば、おそらく HTTP クッキーを応答に自動的に追加の Web ページの MVC とヘルパーの既定の動作が望ましくないと、開発者がトークンを他の場所を保持することができます。 この作業に役立つ 2 つの Api が存在します。

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

*GetTokens*としてメソッドは、既存 XSRF 要求検証セッション トークン (null にすることがあります) を入力して、新しい XSRF 要求検証セッション トークンとフィールドのトークンを出力として生成します。 トークンは、単に不透明な文字列; の装飾なしの*formToken*値がのインスタンスでラップされなく、&lt;入力&gt;タグ。 *NewCookieToken*値があります。 この場合、null、 *oldCookieToken*値がまだ有効で、新しい応答 cookie を設定する必要はありません。 呼び出し元*GetTokens* 、必要な応答のクッキーを永続化または; 必要なマークアップを生成する責任を負いますが、 *GetTokens*メソッド自体は副作用として、応答を変更しません。 *検証*メソッドは、受信セッションと、フィールドを選択し、トークンの上に、前述の検証ロジックを実行します。

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

開発者がアプリケーションから ANTI-XSRF システムを構成する\_を開始します。 構成はプログラムです。 静的プロパティ*AntiForgeryConfig*型は次のとおりです。 要求を使用するほとんどのユーザーを UniqueClaimTypeIdentifier プロパティを設定します。

| **Property** | **説明** |
| --- | --- |
| **AdditionalDataProvider** | [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)をトークンの生成中に追加のデータを提供し、トークンの検証中に追加のデータを消費します。 既定値は*null*します。 詳細については、次を参照してください。、 [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)セクション。 |
| **CookieName** | ANTI-XSRF セッション トークンを格納するために使用される HTTP クッキーの名前を指定する文字列。 この値が設定されていない場合、名前が自動的に生成されます、アプリケーションのデプロイされた仮想パスに基づいています。 既定値は*null*します。 |
| **RequireSsl** | ANTI-XSRF トークンが SSL で保護されたチャネル経由で送信するために必要かどうかを示すブール値。 この値が場合*true*自動的に生成された cookie は、"secure"フラグを設定するには、していて、ANTI-XSRF Api が SSL 経由で送信が要求内から呼び出された場合にスローされます。 既定値は *false* です。 |
| **SuppressIdentityHeuristicChecks** | ANTI-XSRF システムに要求ベース id のサポートを非アクティブ化するかどうかを示すブール値。 この値が場合*true*、システムで、あると想定されます*IIdentity.Name*ユーザーごとの一意の識別子として使用するための適切なは、特別なケースは試みません*IClaimsIdentity*または*ClClaimsIdentity* 」の説明に従って、 [WIF/ACS]、[クレーム ベース認証](#_WIF_ACS)セクション。 既定値は `false` です。 |
| **UniqueClaimTypeIdentifier** | どの要求の種類を示す文字列は、ユーザーごとの一意の識別子として使用するために適しています。 この値が設定され、現在場合*IIdentity*で指定されたクレームに基づく、システムは、種類のクレームを抽出しよう*UniqueClaimTypeIdentifier*、対応する値が使用されますフィールドのトークンを生成するときに、ユーザーのユーザー名の代わりに 要求の種類が見つからない場合、システムには、要求は失敗します。 既定値は*null*、(id プロバイダー、名前識別子)、システムを使用することを示します組のユーザー名の代わりに前述のようです。 |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

*[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* 型により、開発者は各トークン内の追加データのラウンド トリップで ANTI-XSRF システムの動作を拡張します。 *GetAdditionalData*メソッドが呼び出されるフィールドのトークンが生成され、戻り値が生成されたトークン内に埋め込まれています。 実装するときは、このメソッドから、タイムスタンプ、nonce、またはユーザーがその他の値を返すことができます。

同様に、 *ValidateAdditionalData*メソッドが呼び出されるフィールドのトークンの検証し、トークン内に埋め込まれた「追加のデータ」文字列は、メソッドに渡されます。 検証ルーチン (トークンの作成時に格納されている時間に対して、現在の時刻をチェック) をタイムアウトを実装することが、必要なロジックのルーチン、またはその他のチェック nonce。

## <a name="design-decisions-and-security-considerations"></a>設計に関する決定事項とセキュリティに関する考慮事項

セッションとフィールドのトークンをリンクするセキュリティ トークンは技術的には必要な場合にのみ XSRF 攻撃からの匿名/認証されていないユーザーを保護しようとしています。 認証トークン (おそらくは cookie の形式で送信された) 自体が 1 つとしてされる可能性があります、ユーザーが認証されると、トークンのペアのシンクロナイザーの半分です。 ただし、ログイン ページが認証されていないユーザーは、ヒットを保護するための有効なシナリオがされ、ANTI-XSRF ロジックは、常に生成して、認証されたユーザーの場合でも、セキュリティ トークンを検証して単純なされていました。 いくつか追加の保護を設定またはセッション トークンを克服するために攻撃者の別のハードルとなる推測と、攻撃者によってフィールドのトークンに欠陥があること。

複数のアプリケーションが 1 つのドメインでホストされている場合は、開発者が注意を使用してください。 たとえば、場合でも*example1.cloudapp.net*と*example2.cloudapp.net*はさまざまなホストは、すべてのホスト間で暗黙的な信頼リレーションシップがある、  *\*. cloudapp.net*ドメイン。 この暗黙的な信頼関係[互いの cookie に影響を与える可能性のある信頼されていないホストできる](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks)(AJAX 要求を管理する同一オリジン ポリシーに必ずしも適用ない HTTP cookie)。 ASP.NET Web スタックのランタイムは、フィールドのトークンにユーザー名が埋め込まれているため、場合でも、悪意のあるサブドメインは、セッション トークンを上書きすることができなく、ユーザーの有効なフィールドのトークンを生成することで、いくつかの軽減策を提供します。 ただし、このような環境でホストされている場合、組み込みの ANTI-XSRF ルーチンもことはできません防御セッション ハイジャックまたは XSRF のログイン。

ANTI-XSRF ルーチンがに対する防御はいない現在[クリックジャッ キング](https://www.owasp.org/index.php/Clickjacking)します。 アプリケーション自体をクリックジャッ キングに対する防御が行うことができます簡単に、X のフレームのオプションを送信して: の各応答と共に SAMEORIGIN ヘッダー。 このヘッダーは、最近のすべてのブラウザーでサポートされます。 詳細については、次を参照してください。、 [IE ブログ](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx)、 [SDL ブログ](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx)、および[OWASP](https://www.owasp.org/index.php/Clickjacking)します。 いくつかの将来のリリースください、MVC、ASP.NET Web スタック ランタイム可能性があり、Web ページの ANTI-XSRF ヘルパーは、アプリケーションは、この攻撃に対しては自動的に保護できるように自動的にこのヘッダーを設定します。

Web 開発者は、そのサイトが XSS 攻撃に対して脆弱でないことを確認する続行する必要があります。 XSS 攻撃が非常に強力なと悪用、XSRF 攻撃からの ASP.NET Web スタック ランタイム防御機能でも改ページします。

## <a name="acknowledgment"></a>受信確認

[@LeviBroderick](https://twitter.com/LeviBroderick)、作成者、ASP.NET のセキュリティ コードの多くはこの情報の一括します。
