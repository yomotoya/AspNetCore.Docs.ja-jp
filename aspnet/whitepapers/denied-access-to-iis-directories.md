---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET IIS ディレクトリへのアクセスの拒否 |Microsoft Docs
author: rick-anderson
description: このホワイト ペーパーでは、必要な作業が、ASP.NET アプリケーションの要求は、"ディレクトリを拒否する このエラーを返した場合について説明します。 %S に失敗しました.
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: c3a14f51df7aaf5c5935cf60ee4e687c10048e91
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833415"
---
<a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET は、IIS ディレクトリへのアクセスを拒否します。
====================
> このホワイト ペーパーでは、必要な作業が、ASP.NET アプリケーションの要求には、エラーが返された場合について説明します"へのアクセスを拒否*DirectoryName*ディレクトリ。 起動しないディレクトリの変更を監視します。"
> 
> ASP.NET 1.0 と ASP.NET 1.1 に適用されます。


小なりを使用して ASP.NET V1 RTM 実行されるように特権 windows アカウント、ローカル コンピューターで"ASPNET"アカウントとして登録します。

一部のシステムをロック、このアカウントが既定ではなく読み取りが security web サイトのコンテンツ ディレクトリ、アプリケーションのルート ディレクトリまたは web サイトのルート ディレクトリにアクセスです。 ここで指定された web アプリケーションからページを要求するときに、次のエラーを受信は。

![](denied-access-to-iis-directories/_static/image1.jpg)

これを解決するには、適切なディレクトリのセキュリティ アクセス許可を変更する必要があります。

具体的には、ASP.NET には、読み取りが必要です、実行、および web サイトのルートの ASPNET アカウントのアクセスの一覧 (例: c:\inetpub\wwwroot または任意の代替サイト ディレクトリを IIS で構成した可能性があります)、コンテンツのディレクトリとアプリケーションのルート ディレクトリ構成ファイルの変更を監視します。 アプリケーションのルートは、IIS 管理ツール (inetmgr) でアプリケーションの仮想ディレクトリに関連付けられているフォルダーのパスに対応します。

たとえば、wwwroot フォルダーの下で次のアプリケーションの階層があるとします。

`C:\inetpub\wwwroot\myapp\default.aspx`

この例では、ASPNET アカウントには、上記で定義された、myapp と wwwroot ディレクトリの両方でのコンテンツの読み取りアクセス許可が必要があります。 ルート フォルダーに継承された ACL を 1 つも必要に応じてできます両方のディレクトリに対して、入れ子になっている場合。

ディレクトリへのアクセス許可を追加するには、次の手順を実行します。

- Windows エクスプ ローラーを使用して、ディレクトリに移動します
- ディレクトリのフォルダーを右クリックし、[プロパティ] を選択
- [プロパティ] ダイアログの [セキュリティ] タブに移動します
- [追加] ボタンをクリックし、ASPNET アカウント名を続けてコンピューター名を入力します。 たとえば、"webdev"という名前のマシン上には、webdev\ASPNET を入力し、[OK] をクリックするとします。
- ASPNET アカウントを持っていることを確認、"読み取り&amp;Execute"、「リスト フォルダーの内容」、「読み取り」のチェック ボックスがオンします。
- ダイアログ ボックスを閉じるし、変更を保存するには、[ok] をクリックします。

![](denied-access-to-iis-directories/_static/image2.jpg)

必要な場合、Windows でのスクリプトまたはに付属している"cacls.exe"ツールを使用してこれらの変更を自動化できます。 ASPNET アカウントの詳細についてを参照してください、 [FAQ ドキュメント](https://go.microsoft.com/fwlink/?LinkId=5828)します。

指定された web アプリケーション、書き込んだりする依存または特定のフォルダーまたはファイルへのアクセス許可を変更、同じ手順を実行して、「書き込み」や「変更」のチェック ボックスをチェックするには、これを付与できます。

すべてのユーザーまたはユーザー グループのこれらのディレクトリ (つまり、既定の構成) への読み取りアクセスを許可するコンピューターで問題が発生しないと、上記の手順は必要ありません。
