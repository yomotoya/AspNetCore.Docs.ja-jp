---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
title: フォーム認証の構成と高度なトピック (VB) |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、さまざまなフォーム認証設定を調べてをフォーム要素を使用して変更する方法を参照してください。 詳細なを伴うこのされます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 829d2f56-5c48-445b-b826-3418a450c788
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
msc.type: authoredcontent
ms.openlocfilehash: c6ef046100cf4773da57f6693a88e9bc6ec1790f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891722"
---
<a name="forms-authentication-configuration-and-advanced-topics-vb"></a>フォーム認証の構成と高度なトピック (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_vb.pdf)

> このチュートリアルでは、さまざまなフォーム認証設定を調べてをフォーム要素を使用して変更する方法を参照してください。 (SignIn.aspx Login.aspx ではなく) のようにカスタム URL と cookie なしのフォーム認証チケットのログイン ページを使用して、フォーム認証チケットのタイムアウト値のカスタマイズの詳細についてを伴うこのされます。


## <a name="introduction"></a>はじめに

[前のチュートリアル](an-overview-of-forms-authentication-vb.md)Web.config で別のページを表示するのにログを作成する構成設定を指定することから、ASP.NET アプリケーションでフォーム認証を実装するために必要な手順について説明しました認証および匿名ユーザーのコンテンツです。 私たちの mode 属性を設定してフォーム認証を使用する web サイトが構成されていることに注意してください、&lt;認証&gt;フォームへの要素。 &lt;認証&gt;要素には、必要に応じて、&lt;フォーム&gt;子要素を使用するさまざまなフォーム認証設定を指定する可能性があります。

このチュートリアルでは、さまざまなフォーム認証設定を調べてを使用して変更する方法を参照してください、&lt;フォーム&gt;要素。 (SignIn.aspx Login.aspx ではなく) のようにカスタム URL と cookie なしのフォーム認証チケットのログイン ページを使用して、フォーム認証チケットのタイムアウト値のカスタマイズの詳細についてを伴うこのされます。 より密接にフォーム認証チケットの構成の検査もし ASP.NET がチケットのデータが検査や改ざんからセキュリティで保護されたことを確認するには、予防措置を参照してください。 最後に、フォーム認証チケットに余分なユーザー データを格納する方法とカスタム プリンシパル オブジェクトを使用してこのデータをモデル化する方法を紹介します。

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>手順 1: チェック、&lt;フォーム&gt;構成設定

Asp.net フォーム認証システムでは、多数のアプリケーション間ごとにカスタマイズ可能な構成設定を提供します。 などの設定: チケットのフォーム認証の有効期間です。チケット; にどのような保護を適用します。どのような条件 cookie なしの認証チケットが使用されます。ログイン ページへのパスその他の情報です。 既定の値を変更するには追加、 [&lt;フォーム&gt;要素](https://msdn.microsoft.com/library/1d3t3c61.aspx)の子として、 [&lt;認証&gt;要素](https://msdn.microsoft.com/library/532aee0e.aspx)、これらのプロパティを指定します。値を XML 属性としてをカスタマイズする次のよう。

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample1.xml)]

表 1 でカスタマイズ可能なプロパティをまとめたもの、&lt;フォーム&gt;要素。 Web.config は XML ファイルなので、左の列に属性名は大文字小文字を区別します。


| <strong>属性</strong> |                                                                                                                                                                                                                                     <strong>説明</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         cookie なし         |                                                                                                                この属性は、URL に埋め込まれている中ではなく、cookie にどのような条件下で、認証チケットが格納されているを指定します。 使用可能な値: 既定値です。UseUri です。自動検出します。および UseDeviceProfile (既定)。 手順 2 では、さらに詳しくは、この設定について説明します。                                                                                                                |
|         defaultUrl         |                                                                                                                                                         ユーザーは、クエリ文字列で指定された RedirectUrl 値が存在しない場合、ログイン ページからサインインした後にリダイレクトされる URL を示します。 既定値は、default.aspx です。                                                                                                                                                         |
|           ドメイン           | Cookie ベースの認証チケットを使用する場合、この設定は、cookie のドメインの値を指定します。 既定値は、ブラウザー (www.yourdomain.com) などから発行されたドメインを使用すると、空の文字列です。 この場合、cookie は<strong>されません</strong>admin.yourdomain.com などのサブドメインに要求を作成する場合に送信されます。すべてのサブドメインに渡される cookie をする場合は、ユーザーに設定すると、ドメイン属性をカスタマイズする必要があります。 |
|  enableCrossAppRedirects   |                                                                                                                                                                   同じサーバー上の他の web アプリケーションの Url にリダイレクトされたときに認証されたユーザーを記憶するかどうかを示すブール値。 既定値は false です。                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      ログイン ページの URL です。 既定値は login.aspx です。                                                                                                                                                                                                                      |
|            name            |                                                                                                                                                                                                   ときに、cookie ベースの認証チケット、cookie の名前を使用します。 既定値です。ASPXAUTH です。                                                                                                                                                                                                   |
|            path            |                                                                             Cookie ベースの認証チケットを使用する場合、この設定は、cookie s path 属性を指定します。 Path 属性は、開発者は、特定のディレクトリ階層にクッキーのスコープを制限できます。 既定値は、/、ドメインに加えられたすべての要求に認証チケットを送信するブラウザーに通知します。                                                                              |
|         保護         |                                                                                                                                            どのような手法を使用してフォーム認証チケットを保護することを示します。 使用可能な値: すべて (既定)。暗号化です。[なし] です。および検証します。 これらの設定は、手順 3. で詳しく説明します。                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                認証 cookie の送信に SSL 接続が必要かどうかを示すブール値。 既定値は false です。                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 ユーザーが 1 つのセッション中にサイトを訪問認証 cookie のタイムアウトはたびにリセットするかどうかを示すブール値。 既定値は true です。 認証チケットのタイムアウト ポリシーが、指定することでより詳しく説明されているチケットのタイムアウト値のセクションでします。                                                                                                 |
|          タイムアウト           |                                                                                                                               認証チケットの有効期限が切れるまでの分単位で時間を指定します。 既定値は 30 です。 認証チケットのタイムアウト ポリシーが、指定することでより詳しく説明されているチケットのタイムアウト値のセクションでします。                                                                                                                               |

**表 1**: の A の概要、&lt;フォーム&gt;要素の属性

ASP.NET 2.0 以降で、既定値は、フォーム認証の値が FormsAuthenticationConfiguration クラス、.NET Framework でハードコーディングします。 変更は、Web.config ファイルで、アプリケーションごとを適用する必要があります。 これは ASP.NET 異なります 1.x では、フォーム認証の既定値、machine.config ファイルに格納されていた (、machine.config を編集で変更できませんでした。 そのため)。 ASP.NET のトピックに関する while 1.x では、する意義があることを伝えますフォーム認証のシステム設定の数がある ASP.NET 2.0 では、異なる既定値と ASP.NET のよりもそれを上回る 1.x です。 ASP.NET 1.x 環境から、アプリケーションを移行する場合は、これらの相違点に注意する必要があります。 参照してください[、&lt;フォーム&gt;要素のテクニカル ドキュメント](https://msdn.microsoft.com/library/1d3t3c61.aspx)相違点の一覧についてはします。

> [!NOTE]
> タイムアウト、ドメイン、および、パスなど、いくつかのフォーム認証設定は、結果として得られるフォーム認証 cookie チケットの詳細を指定します。 Cookie、しくみ、およびそのさまざまなプロパティの詳細については、「[この Cookie チュートリアル](http://www.quirksmode.org/js/cookies.html)です。


### <a name="specifying-the-tickets-timeout-value"></a>チケットのタイムアウト値を指定します。

フォーム認証チケットは、id を表すトークンです。 Cookie ベースの認証チケットのこのトークンは cookie の形式で保持されているし、要求ごとに web サーバーに送信します。 トークンを所有しているが基本的には、宣言、私は*ユーザー名*、ログインしている、してページ訪問間で記憶できるユーザーの id を使用します。

フォーム認証チケットは、ユーザーの id が含まれていますだけでなく、整合性とトークンのセキュリティを確保する情報も含まれます。 結局のところ、悪意のユーザーが偽造のトークンを作成または underhanded 何らかの正当なトークンを変更するかどうかをできるしたくありません。

チケットに含まれる情報のような 1 つのビットは、*有効期限*、これは、チケットが無効になったときの日時です。 FormsAuthenticationModule は、認証チケットを検査するたびに、チケットの有効期限がまだ達していないことを保証します。 場合は、そのチケットを無視し、匿名としてユーザーを識別します。 この防策は、リプレイ攻撃から保護するのに役立ちます。 場合は、ハッカーが自分のコンピューターに物理的にアクセスし、cookie をルートおそらく自分の手でユーザーの有効な認証チケット - を取得できません - この盗難にあった認証チケットを使用してサーバーに要求を送信する可能性があります、有効期限なしとエントリを取得します。 有効期限は、このシナリオを防止できない、中に、ウィンドウがこのような攻撃が成功するを制限することをはします。

> [!NOTE]
> 手順 3. 詳細その他の手法、フォーム認証システムで認証チケットを保護するために使用します。


認証チケットを作成するときに、フォーム認証システムは、タイムアウトの設定を参照してその有効期限を決定します。 表 1、タイムアウトを 30 分は、既定値の設定に示すようには、フォーム認証チケットの作成時に、その有効期限が日付と時間 30 分、将来に設定されていることを意味します。

有効期限が切れる絶対時刻後で、フォーム認証チケットの有効期限が切れるを定義します。 通常開発者が、スライド式有効期限、たびにユーザーを見直して、サイトをリセットする 1 つを実装します。 この動作は、slidingExpiration 設定によって決定されます。 かどうかを true に設定 (既定)、FormsAuthenticationModule ユーザーを認証するたびにその更新チケットの有効期限。 有効期限が切れる false に設定が要求のたびに更新しないと、原因となり、過去の場合、チケットが最初の分のタイムアウト数正確に有効期限が切れるチケット作成されます。

> [!NOTE]
> 認証チケットに格納されている有効期限は、絶対日付と 2008 年 8 月 2 日午前 11時 34分と同様に、時刻の値です。 さらに、日付と時刻は、web サーバーのローカル時刻です。 この設計の決定には、いくつか興味深い副作用の周囲夏時間 (DST)、米国の州のクロックを移動先 (web サーバーが夏時間の期間が計測されたロケールでホストされていると仮定) 1 時間であることができます。 夏時間の開始時刻の近くの 30 分間の有効期限と ASP.NET web サイトに何が起こるかを検討してください (これは午前 2 時に)。 訪問者にサインオンするサイト、2008 年 3 月 11 日の午前 1時 55分想像してください。 これにより、(30 分後で) の午前 2 時 25 分に、2008 年 3 月 11 日で期限切れになるフォーム認証チケットが生成されます。 ただし、2時 00分 AM をロールの周り、時計ジャンプ 3時 00分 am DST によりします。 ユーザーには、(3時 01分 AM) でサインインした後に 6 分間に新しいページが読み込まれると、FormsAuthenticationModule ノート チケットが有効期限が切れたし、ユーザーをログイン ページにリダイレクトされています。 この他の認証チケットのタイムアウト奇妙な動作と回避策の詳細については、Stefan Schackow のコピーを取得します。 *Professional ASP.NET 2.0 のセキュリティ、メンバーシップ、およびロール管理*(isbn を保存します。978-0-7645-9698-8)。


図 1 は、slidingExpiration が false に設定されているし、タイムアウトが 30 に設定されているときに、ワークフローを示します。 ログイン時に生成された認証チケットには有効期限の日付が含まれています、以降の要求でこの値は更新されないことに注意してください。 チケットの有効期限が切れている、FormsAuthenticationModule が見つかると、それを破棄し、匿名として、要求を処理します。


[![フォーム認証チケットの有効期限と slidingExpiration のグラフィック表示が false](forms-authentication-configuration-and-advanced-topics-vb/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image1.png)

**図 01**: フォーム認証チケットの有効期限と slidingExpiration の A のグラフィック表示が false ([フルサイズのイメージを表示するをクリックして](forms-authentication-configuration-and-advanced-topics-vb/_static/image3.png))


図 2 は、slidingExpiration に設定されているときにワークフローを示しています true とタイムアウトが 30 に設定します。 (期限切れでないチケット) を使用して認証された要求が受信したときに、その有効期限が今後分のタイムアウト数に更新されます。


[![フォーム認証のチケットのグラフィック表示 slidingExpiration が true の場合](forms-authentication-configuration-and-advanced-topics-vb/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image4.png)

**図 02**: フォーム認証のチケットのグラフィカル表現を A slidingExpiration が true の場合 ([フルサイズのイメージを表示するをクリックして](forms-authentication-configuration-and-advanced-topics-vb/_static/image6.png))


Cookie ベースの認証チケット (既定値) を使用して、ここでは cookie が指定された、独自の切れを持つこともあるため、もう少し複雑になる場合になります。 Cookie の有効期限 (または欠如) cookie を破棄する必要があるときに、ブラウザーに指示します。 Cookie では、有効期限がない、ブラウザーが終了したときに破棄されます。 有効期限がある場合、ただし、cookie が日まで、ユーザーのコンピューターに保存されているし、有効期限で指定された時間が経過します。 ブラウザーで cookie が破棄されると、不要になった web サーバーに送信されます。 そのため、cookie の破棄は、ユーザーのサイトからのログインに似ています。

> [!NOTE]
> もちろん、ユーザーは、自分のコンピューターに格納されているすべての cookie を能動的に削除できます。 Internet Explorer 7 はツール、オプション に移動し、閲覧履歴のセクションで 削除 ボタンをクリックします。 そこから、cookie の削除 ボタンをクリックします。


フォーム認証システムに渡された値によってセッションまたは期限切れの cookie を作成する、 *persistCookie*パラメーター。 FormsAuthentication クラスの GetAuthCookie、SetAuthCookie、および RedirectFromLoginPage メソッドを 2 つの入力パラメーターを思い出して: *username*と*persistCookie*です。 前のチュートリアルで作成したログイン ページには、次回のチェック ボックス、永続的な cookie が作成されたかどうかを決定が含まれています。 永続的 cookie は、有効期限ベースです。非永続的 cookie は、セッション ベースです。

既に説明したタイムアウトおよび slidingExpiration の概念は、同じを両方のセッションおよび有効期限に基づく cookie に適用されます。 実行中の 1 つだけ小さな相違点がある: と slidingTimeout true に設定する cookie の有効期限ベースを使用して、cookie の有効期限がのみ更新と複数の指定した時間の半分が経過しました。

みましょうチケットのスライド式有効期限を使用して 1 時間 (60 分) 後にタイムアウトを実行するため、web サイトの認証チケットのタイムアウトのポリシーを更新します。 この変更の影響を与える、Web.config ファイルの更新の追加、&lt;フォーム&gt;要素を&lt;認証&gt;次のマークアップ要素。

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Login.aspx 以外のログイン ページの URL を使用します。

FormsAuthenticationModule は、ログイン ページに自動的に承認されていないユーザーをリダイレクトするため、ログイン ページの URL を知っている必要があります。 この URL が、loginUrl 属性で指定された、&lt;フォーム&gt;要素と既定値は login.aspx です。 既存の web サイト経由で移植する場合は、いずれかのブックマークが付けられたおよび検索エンジンによってインデックス設定された既に別の URL でのログイン ページを既にがあります。 Login.aspx に、既存のログイン ページの名前変更、リンクやユーザーのブックマークを重大なではなく代わりに、ログイン ページを指す loginUrl 属性を変更できます。

たとえば、ログイン ページ SignIn.aspx をという名前が、ユーザーのディレクトリ内に存在する場合は、~/Users/SignIn.aspx loginUrl 構成設定をポイントでした次のようにします。

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample3.xml)]

カスタム値を指定する必要はありません、現在のアプリケーションは既には Login.aspx をという名前のログイン ページがあるので、&lt;フォーム&gt;要素。

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>手順 2: cookie なしのフォーム認証チケットの使用

フォーム認証システムでは、既定では、cookie のコレクションでその認証チケットを保管するためのサイトにアクセスするユーザー エージェントに基づく URL に埋め込むかどうかを判断します。 すべての主要デスクトップ ブラウザーなどの Internet Explorer、Firefox、Opera、および Safari、サポート cookie がないすべてのモバイル デバイスの操作を行います。

フォーム認証システムによって使用される cookie のポリシー設定によって異なります、cookie なしで、&lt;フォーム&gt;要素は、次の 4 つの値の 1 つ割り当てることができます。

- 既定値には、クッキー ベースの認証チケットを常に使用することを指定します。
- UseUri - は、クッキー ベースの認証チケットが使用されないことを示します。
- 自動検出 - デバイスのプロファイルは、クッキー ベースの認証チケットの cookie をサポートしていない場合は使用されません。デバイスのプロファイルが cookie をサポートする cookie が有効になっているかどうかを判断するプローブのメカニズムが使用されます。
- UseDeviceProfile - 既定値です。デバイスのプロファイルが cookie をサポートしている場合にのみ、クッキー ベースの認証チケットを使用します。 プローブのメカニズムは使用されません。

自動検出と値の設定に依存、*デバイス プロファイル*cookie ベースまたは cookie なしの認証チケットを使用するかどうかによってにします。 ASP.NET では、さまざまなデバイスとその機能、cookie をサポートしているかどうか、サポートしている JavaScript のバージョンなどのデータベースを保持します。 たびにデバイスがに沿って送信 web サーバーから web ページを要求、*ユーザー エージェント*HTTP ヘッダーをデバイスの種類を識別します。 ASP.NET では、そのデータベースで指定された、対応するこのプロファイルに指定されたユーザー エージェント文字列自動的に一致します。

> [!NOTE]
> 準拠する XML ファイルの数にこのデバイスの機能のデータベースが格納されている、[ブラウザー定義ファイルのスキーマ](https://msdn.microsoft.com/library/ms228122.aspx)です。 既定のデバイス プロファイル ファイルは、%windir%\microsoft.net\framework\v2.0.50727\config\browsers に配置されます。 アプリケーションのアプリにカスタム ファイルを追加することも\_ブラウザー フォルダーです。 詳細については、次を参照してください。 [How To: ブラウザーの種類を検出する ASP.NET Web Pages で](https://msdn.microsoft.com/library/3yekbd5b.aspx)です。


既定の設定は値であるため、プロファイルを持つレポート cookie をサポートしないデバイスでアクセスすると、このサイト cookie なしのフォーム認証チケットが使用されます。

### <a name="encoding-the-authentication-ticket-in-the-url"></a>URL の認証チケットのエンコード

Cookie が変わっても訪問先デバイスには、それらがサポートされている場合、既定のフォーム認証設定が cookie を使用して特定の web サイトへの各要求に、ブラウザーからの情報を含めるための自然なメディアです。 Cookie がサポートされていない場合は、クライアントからサーバーに認証チケットを渡すための別の方法が採用されている必要があります。 一般的な問題を回避する cookie なしの環境で使用される cookie のデータを URL でエンコードを開始します。

URL 内でこのような情報を埋め込むことがどのように表示する最善の方法では、サイトが cookie なしの認証チケットの使用を強制します。 これは、UseUri に cookie なしの構成設定を設定して実行できます。

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample4.xml)]

この変更を行った後は、ブラウザーを使用してサイトを参照してください。 匿名ユーザーとしてへのアクセス、Url は以前と同じようになります。 たとえば、Default.aspx ページにアクセスしたとき、ブラウザーのアドレス バーは、次の URL を示しています。

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

ただし、ログイン時に、フォーム認証チケットは、URL に埋め込まれます。 たとえば、ログイン ページにアクセスし、Sam としてログオンすること後に、、Default.aspx ページに戻ることは URL 現時点では。

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

URL 内で、フォーム認証チケットが埋め込まれています。 文字列 (F (jaIOIDTJxIr12xYS VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) cookie に通常格納されている同じデータを 16 進でエンコードされた認証チケット情報を表します。

Cookie なしの認証チケット動作するためには、システムは認証チケット データを含める ページですべての Url をエンコードする必要があります、それ以外の場合、認証チケットは、ユーザーがリンクをクリックすると失われます。 さいわい、埋め込みこのロジックは自動的に実行します。 この機能を示すためには、Default.aspx ページを開き、テキストと NavigateUrl プロパティをテスト用のリンクと SomePage.aspx に設定すると、それぞれ、ハイパーリンク コントロールを追加します。 本当にしていないこと、ページ SomePage.aspx をという名前のプロジェクトには関係ありません。

Default.aspx に変更を保存し、ブラウザーを使用しを参照してください。 フォーム認証チケットが URL に埋め込まれているように、サイトにログオンします。 次に、Default.aspx からには、テスト用のリンクのリンクをクリックします。 どうなっているのでしょうか。 SomePage.aspx をという名前のページが存在しない場合、404 エラーが発生しましたが、いない重要なはここで。 代わりに、お使いのブラウザーのアドレス バーに注目します。 URL でフォーム認証チケットが含まれていることを注意してください。

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

リンクの URL SomePage.aspx が認証チケットに含まれている URL に自動的に変換されますが書き込むコードがまったくありませんでした。 Http:// で始まらないすべてのハイパーリンクの URL には、フォーム認証チケットが自動的に埋め込まれますまたは/です。 Response.Redirect への呼び出しで、ハイパーリンク コントロール、または HTML アンカー要素にハイパーリンクが表示される場合に重要ではありません (つまり、 &lt;、href =「...」&gt;.&lt;/a&gt;)。 URL のようなものでない限り、http://www.someserver.com/SomePage.aspxまたは/SomePage.aspx、フォーム認証チケットは、ご利用の米国埋め込まれます。

> [!NOTE]
> Cookie なしのフォーム認証チケットは、クッキー ベースの認証チケットとして同じタイムアウト ポリシーに準拠します。 ただし、cookie なしの認証チケットは、認証チケットが URL に直接埋め込まれているために、リプレイ攻撃を受けやすい。 Web サイトにアクセスでログインし、同僚に電子メールで URL を貼り付けますユーザーを想像してください。 仕事仲間は、有効期限に達する前にそのリンクをクリックすると、それらに記録されます、電子メールを送信したユーザーとして!


## <a name="step-3-securing-the-authentication-ticket"></a>手順 3: 認証チケットのセキュリティ保護

フォーム認証チケットは、ネットワーク経由で送信される cookie のいずれか、URL 内で直接埋め込まれているなどです。 Id 情報だけでなく認証チケット含めることができますもユーザー データ (手順 4. で見ていきます)。 そのため、チケットのデータが暗号化されている重要なは、フォーム認証システム覗き見と (そしてさらに) からチケットに変更されていないことを保証できます。

チケットのデータのプライバシーを保護するには、フォーム認証システムがチケット データを暗号化できます。 チケット データを暗号化する障害は、プレーン テキストでネットワーク経由で機密性の高い情報を送信します。

チケットの信頼性を保証するために、フォーム認証システムがある必要があります*検証*チケット。 検証は、特定のデータが変更されていないと、によって実現されます。 保証するための、 *[メッセージ認証コード (MAC)](http://en.wikipedia.org/wiki/Message_authentication_code)* です。 簡単に言うと、MAC は、(この場合は、チケット) を検証する必要があるデータを識別する情報の断片です。 MAC で表されるデータを変更する場合、MAC とデータが一致しません。 さらに、ハッカーが両方のデータを変更し、変更されたデータに対応するためには、自分の MAC を生成する計算量が非常に困難です。

作成 (または変更する) ときに、チケット、フォーム認証システムは、MAC を作成し、チケットのデータにアタッチします。 後続の要求が到着すると、フォーム認証システムはチケット データの信頼性を検証する MAC とチケットのデータを比較します。 図 3 は、このワークフローを視覚的に示します。


[![MAC によりチケットの信頼性が確保されます。](forms-authentication-configuration-and-advanced-topics-vb/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image7.png)

**図 03**: MAC により、チケットの信頼性が確保されます ([フルサイズのイメージを表示するをクリックして](forms-authentication-configuration-and-advanced-topics-vb/_static/image9.png))


保護の設定によって異なります、認証チケットにどのようなセキュリティ対策が適用されます、&lt;フォーム&gt;要素。 保護の設定は、次の 3 つの値のいずれかに割り当てることができます。

- すべてのチケットが暗号化されており、(既定)、デジタル署名します。
- 暗号化 - のみの暗号化が適用される - MAC は生成されません。
- 暗号化なし - チケットもデジタル署名。
- 検証の MAC が生成されたが、チケット データがプレーン テキストでネットワーク経由で送信されます。

Microsoft では、すべての設定を使用することを強くお勧めします。

### <a name="setting-the-validation-and-decryption-keys"></a>検証と復号化キーの設定

暗号化とハッシュを暗号化および認証チケットを検証するフォームの認証システムで使用されるアルゴリズムはを通じてカスタマイズ可能な[ &lt;machineKey&gt;要素](https://msdn.microsoft.com/library/w8h3skw9.aspx)Web.config でします。表 2 のアウトライン、 &lt;machineKey&gt;要素の属性とその考えられる値です。

| **属性** | **説明** |
| --- | --- |
| 復号化 | 暗号化に使用するアルゴリズムを示します。 この属性は、次の 4 つの値のいずれかを持つことができます: - 自動の既定値です。decryptionKey 属性の長さに基づいて、アルゴリズムを決定します。 -AES - を使用して、 [Advanced Encryption Standard (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard)アルゴリズムです。 DES - を使用して、[データ暗号化標準 (DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard)このアルゴリズムは、負荷の強度の弱さし、は使用できません。 -3 des - を使用して、 [Triple DES](http://en.wikipedia.org/wiki/Triple_DES)アルゴリズムで、3 回、DES アルゴリズムを適用することによって動作します。 |
| decryptionKey | 暗号化アルゴリズムで使用される共有キー。 この値かする必要があります (復号化の値に基づいて) 適切な長さ、自動生成、またはいずれかの値が、追加の 16 進文字列である IsolateApps です。 IsolateApps を追加するには、アプリケーションごとに一意の値を使用する ASP.NET がように指示します。 既定値は、AutoGenerate, IsolateApps です。 |
| 検証 | 検証に使用するアルゴリズムを示します。 この属性は、次の 4 つの値のいずれかを持つことができます: - AES - は Advanced Encryption Standard (AES) アルゴリズムを使用します。 MD5 の使用、[メッセージ ダイジェスト 5 (MD5)](http://en.wikipedia.org/wiki/MD5)アルゴリズムです。 -SHA1 の使用、 [SHA1](http://en.wikipedia.org/wiki/Sha1)アルゴリズム (既定)。 -3 des、Triple DES アルゴリズムを使用します。 |
| validationKey | 検証アルゴリズムで使用される秘密キーです。 この値必要がありますか (検証の値に基づいて) 適切な長さ、自動生成、またはいずれかの値が、追加の 16 進文字列である IsolateApps です。 IsolateApps を追加するには、アプリケーションごとに一意の値を使用する ASP.NET がように指示します。 既定値は、AutoGenerate, IsolateApps です。 |

**表 2**: &lt;machineKey&gt;要素の属性

暗号化と検証のオプション、および担当者の完全な説明と、さまざまなアルゴリズムの短所は、このチュートリアルの範囲外です。 どのようなキーの長さを使用するのに使用するにはどのような暗号化と検証アルゴリズムに関するガイダンスを含め、これらの問題で、詳細な検索のこれらのキーを生成しを参照する最適な方法および*Professional ASP.NET 2.0 のセキュリティ、メンバーシップ、およびロール管理*.

既定では、暗号化と検証に使用されるキーは、アプリケーションごとに自動的に生成およびこれらのキーはローカル セキュリティ機関 (LSA) で格納されます。 要するに、既定の設定は、web サーバー-web によってサーバーとアプリケーションによってごとに一意キーを保証します。 そのため、この既定の動作は、次の 2 つのシナリオでは機能しません。

- **Web ファーム**-、 [web ファーム](http://en.wikipedia.org/wiki/Web_farm)シナリオでは、1 つの web アプリケーションがスケーラビリティおよび冗長性のために複数の web サーバーでホストされています。 各受信要求をユーザーのセッションの有効期間、さまざまなサーバー可能性がある使用、さまざまな要求を処理する、ファーム内のサーバーにディスパッチされます。 したがって、フォーム認証チケットを作成できるように、各サーバーで同じ暗号化と検証キーを使用する必要があります、暗号化、および 検証済みの 1 つのサーバーを暗号化解除され、ファーム内の別のサーバー上で検証できます。
- **アプリケーション チケットの共有をクロス**-1 つの web サーバーは、複数の ASP.NET アプリケーションをホストできます。 これらのさまざまなアプリケーションを 1 つのフォーム認証チケットを共有する必要がある場合、暗号化と検証キーと一致するが重要です。

Web ファームの設定や、同じサーバー上のアプリケーション間での認証チケットの共有で作業するときは、構成する必要があります、 &lt;machineKey&gt;影響を受けるアプリケーション内の要素をその decryptionKey とキー validationkey キー値とも一致します。

このサンプル アプリケーションに上記のシナリオのどちらも適用するときにも明示的な decryptionKey と validationKey の値を指定し、使用するアルゴリズムを定義おできます。 追加、 &lt;machineKey&gt; Web.config ファイルに設定します。

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample5.xml)]

詳細については、チェック アウト[How To: ASP.NET 2.0 で MachineKey を構成する](https://msdn.microsoft.com/library/ms998288.aspx)です。

> [!NOTE]
> DecryptionKey と validationKey の値から取得されました[Steve Gibson](http://www.grc.com/stevegibson.htm)の[完璧なパスワードの web ページ](https://www.grc.com/passwords.htm)、各ページの訪問で 64 のランダムな 16 進数文字を生成します。 これらのキーを実稼働アプリケーションには、その方法を行う可能性を低くために、完璧なパスワード ページからランダムに生成されたもので上記のキーを置き換えることをお勧めすます。


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>手順 4: チケットに追加のユーザー データを格納します。

多くの web アプリケーションでは、に関する情報を表示、現在ログオンしているユーザーに対するページの表示をベースまたはします。 たとえば、web ページには、ユーザーの名前と彼女は最後にログオンの各ページの上部の隅にある日付が場合があります。 フォーム認証チケットには、現在ログオンしているユーザーのユーザー名が格納されますが、その他の情報が必要なときに、ページする必要がありますストアに移動するユーザーの通常のデータベースの認証チケットに格納されていない情報を参照します。

少しのコードでは、フォーム認証チケットに追加のユーザー情報を格納おできます。 このようなデータを表現することができます、[所属クラス](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)の[UserData プロパティ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx)です。 これは、少量が一般的に必要なユーザーに関する情報を格納するのに便利な場所です。 プロパティが含まれる一部として認証チケットの cookie と他のチケットのフィールドのように、暗号化は、検証 UserData で指定された値は、フォーム認証システムの構成に基づいています。 既定は、UserData は、空の文字列です。

認証チケットのユーザー データを格納するためには、ユーザーに固有の情報を取得し、チケット内に格納する、ログイン ページに少量のコードを記述する必要があります。 UserData が文字列型のプロパティであるために、文字列としてに格納されているデータを正しくシリアル化する必要があります。 たとえば、弊社ユーザー ストアには、各ユーザーの生年月日とその雇用者の名前が含まれていること、および認証チケットにこれらの 2 つのプロパティ値を格納するでした。 文字列にこれらの値をシリアル化、続けて雇用者名にパイプ (|)、生年月日の文字列のユーザーの日付を連結することによっておでした。 1974 年 8 月 15日 生まれたは、Northwind traders 社に対して機能する種類のユーザー、私たちがプロパティに割り当てる UserData 文字列: 1974-08-15 |Northwind traders 社です。

チケットに格納されているデータにアクセスする必要があります、ときにこれにを現在の要求の所属をグラブして UserData プロパティを逆シリアル化します。 生年月日、雇用者名の例の日付の場合は、2 つの部分文字列が、区切り記号 (|) を基に、UserData 文字列を分割します。


[![認証チケットに追加のユーザー情報を格納できます。](forms-authentication-configuration-and-advanced-topics-vb/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image10.png)

**図 04**: その他のユーザー情報に格納できる認証チケット ([フルサイズのイメージを表示するをクリックして](forms-authentication-configuration-and-advanced-topics-vb/_static/image12.png))


### <a name="writing-information-to-userdata"></a>UserData 情報を書き込む

残念ながら、フォーム認証チケットをユーザーに固有の情報を追加するは想像のいずれかの単純ではありません。 UserData のクラスのプロパティの所属は読み取り専用と、所属クラスのコンス トラクターでのみ指定できます。 また、チケットの他の値を提供する必要がありますをコンス トラクターで UserData プロパティを指定する場合: ユーザー名、発行日、期限切れ、およびなど。 前のチュートリアルでログイン ページを作成したときにこのすべてによって処理されたうえで FormsAuthentication クラスです。 UserData を所属に追加するは既に FormsAuthentication クラスによって提供される機能の多くをレプリケートするコードを記述する必要があります。

ユーザーが認証チケットに関する追加情報を記録する Login.aspx ページを更新して UserData 操作に必要なコードを見てみましょう。 振る舞う、ユーザー ストアに、ユーザーは会社と、タイトルに関する情報が含まれていると、認証チケットでこの情報をキャプチャします。 コードは、次のように見えるように Login.aspx ページのログイン ボタンの Click イベント ハンドラーを更新します。

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample6.vb)]

一度にこのコードを 1 行ずつステップみましょう。 メソッドを次の 4 つの文字列配列を定義することによって開始: ユーザー、パスワード、companyName、および titleAtCompany です。 これらの配列を保持ユーザー名、パスワード、会社名、およびユーザー アカウントのタイトル システムでは、3 つがありますを: Scott、Jisun、および Sam です。 実際のアプリケーションでは、されませんハードコーディング ページのソース コードで、ユーザー ストアからこれらの値を照会するは。

前のチュートリアルで指定された資格情報が有効な場合単と呼ばれる、次の手順を実行する FormsAuthentication.RedirectFromLoginPage (UserName.Text、RememberMe.Checked)。

1. フォーム認証チケットの作成
2. 適切なストアに、チケットを書き込みました。 Cookie ベースの認証チケットの場合、ブラウザーの cookie のコレクションが使用されます。cookie なしの認証チケットの場合は、チケット データは、URL にシリアル化します。
3. 適切なページにユーザーをリダイレクト

次の手順は、上記のコードでレプリケートされます。 最初に、最終的に UserData プロパティに格納されますが形成される文字列に会社名と、パイプ文字 (|) で 2 つの値を区切り、タイトルを組み合わせることによってです。

UserDataString として文字列を dim String.Concat(companyName(i)、="|"、titleAtCompany(i))

次に、認証チケットを作成するには、メソッドが呼び出され、FormsAuthentication.GetAuthCookie 暗号化し、構成設定に従って検証し、HttpCookie オブジェクトに格納します。

Dim authCookie として HttpCookie FormsAuthentication.GetAuthCookie (UserName.Text、RememberMe.Checked) を =

Cookie に埋め込まれた FormAuthenticationTicket を使用するために必要がありますを呼び出して、FormAuthentication クラスの[メソッドを復号化](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx)cookie の値を渡して、します。

チケットとして所属を dim FormsAuthentication.Decrypt(authCookie.Value) を =

作成し、*新しい*既存所属の値に基づいて所属インスタンス。 ただし、この新しいチケットには、ユーザーに固有の情報 (userDataString) が含まれています。

Dim newTicket 所属として新しい FormsAuthenticationTicket(ticket. を =チケットのバージョンです。チケットの名前。IssueDate、チケットします。有効期限、チケット。IsPersistent、userDataString)

暗号化 (し検証) を呼び出して新しい所属インスタンス、[メソッドを暗号化](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)authCookie にこの暗号化された (および検証済み) のデータを戻すとします。

authCookie.Value = FormsAuthentication.Encrypt(newTicket)

最後に、authCookie が Response.Cookies コレクションに追加され、GetRedirectUrl メソッドをユーザーに送信する、適切なページを確認します。

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample7.vb)]

UserData プロパティは読み取り専用と FormsAuthentication クラスがその GetAuthCookie、SetAuthCookie、または RedirectFromLoginPage メソッドで UserData 情報を指定するメソッドを提供していないために、このすべてのコードが必要です。

> [!NOTE]
> 検査は、コードでは、cookie ベースの認証チケットのユーザー固有の情報を格納します。 URL にフォーム認証チケットをシリアル化するクラスは、.NET Framework の内部です。 手短、cookie なしのフォーム認証チケットのユーザー データを格納することはできません。


### <a name="accessing-the-userdata-information"></a>UserData 情報へのアクセス

この時点で各ユーザーの会社名とタイトルにログインするときに、フォーム認証チケットの UserData プロパティに格納されます。 この情報は、ユーザー ストアへのトリップを必要とせず、任意のページで、認証チケットからアクセスできます。 UserData プロパティからこの情報を取得する方法を示すためには、みましょう更新 Default.aspx、ようこそメッセージだけでなく、ユーザーの名前も、会社の動作と、タイトルが含まれるようにします。

現時点では、Default.aspx AuthenticatedMessagePanel パネルの WelcomeBackMessage をという名前のラベル コントロールに含まれています。 このパネルは認証されたユーザーにのみ表示されます。 Default.aspx のページでコードを更新\_次のように見えるように、イベント ハンドラーを読み込みます。

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample8.vb)]

Request.IsAuthenticated が true の場合場合 WelcomeBackMessage のテキストのプロパティは、まずへようこそ の背面へ移動*username*です。 その後、//新しい一般プリンシパル プロパティは、基になる所属アクセスできるように、FormsIdentity オブジェクトにキャストされます。 所属したらは、会社名とタイトルに UserData プロパティ逆シリアル化します。 これは、パイプ文字の文字列を分割して行います。 会社名とタイトルは WelcomeBackMessage ラベルに表示されます。

図 5 は、アクションでこのディスプレイのスクリーン ショットを示します。 Scott としてログインするには、Scott の会社とタイトルを含むウェルカム バック メッセージが表示されます。


[![現在にログオンしたユーザーの会社およびタイトルが表示されます。](forms-authentication-configuration-and-advanced-topics-vb/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image13.png)

**図 05**: 現在にログオンしたユーザーの会社およびタイトルが表示されます ([フルサイズのイメージを表示するをクリックして](forms-authentication-configuration-and-advanced-topics-vb/_static/image15.png))


> [!NOTE]
> 認証チケットの UserData プロパティは、ユーザー ストアのキャッシュとして機能します。 、任意のキャッシュのように、基になるデータが変更されたときに更新する必要があります。 たとえば、ユーザーが自分のプロファイルを更新できる web ページがある場合 UserData プロパティにキャッシュされているフィールドはユーザーによって行われた変更を反映するように更新する必要があります。


## <a name="step-5-using-a-custom-principal"></a>手順 5: カスタム プリンシパルを使用します。

受信要求ごとに、FormsAuthenticationModule はユーザーを認証しようとします。 期限切れでない認証チケットが存在する場合、FormsAuthenticationModule は HttpContext.User プロパティを新しい GenericPrincipal オブジェクトに割り当てます。 この GenericPrincipal オブジェクトには、フォーム認証チケットへの参照を含む FormsIdentity の種類の Id があります。 GenericPrincipal クラスには、IPrincipal を実装するクラスで必要とベアの最低限の機能が含まれています - Identity プロパティと IsInRole メソッドだけができます。

プリンシパル オブジェクトが 2 つの役割: ユーザーが所属するロールを示すために、id 情報を提供します。 IPrincipal インターフェイスの IsInRole を介して行われます (*roleName*) メソッドと Identity プロパティでは、それぞれします。 GenericPrincipal クラスでは、ロール名の文字列配列をそのコンス トラクターを使用して指定します。その IsInRole (*roleName*) メソッドは単なるかどうかをチェック、渡されたで*roleName*文字列の配列内に存在します。 FormsAuthenticationModule、GenericPrincipal を作成するときに GenericPrincipal のコンス トラクターに空の文字列配列に渡します。 その結果、IsInRole への呼び出しは常に False を返します。

GenericPrincipal クラスではの役割が使用されていないほとんどのフォーム ベースの認証シナリオのニーズを満たしています。 既定の役割の処理が十分なできない場合はそれらのまたはカスタム IIdentity オブジェクト、ユーザーと関連付ける必要がある場合は、認証ワークフローの間にカスタム IPrincipal オブジェクトを作成し、HttpContext.User プロパティに割り当てます。

> [!NOTE]
> わかるように今後のチュートリアルについては、ときに ASP です。NET の役割のフレームワークが有効になっている型のカスタム プリンシパル オブジェクトを作成[RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx)し、フォーム認証が作成した GenericPrincipal オブジェクトが上書きされます。 これは、プリンシパルの IsInRole ロール フレームワークの API でインターフェイス メソッドをカスタマイズするためにします。


おが考慮ありません自分たちの役割とまだ、ので、プリンシパルにカスタム IIdentity オブジェクトに関連付けることをこの時点でカスタム プリンシパルを作成する必要が唯一の理由になります。 手順 4. で、ユーザーの会社名と、タイトルは、特に、認証チケットの UserData プロパティで追加のユーザー情報を格納するについて説明しました。 ただし、UserData 情報は、認証チケットを使用してアクセスし、シリアル化された文字列でいつでも、チケットに格納されているユーザー情報を表示する必要がある UserData プロパティを解析を意味し、のみのみ。

機能を向上させる開発者から見れば、CompanyName とタイトルのプロパティが含まれていますを IIdentity を実装するクラスを作成します。 そのようには、開発者が、現在ログオンしているユーザーの会社名にアクセスできるされ、せず CompanyName とタイトルのプロパティを通じて直接タイトル UserData プロパティを解析する方法を理解するために必要です。

### <a name="creating-the-custom-identity-and-principal-classes"></a>カスタム Id およびプリンシパル クラスを作成します。

このチュートリアルでは、作成しましょうカスタム プリンシパルと id オブジェクト アプリで\_コード フォルダーです。 アプリを追加することによって開始\_コードをプロジェクトにフォルダー、ソリューション エクスプ ローラーでプロジェクト名を右クリックして - ASP.NET フォルダーの追加オプションを選択してアプリを選択\_コード。 アプリ\_コード フォルダーは、ファイル、web サイトに固有のクラスを保持する特別な ASP.NET フォルダーです。

> [!NOTE]
> アプリ\_コード フォルダーは、web サイト プロジェクトのモデルを使用したプロジェクトを管理する場合にのみ使用する必要があります。 使用している場合、 [Web アプリケーション プロジェクト モデル](https://msdn.microsoft.com/asp.net/Aa336618.aspx)標準フォルダを作成し、するクラスを追加します。 たとえば、クラスをという名前の新しいフォルダーを追加し、あるコードを配置します。


次に、2 つの新しいクラス ファイルをアプリに追加\_コード フォルダー、1 つの名前付き CustomIdentity.vb と 1 CustomPrincipal.vb をという名前です。


[![CustomIdentity と CustomPrincipal クラスをプロジェクトに追加します。](forms-authentication-configuration-and-advanced-topics-vb/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image16.png)

**図 06**: CustomIdentity と CustomPrincipal クラスをプロジェクトに追加 ([フルサイズのイメージを表示するをクリックして](forms-authentication-configuration-and-advanced-topics-vb/_static/image18.png))


CustomIdentity クラスは、AuthenticationType、IsAuthenticated、および名前のプロパティを定義する IIdentity インターフェイスを実装します。 これらの必須プロパティだけでなくここでは、基になるフォーム認証チケットだけでなく、ユーザーの会社名とタイトルのプロパティを公開します。 CustomIdentity クラスに次のコードを入力します。

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample9.vb)]

クラスに所属メンバー変数が含まれることに注意してください (\_チケット) と、コンス トラクターでこのチケット情報を提供する必要があります。 このチケット データは、その id の名前を返すときに使用します。CompanyName とタイトルのプロパティの値を返すには、その UserData プロパティが解析されます。

次に、CustomPrincipal クラスを作成します。 CustomPrincipal クラスのコンス トラクターが、CustomIdentity オブジェクトのみです。 を受け入れるため、これは問題ありませんロールでこの時点で、その IsInRole メソッドは、常に False を返します。

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample10.vb)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>受信要求のセキュリティ コンテキストを CustomPrincipal オブジェクトを割り当てる

カスタム id を使用するカスタム プリンシパル クラスと同様に CompanyName とタイトルのプロパティを含める既定 IIdentity 仕様を拡張するクラスがあるようになりました。 ASP.NET パイプラインにステップ インする準備が整いましたし、受信要求のセキュリティ コンテキストに、カスタムのプリンシパル オブジェクトを割り当てます。

ASP.NET パイプラインは受信要求を受け取りし、いくつかの手順を通じてを処理します。 各手順で、特定のイベントが発生開発者は、ASP.NET パイプラインをタップし、ライフ サイクルの特定の時点で要求を修正できるようにします。 FormsAuthenticationModule は、ASP.NET を発生させるなど、待機、 [AuthenticateRequest イベント](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)、この時点で、認証チケットを受信した要求を検査します。 認証チケットが見つかった場合は、GenericPrincipal オブジェクトが作成され、HttpContext.User プロパティに割り当てられます。

AuthenticateRequest イベント後に、ASP.NET パイプラインを発生させます、 [PostAuthenticateRequest イベント](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx)するのインスタンスと FormsAuthenticationModule によって作成された GenericPrincipal オブジェクトを置き換えるおことができますが、CustomPrincipal オブジェクトです。 図 7 は、このワークフローを示しています。


[![PostAuthenticationRequest イベントで CustomPrincipal は、GenericPrincipal に置換します。](forms-authentication-configuration-and-advanced-topics-vb/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image19.png)

**図 07**: PostAuthenticationRequest イベントで CustomPrincipal は、GenericPrincipal に置換 ([フルサイズのイメージを表示するをクリックして](forms-authentication-configuration-and-advanced-topics-vb/_static/image21.png))


コードを実行するには、ASP.NET パイプライン イベントに応答するためには、Global.asax で適切なイベント ハンドラーを作成するか、独自の HTTP モジュールを作成おことができます。 このチュートリアルには、Global.asax でイベント ハンドラーを作成してみましょう。 まず、web サイトを Global.asax を追加します。 ソリューション エクスプ ローラーでプロジェクト名を右クリックし、Global.asax というグローバル アプリケーション クラスの種類のアイテムを追加します。


[![Global.asax ファイルを web サイトに追加します。](forms-authentication-configuration-and-advanced-topics-vb/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image22.png)

**図 08**: Global.asax ファイルの web サイトを追加 ([フルサイズのイメージを表示するをクリックして](forms-authentication-configuration-and-advanced-topics-vb/_static/image24.png))


Global.asax の既定のテンプレートには、開始、終了など、ASP.NET パイプライン イベントの数のイベント ハンドラーが含まれています。 および[エラー イベント](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx)、その他。 自由にこれらのイベント ハンドラーを削除するようにこのアプリケーションの必要ことはありません。 ここでは、イベントは、PostAuthenticateRequest です。 マークアップは、次のようになりますので、Global.asax ファイルを更新します。

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample11.aspx)]

アプリケーション\_OnPostAuthenticateRequest メソッドは、ASP.NET ランタイムが、着信する各ページ要求では、1 回 PostAuthenticateRequest イベントを発生させるたびに、実行します。 イベント ハンドラーは、ユーザーが認証され、フォーム認証によって認証されたかどうかにどうかを確認して起動します。 場合は、新しい CustomIdentity オブジェクトが作成され、現在の要求の認証チケットのコンス トラクターに渡されます。 次に、CustomPrincipal オブジェクトが作成され、CustomIdentity だけが作成したオブジェクトをコンス トラクターに渡されました。 最後に、現在の要求のセキュリティ コンテキストは、新しく作成された CustomPrincipal オブジェクトに割り当てられます。

-要求のセキュリティ コンテキストを持つ CustomPrincipal オブジェクトに関連付ける - 最後の手順が 2 つのプロパティをプリンシパルを割り当てることに注意してください: HttpContext.User と Thread.CurrentPrincipal です。 これら 2 つの割り当ては、方法により、ASP.NET でのセキュリティ コンテキストが処理される必要があります。 .NET Framework は、各実行中のスレッドをセキュリティ コンテキストを関連付けますこの情報はにより IPrincipal オブジェクトとして使用できる、[スレッド オブジェクト](https://msdn.microsoft.com/library/system.threading.thread.aspx)の[CurrentPrincipal プロパティ](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx)です。 混乱を招くは ASP.NET には、独自のセキュリティ コンテキスト情報 (HttpContext.User) です。

特定のシナリオで、Thread.CurrentPrincipal プロパティが検査のセキュリティ コンテキストを決定するとき他のシナリオでは、HttpContext.User が使用されます。 たとえば、開発者宣言によってどのようなユーザーの状態に使用できる .NET のセキュリティ機能があるまたはロールのクラスのインスタンスを作成または固有のメソッドを呼び出すことができます (を参照してください[ビジネス レイヤーを使用してデータを承認規則を追加します。PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx))。 背後は、これらの宣言方法は、Thread.CurrentPrincipal プロパティを使用してセキュリティ コンテキストを決定します。

他のシナリオでは、HttpContext.User プロパティが使用されます。 たとえば、前のチュートリアルで使用してこのプロパティに現在ログオンしているユーザーのユーザー名を表示します。 明確に、その後は Thread.CurrentPrincipal および HttpContext.User プロパティでセキュリティ コンテキスト情報が一致します。

ASP.NET ランタイムは、ご利用の米国、これらのプロパティ値を自動的に同期します。 ただし、AuthenticateRequest イベント後にこの同期が行われますが、*する前に*PostAuthenticateRequest イベント。 その結果、PostAuthenticateRequest イベントにカスタム プリンシパルを追加するときに、Thread.CurrentPrincipal か、または Thread.CurrentPrincipal を手動で割り当てることである必要があり、HttpContext.User が同期されます。参照してください[Context.User vs です。Thread.CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/)この問題については詳細です。

### <a name="accessing-the-companyname-and-title-properties"></a>CompanyName とタイトルのプロパティにアクセスします。

要求が受信され ASP.NET エンジンをアプリケーションにディスパッチされるたびに\_Global.asax で OnPostAuthenticateRequest イベント ハンドラーが起動されます。 要求が、FormsAuthenticationModule によって正常に認証された場合、イベント ハンドラーはフォーム認証チケットに基づく CustomIdentity オブジェクトに新しい CustomPrincipal オブジェクトを作成します。 このロジックで、現在ログオンしているユーザーの会社名とタイトルに関する情報へのアクセスは、非常に簡単です。

ページに戻る\_Default.aspx で、フォーム認証チケットを取得して、ユーザーの会社名とタイトルを表示するために、UserData プロパティを解析するコードを作成しました手順 4. で、イベント ハンドラーをロードします。 CustomPrincipal と CustomIdentity 内のオブジェクトを使用して今すぐ、チケットの UserData プロパティから値を解析する必要はありません。 代わりに、単に CustomIdentity オブジェクトへの参照を取得し、CompanyName とタイトルのプロパティを使用します。

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample12.vb)]

## <a name="summary"></a>まとめ

このチュートリアルでは、Web.config を使用して、フォーム認証システムの設定をカスタマイズする方法を確認します。認証チケットの有効期限を処理する方法と、暗号化と検証のセーフガードの範囲を使用して検査と変更からチケットを保護する方法について説明しました。 最後に、認証チケットの UserData プロパティを使ってカスタム プリンシパル オブジェクトと id オブジェクトを使用して、複数の開発者向けの方法でこの情報を公開する方法と、それ自体がチケットの追加のユーザー情報を格納するについて説明します。

このチュートリアルでは、asp.net フォーム認証弊社の調査で終了します。 次のチュートリアルでは、メンバーシップ フレームワークへの道のりが起動します。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [フォーム認証の分解](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [ASP.NET 2.0 でフォーム認証をについて説明します。](https://msdn.microsoft.com/library/aa480476.aspx)
- [方法: ASP.NET 2.0 でフォーム認証を保護します。](https://msdn.microsoft.com/library/ms998310.aspx)
- [プロフェッショナル向けの ASP.NET 2.0 セキュリティ、メンバーシップ、およびロール管理](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html)(ISBN: 978-0-7645-9698-8)
- [ログイン コントロールのセキュリティ保護](https://msdn.microsoft.com/library/ms178346.aspx)
- [&lt;認証&gt;要素](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;フォーム&gt;要素&lt;認証&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [&lt;MachineKey&gt;要素](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [フォーム認証チケットと Cookie を理解します。](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>このチュートリアルに含まれるトピックに関するビデオ トレーニング

- [フォーム認証のプロパティを変更する方法](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [ASP.NET アプリケーションでクッキーのない認証をセットアップして使用する方法](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [ASP フォーム ログイン再配置](../../../videos/authentication/asp-forms-login-relocation.md)
- [フォーム ログイン カスタム キー構成](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [認証方法にカスタム データを追加する](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [カスタム プリンシパル オブジェクトを使用する](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>作成者について

Scott Mitchell、複数の受け取りますブックの作成者と 4GuysFromRolla.com の創設者は、Microsoft の Web テクノロジと 1998 年取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書 *[Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* です。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログでを介して[ http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Alicja Maziarz しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[ mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com)です。

> [!div class="step-by-step"]
> [前へ](an-overview-of-forms-authentication-vb.md)
