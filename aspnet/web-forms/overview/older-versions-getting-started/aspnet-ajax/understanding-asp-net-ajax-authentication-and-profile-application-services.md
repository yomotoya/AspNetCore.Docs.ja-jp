---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: ASP.NET AJAX 認証とプロファイル アプリケーション サービスを理解する |Microsoft Docs
author: scottcate
description: 認証サービスは、認証クッキーを受信するために資格情報を提供することができ、ゲートウェイ サービスのカスタム ユーザーを許可するのには、.
ms.author: riande
ms.date: 03/14/2008
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: d722130e625a9f867923280fce0ef35f19bfeb9d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837982"
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>ASP.NET AJAX 認証とプロファイル アプリケーション サービスを理解します。
====================
によって[Scott Cate](https://github.com/scottcate)

[PDF のダウンロード](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> 認証サービスにより、ユーザー、認証クッキーを受信するために資格情報を指定して、ゲートウェイ サービスのカスタム ユーザー プロファイルを許可するのには ASP.NET によって提供されます。 ASP.NET AJAX の認証サービスの使用は、フォーム認証を使用中のアプリケーション (ログインの制御など) しない機能しなくなります AJAX 認証サービスにアップグレードすることで、標準の ASP.NET フォーム認証と互換性のあります。


## <a name="introduction"></a>はじめに

.NET Framework 3.5 の一部として、Microsoft は、かなり大きな環境のアップグレードでは; を配信します。新しい開発環境があるだけでなく統合言語クエリ (LINQ) の新機能とその他の言語拡張機能は近日公開予定。 さらに、.NET Framework 基本クラス ライブラリのファースト クラスのメンバーとして他のツールセットでは、特に、ASP.NET AJAX Extensions の使い慣れた機能の一部が含まれているがされています。 これらの拡張機能は、ページ全体の更新では、(ASP.NET のプロファイル API を含む)、クライアント スクリプトと広範なクライアント側 API を使用して Web サービスにアクセスする機能を必要とせずにページの部分的なレンダリングを含む、多くの新機能豊富なクライアント機能を有効にします。ASP.NET サーバー側コントロール セットに表示されるコントロール パターンの多くをミラーに設計されています。

このホワイト ペーパーでは、実装と ASP.NET のプロファイルの使用してフォーム認証サービスは、Microsoft ASP.NET AJAX ExtensionsThe AJAX Extensions で公開されて、フォーム認証としてそれをサポートするは驚くほど簡単 (だけでなくプロファイリング サービス) は、Web サービス プロキシ スクリプトを通じて公開されます。 AJAX Extensions には、AuthenticationServiceManager クラスを使用したカスタム認証もサポートします。

このホワイト ペーパーでは、Visual Studio 2008 の Beta 2 リリースと、.NET Framework 3.5 に基づいています。 このホワイト ペーパーには、いない Visual Web Developer Express を Visual Studio 2008 Beta 2 で使用され、チュートリアルに従って、Visual Studio のユーザー インターフェイスを提供することも想定しています。 一部のコード サンプルでは、Visual Web Developer Express で使用できないプロジェクト テンプレートを利用可能性があります。

## <a name="profiles-and-authentication"></a>*プロファイルと認証*

Microsoft ASP.NET のプロファイルと認証サービスは、ASP.NET フォーム認証システムによって提供され、ASP.NET の標準的なコンポーネントです。 ASP.NET AJAX Extensions では、クライアント AJAX ライブラリの名前空間の Sys.Services 非常に簡単ですモデルでのスクリプト プロキシを介してこれらのサービスへのスクリプト アクセスを提供します。

認証サービスにより、ユーザー、認証クッキーを受信するために資格情報を指定して、ゲートウェイ サービスのカスタム ユーザー プロファイルを許可するのには ASP.NET によって提供されます。 ASP.NET AJAX の認証サービスの使用は、フォーム認証を使用中のアプリケーション (ログインの制御など) しない機能しなくなります AJAX 認証サービスにアップグレードすることで、標準の ASP.NET フォーム認証と互換性のあります。

プロファイル サービスは、自動統合と、認証サービスによって提供されるメンバーシップに基づいてユーザー データのストレージを使用します。 格納されたデータが、web.config ファイルで指定された、さまざまなプロファイリング サービス プロバイダーは、データ管理を処理します。 認証サービスと同様、AJAX プロファイル サービスは、できるように、現在 ASP.NET プロファイル サービスの機能を組み込むページは、AJAX のサポートを含めることによって壊れていない必要があります、標準の ASP.NET プロファイル サービスとの互換性。

ASP.NET の認証とプロファイル サービス自体を組み込むアプリケーションがこのホワイト ペーパーの範囲外です。 トピックの詳細については、MSDN ライブラリを参照してください参照資料でのメンバーシップを使用したユーザーを管理する[ https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx)します。 ASP.NET には、ASP.NET メンバーシップの既定の認証サービス プロバイダーは、SQL Server でのメンバーシップを自動的に設定するためのユーティリティも含まれています。 詳細については、ASP.NET SQL Server の登録ツールの記事を参照してください (Aspnet\_regsql.exe) で[ https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)します。

## <a name="using-the-aspnet-ajax-authentication-service"></a>*ASP.NET AJAX の認証サービスの使用*

Web.config ファイルでは、ASP.NET AJAX 認証サービスを有効にする必要があります。

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

認証サービスでは、ASP.NET フォーム認証を有効にする必要があり、cookie (スクリプトを有効にできませんクッキーなしのセッション cookie なしのセッションが URL パラメーターが必要なため)、クライアント ブラウザーで有効にする必要があります。

AJAX 認証サービスが有効にし、構成、クライアント スクリプトすぐに利用できます Sys.Services.AuthenticationService オブジェクト。 クライアント スクリプトを活用するためにしますが、主として、`login`メソッドと`isLoggedIn`プロパティ。 多くのパラメーターを受け入れることができます、login メソッドの既定値を提供するいくつかのプロパティが存在します。

*Sys.Services.AuthenticationService メンバー*

*ログイン方法:*

Login() メソッドは、ユーザーの資格情報を認証するための要求を開始します。 このメソッドは、非同期実行をブロックしません。

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| userName | 必須。 認証するユーザー名。 |
| パスワード | 省略可能 (既定値は null) です。 ユーザーのパスワード。 |
| isPersistent | 省略可能 (既定値は false)。 かどうか、ユーザーの認証クッキーがセッション間で保持する必要があります。 False の場合、ブラウザーを閉じるし、セッションの有効期限が切れるユーザーがログオンします。 |
| redirectUrl | 省略可能 (既定値は null) です。認証に成功するブラウザーをリダイレクトする URL。 このパラメーターが null または空の文字列の場合は、リダイレクトは行われません。 |
| customInfo | 省略可能 (既定値は null) です。 このパラメーターは、現在使用されていないと、将来使用するために予約されています。 |
| loginCompletedCallback | 省略可能 (既定値は null) です。ログインが正常に完了したときに呼び出す関数。 指定した場合、このパラメーターは、defaultLoginCompleted プロパティをオーバーライドします。 |
| failedCallback | 省略可能 (既定値は null) です。ログインに失敗したときに呼び出す関数。 指定した場合、このパラメーターは、defaultFailedCallback プロパティをオーバーライドします。 |
| userContext | 省略可能 (既定値は null) です。 カスタム ユーザー コンテキスト データをコールバック関数に渡す必要があります。 |

*値が返されます。*

この関数では、戻り値は含まれません。 ただし、複数の動作は、この関数の呼び出しの完了時にインクルードされます。

- 現在のページが更新されますか、または場合に変更する、`redirectUrl`パラメーターが null でも空の文字列。
- ただし、パラメーターが null または空の文字列の場合、`loginCompletedCallback`パラメーター、または`defaultLoginCompletedCallback`プロパティが呼び出されます。
- Web サービスへの呼び出しに失敗した場合、`failedCallback`パラメーターの`defaultFailedCallback`プロパティが呼び出されます。

*logout メソッド:*

Logout() メソッドは、資格情報の cookie を削除し、web アプリケーションから現在のユーザーをログアウトします。

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| redirectUrl | 省略可能 (既定値は null) です。認証に成功するブラウザーをリダイレクトする URL。 このパラメーターが null または空の文字列の場合は、リダイレクトは行われません。 |
| logoutCompletedCallback | 省略可能 (既定値は null) です。ログアウトが正常に完了したときに呼び出す関数。 指定した場合、このパラメーターは、defaultLogoutCompleted プロパティをオーバーライドします。 |
| failedCallback | 省略可能 (既定値は null) です。ログインに失敗したときに呼び出す関数。 指定した場合、このパラメーターは、defaultFailedCallback プロパティをオーバーライドします。 |
| userContext | 省略可能 (既定値は null) です。 カスタム ユーザー コンテキスト データをコールバック関数に渡す必要があります。 |

*値が返されます。*

この関数では、戻り値は含まれません。 ただし、複数の動作は、この関数の呼び出しの完了時にインクルードされます。

- 現在のページが更新されますか、または場合に変更する、`redirectUrl`パラメーターが null でも空の文字列。
- ただし、パラメーターが null または空の文字列の場合、`logoutCompletedCallback`パラメーター、または`defaultLogoutCompletedCallback`プロパティが呼び出されます。
- Web サービスへの呼び出しに失敗した場合、`failedCallback`パラメーターの`defaultFailedCallback`プロパティが呼び出されます。

*defaultFailedCallback プロパティ (get、set):*

このプロパティは、web サービスとの通信に障害が発生した場合に呼び出す必要がある関数を指定します。 デリゲート (または関数の参照)、受信する必要があります。

このプロパティで指定された関数の参照は、次のシグネチャが必要です。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| エラー | エラー情報を指定します。 |
| userContext | ログインまたはログアウト関数が呼び出されたときにユーザー コンテキスト情報を指定します。 |
| methodName | 呼び出し元のメソッドの名前。 |

*defaultLoginCompletedCallback プロパティ (get、set):*

このプロパティは、ログイン web サービスの呼び出しが完了したときに呼び出される関数を指定します。 デリゲート (または関数の参照)、受信する必要があります。

このプロパティで指定された関数の参照は、次のシグネチャが必要です。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| validCredentials | ユーザーが有効な資格情報を指定するかどうかを指定します。 `true` で、ユーザーが正常にログインしている場合それ以外の場合`false`します。 |
| userContext | Login 関数が呼び出されたときにユーザー コンテキスト情報を指定します。 |
| methodName | 呼び出し元のメソッドの名前。 |

*defaultLogoutCompletedCallback プロパティ (get、set):*

このプロパティは、ログアウトの web サービス呼び出しが完了したときに呼び出される関数を指定します。 デリゲート (または関数の参照)、受信する必要があります。

このプロパティで指定された関数の参照は、次のシグネチャが必要です。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| 結果 | このパラメーターは必ず`null`; 将来使用するために予約されています。 |
| userContext | Login 関数が呼び出されたときにユーザー コンテキスト情報を指定します。 |
| methodName | 呼び出し元のメソッドの名前。 |

*isloggedin 関数のプロパティ (get):*

このプロパティは、ユーザーの現在の認証状態を取得します。ページ要求中に、ScriptManager オブジェクトによって設定されます。

このプロパティを返します`true`場合は、ユーザーは、それ以外の場合以外に現在ログインしているを返します`false`します。

*パスのプロパティ (get、set):*

このプロパティは、認証 web サービスの場所をプログラムで決定します。 いずれかの既定の認証プロバイダーをオーバーライドするために使用できる、ScriptManager コントロールの AuthenticationService 子ノードのパスのプロパティの宣言を設定 (詳細については、使用方法を参照してくださいカスタム認証サービス プロバイダー。トピックの下)。

既定の認証サービスの場所が変更されないことに注意してください。 ただし、ASP.NET AJAX を使用すると、ASP.NET AJAX 認証サービスのプロキシと同じクラス インターフェイスを提供する web サービスの場所を指定できます。

また、現在のサイトから、スクリプト要求を転送する値にこのプロパティを設定しないことに注意してください。 現在のアプリケーションでは、認証資格情報が表示されないが、ため役に立ちません。 なりますまた、テクノロジの基になる AJAX は、サイト間の要求を投稿する必要があります、クライアントのブラウザーにセキュリティ例外を生成する可能性があります。

このプロパティは、`String`認証 web サービスへのパスを表すオブジェクト。

*timeout プロパティ (get、set):*

このプロパティは、失敗したと仮定すると、ログイン要求の前に、認証サービスを待機する時間の長さを決定します。 呼び出しが完了するを待機中にタイムアウトになると、要求が失敗しましたコールバックが呼び出されます、呼び出しは完了しません。

このプロパティは、`Number`認証サービスから結果を待機するミリ秒数を表すオブジェクト。

*認証サービスにログインするコード サンプル。*

次のマークアップでは、AuthenticationService クラスのメソッドのログインとログアウトの単純なスクリプトの呼び出しでの ASP.NET ページの例を示します。

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>ASP.NET AJAX を使用してデータをプロファイルにアクセスします。

サービスのプロファイリング、ASP.NET は、ASP.NET AJAX 拡張機能を通じても公開されます。 ASP.NET プロファイル サービスはユーザー データを格納および取得する度で詳細な API を提供する優れた生産性ツールを指定できます。

プロファイル サービスは、web.config; で有効にする必要があります。既定ではありません。 そうことを確認、`profileService`子要素が有効になっている = true に指定された web.config、およびプロパティの読み取りまたは次のように記述されたことを指定したこと。

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

プロファイル サービスも構成する必要があります。 プロファイリング サービスの構成は、このホワイト ペーパーの範囲外では、プロファイルの構成設定で定義されているグループをグループ名のサブプロパティとしてアクセス可能になることに注意してください。 ことをお勧めします。 で指定した次のプロファイル セクション。

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

クライアント スクリプトでは、名前、Address.Line1、Address.Line2、Address.City、Address.State、Address.Zip、および BackgroundColor ProfileService クラスのプロパティ フィールドのプロパティとしてへのアクセスにはできます。

AJAX プロファイル サービスが構成されているとページですぐに使用されます。ただしを使用する前に 1 回読み込むことになります。

*Sys.Services.ProfileService メンバー*

*プロパティ フィールド:*

プロパティ フィールドでは、ドット、演算子の名前の規則で参照できる子プロパティとして構成されているプロファイルのすべてのデータを公開します。 プロパティ グループの子であるプロパティは、"GroupName.PropertyName"と呼ばれます。 ユーザーの状態を取得する上で示した例のプロファイルの構成、次の識別子を使用できます。

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*メソッドを読み込みます。*

サーバーから、選択した一覧またはすべてのプロパティを読み込みます。

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| propertyNames | 省略可能 (既定値は null) です。 サーバーから読み込まれるプロパティです。 |
| loadCompletedCallback | 省略可能 (既定値は null) です。 読み込みが完了したときに呼び出す関数。 |
| failedCallback | 省略可能 (既定値は null) です。 エラーが発生した場合に呼び出す関数。 |
| userContext | 省略可能 (既定値は null) です。 コールバック関数に渡されるコンテキスト情報。 |

Load 関数の戻り値ではありません。 かどうか、呼び出しが正常に完了にはいずれかを呼び出す、`loadCompletedCallback`パラメーターまたは`defaultLoadCompletedCallback`プロパティ。 呼び出しに失敗した、またはいずれか、タイムアウトが期限切れの場合、`failedCallback`パラメーターまたは`defaultFailedCallback`プロパティが呼び出されます。

場合、`propertyNames`パラメーターが指定されていない、すべての読み取りに構成されたプロパティは、サーバーから取得されます。

*メソッドを保存します。*

Save() メソッドは、ASP.NET のユーザーのプロファイルを指定したプロパティ リスト (またはすべてのプロパティ) を保存します。

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| propertyNames | 省略可能 (既定値は null) です。 サーバーに保存するプロパティです。 |
| saveCompletedCallback | 省略可能 (既定値は null) です。 保存するときに呼び出される関数が完了しました。 |
| failedCallback | 省略可能 (既定値は null) です。 エラーが発生した場合に呼び出す関数。 |
| userContext | 省略可能 (既定値は null) です。 コールバック関数に渡されるコンテキスト情報。 |

Save 関数には、戻り値はありません。 いずれかが呼び出す、呼び出しが正常に完了した場合、`saveCompletedCallback`パラメーターまたは`defaultSaveCompletedCallback`プロパティ。 呼び出しに失敗した、またはいずれか、タイムアウトが期限切れの場合、`failedCallback`または`defaultFailedCallback`プロパティが呼び出されます。

場合、`propertyNames`パラメーターが null、すべてのプロファイル プロパティは、サーバーに送信され、サーバーによって決定するプロパティを保存できるし、することはできません。

*defaultFailedCallback プロパティ (get、set):*

このプロパティは、web サービスとの通信に障害が発生した場合に呼び出す必要がある関数を指定します。 デリゲート (または関数の参照)、受信する必要があります。

このプロパティで指定された関数の参照は、次のシグネチャが必要です。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| Error | エラー情報を指定します。 |
| userContext | 時に提供されたユーザーのコンテキスト情報を指定します、読み込みまたは保存関数が呼び出されました。 |
| methodName | 呼び出し元のメソッドの名前。 |

*defaultSaveCompleted プロパティ (get、set):*

このプロパティは、ユーザーのプロファイル データの保存の完了時に呼び出す必要がある関数を指定します。 デリゲート (または関数の参照)、受信する必要があります。

このプロパティで指定された関数の参照は、次のシグネチャが必要です。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| numPropsSaved | 保存されたプロパティの数を指定します。 |
| userContext | 時に提供されたユーザーのコンテキスト情報を指定します、読み込みまたは保存関数が呼び出されました。 |
| methodName | 呼び出し元のメソッドの名前。 |

*defaultLoadCompleted プロパティ (get、set):*

このプロパティは、ユーザーのプロファイル データの読み込みの完了時に呼び出す必要がある関数を指定します。 デリゲート (または関数の参照)、受信する必要があります。

このプロパティで指定された関数の参照は、次のシグネチャが必要です。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| numPropsLoaded | 読み込まれるプロパティの数を指定します。 |
| userContext | 時に提供されたユーザーのコンテキスト情報を指定します、読み込みまたは保存関数が呼び出されました。 |
| methodName | 呼び出し元のメソッドの名前。 |

*パスのプロパティ (get、set):*

このプロパティは、プログラムでプロファイル web サービスの場所を決定します。 既定のプロファイル サービス プロバイダーと 1 つをオーバーライドするために使用できます、ScriptManager コントロールの ProfileService 子ノードのパスのプロパティの宣言を設定します。

既定のプロファイル サービスの場所が変更されないことに注意してください。 ただし、ASP.NET AJAX を使用すると、ASP.NET AJAX 認証サービスのプロキシと同じクラス インターフェイスを提供する web サービスの場所を指定できます。

また、現在のサイトから、スクリプト要求を転送する値にこのプロパティを設定しないことに注意してください。 テクノロジの基になる AJAX では、サイト間の要求を投稿する必要があり、クライアントのブラウザーにセキュリティ例外を生成する可能性があります。

このプロパティは、`String`プロファイル web サービスへのパスを表すオブジェクト。

*timeout プロパティ (get、set):*

このプロパティは、失敗したと仮定すると、読み込み前に、プロファイル サービスを待つか、要求を保存する時間の長さを決定します。 呼び出しが完了するを待機中にタイムアウトになると、要求が失敗しましたコールバックが呼び出されます、呼び出しは完了しません。

このプロパティは、`Number`プロファイル サービスから結果を待機するミリ秒数を表すオブジェクト。

*コード サンプル: ページの読み込み時のプロファイル データの読み込み*

次のコードでは、ユーザーが認証されたかどうかを確認し、として、ページのユーザーの推奨される背景色を読み込む場合は。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*カスタム認証のサービス プロバイダーを使用します。*

ASP.NET AJAX Extensions を使用すると、カスタム web サービスを通じて、機能を公開することで、カスタム スクリプトの認証サービス プロバイダーを作成できます。 使用するために、web サービスが 2 つのメソッドを公開する必要があります`Login`と`Logout`; 既定の ASP.NET AJAX 認証 web サービスと同じメソッド シグネチャを持つこれらのメソッドを指定する必要があります。

カスタム web サービスを作成した後は、コード、またはクライアント スクリプトを使用してプログラムでは、宣言によって、ページのいずれかへのパスを指定する必要があります。

*宣言によってパスを設定します。*

宣言によって、パスを設定するには、ASP.NET ページの scriptmanager コントロール オブジェクトの子である AuthenticationService を含めます。

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*コードでパスを設定します。*

パスをプログラムで設定するには、スクリプト マネージャーのインスタンスを使用してパスを指定します。

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*スクリプトでパスを設定します。*

スクリプトでパスをプログラムで設定するには、利用、 `path` AuthenticationService クラスのプロパティ。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*カスタム認証のサンプル Web サービス*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>まとめ

ASP.NET サービス - 具体的にはプロファイル、メンバーシップ、および認証サービスでは、クライアント ブラウザーで JavaScript を簡単に公開されます。 これにより、開発者は複雑に Updatepanel などのコントロールに依存することがなく、シームレスに認証メカニズムと、クライアント側のコードを統合できます。 Web の構成設定を利用することで同様に、クライアントからプロファイル データを保護することができます。既定では、データがないと開発者する必要があります選択でプロファイル プロパティ。

さらに、同等のメソッド シグネチャを持つ簡略化された web サービスの実装を作成すると、開発者はこれらの組み込みの ASP.NET サービス プロバイダーがカスタム スクリプトを作成できます。 これらの手法のサポートは、さまざまな特定のニーズに対応する柔軟性を開発者に提供しながら、リッチ クライアント アプリケーションの開発を簡略化します。

## <a name="bio"></a>*自己紹介*

1997 年からマイクロソフトの Web テクノロジで働いてあり myKB.com プレジデント、Scott Cate ([www.myKB.com](http://www.myKB.com)) ベースのナレッジ ベースのソフトウェア ソリューションに重点を置いてアプリケーションを ASP.NET の記述を専門としています。 Scott は時に電子メールが接続可能[ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)またはで彼のブログ[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [前へ](understanding-asp-net-ajax-updatepanel-triggers.md)
> [次へ](understanding-asp-net-ajax-localization.md)
