---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: "ASP.NET MVC、Web ページに XSRF/CSRF 防止 |Microsoft ドキュメント"
author: Rick-Anderson
description: "これにより、悪意のある web サイトに影響を与える、interacti web ホスト アプリケーションへの攻撃をクロスサイト リクエスト フォージェリ (XSRF または CSRF とも呼ばれます) には."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2013
ms.topic: article
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 4ff4ed20d0768a48f8afb2deeb7cdb6b4c60b5bc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>ASP.NET MVC、Web ページに XSRF/CSRF 防止
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> サイト間の要求が偽造 (XSRF または CSRF とも呼ばれます) は、悪意のある web サイトに影響を与える、クライアント ブラウザーおよびブラウザーによって信頼されている web サイト間の相互作用 web ホスト アプリケーションへの攻撃です。 Web ブラウザーが web サイトにすべての要求で自動的に認証トークンを送信するため、このような攻撃が可能になります。 標準的な例は、ASP など、認証 cookie です。NET のフォーム認証チケット。 ただし、(Windows 認証、Basic、およびなど) などの任意の永続的な認証メカニズムを使用する web サイトの場合は、これらの攻撃対象となることができます。
> 
> XSRF 攻撃がフィッシング攻撃とは異なります。 フィッシング攻撃では、対象ユーザーとの対話が必要です。 フィッシング攻撃では、悪意のある web サイトのターゲットの web サイト、うまくまねます、攻撃者に機密情報を提示するように、対象を間違うです。 XSRF 攻撃ではありません多くの場合、相互作用犠牲者から必要です。 代わりに、攻撃者は、移行先の web サイトに関連するすべての cookie を自動的に送信するブラウザーで証明書利用者です。
> 
> 詳細については、次を参照してください。、 [Web アプリケーションのセキュリティのプロジェクトを開く](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))です。


## <a name="anatomy-of-an-attack"></a>攻撃の構造

XSRF 攻撃を使用してすべての要素をユーザーが一部オンライン バンキングのトランザクションを実行するにを検討してください。 最初に、このユーザーは、この時点で、応答ヘッダーには、認証 cookie WoodgroveBank.com と、ログをアクセスします。

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

認証 cookie は、セッションの cookie であるため、これは自動的にクリアされますブラウザーによってブラウザー プロセスが終了したとき。 ただし、それまでは、ブラウザーが自動的に含まれる WoodgroveBank.com 要求ごとに cookie です。ユーザーは、彼女銀行サイト上のフォームに入力し、ブラウザーがサーバーにこの要求を行うために、別のアカウントに $1000 を転送するようになりましたがします。

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

この操作には、(通貨のトランザクションを開始) の副作用があるためバンキングのサイトがこの操作を開始するために HTTP POST を要求するように選択します。 サーバーは、要求から認証トークンを読み取りますは現在のユーザーのアカウント番号を検索、十分な資金が存在することを検証し、接続先のアカウントに、トランザクションを開始します。

オンライン バンキング完了彼女ユーザー バンキングのサイトから離れた場所に移動し、web 上の他の場所にアクセスします。 埋め込まれた ページで、次のマークアップには – fabrikam.com – これらのサイトのいずれかが含まれています、 &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

この要求にブラウザーが発生しています。

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

攻撃者のサーバーを利用している彼女は、ターゲット サイトに HTTP POST を自動的に作成するブラウザーに Javascript の小さなスニペットを使用していて、ユーザーは、ターゲット web サイトの有効な認証トークンを必要があります。 認証トークンがまだ有効では、銀行サイトが 250 ドルの攻撃者のアカウントへの転送を開始します。

### <a name="ineffective-mitigations"></a>非効率的な緩和策

上記のシナリオでは、という事実 WoodgroveBank.com SSL 経由でアクセスされていた SSL のみの認証クッキーができなかったこと、攻撃を阻止するために十分な興味深い勧めします。 攻撃者を指定すること、 [URI スキーム](http://en.wikipedia.org/wiki/URI_scheme)(https) 自分の&lt;フォーム&gt;要素、およびブラウザーは引き続きこれらの cookie は、URI と一致している限り、対象のサイトに期限が切れていない cookie を送信目的のターゲットのスキーム。

ユーザー必要があります単にいないを参照してください信頼されていないサイトを信頼済みサイトがオンラインで安全なままにしておくことだけを訪問として 1 つと言えるでしょう。 いくつかの情報源が残念ながらこのアドバイス常に実用的ではありません。 おそらく、ユーザー「信頼」ローカル ニュース サイト ConsolidatedMessenger します。 ConsolidatedMessenger.com、代わりに、サイトのアクセスがそのサイトに進み、攻撃者が fabrikam.com 上で実行されるコードの同じスニペットの挿入を使用できる XSS 脆弱性があります。

受信要求があることを確認することができます、 [Referer ヘッダー](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14)ドメインを参照します。 これにより、知らないうちにサード パーティ製のドメインから送信された要求は停止します。 ただし、一部のユーザーには、プライバシー保護のため、ブラウザーの Referer ヘッダーが無効にして、対象にインストールされている特定のセキュリティ保護されていないソフトウェアがある場合、攻撃者はそのヘッダーを偽装場合があることができます。 確認、 [Referer ヘッダー](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) XSRF 攻撃を防ぐための安全な方法とは見なされません。

## <a name="web-stack-runtime-xsrf-mitigations"></a>Web スタック ランタイム XSRF の緩和策

ASP.NET Web スタック ランタイムのバリアントを使用して、[シンクロナイザー トークン パターン](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern)XSRF 攻撃から防御するためにします。 シンクロナイザー トークン パターンの一般的な形式は次の 2 つの ANTI-XSRF トークンが (に加えて、認証トークン) は、各 HTTP POST を使用してサーバーに送信されたこと: として、cookie、フォームの値としてもう 1 つのトークン。 ASP.NET ランタイムによって生成されたトークンの値は、決定的であるか、攻撃者が予測可能ではありません。 トークンが送信されると、サーバーは両方のトークン比較チェックに合格する場合にのみ継続する要求を許可します。

XSRF 要求検証*セッション トークン*は HTTP クッキーとして格納され、現在そのペイロードに次の情報が含まれています。

- 128 ビットのランダムな識別子で構成されるセキュリティ トークンです。   
 次の図は、XSRF 要求検証セッション トークン Internet Explorer F12 開発者ツールで表示されます: (注これは、現在の実装は、件名、でも変更される可能性をします)。

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

*フィールド トークン*として格納されて、`<input type="hidden" />`そのペイロードに次の情報が含まれています。

- ログインしているユーザーのユーザー名 (認証された) 場合。
- その他のデータによって提供される、 [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)です。

ANTI-XSRF トークンのペイロードが暗号化および署名済み、ツールを使用して、トークンを調べるときに、ユーザー名を表示できないようにします。 提供される暗号サービスで web アプリケーションで ASP.NET 4.0 がターゲットとするときに、 [MachineKey.Encode](https://msdn.microsoft.com/en-us/library/system.web.security.machinekey.encode.aspx)ルーチンです。 Web アプリケーションが ASP.NET 4.5 を対象とするまたは高い、暗号サービスは、によって提供される場合、 [MachineKey.Protect](https://msdn.microsoft.com/en-us/library/system.web.security.machinekey.protect(v=vs.110))ルーチンより優れたパフォーマンス、拡張性、およびセキュリティを提供します。 詳細については、次のブログの投稿を参照してください。

- [ASP.NET 4.5 での暗号化の機能強化、pt です。1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [ASP.NET 4.5 での暗号化の機能強化、pt です。2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [ASP.NET 4.5 での暗号化の機能強化、pt です。3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>トークンを生成します。

ANTI-XSRF トークンを生成するには、呼び出し、 [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/en-us/library/dd470175.aspx) MVC ビューからのメソッドまたは@AntiForgery.GetHtmlRazor ページから ()。 ランタイムは、次の手順を実行し、されます。

1. 現在の HTTP 要求には既に ANTI-XSRF セッション トークンが含まれている場合 (ANTI-XSRF cookie \_ \_RequestVerificationToken)、これからセキュリティ トークンを抽出します。 HTTP 要求に ANTI-XSRF セッション トークンが含まれていない場合、またはセキュリティ トークンの抽出が失敗した場合は、新しいランダムな ANTI-XSRF トークンが生成されます。
2. 上記の手順 (1) と現在のログイン ユーザーの id からセキュリティ トークンを使用して ANTI-XSRF フィールド トークンが生成されます。 (ユーザー id を確認する方法については、次を参照してください、 **[特別なサポートを使用するシナリオ](#_Scenarios_with_special)**以下のセクションです。)。さらに場合、 [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/jj158328(v=vs.111).aspx)が構成されている場合、ランタイムが呼び出す、 [GetAdditionalData](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx)メソッド フィールド トークンに返される文字列を含めるとします。 (を参照してください、 **[構成および機能拡張](#_Configuration_and_extensibility)**詳細についてはします)。
3. 新しい ANTI-XSRF トークン生成した場合 (1) の手順で、新しいセッション トークンは内包するが作成され、送信 HTTP クッキーのコレクションに追加されます。 手順 (2) のフィールドのトークンにラップされます、`<input type="hidden" />`要素、および HTML マークアップをこの値となる、戻り値の`Html.AntiForgeryToken()`または`AntiForgery.GetHtml()`です。

## <a name="validating-the-tokens"></a>トークンを検証します。

入力方向の ANTI-XSRF トークンを検証するには、開発者が含まれています、 [ValidateAntiForgeryToken](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx)彼女 MVC アクションまたはコント ローラー、または彼女の呼び出しの属性を`@AntiForgery.Validate()`自分の Razor ページから。 ランタイムでは、次の手順を実行します。

1. フィールド トークンが、受信セッション トークンが読み取られ、それぞれから抽出された ANTI-XSRF トークンです。 ANTI-XSRF トークンは、各手順 (2) の生成のルーチンで同一でなければなりません。
2. 現在のユーザーが認証されている場合、自分のユーザー名はフィールドのトークンに格納されているユーザー名と比較されます。 ユーザー名が一致する必要があります。
3. 場合、 [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)が構成されているランタイムの呼び出し、 *ValidateAdditionalData*メソッドです。 メソッドは、ブール値を返す必要があります*true*です。

検証が成功した場合、要求の続行が許可されます。 検証に失敗した場合、フレームワーク、 *HttpAntiForgeryException*です。

## <a name="failure-conditions"></a>エラー条件

以降、ASP.NET Web スタック ランタイム v2 で任意*HttpAntiForgeryException*中にスローされる検証が失敗の原因に関する詳細情報が含まれています。 現在定義されているエラー状態は次のとおりです。

- セッション トークンまたはフォームのトークンが要求内に存在ではありません。
- セッション トークンまたはフォームのトークンは、読み取り可能ではありません。 これの最も一般的な原因は、ASP.NET Web スタック ランタイムまたはファームのバージョンに不一致が実行されているファーム場所、 &lt;machineKey&gt; Web.config 内の要素がマシン間で異なります。 Fiddler などのツールを使用すると、いずれかの ANTI-XSRF トークンを改ざんによってこの例外を強制します。
- セッション トークンとフィールド トークンがスワップされました。
- セッション トークンとフィールドのトークンには、一致していないセキュリティ トークンが含まれます。
- フィールドのトークンに埋め込まれたユーザー名では、現在のログインのユーザーのユーザー名が一致しません。
-  *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* メソッドが返される*false*です。

ANTI-XSRF 設備もトークンの生成または検証中に追加のチェックを実行して、例外がスロー、これらのチェック中にエラー。 参照してください、 [WIF/ACS/クレーム ベース認証](#_WIF_ACS)と**[構成および機能拡張](#_Configuration_and_extensibility)**詳細については、セクションです。

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>特別なサポートを使用するシナリオ

### <a name="anonymous-authentication"></a>匿名認証

ANTI-XSRF システムには、「匿名」が定義されているユーザーとして、匿名ユーザー用の特別なサポートが含まれています。 ここで、 *IIdentity.IsAuthenticated*プロパティから返される*false*です。 (前に、ユーザーが認証される) のログイン ページと、アプリケーションで使用されているメカニズム以外のカスタム認証方式に XSRF の保護を提供するようなシナリオ*IIdentity*ユーザーを識別します。

これらのシナリオをサポートするには、セッションとフィールドのトークンは、128 ビット ランダムに生成された非透過識別子であるセキュリティ トークンが参加していることを思い出してください。 このセキュリティ トークンは、彼女は匿名 id の目的を効果的に機能するために、サイトが移動したときに、個々 のユーザーのセッションを追跡するために使用されます。 空の文字列は、上記の生成と検証ルーチンにユーザー名の代わりに使用されます。

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF/ACS/クレーム ベース認証

通常、 *IIdentity* 、.NET Framework に組み込まれているクラス プロパティが設定されている*IIdentity.Name*は特定のアプリケーション内で特定のユーザーを一意に識別するための十分なです。 たとえば、 *FormsIdentity.Name* (これは、そのデータベースに応じて、すべてのアプリケーションに対して一意では)、メンバーシップ データベースに格納されているユーザー名を返します*WindowsIdentity.Name*を返します、ユーザー、およびなどのドメイン修飾 id です。 これらのシステムだけでなく認証を提供します。これらも*識別*をアプリケーションにユーザー。

クレーム ベースの認証では、その一方は必ずしも必要ありません、特定のユーザーを識別します。 代わりに、 *ClaimsPrincipal*と*ClaimsIdentity*型のセットに割り当てられた*クレーム*インスタンス、個別の要求が「は 18 + 歳以上」をする可能性がありますか"管理者"を何でもです。 ユーザーを識別されているとは限りませんしていないため、ランタイムは使用できません、 *ClaimsIdentity.Name*プロパティを特定のユーザーの一意の識別子として。 チームが実際の例を表示、 *ClaimsIdentity.Name*返します*null*フレンドリ (表示) 名を返しますまたは、が一意の識別子として使用するために適切でない文字列を返しますそれ以外の場合、。ユーザー。

クレーム ベース認証を使用する展開の多くを使用している[Azure Access Control Service](https://msdn.microsoft.com/en-us/library/windowsazure/gg429786.aspx) (ACS) に特にです。 ACS では、個々 の開発者は構成がによって*id プロバイダー* (ADFS、Microsoft アカウント プロバイダーなど OpenID プロバイダーなどの yahoo! など)、id プロバイダーを返すと*の識別子の名前*. これらの名前識別子は、電子メール アドレスのように Personally Identifiable Information (PII) を含めることは、または匿名化された Private Personal Identifier (PPID) のような可能性があります。 関係なく、(id プロバイダー、名前の識別子) 組十分には機能し、特定のユーザーの適切な監視トークンとして生成するときに、ASP.NET Web スタック ランタイムは、ユーザー名の代わりに、組を使用できるように、サイトを閲覧彼女とANTI-XSRF フィールド トークンを検証しています。 Id プロバイダーと名前の識別子の特定の Uri は次のとおりです。

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(この[ACS doc ページ](https://msdn.microsoft.com/en-us/library/windowsazure/gg185971.aspx)詳細についてはします)。

生成するか、トークンを検証する、ASP.NET Web スタック ランタイムは実行時に再試行してください型にバインド。

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35`(用、WIF SDK。)
- `System.Security.Claims.ClaimsIdentity`(For .NET 4.5)。

これらの型が存在する場合、現在のユーザーの*IIIIdentity*これらのいずれかの型を実装クラスまたはサブクラス、ANTI-XSRF 機能は、(id プロバイダー、名前の識別子) を使用する組を生成するときに、ユーザー名の代わりにし、トークンを検証しています。 このような組が存在しない場合、要求は開発者に、使用中、特定のクレーム ベースの認証メカニズムを理解する ANTI-XSRF システムを構成する方法を説明するエラーで失敗します。 参照してください、 **[構成および機能拡張](#_Configuration_and_extensibility)**詳細についてはします。

### <a name="oauth--openid-authentication"></a>OAuth または OpenID 認証

最後に、ANTI-XSRF 機能は、OAuth または OpenID 認証を使用するアプリケーションにとって特別なサポートがします。 このサポートは、ヒューリスティックに基づく: 場合、現在*IIdentity.Name*はユーザー名の比較を実行し、http:// または https://で始まる既定 OrdinalIgnoreCase の比較子ではなく序数の比較子を使用します。

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>構成および機能拡張

場合によっては、開発者は、厳密な制御、ANTI-XSRF 生成と検証の動作をする可能性があります。 たとえば、応答に HTTP クッキーを自動的に追加の Web ページと MVC ヘルパーの既定の動作が望ましく、おそらくと、開発者がトークンを他の場所を保持することもします。 この作業に役立つ 2 つの Api が存在します。

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

*GetTokens*既存 XSRF 要求検証セッションのトークン (を null にすることがあります) を入力としてのメソッドの受け取りと新しい XSRF 要求検証セッションのトークンとフィールドのトークンを出力として生成します。 トークンは、単に不透明な文字列; の装飾なしの*formToken*値のインスタンスは折り返されません内、&lt;入力&gt;タグ。 *NewCookieToken*値 null も指定できます。 これが発生した場合、 *oldCookieToken*値がまだ有効では、新しい応答 cookie を設定する必要はありません。 呼び出し元*GetTokens* 、必要な応答のクッキーを永続化または; 必要なマークアップを生成する役割が、 *GetTokens*メソッド自体は、副作用として、応答を変更しません。 *検証*メソッドは、受信セッション トークンとフィールド上に、ここに挙げた検証ロジックを実行します。

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

開発者は、アプリケーションから ANTI-XSRF システムを構成することがあります\_を開始します。 構成はプログラムです。 静的プロパティ*AntiForgeryConfig*型について以下に説明します。 要求を使用するほとんどのユーザーを UniqueClaimTypeIdentifier プロパティを設定します。

| **Property** | **説明** |
| --- | --- |
| **AdditionalDataProvider** | [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)をトークンの生成中に追加のデータを提供し、トークンの検証中に追加のデータを使用します。 既定値は*null*です。 詳細については、次を参照してください。、 [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)セクションです。 |
| **CookieName** | HTTP クッキーの名前を提供する文字列を使用して、セッションの ANTI-XSRF トークンを格納します。 この値が設定されていない場合、名前が自動的に生成されます、アプリケーションの展開された仮想パスに基づきます。 既定値は*null*です。 |
| **RequireSsl** | ANTI-XSRF トークンは、SSL で保護されたチャネル経由で送信するために必要かどうかを決定するブール値。 この値が場合*true*、自動的に生成された cookie は、「セキュリティで保護された」フラグを設定すると、持ち ANTI-XSRF Api が SSL を使用していない送信される要求から呼び出された場合にスローされます。 既定値は *false* です。 |
| **SuppressIdentityHeuristicChecks** | ANTI-XSRF システムにクレーム ベース id のサポートを非アクティブ化するかどうかを決定するブール値。 この値が場合*true*、システムと見なされます*IIdentity.Name*ユーザーごとの一意の識別子として使用するための適切なは、特別なケースは試みません*IClaimsIdentity*または*ClClaimsIdentity* 」の説明に従って、 [WIF/ACS/クレーム ベース認証](#_WIF_ACS)セクションです。 既定値は `false` です。 |
| **UniqueClaimTypeIdentifier** | どの要求の種類を示す文字列は、ユーザーごとに一意の識別子として使用するための適切なです。 この値が設定され、現在場合*IIdentity*で指定されたクレームに基づく、システムは、種類のクレームを抽出しよう*UniqueClaimTypeIdentifier*、され、対応する値が使用されますフィールドのトークンを生成するときにユーザーのユーザー名の代わりに 要求の種類が見つからない場合、システムには、要求は失敗します。 既定値は*null*、(id プロバイダー、名前の識別子)、システムを使用することを示します組、ユーザーのユーザー名の代わりに、上記のとおりです。 |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

 *[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* 型により、各トークン内の追加データのラウンド トリップで ANTI-XSRF システムの動作を拡張する開発者です。 *GetAdditionalData*メソッドが呼び出された各フィールド トークンが生成され、戻り値が生成されたトークンに含まれています。 実装者は、このメソッドから、タイムスタンプ、nonce、またはユーザーがその他の値を返す可能性があります。

同様に、 *ValidateAdditionalData*メソッドが呼び出された各フィールド トークンが検証され、トークン内に埋め込まれた「その他のデータ」文字列は、メソッドに渡されます。 検証ルーチンは、(トークンの作成時に格納されている時間に対して現在の時刻を確認しています) でのタイムアウトを実装する可能性があります、nonce ルーチン、またはその他のチェックに必要なロジック。

## <a name="design-decisions-and-security-considerations"></a>設計に関する決定事項とセキュリティの考慮事項

セッションとフィールドのトークンをリンクするセキュリティ トークンは技術的には必要な場合にのみ XSRF 攻撃からの匿名/未認証のユーザーを保護しようとしています。 1 つとして、ユーザーが認証されると、(おそらくは cookie の形式で送信された) 自体の認証トークンを使用できる、シンクロナイザーの半分のトークンの組。 ただし、ヒット未認証のユーザーがログイン ページを保護するための有効なシナリオはされ、ANTI-XSRF ロジックは常に生成し、認証されたユーザーについても、セキュリティ トークンを検証して単純なされていました。 またはいくつか追加の保護を設定またはセッション トークンを別の障壁を克服する攻撃者となる推測と、攻撃者によってフィールド トークンが侵害されたことを。

複数のアプリケーションが 1 つのドメインでホストされている場合は、開発者が注意を使用してください。 たとえば、にもかかわらず*example1.cloudapp.net*と*example2.cloudapp.net*別のホストは、下にあるすべてのホスト間で暗黙的な信頼関係がある、  *\*. cloudapp.net*ドメイン。 この暗黙的な信頼関係[により、互いの cookie に影響を与える可能性のある信頼されていないホスト](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks)(同じオリジンのポリシー AJAX 要求を必ずしも適用されません HTTP クッキーに)。 ASP.NET Web スタック ランタイムは、フィールド トークンにユーザー名が埋め込まれている悪意のあるサブドメインがセッション トークンを上書きできない場合でもそのことはできません、ユーザーの有効なフィールド トークンを生成するために、いくつかの対策を提供します。 ただし、このような環境でホストされている場合、組み込みの ANTI-XSRF ルーチンまだことはできません防御セッションの乗っ取りまたは XSRF のログイン。

ANTI-XSRF ルーチン現在はいない防御[clickjacking](https://www.owasp.org/index.php/Clickjacking)です。 Clickjacking 自体防御するアプリケーションが簡単に行う、X のフレームのオプションを送信することによって: SAMEORIGIN を持つ各応答ヘッダー。 このヘッダーは、すべての最近使用したブラウザーでサポートされています。 詳細については、次を参照してください。、 [IE ブログ](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx)、 [SDL ブログ](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx)、および[OWASP](https://www.owasp.org/index.php/Clickjacking)です。 将来のリリース、MVC、ASP.NET Web スタック ランタイム可能性があり、Web ページ ANTI-XSRF ヘルパーは、アプリケーションは自動的にこの攻撃から保護できるように自動的にこのヘッダーを設定します。

Web 開発者は、サイトがない XSS 攻撃に対して脆弱であることを確認する続行する必要があります。 XSS 攻撃は非常に強力でありが成功するには、XSRF 攻撃からの ASP.NET Web スタック ランタイム防御とも改ページします。

## <a name="acknowledgment"></a>受信確認

[@LeviBroderick](https://twitter.com/LeviBroderick)、作成者、ASP.NET セキュリティ コードの多くのこの情報の一括です。
