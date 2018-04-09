---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET は IIS ディレクトリへのアクセスを拒否 |Microsoft ドキュメント
author: rick-anderson
description: このホワイト ペーパーでは、必要なことを ASP.NET アプリケーションに要求が、"ディレクトリ名のディレクトリを拒否する エラーを返す場合について説明します。 %S に失敗しました.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: d95423776a6b58fc67ae6c791685543dadd2480c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
<a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET は、IIS ディレクトリへのアクセスを拒否します。
====================
> このホワイト ペーパーでは、必要なことを ASP.NET アプリケーションに要求が、エラーを返した場合について説明します"へのアクセスを拒否*ディレクトリ名*ディレクトリ。 開始できませんでしたディレクトリの変更を監視します。"
> 
> ASP.NET 1.0 および 1.1 を ASP.NET に適用されます。


ASP.NET V1 RTM 小を使用してを実行して今すぐローカル コンピューター上に"ASPNET"アカウントとして登録されている windows アカウントの特権モードします。

一部のシステムがロックされて、このアカウント読み取ることが既定ではなくがセキュリティのアクセスを web サイトのコンテンツ ディレクトリ、アプリケーションのルート ディレクトリ、または web サイトのルート ディレクトリ。 ここで指定された web アプリケーションからページを要求するときに、次のエラーが表示されます。

![](denied-access-to-iis-directories/_static/image1.jpg)

これを解決するには、適切なディレクトリのセキュリティ アクセス許可を変更する必要があります。

具体的には、ASP.NET には、読み取りが必要です、execute、および web サイトのルートの ASPNET アカウントのアクセスの一覧 (例: c:\inetpub\wwwroot または任意の代替サイト ディレクトリを IIS で構成した可能性があります)、コンテンツのディレクトリとアプリケーションのルート ディレクトリ構成ファイルの変更を監視するのにはします。 アプリケーションのルートは、IIS 管理ツール (inetmgr) でアプリケーションの仮想ディレクトリに関連付けられているフォルダーのパスに対応しています。

たとえば、wwwroot フォルダーの下にある次のアプリケーションの階層があるとします。

`C:\inetpub\wwwroot\myapp\default.aspx`

この例では、ASPNET アカウントには、上記で定義された、myapp と wwwroot ディレクトリの両方でコンテンツの読み取りアクセス許可が必要があります。 ルート フォルダーの 1 つの継承された ACL ことも必要に応じてできます両方のディレクトリに対して、入れ子になっている場合。

ディレクトリにアクセス許可を追加するには、次の手順を実行します。

- Windows エクスプ ローラーを使用して、ディレクトリに移動します。
- ディレクトリのフォルダーを右クリックし、[プロパティ] を選択
- [プロパティ] ダイアログ ボックスの [セキュリティ] タブに移動します。
- [追加] ボタンをクリックし、ASPNET アカウント名を続けてコンピューター名を入力します。 たとえば、"webdev"という名前のマシン上には、webdev\ASPNET を入力し、[OK] をクリックするとします。
- ASPNET アカウントを持っていることを確認、"読み取り&amp;Execute"、「フォルダーの内容の一覧」および「読み取り」のチェック ボックスがオンになっています。
- ダイアログ ボックスを閉じるし、変更を保存するには、[ok] をクリックします。

![](denied-access-to-iis-directories/_static/image2.jpg)

必要な場合、Windows にスクリプトまたは、付属する"cacls.exe"ツールを使用してこれらの変更を自動化できます。 詳細については、ASPNET アカウントを参照してください、 [FAQ ドキュメント](https://go.microsoft.com/fwlink/?LinkId=5828)です。

場合は、指定された web アプリケーションでは、書き込みに必要か、特定のフォルダーまたはファイルへのアクセス許可を変更、同じ手順に従って、「書き込み」または「変更」のチェック ボックスをチェックしてこれを許可できます。

すべてのユーザーまたはユーザー グループの (これは既定の構成)、これらのディレクトリへの読み取りアクセスを許可するコンピューターでは、問題が発生しないと、上記の手順は必要ありません。
