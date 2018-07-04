---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: フォーム認証 (VB) を使用したユーザー認証 |Microsoft Docs
author: microsoft
description: '[Authorize] 属性を使用する方法について説明します、MVC アプリケーションで特定のページで保護するパスワード。 Web サイトの管理をも使用する方法を学習します.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 4c94cc0d44ec2ef5e300567a07664cd7e6605199
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369374"
---
<a name="authenticating-users-with-forms-authentication-vb"></a>フォーム認証 (VB) でユーザーを認証します。
====================
によって[Microsoft](https://github.com/microsoft)

> [Authorize] 属性を使用する方法について説明します、MVC アプリケーションで特定のページで保護するパスワード。 Web サイトの管理ツールを使用して作成し、ユーザーとロールを管理する方法について説明します。 また、ユーザー アカウントとロールの情報が格納される場所を構成する方法も説明します。


このチュートリアルの目的は、フォームを使用する方法について説明するパスワードへの認証は、ASP.NET MVC アプリケーション内のビューを保護します。 Web サイトの管理ツールを使用して、ユーザーとロールを作成する方法について説明します。 また、不正なユーザーがコント ローラー アクションを呼び出すことを防止する方法も説明します。 最後に、ユーザー名とパスワードを格納する場所を構成する方法について説明します。

#### <a name="using-the-web-site-administration-tool"></a>Web サイトの管理ツールを使用します。

他のことを行う前にいくつかのユーザーとロールの作成から始めます。 新しいユーザーとロールを作成する最も簡単な方法は、Visual Studio 2008 Web サイトの管理ツールを活用するためにです。 このツールを起動するには、メニュー オプションを選択して**プロジェクト、ASP.NET 構成**します。 または、ソリューション エクスプ ローラー ウィンドウの上部に表示される、世界中に達するハンマーの (多少恐ろしい) アイコンをクリックして、Web サイト管理ツールを起動できます (図 1 参照)。

**図 1 – Web サイトの管理ツールを起動します。**

![clip_image002[4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

Web サイト管理ツールで、[セキュリティ] タブを選択して、新しいユーザーとロールを作成します。をクリックして、**ユーザーの作成**Stephen をという名前の新しいユーザーを作成するリンク (図 2 参照)。 Stephen ユーザーが任意のパスワードを提供 (たとえば、*シークレット*)。

**図 2-新しいユーザーの作成**

![clip_image004[4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

新しいロールを作成するには、最初の役割を有効にして 1 つまたは複数のロールを定義します。 クリックしてロールを有効にする、**の役割を有効にする**リンク。 という名前のロールを次に、作成*管理者*をクリックして、**を作成または管理ロール**(図 3 参照) をリンクします。

**図 3 – 新しいロールの作成**

![clip_image006[4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

最後に、Sally をという名前の新しいユーザーを作成し、ユーザーの作成リンクをクリックし、Sally を作成するときに、管理者を選択すると、Sally を管理者ロールに関連付ける (図 4 参照)。

**図 4 – ロールにユーザーを追加します。**

![clip_image008[4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

すべての作業を行う、ときに、Stephen Sally という 2 つの新しいユーザーが必要です。 管理者をという名前の新しいロールも必要です。 Sally 管理者ロールのメンバーであるし、Stephen はありません。

#### <a name="requiring-authorization"></a>承認を必要とします。

ユーザーがアクションに [Authorize] 属性を追加することでコント ローラーのアクションを呼び出す前に認証されるユーザーを要求することができます。 [Authorize] 属性を適用するには、個々 のコント ローラー アクションにまたは全体のコント ローラー クラスにこの属性を適用することができます。

たとえば、リスト 1 で、コント ローラーは、CompanySecrets() という名前のアクションを公開します。 このアクションが [Authorize] 属性で装飾されているため、この操作は、ユーザーが認証されていない場合に呼び出すことができません。

**1 – Controllers\HomeController.vb を一覧表示します。**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

かどうかは、ブラウザーのアドレス バーに URL/Home/CompanySecrets を入力して CompanySecrets() アクションを呼び出すと、認証されたユーザーは、自動的にログイン ビューにリダイレクトされます (図 5 を参照してください)。

**図 5 – ログイン ビュー**

![clip_image010 [4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

ログイン ビューを使用すると、ユーザー名とパスワードを入力します。 登録済みユーザーがいないかどうかは、クリックすることができます、**登録**レジスタに移動するリンク (図 6 参照) を表示します。 Register ビューを使用すると、新しいユーザー アカウントを作成します。

**図 6 – Register ビュー**

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

正常にログインした後 (図 7 を参照してください) を表示する CompanySecrets を確認できます。 既定では、ブラウザー ウィンドウを閉じるまでにログインする続けます。

**図 7-CompanySecrets ビュー**

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>ユーザー名またはユーザー ロールで認証します。

[Authorize] 属性を使用すると、ユーザーまたは特定のユーザー ロールのセットの特定のセットをコント ローラー アクションへのアクセスを制限します。 たとえば、リスト 2 で修正されたホーム コント ローラーには、StephenSecrets() AdministratorSecrets() という 2 つの新しいアクションが含まれています。

**2 – Controllers\HomeController.vb を一覧表示します。**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

Stephen ユーザー名を持つユーザーのみが StephenSecrets() アクションを呼び出すことができます。 その他のすべてのユーザー ログイン ビューにリダイレクトされます。 ユーザー プロパティは、ユーザー アカウント名のコンマ区切りリストを受け入れます。

管理者ロールのユーザーのみが AdministratorSecrets() アクションを呼び出すことができます。 たとえば、Sally は、Administrators グループのメンバーであるため彼女 AdministratorSecrets() アクションに呼び出すことができます。 その他のすべてのユーザー ログイン ビューにリダイレクトされます。 ロールのプロパティは、ロール名のコンマ区切りの一覧を受け入れます。

#### <a name="configuring-authentication"></a>認証を構成します。

この時点では、かもしれません。 ユーザー アカウントとロールの情報が格納されています。 MVC アプリケーションのアプリ内にある名前付き ASPNETDB.mdf (RANU) SQL Express データベースに既定では、情報が格納されている\_データ フォルダー。 メンバーシップの使用を開始すると、このデータベースは、ASP.NET フレームワークによって自動的に生成します。

ソリューション エクスプ ローラー ウィンドウにある ASPNETDB.mdf データベースを確認するためには、まず、すべてのファイル メニュー オプション、プロジェクトを選択する必要があります。

アプリケーションを開発する際に、この既定の SQL Express データベースを使用することは問題ありません。 ほとんどの場合、ただし、しませんする実稼働アプリケーションの既定値にある ASPNETDB.mdf データベースを使用します。 その場合は、次の 2 つの手順を実行してユーザー アカウント情報を格納する場所を変更できます。

1. アプリケーション サービスのデータベース オブジェクトを実稼働データベースに追加する - アプリケーションの運用データベースを指す接続文字列の変更

最初の手順は、実稼働データベースに、すべての必要なデータベース オブジェクト (テーブルとストアド プロシージャ) を追加します。 新しいデータベースにこれらのオブジェクトを追加する最も簡単な方法が ASP.NET SQL Server セットアップ ウィザードを活用するためには (図 8 参照)。 このツールを起動するには、Microsoft Visual Studio 2008 プログラム グループから、Visual Studio 2008 コマンド プロンプトを開き、コマンド プロンプトから次のコマンドを実行します。

aspnet\_regsql

**8-ASP.NET SQL Server のセットアップ ウィザードを図します。**

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

ASP.NET SQL Server セットアップ ウィザードを使用すると、ネットワーク上の SQL Server データベースを選択し、すべての ASP.NET アプリケーション サービスが必要なデータベース オブジェクトをインストールできます。 データベース サーバーは、ローカル コンピューター上にある必要はありません。

> [!NOTE]
> ASP.NET SQL Server セットアップ ウィザードを使用しない場合は、次のフォルダーで、アプリケーション サービスのデータベース オブジェクトを追加するための SQL スクリプトを見つけることができます。
> 
> 
> C:\Windows\Microsoft.NET\Framework\v2.0.50727


必要なデータベース オブジェクトを作成した後は、MVC アプリケーションで使用されるデータベース接続を変更する必要があります。 Web 構成 (web.config) ファイルで、ApplicationServices 接続文字列を変更するは、実稼働データベースを指すようにします。 たとえば、リスト 3 の変更後の接続は MyProductionDB (元の ApplicationServices 接続文字列コメント アウトされている) という名前のデータベースを指します。

**3-Web.config を一覧表示します。**

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>データベースのアクセス許可の構成

統合セキュリティを使用して、データベースに接続する場合は、データベースにログインとして適切な Windows ユーザー アカウントを追加する必要があります。 正しいアカウントは、web サーバーとして、ASP.NET 開発サーバーまたはインターネット インフォメーション サービスを使用するかによって異なります。 適切なユーザー アカウントは、オペレーティング システムによっても異なります。

ASP.NET 開発サーバー (既定 web サーバー Visual Studio で使用される) を使用している場合、アプリケーションは、Windows ユーザー アカウントのコンテキスト内で実行します。 その場合は、データベース サーバーのログインとして、Windows ユーザー アカウントを追加する必要があります。

または、インターネット インフォメーション サービスを使用している場合する必要があります、ASPNET アカウントまたは NT AUTHORITY/NETWORK SERVICE アカウントをデータベース サーバーのログインとして追加します。 Windows XP を使用している場合は、ASPNET アカウントとしてログインをデータベースに追加します。 Windows Vista または Windows Server 2008 より新しいオペレーティング システムを使用している場合は、データベースのログインとして NT AUTHORITY/NETWORK SERVICE アカウントを追加します。

Microsoft SQL Server Management Studio を使用して、データベースに新しいユーザー アカウントを追加することができます (図 9 参照)。

**図 9: 新しい Microsoft SQL Server ログインを作成します。**

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

必要なログインを作成した後は、適切なデータベースの役割を持つデータベース ユーザー ログインをマップする必要があります。 ログインをダブルクリックし、ユーザー マッピング タブを選択します。1 つまたは複数のアプリケーション サービス データベース ロールを選択します。 たとえば、aspnet を有効にする必要するユーザーを認証するために\_メンバーシップ\_BasicAccess データベース ロール。 新しいユーザーを作成するには、aspnet を有効にする必要があります。\_メンバーシップ\_FullAccess データベース ロール (図 10 参照)。

**図 10-アプリケーション サービス データベース ロールの追加**

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a>まとめ

このチュートリアルでは、ASP.NET MVC アプリケーションを構築するときに、フォーム認証を使用する方法について説明しました。 最初に、Web サイトの管理ツールを活用して、新しいユーザーとロールを作成する方法を学習しました。 次に、[Authorize] 属性を使用して、不正なユーザーがコント ローラー アクションを呼び出すことを防止する方法を学習しました。 最後に、実稼働データベースでユーザーとロール情報を格納する MVC アプリケーションを構成する方法を学習しました。

> [!div class="step-by-step"]
> [前へ](preventing-javascript-injection-attacks-cs.md)
> [次へ](authenticating-users-with-windows-authentication-vb.md)
