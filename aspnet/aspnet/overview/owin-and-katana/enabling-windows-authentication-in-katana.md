---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: "Katana で Windows 認証を有効化 |Microsoft ドキュメント"
author: MikeWasson
description: "この記事では、Katana で Windows 認証を有効にする方法を示します。 2 つのシナリオに対応します IIS を使用してホスト Katana、を使用して HttpListener を Kat を自己ホストしています.。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: cc23a053fb1ba60ea84eca59e99f0e375fefc4cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="enabling-windows-authentication-in-katana"></a>Katana で Windows 認証を有効にします。
====================
によって[Mike Wasson](https://github.com/MikeWasson)

> この記事では、Katana で Windows 認証を有効にする方法を示します。 2 つのシナリオに対応します。 IIS を使用してホスト Katana、を使用して HttpListener をカスタム プロセスでセルフホスト Katana。 この記事のレビュー Barry Dorrans、David Matson および Chris Ross に感謝します。


Katana は Microsoft の実装[OWIN](http://owin.org/).NET の Web インターフェイスを開きます。 OWIN と Katana の概要を読み取ることができる[ここ](an-overview-of-project-katana.md)です。 OWIN アーキテクチャでは、複数のレイヤーがあります。

- ホストは、OWIN パイプラインを実行するプロセスを管理します。
- サーバー: は、ネットワーク ソケットを開き、要求をリッスンします。
- ミドルウェア: は、HTTP 要求と応答を処理します。

現在、Katana には、Windows 統合認証をサポートする 2 つのサーバーが提供しています。

- **Microsoft.Owin.Host.SystemWeb**です。 ASP.NET パイプラインを持つには、IIS を使用します。
- **Microsoft.Owin.Host.HttpListener**です。 使用して[System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx)です。 このサーバーは現在、既定のオプションがセルフホストされているときに Katana です。

> [!NOTE]
> Katana は現在提供されませんの OWIN ミドルウェア Windows 認証のため、この機能は、サーバーに表示されています。


## <a name="windows-authentication-in-iis"></a>IIS で Windows 認証

Microsoft.Owin.Host.SystemWeb を使用すると、単に、IIS で Windows 認証を有効にできます。

「ASP.NET 空の Web アプリケーション」のプロジェクト テンプレートを使用して、新しい ASP.NET アプリケーションを作成して開始しましょう。

![](enabling-windows-authentication-in-katana/_static/image1.png)

次に、NuGet パッケージを追加します。 **ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

という名前のクラスを今すぐ追加`Startup`を次のコード。

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

IIS で実行されている、OWIN の"Hello world"アプリケーションを作成する必要がありますすべてです。 F5 キーを押してアプリケーションをデバッグします。 "Hello World!"が表示されます。 ブラウザー ウィンドウです。

![](enabling-windows-authentication-in-katana/_static/image2.png)

次に、私たちは IIS Express での Windows 認証を有効にします。 **ビュー**メニューの **プロパティ**です。 プロジェクトのプロパティを表示するソリューション エクスプ ローラーでプロジェクト名をクリックします。

**プロパティ**ウィンドウで、設定**匿名認証**に**無効になっている**設定と**Windows 認証**に**有効になっている**です。

![](enabling-windows-authentication-in-katana/_static/image3.png)

Visual Studio からアプリケーションを実行するときに IIS Express とユーザーの Windows 資格情報が必要です。 使用してこれが表示[Fiddler](http://fiddler2.com/home)または別の HTTP デバッグ ツールです。 HTTP 応答の使用例を次に示します。

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

この応答で Www-authenticate ヘッダーは、サーバーをサポートしていることを示すため、 [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) Kerberos または NTLM を使用するプロトコル。

その後、サーバーにアプリケーションを展開するときに次の[手順](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)をそのサーバーに IIS で Windows 認証を有効にします。

## <a name="windows-authentication-in-httplistener"></a>HttpListener で Windows 認証

Microsoft.Owin.Host.HttpListener Katana を自己ホストを使用している場合は上で直接に Windows 認証が有効にすることができます、 **HttpListener**インスタンス。

最初に、新しいコンソール アプリケーションを作成します。 次に、NuGet パッケージを追加します。 **ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

という名前のクラスを今すぐ追加`Startup`を次のコード。

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

このクラスを実装する前に、同じ"Hello world"の例は、認証方法として Windows 認証を設定します。

内部、`Main`関数を OWIN パイプラインを開始します。

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Fiddler は、アプリケーションが Windows 認証を使用していることを確認するには、要求を送信することができます。

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>関連トピック

[プロジェクトの Katana の概要](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx)

[MVC 5 の OWIN フォーム認証を理解します。](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
