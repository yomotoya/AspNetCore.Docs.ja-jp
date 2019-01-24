---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: SignalR でグループの操作 |Microsoft Docs
author: bradygaster
description: このトピックでは、Hub API を使用したグループのメンバーシップ情報を永続化する方法について説明します。
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 384b7e5f07fa46ea3cc32e5c18c3c2327b7aedd3
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836806"
---
<a name="working-with-groups-in-signalr"></a>SignalR でグループの操作
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> このトピックでは、ユーザーをグループに追加し、グループのメンバーシップ情報を保持する方法について説明します。
>
> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されるソフトウェアのバージョン
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR 2 のバージョン
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>このトピックの以前のバージョン
>
> SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。
>
> ## <a name="questions-and-comments"></a>意見やご質問
>
> このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。 チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)にて投稿してください。

## <a name="overview"></a>概要

SignalR でグループは、接続されているクライアントのサブセットを指定するメッセージをブロードキャストする方法を提供します。 グループは、クライアントの任意の数を持つことができ、クライアントは任意の数のグループのメンバーであることができます。 グループを明示的に作成する必要はありません。 実際には、初めて Groups.Add への呼び出しでその名前を指定するグループを自動的に作成し、そのメンバーシップから最後の接続を削除する場合は削除します。 グループの使用の概要については、次を参照してください。[ハブ クラスからグループ メンバーシップを管理する方法](hubs-api-guide-server.md#groupsfromhub)Hubs API - サーバー ガイドにします。

グループ メンバーシップの一覧またはグループの一覧を取得するための API はありません。 SignalR クライアントおよび、パブリッシュ/サブスクライブ モデルに基づいてグループにメッセージを送信して、サーバーは、グループまたはグループ メンバーシップの一覧を保持しません。 こうため、SignalR を保持する任意の状態が新しいノードに適用するのには web ファームにノードを追加するたびに、スケーラビリティを最大化します。

使用してグループにユーザーを追加すると、`Groups.Add`メソッドでは、ユーザーは、現在の接続の期間中、そのグループに送信されるメッセージを受け取りますが、そのグループ内のユーザーのメンバーシップは、現在の接続を超えて保持されません。 グループとグループ メンバーシップに関する情報を完全に保持する場合は、データベースまたは Azure テーブル ストレージなどのリポジトリにデータを格納する必要があります。 ユーザーがアプリケーションに接続するたびにするリポジトリから、ユーザーが属するグループを取得し、それらのグループにそのユーザーを手動で追加します。

一時中断の後再接続時に、ときに、ユーザーが自動的に再結合以前に割り当てられているグループ。 グループを自動的に再参加と、新しい接続を確立するときではなく、再接続時にのみ適用します。 デジタル署名されたトークンは、以前に割り当てられているグループの一覧を含む、クライアントから渡されます。 ユーザーが要求されたグループに属しているかどうかを確認する場合は、既定の動作をオーバーライドできます。

このトピックには、次のセクションがあります。

- [追加して、ユーザーを削除します。](#add)
- [グループのメンバーの呼び出し](#call)
- [グループのメンバーシップをデータベースに保存します。](#storedatabase)
- [Azure table storage に格納するグループのメンバーシップ](#storeazuretable)
- [再接続時にグループ メンバーシップの確認](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>追加して、ユーザーを削除します。

追加またはグループからユーザーを削除を呼び出す、[追加](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)または[削除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)メソッド、およびユーザーの接続の id とグループの名前のパラメーターとして渡します。 接続の終了時に、グループからユーザーを手動で削除する必要はありません。

次の例は、`Groups.Add`と`Groups.Remove`ハブ メソッドで使用されるメソッド。

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

`Groups.Add`と`Groups.Remove`メソッドが非同期的に実行します。

クライアント グループを追加して、すぐに、グループを使用して、クライアントにメッセージを送信する場合は、Groups.Add メソッドが最初に終了するかどうかを確認する必要があります。 次のコード例では、その方法を示します。

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

一般に、含める必要はありません`await`を呼び出すときに、`Groups.Remove`メソッドを削除しようとしている接続の id が使用可能な不要になった可能性があるためです。 その場合は、`TaskCanceledException`が、要求がタイムアウトした後にスローされます。追加できるかどうか、アプリケーションが、ユーザーがグループにメッセージを送信する前に、グループから削除されたことを確認する必要があります、`await`する前に`Groups.Remove`をキャッチし、`TaskCanceledException`がスローされる例外。

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>グループのメンバーの呼び出し

次の例に示すように、すべてのグループのメンバーまたはグループの唯一の指定したメンバーにメッセージを送信できます。

- **すべて**指定したグループ内のクライアントを接続します。

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- 指定したグループ内のクライアントが接続されているすべて **、指定されたクライアントを除く**接続 ID によって識別されます。

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- 指定したグループ内のクライアントが接続されているすべて**呼び出し元のクライアントを除く**します。

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>グループのメンバーシップをデータベースに保存します。

次の例では、データベース内のグループとユーザーの情報を保持する方法を示します。 任意のデータ アクセス テクノロジを使用することができます。ただし、次の例では、Entity Framework を使用してモデルを定義する方法を示します。 これらのエンティティ モデルは、データベース テーブルとフィールドに対応します。 データ構造は、アプリケーションの要件に応じて大きく異なる可能性があります。 この例には、という名前のクラスが含まれています。`ConversationRoom`スポーツやガーデンなどの異なるサブジェクトについての会話に参加できるアプリケーション固有であります。 この例では、接続するためのクラスも含まれています。 接続クラスは、グループ メンバーシップを追跡するために必須ではありませんが、ユーザーを追跡する堅牢なソリューションの一部では頻繁に。

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

次に、ハブでは、データベースから、グループとユーザー情報を取得し、適切なグループにユーザーを手動で追加できます。 この例では、ユーザー接続を追跡するためのコードは含まれません。 この例で、`await`キーワードは、前に適用されません`Groups.Add`グループのメンバーにメッセージがすぐに送信されないためです。 適用するグループのすべてのメンバーに、新しいメンバーを追加した後すぐにメッセージを送信する場合、`await`キーワードを非同期操作が完了したかどうかを確認します。

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Azure table storage に格納するグループのメンバーシップ

Azure テーブル ストレージを使用して、グループとユーザーの情報を格納するは、データベースを使用してに似ています。 次の例では、ユーザー名とグループ名を格納するテーブル エンティティを示します。

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

ハブで、ユーザーが接続するときに割り当てられているグループを取得します。

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>再接続時にグループ メンバーシップの確認

既定では、SignalR 自動的に再割り当てをユーザー適切なグループに接続が削除され、接続がタイムアウトする前に再確立するなどの一時中断から再接続時にします。再接続時に、時に、ユーザーのグループの情報がトークンで渡され、サーバーでそのトークンを確認します。 グループへのユーザーの再参加の検証プロセスについては、次を参照してください。[再接続時にグループの再参加](../security/introduction-to-security.md#rejoingroup)します。

一般に、グループに再接続を自動的に再参加の既定の動作を使用する必要があります。 SignalR のグループは、機密データへのアクセスを制限するためのセキュリティ メカニズムとして意図されていません。 ただし、アプリケーションは、再接続時に、ユーザーのグループ メンバーシップを再確認する必要がある場合、は、既定の動作を上書きできます。 既定の動作を変更することができます、負担、データベースを追加ため各再接続はなく、ユーザーが接続するときに、ユーザーのグループ メンバーシップを取得する必要があります。

グループのメンバーシップを確認する必要がある場合は、再接続、次に示すように、割り当てられたグループの一覧を返す新しいハブ パイプライン モジュールを作成します。

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

次に、以下の強調表示されている、ハブ パイプラインにそのモジュールを追加します。

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
