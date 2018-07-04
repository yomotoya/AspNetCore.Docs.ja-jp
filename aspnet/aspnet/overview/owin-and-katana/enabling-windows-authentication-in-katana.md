---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Katana で Windows 認証を有効にする |Microsoft Docs
author: MikeWasson
description: この記事では、Katana で Windows 認証を有効にする方法を示しています。 2 つのシナリオについて説明します IIS ホストしていた Katana を使用して、セルフホスト Kat HttpListener を使用しています.。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: e7fbe6af323ecdc09b4d79073f506c5ee056f30f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391616"
---
<a name="enabling-windows-authentication-in-katana"></a>Katana で Windows 認証を有効にします。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

> この記事では、Katana で Windows 認証を有効にする方法を示しています。 2 つのシナリオについて説明します。 IIS ホストしていた Katana を使用して、カスタム プロセスでセルフホスト Katana HttpListener を使用しています。 この記事のレビュー Barry Dorrans、David Matson、および Chris Ross に感謝します。


Katana は Microsoft の実装の[OWIN](http://owin.org/)、Open Web Interface for .NET。 OWIN と Katana の概要については読み取ることができます[ここ](an-overview-of-project-katana.md)します。 OWIN のアーキテクチャには、複数のレイヤーがあります。

- ホスト: は、OWIN パイプラインを実行するプロセスを管理します。
- サーバー: は、ネットワーク ソケットを開き、要求をリッスンします。
- ミドルウェア: は、HTTP 要求と応答を処理します。

Katana には、Windows 統合認証をサポートする 2 つのサーバーが現在提供しています。

- **Microsoft.Owin.Host.SystemWeb**します。 ASP.NET パイプラインでは、IIS を使用します。
- **Microsoft.Owin.Host.HttpListener**します。 使用して[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)します。 このサーバーは現在、既定のオプションと自己ホスト Katana します。

> [!NOTE]
> Katana は提供されていません OWIN ミドルウェアの Windows 認証では、この機能は、サーバーで使用可能な既にあるため。


## <a name="windows-authentication-in-iis"></a>IIS で Windows 認証

Microsoft.Owin.Host.SystemWeb を使用して、単に、IIS で Windows 認証を有効にできます。

「ASP.NET 空の Web アプリケーション」プロジェクト テンプレートを使用して、新しい ASP.NET アプリケーションを作成してみましょう。

![](enabling-windows-authentication-in-katana/_static/image1.png)

次に、NuGet パッケージを追加します。 **ツール**メニューの **ライブラリ パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

という名前のクラスを追加するようになりました`Startup`を次のコード。

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Owin の IIS で実行されている"Hello world"アプリケーションを作成する必要があるだけです。 F5 キーを押してアプリケーションをデバッグします。 "Hello World!"を表示する必要があります。 ブラウザーのウィンドウです。

![](enabling-windows-authentication-in-katana/_static/image2.png)

次に、IIS Express で Windows 認証を有効にします。 **ビュー**メニューの **プロパティ**します。 プロジェクトのプロパティを表示するソリューション エクスプ ローラーでプロジェクト名をクリックします。

**プロパティ**ウィンドウで、設定**匿名認証**に**無効**設定と**Windows 認証**に**有効になっている**します。

![](enabling-windows-authentication-in-katana/_static/image3.png)

Visual Studio からアプリケーションを実行するときに IIS Express とユーザーの Windows 資格情報が必要です。 これを使用して表示できる[Fiddler](http://fiddler2.com/home)または別の HTTP デバッグ ツール。 HTTP 応答例を次に示します。

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

この応答で Www-authenticate ヘッダーを示す、サーバーをサポートしている、[ネゴシエート](http://www.ietf.org/rfc/rfc4559.txt)、Kerberos または NTLM を使用するプロトコル。

後で、サーバーにアプリケーションを展開するときにに従って[手順](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)そのサーバー上の IIS で Windows 認証を有効にします。

## <a name="windows-authentication-in-httplistener"></a>HttpListener で Windows 認証

直接は Windows 認証を有効にする Microsoft.Owin.Host.HttpListener Katana を自己ホストを使用している場合、 **HttpListener**インスタンス。

最初に、新しいコンソール アプリケーションを作成します。 次に、NuGet パッケージを追加します。 **ツール**メニューの **ライブラリ パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

という名前のクラスを追加するようになりました`Startup`を次のコード。

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

このクラスが実装する前に、同じ"Hello world"の例は、認証方法として Windows 認証を設定します。

内で、`Main`関数を OWIN パイプラインを開始します。

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

アプリケーションが Windows 認証を使用していることを確認するように Fiddler では、要求を送信できます。

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>関連トピック

[プロジェクト Katana の概要](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[MVC 5 で OWIN フォーム認証を理解します。](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
