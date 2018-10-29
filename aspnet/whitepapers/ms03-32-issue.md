---
uid: whitepapers/ms03-32-issue
title: IE のセキュリティ更新プログラムを適用した後に 'サーバー アプリケーションが使用できない' エラーの修正 |Microsoft Docs
author: rick-anderson
description: このホワイト ペーパーでは、Wi で実行されている ASP.NET 1.0 アプリケーションに影響する Internet Explorer の ms 03 32 のセキュリティ更新プログラムの問題を修正する修正プログラムについて説明しています.
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: 9041f8d15a449a517594f8051c3d9f0ceb18a8a3
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207018"
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>IE のセキュリティ更新プログラムを適用した後に 'サーバー アプリケーションが使用できない' エラーを修正しました
====================
> このホワイト ペーパーでは、Windows XP Professional で実行されている ASP.NET 1.0 アプリケーションに影響する Internet Explorer の ms 03 32 のセキュリティ更新プログラムの問題を修正する修正プログラムについて説明します。
> 
> ASP.NET 1.0 と Windows XP Professional に適用されます。


Microsoft では、Internet Explorer のセキュリティ更新プログラムの ms 03 32 セキュリティ更新プログラムで Windows XP で実行されている ASP.NET 1.0 問題点を確認します。 この修正プログラムは、手動、または、Windows Update サイトから最新の重要な更新プログラムを入手してインストールできます。

この問題の現象は、Windows XP コンピューターで、修正プログラムをインストールした後、ローカルの IIS 5.1 web サーバーで実行されている ASP.NET アプリケーションへのすべての要求の「サーバー アプリケーションは使用できません」というエラー メッセージで結果することです。 リモート web サーバーへの要求は、影響を受けません。

この問題には、Windows XP での ASP.NET 1.0 を実行しているインストールのみに影響します。 Windows 2000 または Windows Server 2003 を実行しているマシンには影響しません。 ASP.NET 1.1 がインストールされていると、Windows XP を実行しているマシンも影響しません。

注意してくださいこの問題**ない**ASP.NET を使用したセキュリティ バグです。 これは、**しない**開くか、ASP.NET アプリケーションまたはサーバーに対する悪意のある攻撃を許可します。 代わりに、パッチ自体による機能のバグ純粋になります。

取り組んでいますハードこの問題の永続的なソリューションです。 それまでは、問題の回避策として、次のバッチ ファイルを実行できます。 バッチ ファイルは、次を行います。

1. IIS および ASP.NET 状態サービスを停止します
2. 削除し、既知の一時的なパスワードを使用して、ASPNET アカウントを再作成されます。
3. Windows を使用して`runas`ASPNET ユーザー プロファイルを作成する実行可能ファイルを起動するコマンド
4. ASP.NET を再登録します。 これは、アカウントの新しいランダムなパスワードを作成しの既定の ASP.NET へのアクセス制御の設定を適用
5. IIS サービスを再起動します。

バッチ ファイルにはハードコードされた一時パスワードが含まれています"<strong>1pass\@word</strong>"、runas コマンド、バッチ ファイルの実行時に入力するように求めできます。 Runas コマンドが完了した後、ASPNET アカウントのパスワードは強力なランダムな値で再作成します。 ハードコーディングされたパスワードが環境内でパスワードの複雑さの要件を満たしていない場合、バッチ ファイルが失敗する可能性がありますに注意してください。 ケースがある場合は、環境に適したが別の値に変更できます。

*> [!IMPORTANT]* カスタムのアクセス制御の設定や、ASPNET アカウントのデータベース アカウントのアクセス許可を追加した場合は、このバッチ ファイルが完了したら、再作成する必要があります。 アカウントを再作成するときに新しいセキュリティ識別子 (SID) を取得するためです。

*> [!IMPORTANT]* ASPNET アカウント以外のカスタム アカウントを使用して、ASP.NET ワーカー プロセスを実行する場合、このバッチ ファイルいない実行する必要があります。 代わりに、対話形式でログインまたは runas コマンドを使用して、そのアカウントのユーザー プロファイルを作成し、そのアカウントを使用する必要があります。

バッチ ファイルは、次の自己解凍形式のアーカイブに含まれます。 使用方法。

1. 管理者特権では、アカウントとして実行する必要があります。
2. [ダウンロードして、自己解凍実行可能ファイルを開きます](ms03-32-issue/_static/fixup1.exe)
3. C:\ に内容を抽出します。
4. ...、スタート メニューから実行を選択し、入力 `cmd.exe`
5. 開いているコマンド ウィンドウで、入力`c:\fixup.cmd`します。
6. 入力を求められたら入力<strong>1pass\@word</strong>パスワードとして。
7. 以前のカスタム アクセス制御の設定または ASPNET アカウントのデータベース アカウントのアクセス許可がある場合は、これらの設定を今すぐ再適用する必要があります。

これが原因となった不便多く申し訳ありません。 利用可能になった追加情報を掲載します。

次の表は、プラットフォームとこの問題による影響を受けるバージョンについて説明します。

| .NET Framework | プラットフォーム | 影響を受ける |
| --- | --- | --- |
| バージョン 1.0 | Windows 2000 Professional | いいえ |
| バージョン 1.0 | Windows 2000 Server | いいえ |
| バージョン 1.0 | Windows XP Professional | はい |
| バージョン 1.0 | Windows Server 2003 | いいえ |
| バージョン 1.0 | Cassini と Windows XP ホーム | いいえ |
| バージョン 1.1 | Windows 2000 Professional | いいえ |
| バージョン 1.1 | Windows 2000 Server | いいえ |
| バージョン 1.1 | Windows XP Professional | いいえ |
| バージョン 1.1 | Windows Server 2003 | いいえ |
| バージョン 1.1 | Cassini と Windows XP ホーム | いいえ |

よろしくお願いいたします   
 ASP.NET チーム
