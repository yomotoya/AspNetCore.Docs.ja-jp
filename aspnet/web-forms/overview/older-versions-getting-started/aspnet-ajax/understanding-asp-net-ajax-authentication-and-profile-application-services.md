---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: ASP.NET AJAX 認証およびプロファイル アプリケーション サービスを理解する |Microsoft ドキュメント
author: scottcate
description: 認証サービスは、認証 cookie を受信するために資格情報を提供することができ、ゲートウェイ サービスのカスタム ユーザーを許可するのには、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 0bf6538d0c4ae9488e6ac29ccba6d4b243cf070e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>ASP.NET AJAX 認証およびプロファイル アプリケーション サービスを理解します。
====================
によって[Scott カテゴリ](https://github.com/scottcate)

[PDF をダウンロードします。](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> 認証サービスでは、認証 cookie を受信するために資格情報を提供するユーザーとは、ASP.NET によって提供されるゲートウェイ サービスのカスタム ユーザー プロファイルを許可します。 ASP.NET AJAX 認証サービスの使用は標準の ASP.NET フォーム認証では、互換性のあるフォーム認証を使用中のアプリケーション (ログインに制御など) は分けることのできない AJAX 認証サービスをアップグレードすること。


## <a name="introduction"></a>はじめに

.NET Framework 3.5 の一部として、Microsoft を生み出しているかなりの数の環境のアップグレードです。新しい開発環境があるだけでなく、統合言語クエリ (LINQ) の新機能とその他の言語拡張機能は近日公開予定です。 さらに、使い慣れたその他のツールセット、目立った例として、ASP.NET AJAX Extensions の機能の一部含まれている .NET Framework の基本クラス ライブラリのファースト クラスのメンバーとして。 これらの拡張機能は、ページ全体の更新、クライアント スクリプト (プロファイル API、ASP.NET を含む)、および広範なクライアント側 API を介して Web サービスにアクセスする機能を必要とせずにページの部分のレンダリングを含む、多くの新しいのリッチ クライアント機能を有効にします。ASP.NET サーバー側コントロール セットに表示するコントロール パターンの多くのミラーに設計されています。

このホワイト ペーパーでは、実装と ASP.NET のプロファイルの使用、およびフォーム認証サービスは Microsoft ASP.NET AJAX ExtensionsThe AJAX 拡張機能で公開されているようにフォーム認証をサポートするには驚くほど簡単 (だけでなくプロファイリング サービス) は、Web サービス プロキシ スクリプトを通じて公開されます。 AJAX 拡張機能では、AuthenticationServiceManager クラスを通じてカスタムの認証もサポートします。

このホワイト ペーパーでは、Visual Studio 2008 の Beta 2 リリースと、.NET Framework 3.5 に基づいています。 このホワイト ペーパーには、いない Visual Web Developer Express、Visual Studio 2008 Beta 2 を操作して Visual Studio のユーザー インターフェイスに従ってチュートリアルを提供するも想定しています。 一部のコード サンプルでは、Visual Web Developer Express で使用できないプロジェクト テンプレートを利用可能性があります。

## <a name="profiles-and-authentication"></a>*プロファイルと認証*

Microsoft ASP.NET プロファイルや認証サービス、ASP.NET フォーム認証システムによって提供され、ASP.NET の標準的なコンポーネントであります。 ASP.NET AJAX 拡張機能は、クライアント AJAX ライブラリの Sys.Services の名前空間で非常に簡単なモデルを使用して、スクリプトのプロキシを介してこれらのサービスにスクリプトのアクセスを提供します。

認証サービスでは、認証 cookie を受信するために資格情報を提供するユーザーとは、ASP.NET によって提供されるゲートウェイ サービスのカスタム ユーザー プロファイルを許可します。 ASP.NET AJAX 認証サービスの使用は標準の ASP.NET フォーム認証では、互換性のあるフォーム認証を使用中のアプリケーション (ログインに制御など) は分けることのできない AJAX 認証サービスをアップグレードすること。

プロファイル サービスは、自動統合および認証サービスによって提供されるように、メンバーシップに基づくユーザー データの記憶域を使用します。 格納されたデータは、web.config ファイルで指定して、さまざまなプロファイリング サービス プロバイダーがデータの管理を処理します。 認証サービスと同様、AJAX プロファイル サービスは、ページが現在 ASP.NET プロファイル サービスの機能を組み込む必要がありますいないによって損なわれている AJAX のサポートを含むように、標準の ASP.NET プロファイル サービスとの互換性。

このホワイト ペーパーのスコープ外では、ASP.NET 認証とプロファイル サービス自体をアプリケーションに組み込むことです。 トピックの詳細については、MSDN ライブラリを参照してください参照資料にメンバーシップを使用したユーザーを管理する[ https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx)です。 ASP.NET には、ASP.NET メンバーシップの既定の認証サービス プロバイダーである SQL Server でのメンバーシップを自動的に設定するためのユーティリティも含まれています。 詳細については、ASP.NET SQL Server の登録ツールの記事を参照してください (Aspnet\_regsql.exe) で[ https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)です。

## <a name="using-the-aspnet-ajax-authentication-service"></a>*ASP.NET AJAX 認証サービスを使用します。*

Web.config ファイルでは、ASP.NET AJAX 認証サービスを有効にする必要があります。

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

認証サービスでは、ASP.NET フォーム認証を有効にするを必要とし、cookie (スクリプトを有効にできません cookie なしのセッション cookie なしのセッションには、URL パラメーターが必要としな) クライアントのブラウザーで有効にする必要があります。

AJAX 認証サービスが有効になっているし、構成されている、クライアント スクリプトすぐに活用できます Sys.Services.AuthenticationService オブジェクト。 クライアント スクリプトを活用するためにしますが、主に、`login`メソッドおよび`isLoggedIn`プロパティです。 いくつかのプロパティは、既定値メソッドを指定する、ログイン、多数のパラメーターを受け取ることができますが存在します。

*Sys.Services.AuthenticationService メンバー*

*ログイン方法:*

Login() メソッドでは、ユーザーの資格情報を認証するための要求を開始します。 このメソッドは、非同期実行をブロックしません。

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| userName | 必須。 認証にユーザー名。 |
| パスワード | 省略可能な (null の既定値)。 ユーザーのパスワード。 |
| isPersistent | 省略可能な (既定値は false)。 かどうか、ユーザーの認証の cookie は、セッション間で保持する必要があります。 False の場合、ユーザーがログアウト、ブラウザーが閉じているか、セッションの有効期限が切れるときにします。 |
| redirectUrl | 省略可能な (null の既定値)。正常な認証時にブラウザーをリダイレクトする URL。 このパラメーターが null または空の文字列の場合は、リダイレクトは行われません。 |
| customInfo | 省略可能な (null の既定値)。 このパラメーターは、現在使用されていないは将来使用するために予約されています。 |
| loginCompletedCallback | 省略可能な (null の既定値)。ログインが正常に完了したときに呼び出す関数。 指定する場合、このパラメーターは defaultLoginCompleted プロパティを上書きします。 |
| failedCallback | 省略可能な (null の既定値)。ログインが失敗したときに呼び出される関数。 指定する場合、このパラメーターは defaultFailedCallback プロパティを上書きします。 |
| userContext | 省略可能な (null の既定値)。 コールバック関数に渡す必要があるカスタム ユーザー コンテキスト データ。 |

*戻り値。*

この関数では、戻り値は含まれません。 ただし、動作の数はこの関数に対する呼び出しの完了時に説明します。

- 現在のページが更新されますか、または場合、変更する、`redirectUrl`パラメーターが null でも、空の文字列。
- ただし、パラメーターが null または空の文字列の場合、`loginCompletedCallback`パラメーター、または`defaultLoginCompletedCallback`プロパティが呼び出されます。
- Web サービスへの呼び出しに失敗した場合、`failedCallback`のパラメーター`defaultFailedCallback`プロパティが呼び出されます。

*logout メソッド:*

Logout() メソッドは、資格情報 cookie を削除し、web アプリケーションから現在のユーザーをログアウトします。

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| redirectUrl | 省略可能な (null の既定値)。正常な認証時にブラウザーをリダイレクトする URL。 このパラメーターが null または空の文字列の場合は、リダイレクトは行われません。 |
| logoutCompletedCallback | 省略可能な (null の既定値)。ログアウトが正常に完了したときに呼び出す関数。 指定する場合、このパラメーターは defaultLogoutCompleted プロパティを上書きします。 |
| failedCallback | 省略可能な (null の既定値)。ログインが失敗したときに呼び出される関数。 指定する場合、このパラメーターは defaultFailedCallback プロパティを上書きします。 |
| userContext | 省略可能な (null の既定値)。 コールバック関数に渡す必要があるカスタム ユーザー コンテキスト データ。 |

*戻り値。*

この関数では、戻り値は含まれません。 ただし、動作の数はこの関数に対する呼び出しの完了時に説明します。

- 現在のページが更新されますか、または場合、変更する、`redirectUrl`パラメーターが null でも、空の文字列。
- ただし、パラメーターが null または空の文字列の場合、`logoutCompletedCallback`パラメーター、または`defaultLogoutCompletedCallback`プロパティが呼び出されます。
- Web サービスへの呼び出しに失敗した場合、`failedCallback`のパラメーター`defaultFailedCallback`プロパティが呼び出されます。

*defaultFailedCallback プロパティ (get、set):*

このプロパティは、web サービスとの通信に障害が発生した場合に呼び出す必要があります関数を指定します。 デリゲート (または関数の参照) を受信してする必要があります。

このプロパティで指定された関数の参照は、次のシグネチャが必要です。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| エラー | エラー情報を指定します。 |
| userContext | ログインまたはログアウト関数が呼び出されたときを指定されたユーザー コンテキスト情報を指定します。 |
| methodName | 呼び出し元のメソッドの名前。 |

*defaultLoginCompletedCallback プロパティ (get、set):*

このプロパティは、ログインの web サービス呼び出しが完了したときに呼び出される関数を指定します。 デリゲート (または関数の参照) を受信してする必要があります。

このプロパティで指定された関数の参照は、次のシグネチャが必要です。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| validCredentials | ユーザーが有効な資格情報を指定するかどうかを指定します。 `true` ユーザーが正常にログインした場合それ以外の場合`false`です。 |
| userContext | ログイン関数が呼び出されたときを指定されたユーザー コンテキスト情報を指定します。 |
| methodName | 呼び出し元のメソッドの名前。 |

*defaultLogoutCompletedCallback プロパティ (get、set):*

このプロパティは、ログアウト web サービス呼び出しが完了したときに呼び出される関数を指定します。 デリゲート (または関数の参照) を受信してする必要があります。

このプロパティで指定された関数の参照は、次のシグネチャが必要です。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| 結果 | このパラメーターは必ず`null`; 将来使用するために予約されています。 |
| userContext | ログイン関数が呼び出されたときを指定されたユーザー コンテキスト情報を指定します。 |
| methodName | 呼び出し元のメソッドの名前。 |

*isLoggedIn プロパティ (get):*

このプロパティは、ユーザーの現在の認証状態を取得します。ページの要求中には、ScriptManager をオブジェクトで設定されます。

このプロパティを返します`true`を返しますそれ以外の場合、ユーザーは現在ログインしている場合、`false`です。

*path プロパティ (get、set):*

このプロパティは、認証 web サービスの場所をプログラムによって決まります。 既定の認証プロバイダーと 1 つをオーバーライドするために使用できる ScriptManager コントロールの認証サービスの子ノードのパスのプロパティで宣言によって設定 (詳細については、使用方法を参照してくださいカスタム認証サービス プロバイダー。下記のトピック) です。

既定の認証サービスの場所で変更されないことに注意してください。 ただし、ASP.NET AJAX には、ASP.NET AJAX 認証サービスのプロキシと同じクラス インターフェイスを提供する web サービスの場所を指定することができます。

このプロパティを現在のサイトからスクリプト要求に指示する値に設定されないことにも注意してください。 現在のアプリケーションでは、認証資格情報が表示されないが、そのことは無意味です。また、テクノロジの基になる AJAX クロスサイト リクエストを投稿してはいけません、クライアントのブラウザーにセキュリティ例外を生成する可能性があります。

このプロパティは、`String`認証 web サービスへのパスを表すオブジェクト。

*timeout プロパティ (get、set):*

このプロパティは、認証サービスのログイン要求と想定するまで待機する時間の長さが失敗したを決定します。 呼び出しが完了するを待機中にタイムアウトになると、要求が失敗しましたコールバックが呼び出されます、呼び出しは完了していません。

このプロパティは、`Number`認証サービスからの結果を待機するミリ秒数を表すオブジェクト。

*認証サービスにログインするコード サンプル。*

次のマークアップは、単純なスクリプトの呼び出し、ログインとログアウト クラスのメソッドに、認証サービスで ASP.NET ページの例です。

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>ASP.NET AJAX を使用してデータをプロファイルにアクセスします。

サービスのプロファイリング、ASP.NET は、ASP.NET AJAX Extensions を通じて公開されています。 ASP.NET プロファイリング サービスには、ユーザー データを格納および取得する、細分化された豊富な API が用意されています、ため、優れた生産性ツールを指定できます。

Web.config; で、プロファイル サービスを有効にする必要があります。既定ではありません。 これを行うことを確認、`profileService`子要素が有効になっている = true に指定された、web.config ファイルとプロパティの読み取りまたは次のように書き込まれることができますが指定されています。

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

プロファイル サービスも構成する必要があります。 プロファイル サービスの構成は、このホワイト ペーパーのスコープ外では、する意義があるグループ プロファイルの構成設定で定義されているをグループ名のサブ プロパティとしてアクセス可能になることに注意してください。 たとえば、次にプロファイルを指定します。

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

名前、およびアクセスする Address.Line1、Address.Line2、Address.City、Address.State、Address.Zip、BackgroundColor ProfileService クラスのプロパティ フィールドのプロパティとしてできるは、クライアント スクリプト。

AJAX プロファイリング サービスが構成されたら、すぐにで使用できるページです。ただし、使用する前に 1 回読み込む必要があります。

*Sys.Services.ProfileService メンバー*

*プロパティ フィールド:*

プロパティ フィールドは、ドット演算子名規約によって参照可能な子のプロパティとして構成済みのプロファイルのすべてのデータを公開します。 プロパティ グループの子であるプロパティは、"GroupName.PropertyName"と呼ばれます。 上に示した例のプロファイルの構成で、ユーザーの状態を取得するに、次の識別子が使用する可能性があります。

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*load メソッド:*

サーバーから、選択した一覧またはすべてのプロパティを読み込みます。

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| propertyNames | 省略可能な (null の既定値)。 サーバーから読み込まれるプロパティです。 |
| loadCompletedCallback | 省略可能な (null の既定値)。 読み込みが完了したときに呼び出す関数。 |
| failedCallback | 省略可能な (null の既定値)。 エラーが発生した場合に呼び出される関数。 |
| userContext | 省略可能な (null の既定値)。 コールバック関数に渡されるコンテキスト情報。 |

Load 関数の戻り値ではありません。 正常に完了しました、これは呼び出しいずれか、`loadCompletedCallback`パラメーターまたは`defaultLoadCompletedCallback`プロパティです。 呼び出しが失敗した、またはいずれかのタイムアウトの期限が切れた場合、`failedCallback`パラメーターまたは`defaultFailedCallback`プロパティが呼び出されます。

場合、`propertyNames`パラメーターが指定されていない、すべての読み取りに構成されたプロパティは、サーバーから取得されます。

*メソッドを保存します。*

Save() メソッドは、指定したプロパティ一覧 (またはすべてのプロパティ) をユーザーの ASP.NET のプロファイルに保存します。

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| propertyNames | 省略可能な (null の既定値)。 サーバーに保存するプロパティです。 |
| saveCompletedCallback | 省略可能な (null の既定値)。 保存するときに呼び出される関数が完了しました。 |
| failedCallback | 省略可能な (null の既定値)。 エラーが発生した場合に呼び出される関数。 |
| userContext | 省略可能な (null の既定値)。 コールバック関数に渡されるコンテキスト情報。 |

ファイルの関数は戻り値がありません。 呼び出しが正常に完了すると、いずれかが呼び出されます、`saveCompletedCallback`パラメーターまたは`defaultSaveCompletedCallback`プロパティです。 呼び出しが失敗した、またはいずれかのタイムアウトの期限が切れた場合、`failedCallback`または`defaultFailedCallback`プロパティが呼び出されます。

場合、`propertyNames`パラメーターが null で、すべてのプロファイル プロパティは、サーバーに送信されますおよびサーバーによって決定されるプロパティを保存できるし、することはできません。

*defaultFailedCallback プロパティ (get、set):*

このプロパティは、web サービスとの通信に障害が発生した場合に呼び出す必要があります関数を指定します。 デリゲート (または関数の参照) を受信してする必要があります。

このプロパティで指定された関数の参照は、次のシグネチャが必要です。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| Error | エラー情報を指定します。 |
| userContext | 時に提供されたユーザー コンテキスト情報を指定します、読み込みまたは保存関数が呼び出されました。 |
| methodName | 呼び出し元のメソッドの名前。 |

*defaultSaveCompleted プロパティ (get、set):*

このプロパティは、ユーザーのプロファイル データの保存の完了時に呼び出す必要があります関数を指定します。 デリゲート (または関数の参照) を受信してする必要があります。

このプロパティで指定された関数の参照は、次のシグネチャが必要です。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| numPropsSaved | 保存されたプロパティの数を指定します。 |
| userContext | 時に提供されたユーザー コンテキスト情報を指定します、読み込みまたは保存関数が呼び出されました。 |
| methodName | 呼び出し元のメソッドの名前。 |

*defaultLoadCompleted プロパティ (get、set):*

このプロパティは、ユーザーのプロファイル データの読み込みの完了時に呼び出す必要があります関数を指定します。 デリゲート (または関数の参照) を受信してする必要があります。

このプロパティで指定された関数の参照は、次のシグネチャが必要です。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*パラメーター:*

| **パラメーター名** | **意味** |
| --- | --- |
| numPropsLoaded | 読み込まれるプロパティの数を指定します。 |
| userContext | 時に提供されたユーザー コンテキスト情報を指定します、読み込みまたは保存関数が呼び出されました。 |
| methodName | 呼び出し元のメソッドの名前。 |

*path プロパティ (get、set):*

このプロパティは、プロファイルの web サービスの場所をプログラムによって決まります。 既定のプロファイル サービス プロバイダーと 1 つをオーバーライドするために使用できる ScriptManager コントロールの ProfileService 子ノードの Path プロパティには宣言して設定します。

既定のプロファイル サービスの場所で変更されないことに注意してください。 ただし、ASP.NET AJAX には、ASP.NET AJAX 認証サービスのプロキシと同じクラス インターフェイスを提供する web サービスの場所を指定することができます。

このプロパティを現在のサイトからスクリプト要求に指示する値に設定されないことにも注意してください。 テクノロジの基になる AJAX では、サイト間で要求を投稿してはいけません、クライアント ブラウザーにセキュリティ例外を生成する可能性があります。

このプロパティは、`String`プロファイル web サービスへのパスを表すオブジェクト。

*timeout プロパティ (get、set):*

このプロパティは、プロファイル サービスの負荷と想定するまで待機するか、要求を保存する時間の長さが失敗したを決定します。 呼び出しが完了するを待機中にタイムアウトになると、要求が失敗しましたコールバックが呼び出されます、呼び出しは完了していません。

このプロパティは、`Number`プロファイル サービスからの結果を待機するミリ秒数を表すオブジェクト。

*コード サンプル: ページの読み込み時のプロファイル データを読み込んでいます*

次のコードでは、ユーザーが認証されたかどうかを確認でき、ページのとして、ユーザーの推奨される背景色を読み込む場合は、です。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*カスタム認証サービス プロバイダーを使用します。*

ASP.NET AJAX 拡張機能を使用すると、カスタム web サービスを通じて、機能を公開するカスタム スクリプト認証サービス プロバイダーを作成できます。 使用するために、web サービスが 2 つのメソッドを公開する必要があります`Login`と`Logout`; 既定の ASP.NET AJAX の認証 web サービスとして、同じメソッド シグネチャを持つこれらのメソッドを指定する必要があります。

カスタム web サービスを作成した後は、コード、またはクライアント スクリプトを使用してプログラムでその宣言によって、ページのいずれかへのパスを指定する必要があります。

*パスを宣言して設定するには。*

パスを設定するには、宣言によって、ASP.NET ページに ScriptManager オブジェクトの認証サービスの子のとおりです。

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*コードでパスを設定します。*

、パスをプログラムで設定するには、スクリプト マネージャーのインスタンスを使用してパスを指定します。

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*スクリプトでパスを設定します。*

、スクリプト内のプログラムでパスを設定する場合、使用、`path`認証サービス クラスのプロパティ。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*カスタム認証用のサンプル Web サービス*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>まとめ

ASP.NET サービス - 具体的には、プロファイリング、メンバーシップ、および認証サービスには、クライアントのブラウザーで JavaScript を簡単に公開されます。 これにより、面倒な作業を行う Updatepanel などのコントロールに依存することがなく、シームレスに認証メカニズムと、クライアント側のコードを統合する開発者です。 Web の構成設定を利用することにより、同様に、クライアントからプロファイル データを保護することができます。既定では、データがないと開発者必要がありますにオプトイン プロファイル プロパティです。

さらに、簡略化された web サービスの実装を同等のメソッド署名を作成する開発者は、これらの組み込みの ASP.NET サービス プロバイダーがカスタム スクリプトを作成できます。 これらの手法のサポートは、さまざまな特定のニーズを満たすための柔軟性を開発者に提供しながら、リッチ クライアント アプリケーションの開発を簡略化します。

## <a name="bio"></a>*Bio*

Scott カテゴリは、1997 年以降の Microsoft の Web テクノロジの使用されているがあり、myKB.com の代表者 ([www.myKB.com](http://www.myKB.com))、専門分野は、ASP.NET の書き込みの際にベースのアプリケーションのナレッジ ベースのソフトウェア ソリューションに重点を置きます。 Scott が接続時に電子メール[ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)または彼のブログで[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [前へ](understanding-asp-net-ajax-updatepanel-triggers.md)
> [次へ](understanding-asp-net-ajax-localization.md)
