---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: Windows 認証 (VB) でユーザー |Microsoft Docs
author: microsoft
description: MVC アプリケーションのコンテキストで Windows 認証を使用する方法について説明します。 アプリケーションの web co 内での Windows 認証を有効にする方法について説明します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: fd14ae32c286fb67cf75cc103f6a7969c4b5a731
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396690"
---
<a name="authenticating-users-with-windows-authentication-vb"></a>Windows 認証 (VB) でユーザー
====================
によって[Microsoft](https://github.com/microsoft)

> MVC アプリケーションのコンテキストで Windows 認証を使用する方法について説明します。 アプリケーションの web 構成ファイル内で Windows 認証を有効にする方法と iis 認証を構成する方法について説明します。 最後に、[Authorize] 属性を使用して、特定の Windows ユーザーまたはグループにコント ローラー アクションへのアクセスを制限する方法について説明します。


このチュートリアルの目標は、利用を実行できる方法について説明する、セキュリティのパスワードには、インターネット インフォメーション サービスに組み込まれている機能は、MVC アプリケーション内のビューを保護します。 特定の Windows ユーザーまたは特定の Windows グループのメンバーであるユーザーによってのみ呼び出されるコント ローラー アクションを許可する方法について説明します。

Windows 認証を使用して意味を会社の内部 web サイト (イントラネット サイト) を作成すると、ユーザー、web サイトにアクセスするときに、標準の Windows ユーザー名とパスワードを使用できるようにします。 Web サイト (Internet の web サイト、) に接続するの外側を構築している場合は、代わりにフォーム認証の使用を検討してください。

#### <a name="enabling-windows-authentication"></a>Windows 認証を有効にします。

新しい ASP.NET MVC アプリケーションを作成するときに Windows 認証は既定で有効になっていません。 フォーム認証は、MVC アプリケーションを有効になっている既定の認証の種類です。 MVC アプリケーションの web の構成 (web.config) ファイルを変更して、Windows 認証を有効にする必要があります。 検索、&lt;認証&gt;セクションし、このようなフォーム認証ではなく Windows を使用するように変更します。

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

Windows 認証を有効にすると、web サーバーはユーザーを認証するための責任になります。 通常、2 種類の作成と、ASP.NET MVC アプリケーションを展開するときに使用する web サーバーがあります。

最初に、MVC アプリケーションの開発中には、Visual Studio に付属する ASP.NET 開発 Web サーバーを使用します。 既定では、ASP.NET 開発 Web サーバーは、現在の Windows アカウント (任意のアカウントが Windows にログオンするため) のコンテキストですべてのページを実行します。

ASP.NET 開発 Web サーバーには、NTLM 認証もサポートしています。 ソリューション エクスプ ローラー ウィンドウで、プロジェクトの名前を右クリックし、プロパティ を選択して、NTLM 認証を有効にすることができます。 次に、[Web] タブを選択し、NTLM のチェック ボックスをオン (図 1 参照)。

**図 1 – 有効にすると、ASP.NET 開発 Web サーバーの NTLM 認証**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

実稼働 web アプリケーションでは、一方でとして使用する IIS web サーバー。 IIS では、認証などのいくつかの種類をサポートします。

- 基本認証 – は、HTTP 1.0 プロトコルの一部として定義します。 ユーザー名とパスワードをクリア テキスト (Base64 エンコード) で、インターネット経由で送信します。 ダイジェスト認証 – インターネット経由でそれ自体には、パスワードの代わりに、パスワードのハッシュを送信します。 -統合された Windows (NTLM) 認証 – windows を使用するイントラネット環境で使用する認証の最適な型です。 -証明書の認証: クライアント側の証明書を使用して認証を有効にします。 証明書は、Windows ユーザー アカウントにマップされます。

> [!NOTE] 
> 
> これらの認証の種類の詳細な概要については、次を参照してください。 [ https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx)します。


インターネット インフォメーション サービス マネージャーを使用して、特定の種類の認証を有効にすることができます。 すべての種類の認証はすべてのオペレーティング システムの場合は使用できないことに注意します。 さらに、Windows Vista で IIS 7.0 を使用する場合、インターネット インフォメーション サービス マネージャーに表示される前に、別の種類の Windows 認証を有効にする必要があります。 開いている**コントロール パネル、プログラム、プログラムと機能を有効にする Windows 機能のオンまたはオフ**、インターネット インフォメーション サービス ノードを展開し、(図 2 参照)。

**図 2 – Windows IIS を有効にする機能**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

インターネット インフォメーション サービスを使用して、有効または、さまざまな種類の認証を無効にできます。 たとえば、図 3 に示します匿名認証を無効にして、統合 Windows (NTLM) 認証を有効にすると IIS 7.0 を使用する場合。

**図 3: 統合 Windows 認証を有効にします。**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Windows を承認するユーザーとグループ

使用することが Windows 認証を有効にした後、 &lt;Authorize&gt;コント ローラーまたはコント ローラー アクションへのアクセスを制御する属性。 この属性は、MVC コント ローラーが全体または特定のコント ローラー アクションに適用できます。

たとえば、リスト 1 で、Home コント ローラーは、Index()、CompanySecrets()、StephenSecrets() という 3 つのアクションを公開します。 Index() アクションだれでも呼び出すことができます。 ただし、Windows のローカル管理者グループのメンバーだけが CompanySecrets() アクションを呼び出すことができます。 最後に、(Redmond ドメイン) で Stephen という名前の Windows ドメイン ユーザーのみが StephenSecrets() アクションを呼び出すことができます。

**1 – Controllers\HomeController.vb を一覧表示します。**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> により Windows ユーザー アカウント制御 (UAC)、Windows Vista または Windows Server 2008 を使用する場合、ローカルの Administrators グループの他のグループとは異なる動作はします。 &lt;Authorize&gt;コンピューターの UAC 設定を変更しない限り、属性は、ローカルの Administrators グループのメンバーを認識正しくしません。


適切なアクセス許可でなくてコント ローラーのアクションを呼び出すしようとしたときの動作については、有効な認証の種類によって異なります。 既定では、ASP.NET 開発サーバーを使用する場合、空白のページだけを取得します。 ページが提供され、 **401 未承認**HTTP 応答の状態。

かどうか、その一方で、匿名認証を無効になっていると、基本認証を有効にすると、IIS を使用しているし、保護されたページを要求するたびに、ログイン ダイアログ プロンプト続けて表示 (図 4 参照)。

**図 4 – 基本認証のログイン ダイアログ ボックス**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>まとめ

このチュートリアルでは、ASP.NET MVC アプリケーションのコンテキストで Windows 認証を使用する方法について説明します。 アプリケーションの web 構成ファイル内で Windows 認証を有効にする方法と iis 認証を構成する方法を学習しました。 最後に、使用する方法を学習しました、 &lt;Authorize&gt;属性を特定の Windows ユーザーまたはグループにコント ローラー アクションへのアクセスを制限します。

> [!div class="step-by-step"]
> [前へ](authenticating-users-with-forms-authentication-vb.md)
> [次へ](preventing-javascript-injection-attacks-vb.md)
