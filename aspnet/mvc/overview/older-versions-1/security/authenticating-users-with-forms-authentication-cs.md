---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: "フォーム認証 (c#) を持つユーザーを認証 |Microsoft ドキュメント"
author: microsoft
description: "[Authorize] 属性を使用する方法について、MVC アプリケーションで特定のページで保護するパスワード。 Web サイトの管理にも使用する方法を学習するとしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 17bcf02e1351587d64b72ee2b40393e0f748f23e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="authenticating-users-with-forms-authentication-c"></a>フォーム認証 (c#) でユーザーの認証
====================
によって[Microsoft](https://github.com/microsoft)

> [Authorize] 属性を使用する方法について、MVC アプリケーションで特定のページで保護するパスワード。 Web サイト管理ツールを使用して作成し、ユーザーおよびロールを管理する方法について学びます。 ユーザー アカウントとロール情報を格納する場所を構成する方法についても説明します。


このチュートリアルの目的は、フォームを使用する方法を説明する認証のパスワードを ASP.NET MVC アプリケーション内のビューを保護します。 Web サイト管理ツールを使用して、ユーザーおよびロールを作成する方法を学びます。 承認されていないユーザーがコント ローラーのアクションを呼び出すことを防止する方法についても説明します。 最後に、ユーザー名とパスワードの格納場所を構成する方法を説明します。

#### <a name="using-the-web-site-administration-tool"></a>Web サイト管理ツールを使用します。

何でも前に一部のユーザーとロールを作成することで開始する必要があります。 新しいユーザーとロールを作成する最も簡単な方法は、Visual Studio 2008 Web サイト管理ツールを活用するためにです。 このツールを起動するには、メニュー オプションを選択して**プロジェクトで、ASP.NET 構成**です。 または、ソリューション エクスプ ローラー ウィンドウの上部に表示される、世界のヒット ハンマーの (ある程度恐ろしい) アイコンをクリックして、Web サイト管理ツールを起動できます (図 1 を参照してください)。

**図 1 – Web サイト管理ツールを起動します。**

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

Web サイト管理ツール内で、[セキュリティ] タブを選択して、新しいユーザーとロールを作成します。クリックして、 **Create user** Stephen をという名前の新しいユーザーを作成するリンク (図 2 を参照してください)。 Stephen ユーザー パスワードを入力、使用する (たとえば、*シークレット*)。

**図 2 – 新しいユーザーを作成します。**

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

新しいロールを作成するには、最初の役割を有効にして 1 つまたは複数のロールを定義します。 クリックしてロールを有効にする、**ロールを有効にする**リンクします。 次に、という名前のロールを作成*管理者* をクリックして、**作成または管理ロール**(図 3 を参照してください) をリンクします。

**図 3 – 新しいロールを作成します。**

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

最後に、Sally をという名前の新しいユーザーを作成し、ユーザーの作成 リンクをクリックし、Sally を作成するときに管理者を選択すると、管理者ロールに Sally を関連付ける (図 4 を参照してください)。

**図 4: ロールにユーザーを追加します。**

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

すべてが完了して、ときに、Stephen および Sally という 2 つの新しいユーザーが必要です。 管理者をという名前の新しいロールも必要です。 Sally 管理者ロールのメンバーは、Stephen です。

#### <a name="requiring-authorization"></a>承認を必要とします。

ユーザーが、アクションに [Authorize] 属性を追加することでコント ローラーのアクションを呼び出す前に認証されるユーザーを要求できます。 個々 のコント ローラー アクションに [Authorize] 属性を適用することができますか、全体のコント ローラー クラスにこの属性を適用することができます。

たとえば、リスト 1 のコント ローラーは、CompanySecrets() をという名前のアクションを公開します。 このアクションが [Authorize] 属性で装飾されているために、ユーザーが認証される場合を除き、この操作を呼び出すことができません。

**1 – controllers \homecontroller.cs を一覧表示します。**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

かどうかは CompanySecrets() 動作を実行して、ブラウザーのアドレス バーに URL/Home/CompanySecrets を入力して、認証されたユーザーが自動的にログイン ビューにリダイレクトされます (図 5 を参照してください)。

**図 5 – ログイン ビュー**

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

ログイン ビューを使用すると、ユーザー名とパスワードを入力します。 登録済みユーザーがいないかどうかをクリックして、**登録**レジスタに移動するリンク (図 6 を参照してください) を表示します。 レジスタ ビューを使用すると、新しいユーザー アカウントを作成します。

**図 6 – レジスタ ビュー**

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

正常にログインした後は、(図 7 を参照してください) を表示 CompanySecrets を表示できます。 既定では、引き続き、ブラウザー ウィンドウを閉じるまでに記録されます。

**図 7-CompanySecrets ビュー**

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>ユーザー名またはユーザー ロールを承認します。

[Authorize] 属性を使用すると、ユーザーまたは特定のユーザー ロールのセットの特定のセットをコント ローラー アクションへのアクセスを制限します。 たとえば、リスト 2 での変更、Home コント ローラーには、2 つの新しいアクション StephenSecrets() および AdministratorSecrets() という名前が含まれています。

**2 – controllers \homecontroller.cs を一覧表示します。**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

Stephen ユーザー名を持つユーザーのみが StephenSecrets() アクションを呼び出すことができます。 その他のすべてのユーザー ログイン ビューにリダイレクトします。 ユーザーのプロパティは、ユーザー アカウント名のコンマ区切り一覧を受け入れます。

管理者ロールのユーザーだけでは、AdministratorSecrets() アクションを呼び出すことができます。 たとえば、Sally は Administrators グループのメンバーであるため、彼女に、AdministratorSecrets() アクションを呼び出します。 その他のすべてのユーザー ログイン ビューにリダイレクトします。 ロール プロパティは、ロール名のコンマ区切り一覧を受け入れます。

#### <a name="configuring-authentication"></a>認証を構成します。

この時点では、思うかもしれませんユーザー アカウントとロールの情報が格納されています。 MVC アプリケーションのアプリ内にある名前付き ASPNETDB.mdf (RANU) SQL Express データベースに既定では、情報が格納されている\_データ フォルダーです。 メンバーシップの使用を開始するときに、このデータベースは、ASP.NET フレームワークによって自動的に生成されます。

ソリューション エクスプ ローラー ウィンドウで、ASPNETDB.mdf データベースを表示するのには、まず、すべてのファイル メニュー オプション、プロジェクトを選択する必要があります。

アプリケーションを開発するときに、既定の SQL Express データベースを使用しては正常です。 ほとんどの場合、ただし、されませんする ASPNETDB.mdf は既定のデータベースを使用して、実稼働アプリケーションです。 その場合は、次の 2 つの手順を完了してユーザー アカウント情報を格納する場所を変更できます。

1. アプリケーション サービスのデータベース オブジェクトを実稼働データベースに追加する -、実稼働データベースを指すようにアプリケーションの接続文字列の変更

最初の手順は、すべての必要なデータベース オブジェクト (テーブルとストアド プロシージャ) を追加する、実稼働データベースには。 ASP.NET SQL Server セットアップ ウィザードを活用するためには、新しいデータベースにこれらのオブジェクトを追加する最も簡単な方法 (図 8 を参照してください)。 このツールを起動するには、Microsoft Visual Studio 2008 プログラム グループから、Visual Studio 2008 コマンド プロンプトを開き、コマンド プロンプトから次のコマンドを実行します。

aspnet\_regsql

**図 8: に、ASP.NET SQL Server セットアップ ウィザード**

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

ASP.NET SQL Server セットアップ ウィザードを使用すると、ネットワーク上の SQL Server データベースを選択し、すべての ASP.NET アプリケーション サービスで必要なデータベース オブジェクトをインストールできます。 データベース サーバーは、ローカル コンピューター上に配置する必要はありません。

> [!NOTE] 
> 
> ASP.NET SQL Server セットアップ ウィザードを使用しない場合は、次のフォルダーに、アプリケーション サービスのデータベース オブジェクトを追加するための SQL スクリプトを見つけることができます。
> 
> > C:\Windows\Microsoft.NET\Framework\v2.0.50727


必要なデータベース オブジェクトを作成した後は、MVC アプリケーションで使用されるデータベース接続を変更する必要があります。 実稼働データベースを指すように、web の構成 (web.config) ファイルで、ApplicationServices 接続文字列を変更します。 たとえば、3 の一覧表示するのには、変更後の接続は、MyProductionDB (元の ApplicationServices 接続文字列コメント アウトされています) という名前のデータベースを指します。

**3-Web.config を一覧表示します。**

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>データベースのアクセス許可を構成します。

データベースへの接続に統合セキュリティを使用する場合は、データベースにログインとして適切な Windows ユーザー アカウントを追加する必要があります。 正しいアカウントは、web サーバーとして、ASP.NET 開発サーバーまたはインターネット インフォメーション サービスを使用するかによって異なります。 正しいユーザー アカウントは、オペレーティング システムによっても異なります。

ASP.NET 開発サーバー (既定の web サーバー Visual Studio で使用される) を使用している場合は、アプリケーションが、Windows ユーザー アカウントのコンテキスト内で実行されます。 その場合は、データベース サーバーのログインとして使用する Windows ユーザー アカウントを追加する必要があります。

または、インターネット インフォメーション サービスを使用している場合、ASPNET アカウントまたは NT AUTHORITY/ネットワーク サービス アカウントをデータベース サーバーのログインとして追加します。 Windows XP を使用している場合は、ASPNET アカウントとしてログインをデータベースに追加します。 Windows Vista または Windows Server 2008 より新しいオペレーティング システムを使用している場合は、データベースのログインとして NT AUTHORITY/ネットワーク サービス アカウントを追加します。

Microsoft SQL Server Management Studio を使用して、データベースに新しいユーザー アカウントを追加することができます (図 9 を参照してください)。

**図 9 – 新しい Microsoft SQL Server ログインを作成します。**

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

必要なログインを作成した後は、適切なデータベース ロールを持つデータベース ユーザーにログインをマップする必要があります。 ログインをダブルクリックし、[ユーザー マッピング] タブを選択します。1 つまたは複数のアプリケーション サービス データベース ロールを選択します。 たとえば、ユーザーを認証するために有効にする必要は aspnet\_メンバーシップ\_BasicAccess データベース ロール。 新しいユーザーを作成するために、aspnet を有効にする必要があります。\_メンバーシップ\_FullAccess データベース ロール (図 10 参照)。

**図 10-アプリケーション サービス データベース ロールの追加**

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a>概要

このチュートリアルでは、ASP.NET MVC アプリケーションをビルドする際に、フォーム認証を使用する方法について学習しました。 最初に、Web サイト管理ツールを活用して、新しいユーザーとロールを作成する方法を学習します。 次に、[Authorize] 属性を使用して、不正なユーザーがコント ローラーのアクションを呼び出すことを防止する方法を学習します。 最後に、実稼働データベースでユーザーとロール情報を格納する MVC アプリケーションを構成する方法を学習しました。

>[!div class="step-by-step"]
[次へ](authenticating-users-with-windows-authentication-cs.md)
