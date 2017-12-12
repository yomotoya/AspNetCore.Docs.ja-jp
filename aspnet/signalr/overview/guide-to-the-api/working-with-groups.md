---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: "SignalR でグループの操作 |Microsoft ドキュメント"
author: pfletcher
description: "このトピックでは、ハブ API でのグループ メンバーシップ情報を永続化する方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 3befcdbbc735dc4f64c714ba583e026c0c19465d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="working-with-groups-in-signalr"></a>SignalR でグループの操作
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)

> このトピックでは、ユーザーをグループに追加し、グループ メンバーシップ情報を永続化する方法について説明します。 
> 
> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されているソフトウェア バージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR バージョン 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>このトピックの以前のバージョン
> 
> SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。


## <a name="overview"></a>概要

SignalR でグループは、接続しているクライアントの指定されたサブセットにメッセージをブロードキャストする方法を提供します。 グループは、クライアントの任意の数を持つことができ、クライアントは任意の数のグループのメンバーであることができます。 グループを明示的に作成する必要はありません。 実際には、初めて Groups.Add への呼び出しでその名前を指定したグループが自動的に作成し、メンバーシップから最後の接続を削除する場合は削除します。 グループの使用の概要については、次を参照してください。[ハブ クラスからグループのメンバーシップを管理する方法](hubs-api-guide-server.md#groupsfromhub)Hubs api - Server ガイドです。

グループ メンバーシップの一覧またはグループの一覧を取得するための API はありません。 SignalR では、クライアントと、パブリッシュ/サブスクライブ モデルに基づくグループにメッセージを送信し、サーバーがグループまたはグループ メンバーシップの一覧を管理しません。 これにより、スケーラビリティを最大化 SignalR を保持するいずれかの状態は、新しいノードに反映する必要が web ファームにノードを追加するたびにできます。

使用してグループにユーザーを追加すると、`Groups.Add`メソッド、ユーザーが、現在の接続の間、そのグループに送信されるメッセージを受け取りますが、そのグループ内のユーザーのメンバーシップは期間を超えて保持されません、現在の接続。 グループとグループ メンバーシップに関する情報を完全に保持する場合は、データベースや Azure テーブル ストレージなどのリポジトリにそのデータを格納する必要があります。 そのため、アプリケーション、ユーザーが接続するたびにするリポジトリから、ユーザーが属するグループを取得し、それらのグループにそのユーザーを手動で追加します。

再接続時に一時的に中断した後、ユーザーに自動的に再参加以前に割り当てられているグループ。 グループを自動的に再参加は、再接続時に、新しい接続を確立するときではなく時にのみ適用します。 デジタル署名されたトークンは、以前に割り当てられているグループの一覧を含む、クライアントから渡されます。 ユーザーが要求されたグループに属しているかどうかを確認する場合は、既定の動作をオーバーライドできます。

このトピックには、次のセクションがあります。

- [追加して、ユーザーを削除します。](#add)
- [グループのメンバーの呼び出し](#call)
- [グループ メンバーシップをデータベースに保存します。](#storedatabase)
- [Azure テーブル ストレージでファイルを格納するグループのメンバーシップ](#storeazuretable)
- [再接続するときに、グループ メンバーシップを確認します。](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>追加して、ユーザーを削除します。

呼び出すを追加またはグループからユーザーを削除する、[追加](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)または[削除](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)メソッド、およびユーザーの接続の id とグループ名のパラメーターとして渡します。 接続の終了時に、グループからユーザーを手動で削除する必要はありません。

次の例は、`Groups.Add`と`Groups.Remove`ハブ メソッドで使用される方法です。

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

`Groups.Add`と`Groups.Remove`メソッドが非同期的に実行します。

クライアントをグループに追加し、すぐに、グループを使用して、クライアントにメッセージを送信する場合は、Groups.Add メソッドが最初に終了するかどうかを確認する必要があります。 次のコード例を実行する方法を示します。

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

一般に、含める必要はありません`await`を呼び出すときに、`Groups.Remove`メソッドを削除しようとしている接続の id が使用可能な不要になった可能性があるためです。 その場合は、`TaskCanceledException`が、要求がタイムアウトした後にスローされます。追加できるかどうか、アプリケーションがグループにメッセージを送信する前に、ユーザー、グループから削除されていることを確認する必要があります、 `await` Groups.Remove、および、catch の前に、`TaskCanceledException`スローされる例外。

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>グループのメンバーの呼び出し

次の例に示すように、すべてのグループのメンバーまたはグループの唯一の指定したメンバーにメッセージを送信できます。

- **すべて**指定したグループ内のクライアントを接続します。 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- 指定したグループ内のクライアントが接続されているすべて**、指定されたクライアントを除く**接続 ID によって識別されます。 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- 指定したグループ内のクライアントが接続されているすべて**呼び出し元のクライアントを除く**です。 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>グループ メンバーシップをデータベースに保存します。

次の例では、データベース内のグループとユーザーの情報を保持する方法を示します。 任意のデータ アクセス テクノロジを使用することができます。ただし、次の例では、Entity Framework を使用してモデルを定義する方法を示します。 これらのエンティティ モデルは、データベースのテーブルとフィールドに対応します。 データ構造は、アプリケーションの要件に応じて大きく異なる可能性があります。 この例には、という名前のクラスが含まれています。`ConversationRoom`スポーツまたはガーデニングなどのさまざまな主題メッセージ交換に参加できるようにするアプリケーションに一意などれか。 この例では、接続するためのクラスも含まれます。 接続クラスは、グループ メンバーシップを追跡するため、どうしても必要はありませんが、ユーザーを追跡する堅牢なソリューションの一部では多くの場合。

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

次に、ハブでは、データベースから、グループとユーザー情報を取得し、適切なグループにユーザーを手動で追加できます。 この例では、ユーザー接続を追跡するためのコードは含まれません。 この例では、`await`する前にキーワードが適用されていない`Groups.Add`グループのメンバーに、メッセージがすぐに送信されないためです。 適用するグループのすべてのメンバーに、新しいメンバーを追加した後すぐにメッセージを送信する場合、`await`キーワード、非同期操作が完了したかどうかを確認します。

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Azure テーブル ストレージでファイルを格納するグループのメンバーシップ

Azure テーブル ストレージを使用して、グループとユーザーの情報を格納するは、データベースを使用してに似ています。 次の例では、ユーザー名とグループ名を格納するテーブル エンティティを示します。

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

ハブで、ユーザーが接続するときに割り当てられているグループを取得します。

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>再接続するときに、グループ メンバーシップを確認します。

既定では、SignalR 自動的に再ユーザー、グループに割り当てます適切な接続は削除され、接続がタイムアウトする前に再び確立した場合など、一時的に中断から再接続するときにします。、再接続するときに、ユーザーのグループの情報がトークンで渡され、サーバーでそのトークンを検証します。 グループへのユーザーの再参加の検証プロセスについては、次を参照してください。[再接続するときにグループの再参加](../security/introduction-to-security.md#rejoingroup)です。

一般に、グループに再接続を自動的に再参加の既定の動作を使用する必要があります。 SignalR のグループは、機密データへのアクセスを制限するためのセキュリティ メカニズムとしてのものではありません。 ただし、再接続するときに、アプリケーションはユーザーのグループ メンバーシップを再確認する必要があります、既定の動作をオーバーライドすることができます。 既定の動作を変更することができます、負担、データベースに追加各再接続ではなく、ユーザーが接続するときにだけ、ユーザーのグループ メンバーシップを取得する必要があるためです。

グループのメンバーシップを確認する必要がある場合は、再接続、次に示すように、割り当てられているグループの一覧を返す新しいハブ パイプライン モジュールを作成します。

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

その後、そのモジュールを以下に示すように、ハブ パイプラインに追加します。

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
