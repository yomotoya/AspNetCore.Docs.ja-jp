---
uid: whitepapers/ms03-32-issue
title: "IE のセキュリティ更新プログラムを適用した後に 'サーバー アプリケーションが使用できない' エラーの修正プログラム |Microsoft ドキュメント"
author: rick-anderson
description: "このペーパーでは、セキュリティ更新プログラムの適用 ms 03 32 Wi で実行されている ASP.NET 1.0 アプリケーションに影響する Internet Explorer の問題を修正する修正プログラムについて説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: 8658e387aeb4ea0340080666906b2b89db49a31a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>IE のセキュリティ更新プログラムを適用した後に 'サーバー アプリケーションが使用できない' エラーの修正します。
====================
> このホワイト ペーパーでは、Windows XP Professional で実行されている ASP.NET 1.0 アプリケーションに影響する Internet Explorer の ms 03 32 セキュリティ更新プログラムの問題を修正する修正プログラムについて説明します。
> 
> ASP.NET 1.0 および Windows XP Professional に適用されます。


Microsoft では、Internet Explorer のセキュリティ パッチの ms 03 32 セキュリティ更新プログラムと Windows XP で実行されている ASP.NET 1.0 で問題が識別されます。 手動または Windows Update サイトから最新の重要な更新プログラムを取得することによって、この修正プログラムをインストールできます。

この問題の現象は、修正プログラムをインストールすると、Windows XP マシンをローカルの IIS 5.1 web サーバーで実行されている ASP.NET アプリケーションをすべての要求になる「サーバー アプリケーションは使用できません」というエラー メッセージです。 リモート web サーバーへの要求は、影響を受けません。

この問題は、Windows XP で ASP.NET 1.0 を実行しているインストールにのみ影響を及ぼします。 Windows 2000 または Windows Server 2003 を実行しているコンピューターは影響しません。 ASP.NET 1.1 がインストールされていると、Windows XP を実行しているコンピューターも影響しません。

注意してくださいこの問題**は**asp.net セキュリティ バグです。 これは、**しない**開くか、ASP.NET アプリケーションまたはサーバーに対して悪意のある攻撃を許可します。 代わりに、純粋な機能が原因のバグ パッチ自体を勧めします。

取り組んでいますでこの問題の永続的なソリューションです。 その間は、問題の回避策として、次のバッチ ファイルを実行できます。 バッチ ファイルは次のとおり

1. IIS と ASP.NET 状態サービスを停止します。
2. 削除し、既知の一時的なパスワードを使用して、ASPNET アカウントを再作成
3. Windows を使用して`runas`ASPNET ユーザー プロファイルを作成する実行可能ファイルを起動するコマンド
4. ASP.NET を再登録します。 これは、アカウントの新しいランダムなパスワードを作成し、その既定の ASP.NET アクセス制御の設定を適用します
5. IIS サービスを再起動します。

バッチ ファイルにはハードコードされた一時パスワードが含まれています"**1pass@word**"runas コマンドをバッチ ファイルの実行用の入力を求めできます。 Runas コマンドが完了したら、ASPNET アカウントのパスワードは強力なランダムな値で再作成します。 ハードコードされたパスワードは、環境でパスワードの複雑さの要件を満たしていない場合、バッチ ファイルが失敗することに注意してください。 場合がある場合は、環境に適したその他の値を変更できます。

*> [!IMPORTANT]*カスタム アクセス制御の設定や、ASPNET アカウントのデータベース アカウントのアクセス許可を追加した場合は、このバッチ ファイルの完了後に再作成する必要があります。 これは、新しいセキュリティ識別子 (SID) が取得されるアカウントが再作成されるためです。

*> [!IMPORTANT]*ASPNET アカウント以外のカスタム アカウントを使用して、ASP.NET ワーカー プロセスを実行している場合、このバッチ ファイルいない実行する必要があります。 代わりに対話的ログオンするかはそのアカウントのユーザー プロファイルを作成するアカウントで runas コマンドを使用する必要があります。

バッチ ファイルは、以下の自己解凍形式のアーカイブに含まれます。 使用します。

1. 管理者特権を持つアカウントとして実行しなければなりません
2. [ダウンロードして、自己解凍実行可能ファイルを開きます](ms03-32-issue/_static/fixup1.exe)
3. C:\ にコンテンツを抽出します。
4. スタート メニューから実行をしてください... を選択し、入力`cmd.exe`
5. 開く コマンド ウィンドウで次のように入力します。`c:\fixup.cmd`です。
6. メッセージが表示されたら、入力 **1pass@word** パスワードとして。
7. 以前のカスタム アクセス制御の設定や、ASPNET アカウントのデータベース アカウントのアクセス許可があれば、これらの設定を今すぐ再適用する必要があります。

これが原因となった不便多く申し訳ありません。 利用可能になった追加情報を掲載します。

次の表には、プラットフォームおよびバージョンがこの問題による影響について説明します。

| .NET Framework | プラットフォーム | 影響を受ける |
| --- | --- | --- |
| バージョン 1.0 | Windows 2000 Professional | いいえ |
| バージョン 1.0 | Windows 2000 Server | いいえ |
| バージョン 1.0 | Windows XP Professional | はい |
| バージョン 1.0 | Windows Server 2003 | いいえ |
| バージョン 1.0 | Cassini と Windows XP Home | いいえ |
| バージョン 1.1 | Windows 2000 Professional | いいえ |
| バージョン 1.1 | Windows 2000 Server | いいえ |
| バージョン 1.1 | Windows XP Professional | いいえ |
| バージョン 1.1 | Windows Server 2003 | いいえ |
| バージョン 1.1 | Cassini と Windows XP Home | いいえ |

ありがとうございました、   
 ASP.NET チーム
