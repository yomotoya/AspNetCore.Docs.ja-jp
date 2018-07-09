---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
title: 追加のユーザー情報 (VB) を格納する |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、非常に基本的なゲストブック アプリケーションを構築することによりこの質問に回答されます。 そのため、modeli のさまざまなオプションを紹介しています.
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: ee4b924e-8002-4dc3-819f-695fca1ff867
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 2ea731012f8c053d1e1eac293fdbeaec66db23bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816039"
---
<a name="storing-additional-user-information-vb"></a>ファイルを格納する追加のユーザー情報 (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_vb.pdf)

> このチュートリアルでは、非常に基本的なゲストブック アプリケーションを構築することによりこの質問に回答されます。 そのため、いたします。 データベースでは、ユーザー情報をモデル化するためのさまざまなオプションを見てその後にこのデータをメンバーシップ フレームワークによって作成されたユーザー アカウントに関連付ける方法を参照してください。


## <a name="introduction"></a>はじめに

ASP します。NET のメンバーシップ フレームワークでは、ユーザーを管理するための柔軟なインターフェイスを提供します。 メンバーシップ API には、資格情報の検証、現在ログオンしているユーザーに関する情報を取得する、新しいユーザー アカウントを作成、および他のユーザーの間でのユーザー アカウントを削除するためのメソッドが含まれています。 メンバーシップ フレームワークでは、各ユーザー アカウントには、資格情報を検証し、重要なユーザー アカウントに関連するタスクを実行するために必要なプロパティのみが含まれています。 これは、上昇によって明らかメソッドとプロパティの[`MembershipUser`クラス](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)、メンバーシップ フレームワーク内のユーザー アカウントをモデル化します。 このクラスのようなプロパティには[ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx)、 [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx)、および[ `IsLockedOut`](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx)などのメソッドと[ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx)と[ `UnlockUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)します。

多くの場合、アプリケーションは、メンバーシップ フレームワークに含まれていない追加のユーザー情報を格納する必要があります。 たとえば、オンライン小売業者は、彼女出荷および請求先住所、お支払い情報、配信の設定を格納し、連絡先の電話番号は、各ユーザーができるようにする必要があります。 さらに、各注文システムでは、特定のユーザー アカウントに関連付けられます。

`MembershipUser`クラスなどのプロパティを含まない`PhoneNumber`または`DeliveryPreferences`または`PastOrders`します。 どのように実行アプリケーションで必要なユーザー情報を追跡して、メンバーシップ フレームワークとの統合にでしょうか。 このチュートリアルでは、非常に基本的なゲストブック アプリケーションを構築することによりこの質問に回答されます。 そのため、いたします。 データベースでは、ユーザー情報をモデル化するためのさまざまなオプションを見てその後にこのデータをメンバーシップ フレームワークによって作成されたユーザー アカウントに関連付ける方法を参照してください。 それでは、始めましょう!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>手順 1: ゲストブック アプリケーションのデータ モデルを作成します。

さまざまなデータベース内のユーザー情報をキャプチャし、メンバーシップ フレームワークによって作成されたユーザー アカウントに関連付けるために使用できる手法があります。 これらの手法を説明するためには、何らかのユーザーに関連するデータをキャプチャするために、チュートリアルの web アプリケーションを強化する必要があります。 (現時点では、アプリケーションのデータ モデルに、アプリケーションのサービスで必要なテーブルのみが含まれています、 `SqlMembershipProvider`)。

認証されたユーザーがコメントを残すことが非常に単純なゲストブック アプリケーションを作成してみましょう。 ゲストブックのコメントを保存するだけでなく、出身地、ホーム ページ、および署名を格納するには、各ユーザーを許可します。 ユーザーのホーム町では、指定されている場合は、ホーム ページ、および署名が、ゲストブックに彼が去った各メッセージに表示されます。

### <a name="adding-theguestbookcommentstable"></a>追加、`GuestbookComments`テーブル

という名前のデータベース テーブルの作成に必要なゲストブックのコメントをキャプチャするには`GuestbookComments`のように列を持つ`CommentId`、 `Subject`、 `Body`、および`CommentDate`します。 各レコードが存在する必要もあります、`GuestbookComments`テーブルの参照、コメントを残したユーザー。

に、データベースをこのテーブルを追加する Visual Studio の データベース エクスプ ローラーに移動し、ドリルダウン、`SecurityTutorials`データベース。 [テーブル] フォルダーを右クリックし、新しいテーブルの追加を選択します。 新しいテーブルの列を定義できるようにするインターフェイスが表示されます。


[![SecurityTutorials データベースに新しいテーブルを追加します。](storing-additional-user-information-vb/_static/image2.png)](storing-additional-user-information-vb/_static/image1.png)

**図 1**: 新しいテーブルを追加、`SecurityTutorials`データベース ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image3.png))。


次に、定義、`GuestbookComments`の列。 という名前の列を追加することで開始`CommentId`型の`uniqueidentifier`します。 この列は、ゲストブックの各コメントを一意に識別する、許可しないように`NULL`s し、テーブルの主キーとしてマークします。 値を指定するのではなく、`CommentId`フィールドでそれぞれ`INSERT`、ことを示す新しい`uniqueidentifier`値を自動的に生成するこのフィールドに`INSERT`列の既定値を設定して`NEWID()`。 Primary key、および設定として、既定値を指定して、最初のこのフィールドを追加した後、画面をスクリーン ショット、図 2 に示すようになる必要があります。


[![CommentId をという名前のプライマリ列を追加します。](storing-additional-user-information-vb/_static/image5.png)](storing-additional-user-information-vb/_static/image4.png)

**図 2**: プライマリ列という追加`CommentId`([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image6.png))。


という名前の列を次に、追加`Subject`型の`nvarchar(50)`という名前の列と`Body`型の`nvarchar(MAX)`、許可しない`NULL`s 両方の列にします。 次に、という名前の列を追加`CommentDate`型の`datetime`します。 禁止`NULL`s とセット、`CommentDate`列の既定の値を`getdate()`します。

すべてそのままではゲストブックの各コメントにユーザー アカウントを関連付ける列を追加します。 という名前の列を追加する 1 つのオプションと`UserName`型の`nvarchar(256)`します。 適切な選択肢は、これ以外のメンバーシップ プロバイダーを使用する場合、`SqlMembershipProvider`します。 使用する場合は、 `SqlMembershipProvider`、このチュートリアル シリーズでは、`UserName`内の列、`aspnet_Users`テーブルは、一意であることは保証されません。 `aspnet_Users`テーブルの主キーが`UserId`の型は`uniqueidentifier`します。 そのため、`GuestbookComments`という名前の列がテーブルに必要がある`UserId`型の`uniqueidentifier`(禁止`NULL`値)。 この列を追加してください。

> [!NOTE]
> 説明したように、 [ *SQL Server でメンバーシップ スキーマを作成する*](creating-the-membership-schema-in-sql-server-vb.md)チュートリアル, メンバーシップ フレームワークが複数の web アプリケーションが別のユーザー アカウントを同じ共有を有効にするように設計ユーザー ストア。 この機能を使用するには、さまざまなアプリケーションへのユーザー アカウントのパーティション分割します。 同じユーザー名各ユーザー名は、アプリケーション内で一意であることが保証、中に、同じユーザー ストアを使用して別のアプリケーションで使用できます。 複合`UNIQUE`内の制約、`aspnet_Users`テーブルに対して、`UserName`と`ApplicationId`フィールドがない 1 つでだけ、`UserName`フィールド。 したがって、これが、aspnet の考えられる\_ユーザー テーブルと同じ 2 つ (以上) のレコードに`UserName`値。 ただし、`UNIQUE`の制約、`aspnet_Users`テーブルの`UserId`フィールド (主キーのため)。 A`UNIQUE`なしでは、間の外部キー制約を確立できないため、制約が重要ですが、`GuestbookComments`と`aspnet_Users`テーブル。


追加した後、`UserId`列で、ツールバーの [保存] アイコンをクリックしてテーブルを保存します。 新しいテーブルの名前を`GuestbookComments`します。

作業を最後の 1 つの問題がある、`GuestbookComments`テーブル: を作成する必要があります、[外部キー制約](https://msdn.microsoft.com/library/ms175464.aspx)間、`GuestbookComments.UserId`列と`aspnet_Users.UserId`列。 これを実現するには、外部キー リレーションシップ ダイアログ ボックスを起動するには、ツールバーで リレーションシップ アイコンをクリックします。 (または、起動できますこのダイアログ ボックス、テーブル デザイナー メニューに移動し、リレーションシップを選択します。)

外部キー リレーションシップ ダイアログ ボックスの左下隅の 追加 ボタンをクリックします。 リレーションシップに参加しているテーブルを定義する必要がありますが、新しい外部キー制約は追加します。


[![外部キー リレーションシップ ダイアログ ボックスを使用して、テーブルの外部キー制約を管理するには](storing-additional-user-information-vb/_static/image8.png)](storing-additional-user-information-vb/_static/image7.png)

**図 3**: 外部キー リレーションシップ ダイアログ ボックスを使用して、テーブルの外部キー制約を管理する ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image9.png))。


次に、右側の「テーブルと列の仕様」行にある省略記号アイコンをクリックします。 これは、テーブルと列 ダイアログ ボックス起動、主キー テーブルと列から外部キー列を指定する`GuestbookComments`テーブル。 具体的には、選択`aspnet_Users`と`UserId`として主キー テーブルと列、および`UserId`から、`GuestbookComments`外部キー列とテーブル (図 4 参照)。 主キーと外部キー テーブルと列を定義した後は、外部キー リレーションシップ ダイアログ ボックスに戻るには、ok をクリックします。


[![外部キー制約との間、aspnet_Users と GuesbookComments テーブルを確立します。](storing-additional-user-information-vb/_static/image11.png)](storing-additional-user-information-vb/_static/image10.png)

**図 4**: 外部キー制約の間に確立、`aspnet_Users`と`GuesbookComments`テーブル ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image12.png))。


この時点では、外部キー制約が確立されました。 この制約の存在により[リレーショナル整合性](http://en.wikipedia.org/wiki/Referential_integrity)ことはありませんがあるゲストブックのエントリが存在しないユーザー アカウントを参照するを保証することで 2 つのテーブル。 既定では、外部キー制約は対応する子レコードがある場合に削除する親レコードを禁止します。 ユーザーが 1 つまたは複数のゲストブック コメント、そのユーザー アカウントを削除しようとしました、彼のゲストブックのコメントが最初に削除しない限りの削除では失敗します。

親レコードが削除されたときに自動的に関連付けられている子レコードを削除するのには、外部キー制約を構成できます。 つまり、この外部キー制約をセットアップし、ユーザーのゲストブックのエントリは、自分のユーザー アカウントが削除されたときに自動的に削除できるようにできます。 これを実現するには、「の挿入と更新プログラムの仕様」セクションを展開し、Cascade に「ルールを削除する」プロパティを設定します。


[![連鎖削除する外部キー制約を構成します。](storing-additional-user-information-vb/_static/image14.png)](storing-additional-user-information-vb/_static/image13.png)

**図 5**: 連鎖削除をする外部キー制約を構成する ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image15.png))。


外部キー制約を保存するには、外部キー リレーションシップを終了して閉じる ボタンをクリックします。 テーブルと、これを保存するには、ツールバーで [保存] アイコンをクリックします。 リレーションシップ。

### <a name="storing-the-users-home-town-homepage-and-signature"></a>ユーザーのホーム町、ホーム ページ、および署名を格納します。

`GuestbookComments`テーブルは、ユーザー アカウントと一対多リレーションシップを共有する情報を格納する方法を示しています。 各ユーザー アカウントに関連付けられているコメントの任意の数がありますので、特定のユーザーにリンクが各コメントをバックする列を含むコメントのセットを保持するテーブルを作成してこの関係をモデル化されます。 使用する場合、 `SqlMembershipProvider`、という名前の列を作成してこのリンクが確立されている最も`UserId`型の`uniqueidentifier`とこの列の間で外部キー制約と`aspnet_Users.UserId`します。

これで、ユーザーのホーム町、ホーム ページ、および署名で、彼のゲストブックのコメントに表示されますを格納するには、各ユーザー アカウントに 3 つの列を関連付ける必要があります。 これを実現するさまざまな方法の数。

- <strong>新しい列を追加、</strong><strong>`aspnet_Users`</strong><strong>または</strong><strong>`aspnet_Membership`</strong><strong>テーブル。</strong> 使用されるスキーマを変更するためにこの方法を推奨は、`SqlMembershipProvider`します。 この決定が落ちて発言に戻る可能性があります。 ASP.NET の将来のバージョンは、異なる場合など、`SqlMembershipProvider`スキーマ。 Microsoft は、ASP.NET 2.0 に移行するためのツールを含めることができます`SqlMembershipProvider`ASP.NET 2.0 を変更した場合は、新しいスキーマにデータ`SqlMembershipProvider`スキーマ、このような変換できない可能性があります。

- **ASP を使用します。NET のプロファイルのフレームワーク、出身地、ホーム ページ、および署名のプロファイル プロパティを定義します。** ASP.NET には、その他のユーザーに固有のデータを格納するように設計されたプロファイルのフレームワークが含まれています。 メンバーシップ フレームワークのようなプロバイダー モデルの上、プロファイルのフレームワークが構築されています。 .NET Framework に付属する`SqlProfileProvider`ストアが SQL Server データベース内のデータをプロファイルします。 実際には、データベースが既にで使用されるテーブル、 `SqlProfileProvider` (`aspnet_Profile`) で、アプリケーション サービスのバックアップを追加したときに追加された、 [ *SQL Server でメンバーシップ スキーマを作成する*](creating-the-membership-schema-in-sql-server-vb.md)チュートリアル。   
  プロファイルのフレームワークの主な利点は、開発者のプロファイルのプロパティを定義することができる`Web.config`– コードを書き込むと、基になるデータ ストアの間のプロファイル データをシリアル化する必要はありません。 つまり、プロファイル プロパティのセットを定義して、コードで操作を非常に簡単です。 ただし、プロファイル システムが望ましい場合、バージョン管理する際に多くを離れるため場所を削除または変更するには、既存の後で追加する新しいユーザー固有のプロパティを期待し、プロファイルのフレームワークができない可能性があります、アプリケーションがある場合、 最適なオプションです。 さらに、`SqlProfileProvider`プロファイル データ (など、ユーザーの数には、New York のホーム町がある) に対して直接クエリを実行不可能になります、高度に非正規化方式でプロファイルのプロパティを格納します。   
  プロファイルのフレームワークの詳細については、このチュートリアルの最後に、「これ以上の読み取り」セクションを参照してください。

- <strong>これら 3 つの列をデータベースに新しいテーブルに追加し、このテーブルの間の一対一の関係を確立し、</strong><strong>`aspnet_Users`</strong><strong>します。</strong> この方法は、プロファイルのフレームワークでよりもさらに作業が含まれますが、データベースに追加のユーザー プロパティがモデル化方法で最大限の柔軟性を提供しています。 これは、このチュートリアルで使用するオプションです。

という新しいテーブルは作成`UserProfiles`ホーム町、ホーム ページ、およびユーザーごとの署名を保存します。 データベース エクスプ ローラー ウィンドウで テーブル フォルダーを右クリックし、新しいテーブルを作成することもできます。 最初の列の名前を`UserId`にその型を設定および`uniqueidentifier`します。 禁止`NULL`値し、列を主キーとしてマークします。 次に、という名前の列を追加:`HomeTown`型の`nvarchar(50)`;`HomepageUrl`型の`nvarchar(100)`; 型のシグネチャと`nvarchar(500)`します。 これら 3 つの列の各を受け入れることができます、`NULL`値。


[![ユーザー テーブルを作成します。](storing-additional-user-information-vb/_static/image17.png)](storing-additional-user-information-vb/_static/image16.png)

**図 6**: 作成、`UserProfiles`テーブル ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image18.png))。


テーブルを保存し、名前を`UserProfiles`します。 最後に、間の外部キー制約を確立、`UserProfiles`テーブルの`UserId`フィールドおよび`aspnet_Users.UserId`フィールド。 間の外部キー制約と同様、`GuestbookComments`と`aspnet_Users`テーブルがあるこの制約の連鎖的に削除します。 以降、`UserId`フィールドに`UserProfiles`プライマリ キー、これにより、複数のレコードがある、`UserProfiles`各ユーザー アカウントのテーブル。 このタイプのリレーションシップは、一対一に呼ばれます。

データ モデルを作成したら、それを使用する準備ができています。 手順 2 と 3 では、現在ログオンしてのユーザーが表示およびそのホーム町、ホーム ページ、および署名情報を編集する方法に注目します。 手順 4. で認証されたユーザー、ゲストブックへの新しいコメントを送信し、既存のものを表示するためのインターフェイスを作成します。

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>手順 2: ユーザーのホーム町、ホーム ページ、および署名を表示します。

さまざまな彼のホームの町、ホーム ページ、および署名情報を表示および編集、現在ログオンしているユーザーを許可する方法があります。 テキスト ボックスと、ユーザー インターフェイスを手動で作成することし、ラベル コントロールやは、DetailsView コントロールなどの Web コントロールのデータのいずれかで使用できます。 データベースを実行する`SELECT`と`UPDATE`ADO.NET 作成ステートメントは、ページの分離コード クラス内のコードまたは、代わりに、SqlDataSource で宣言型のアプローチを採用します。 理想的には、アプリケーションには呼び出すことができますか、プログラムで、ページの分離コード クラスから、または ObjectDataSource コントロールを使用して宣言によって、階層型アーキテクチャが含まれます。

このチュートリアル シリーズでは、フォーム認証、承認、ユーザー アカウント、およびロールに重点を置いています、ためにはありませんこれらのさまざまなデータ アクセス オプションまたは階層化されたアーキテクチャは直接 SQL ステートメントを実行するより優先される、理由の徹底的に検討します。ASP.NET ページ。 DetailsView と SqlDataSource – 最もすばやくて簡単なオプション – を使用して説明しますが、説明する概念は、別の Web コントロールとデータ アクセス ロジックに確実に適用できます。 ASP.NET 内のデータの操作方法の詳細についてを参照してください、  *[ASP.NET 2.0 でデータを扱う](../../data-access/index.md)* チュートリアル シリーズです。

開く、`AdditionalUserInfo.aspx`ページで、`Membership`フォルダー ID プロパティを設定 ページに DetailsView コントロールを追加する`UserProfile`を消去して、`Width`と`Height`プロパティ。 DetailsView のスマート タグを展開し、新しいデータ ソース コントロールにバインドを選択します。 これが、データ ソース構成ウィザードを起動 (図 7 を参照してください)。 最初の手順では、データ ソースの種類を指定するように要求されます。 直接接続するため、`SecurityTutorials`データベースで、[データベース] アイコンを指定する、`ID`として`UserProfileDataSource`します。


[![UserProfileDataSource という名前の新しい SqlDataSource コントロールを追加します。](storing-additional-user-information-vb/_static/image20.png)](storing-additional-user-information-vb/_static/image19.png)

**図 7**: 新しい SqlDataSource コントロールという追加`UserProfileDataSource`([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image21.png))。


次の画面を使用するデータベースが求められます。 内の接続文字列を既に定義して`Web.config`の`SecurityTutorials`データベース。 この接続文字列名 – `SecurityTutorialsConnectionString` – ドロップダウン リストでなければなりません。 このオプションを選択し、[次へ] をクリックします。


[![SecurityTutorialsConnectionString をドロップダウン リストから選択します。](storing-additional-user-information-vb/_static/image23.png)](storing-additional-user-information-vb/_static/image22.png)

**図 8**: 選択`SecurityTutorialsConnectionString`ドロップダウン リストから ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image24.png))。


後続の画面では、テーブルと列のクエリを指定するよう求められます。 選択、`UserProfiles`ドロップダウン リストからテーブルし、すべての列を確認します。


[![移動ユーザー テーブルからのすべての列を返します](storing-additional-user-information-vb/_static/image26.png)](storing-additional-user-information-vb/_static/image25.png)

**図 9**: Bring 戻るすべての列から、`UserProfiles`テーブル ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image27.png))。


図 9 を返します。 現在のクエリ*すべて*内のレコードの`UserProfiles`、のみ、現在ログオンしているユーザーのレコードに関心があることができます。 追加する、`WHERE`句では、をクリックして、 `WHERE`  ボタンを追加`WHERE`句 ダイアログ ボックス (図 10 参照)。 ここで、フィルターを適用する列、演算子、およびフィルター パラメーターのソースを選択できます。 選択`UserId`列と演算子として「=」として。

残念ながら、現在ログオンしてユーザーを返す元の組み込みパラメーターがない`UserId`値。 この値をプログラムで取得する必要があります。 そのため、「なし」の追加、パラメーターを追加するボタンをクリックし、ok をクリックして をクリックするソースのドロップダウン リストを設定します。


[![UserId 列のフィルター パラメーターを追加します。](storing-additional-user-information-vb/_static/image29.png)](storing-additional-user-information-vb/_static/image28.png)

**図 10**: で Filter パラメーターを追加、`UserId`列 ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image30.png))。


[Ok] をクリックした後に、図 9 に示すように画面に返されるされます。 今回は、ただし、画面の下部にある SQL クエリ含める必要があります、`WHERE`句。 「テスト クエリ」画面に移動するには、次にクリックします。 ここで、クエリを実行し、結果を確認します。 ウィザードを完了するには、[完了] をクリックします。

Visual Studio では、データ ソース構成ウィザードを完了すると、ウィザードで指定された設定に基づいて、SqlDataSource コントロールを作成します。 さらに、手動で追加 BoundFields SqlDataSource のによって返される列ごとに DetailsView を`SelectCommand`します。 表示する必要はありません、`UserId`ユーザーがこの値を知る必要がないため、DetailsView でフィールド。 DetailsView コントロールの宣言型マークアップから直接フィールドを削除します。 または、「編集フィールド」をクリックして、スマート タグからリンクできます。

この時点で、ページの宣言型マークアップは、次のようになります。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample1.aspx)]

SqlDataSource コントロールをプログラムで設定する必要があります`UserId`パラメーターを現在ログインしているユーザーの`UserId`データが選択される前にします。 これは、SqlDataSource のため、イベント ハンドラーを作成して実行できます`Selecting`イベントと、次を追加するコードがあります。

[!code-vb[Main](storing-additional-user-information-vb/samples/sample2.vb)]

上記のコードを呼び出すことによって、現在ログオンしているユーザーへの参照を取得することで開始、`Membership`クラスの`GetUser`メソッド。 これにより返されます、`MembershipUser`オブジェクト、`ProviderUserKey`プロパティが含まれています、`UserId`します。 `UserId` SqlDataSource に値が割り当てられます`@UserId`パラメーター。

> [!NOTE]
> `Membership.GetUser()`メソッドは、現在ログオンしているユーザーに関する情報を返します。 値が返されます場合は、匿名ユーザーは、ページにアクセスして、`Nothing`します。 このような場合は、これにより、`NullReferenceException`を読み取ろうとしたときにコードの次の行に、`ProviderUserKey`プロパティ。 について心配する必要はありません、`Membership.GetUser()`で何も返さない、`AdditionalUserInfo.aspx`ページの認証されたユーザーのみがこのフォルダー内の ASP.NET リソースへのアクセスできるように、前のチュートリアルで URL 承認を構成したためです。 匿名アクセスが許可されているページで、現在ログオンしているユーザーに関する情報にアクセスする必要がある場合は、ことを確認することを確認してください、`MembershipUser`から返されたオブジェクト、`GetUser()`メソッド Nothing ではないプロパティを参照する前にします。


アクセスした場合、`AdditionalUserInfo.aspx`ページ ブラウザーから任意の行を追加するがあるまだあるので、空白のページが表示されます、`UserProfiles`テーブル。 手順 6. で自動的に新しい行を追加する CreateUserWizard コントロールをカスタマイズする方法に注目するは、`UserProfiles`新しいユーザー アカウントが作成されるテーブルします。 ここでは、ただしは必要があります、テーブルにレコードを手動で作成します。

Visual Studio の データベース エクスプ ローラーに移動し、テーブル フォルダーを展開します。 右クリックし、`aspnet_Users`テーブルは、テーブル内のレコードを表示する「テーブル データの表示」を選択し、同じこと`UserProfiles`テーブル。 図 11 では、垂直方向に並べて表示されたときに、これらの結果を示します。 データベースで発生している`aspnet_Users`Bruce、Fred、および Tito、レコードがレコードがありません、`UserProfiles`テーブル。


[![Aspnet_Users の内容とユーザー テーブルが表示されます。](storing-additional-user-information-vb/_static/image32.png)](storing-additional-user-information-vb/_static/image31.png)

**図 11**: のコンテンツ、`aspnet_Users`と`UserProfiles`テーブルが表示されます ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image33.png))。


新しいレコードを追加、`UserProfiles`テーブルの値を手動で入力して、 `HomeTown`、 `HomepageUrl`、および`Signature`フィールド。 有効なを取得する最も簡単な方法`UserId`、新しい値`UserProfiles`レコードは、選択する、`UserId`フィールドで特定のユーザー アカウントを`aspnet_Users`の表に、コピーして貼り付けます、`UserId`フィールドに`UserProfiles`します。 図 12 は、`UserProfiles`テーブル Bruce の新しいレコードが追加されました。


[![Bruce のユーザーにレコードが追加されました](storing-additional-user-information-vb/_static/image35.png)](storing-additional-user-information-vb/_static/image34.png)

**図 12**: に A レコードが追加された`UserProfiles`の Bruce ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image36.png))。


戻り、 `AdditionalUserInfo.aspx page`、Bruce としてログインしています。 図 13 では、Bruce の設定が表示されます。


[![現在アクセスしてユーザーがいます。 彼の設定の表示](storing-additional-user-information-vb/_static/image38.png)](storing-additional-user-information-vb/_static/image37.png)

**図 13**: 現在アクセスしてユーザーがいます彼の設定の表示 ([フルサイズの画像を表示する をクリックします。](storing-additional-user-information-vb/_static/image39.png))。


> [!NOTE]
> 移動先を手動で追加のレコード、`UserProfiles`メンバーシップ ユーザーごとのテーブル。 手順 6. で自動的に新しい行を追加する CreateUserWizard コントロールをカスタマイズする方法に注目するは、`UserProfiles`新しいユーザー アカウントが作成されるテーブルします。


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>手順 3: 彼のホーム町、ホーム ページ、および署名を編集するユーザーを許可します。

そのホーム町、ホーム ページ、および署名の設定、この時点で、現在ログインしているユーザーを表示できますが、まだ変更することはできません。 データを編集できるように、DetailsView コントロールを更新しましょう。

最初の手順を行うには、追加、 `UpdateCommand` 、SqlDataSource の指定、`UPDATE`ステートメントを実行して、対応するパラメーター。 SqlDataSource を選択し、[プロパティ] ウィンドウからコマンドおよびパラメーターのエディター ダイアログ ボックスを表示する UpdateQuery プロパティの横にある省略記号をクリックします。 次の入力`UPDATE`ステートメントをテキスト ボックスに。

[!code-sql[Main](storing-additional-user-information-vb/samples/sample3.sql)]

次に、パラメーターを作成し、SqlDataSource コントロールの"パラメーターの更新 ボタンをクリックして`UpdateParameters`ごとでは、パラメーターのコレクション、`UPDATE`ステートメント。 なし にパラメーター セットのすべてのソースを残す場合および ダイアログ ボックスに ok ボタンをクリックします。


[![SqlDataSource の UpdateCommand と UpdateParameters を指定します。](storing-additional-user-information-vb/_static/image41.png)](storing-additional-user-information-vb/_static/image40.png)

**図 14**: 指定 SqlDataSource の`UpdateCommand`と`UpdateParameters`([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image42.png))。


追加機能により、SqlDataSource コントロール、コントロールが編集をサポートできるようになりました DetailsView を行いました。 DetailsView のスマート タグからには、"編集を有効にする チェック ボックスを確認します。 コントロールに、[commandfield] が追加されます`Fields`コレクションにその`ShowEditButton`プロパティを True に設定します。 これにより、DetailsView が読み取り専用モードと更新プログラムで表示され、編集モードを [キャンセル] ボタンに表示するとき、Edit ボタンを表示します。 編集 をクリックするユーザーを必要とするのではなくがあっても DetailsView レンダー「常に編集可能」な状態にするには、DetailsView コントロールの[`DefaultMode`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx)に`Edit`します。

これらの変更により、DetailsView コントロールの宣言型マークアップを次のようになります。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample4.aspx)]

[Commandfield] の追加に注意してください、`DefaultMode`プロパティ。

ブラウザーからこのページをテストしてみてください。 対応するレコードを含む、ユーザーにアクセスすると`UserProfiles`ユーザーの設定が編集可能なインターフェイスで表示されます。


[![DetailsView は編集可能なインターフェイスを表示します](storing-additional-user-information-vb/_static/image44.png)](storing-additional-user-information-vb/_static/image43.png)

**図 15**: DetailsView が編集可能なインターフェイスを表示します ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image45.png))。


お試しください値を変更し、[更新] ボタンをクリックします。 何かのように表示されます。 ポストバックがあると、値は、データベースに保存されますが、保存が発生した視覚的なフィードバックはありません。

これを解決するには、Visual Studio に戻り、DetailsView 上のラベル コントロールを追加します。 設定の`ID`に`SettingsUpdatedMessage`その`Text`プロパティを「の設定が更新されました」とその`Visible`と`EnableViewState`プロパティを`False`します。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample5.aspx)]

表示する必要があります、 `SettingsUpdatedMessage` DetailsView が更新されるたびにラベルを付けます。 これを実現するには、DetailsView のイベント ハンドラーを作成`ItemUpdated`イベントと、次のコードを追加します。

[!code-vb[Main](storing-additional-user-information-vb/samples/sample6.vb)]

戻り、`AdditionalUserInfo.aspx`ブラウザーでページし、データを更新します。 今回は、役立つステータス メッセージが表示されます。


[![短いメッセージが表示されるときに、設定が更新されます。](storing-additional-user-information-vb/_static/image47.png)](storing-additional-user-information-vb/_static/image46.png)

**図 16**: 設定が更新されたときに、短いメッセージが表示されます ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image48.png))。


> [!NOTE]
> DetailsView コントロールはが望ましい場合に多くを編集インターフェイスのままにします。 標準サイズのテキスト ボックスを使用しますが、署名フィールドには、複数行テキスト ボックスに、ある必要があります。 ホームページの URL を入力した場合で開始する"http://"または"https://"ことを確認する、RegularExpressionValidator を使用する必要があります。 さらに、コントロールに DetailsView 以降その`DefaultMode`プロパティに設定`Edit`、[キャンセル] ボタンは何も行いません。 いずれかを削除するか、または、クリックすると、リダイレクトする必要が、ユーザー別のページ (など`~/Default.aspx`)。 ままを演習として読者のため、これらの機能強化にします。


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>リンクを追加、`AdditionalUserInfo.aspx`マスター ページ内のページ

現時点では、web サイトにへのリンクを提供しない、`AdditionalUserInfo.aspx`ページ。 それに到達する唯一の方法では、ブラウザーのアドレス バーに直接、ページの URL を入力します。 このページへのリンクを追加してみましょう、`Site.master`マスター ページ。

マスター ページにはで、LoginView Web コントロールが含まれていることを思い出してください。 その`LoginContent`認証済みおよび匿名の訪問者のさまざまなマークアップが表示されるプレース ホルダーです。 LoginView コントロールの更新`LoggedInTemplate`へのリンクを含める、`AdditionalUserInfo.aspx`ページ。 LoginView にこれらの変更を行った後、コントロールの宣言型マークアップは、次のような検索する必要があります。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample7.aspx)]

メモの追加、`lnkUpdateSettings`ハイパーリンク コントロールを`LoggedInTemplate`します。 このリンクで認証されたユーザーすばやくそのホーム町、ホーム ページ、および署名の設定を表示したり、ページにジャンプできます。

## <a name="step-4-adding-new-guestbook-comments"></a>手順 4: 新しいゲストブックのコメントを追加します。

`Guestbook.aspx`ページが認証されたユーザーが、ゲストブックを表示およびコメントを残してください。 ゲストブックの新しいコメントを追加するインターフェイスの作成から始めましょう。

開く、 `Guestbook.aspx` Visual Studio でのページし、新しいコメントの件名と本文の 2 つの TextBox コントロールで構成されるユーザー インターフェイスを構築します。 最初のテキスト ボックス コントロールの設定`ID`プロパティを`Subject`とその`Columns`を 40 プロパティです設定の 2 番目の`ID`に`Body`その`TextMode`に`MultiLine`、およびその`Width`と`Rows`。プロパティを「95%」と 8 では、それぞれします。 ユーザー インターフェイスを完了するには、という名前のボタンの Web コントロールを追加`PostCommentButton`設定とその`Text`プロパティを「投稿、コメント」にします。

ゲストブックの各コメントには、件名と本文が必要とするためのテキスト ボックスの各を RequiredFieldValidator を追加します。 設定、 `ValidationGroup` "EnterComment"です。 これらのコントロールのプロパティ同様に、設定、`PostCommentButton`コントロールの`ValidationGroup`プロパティを"EnterComment"。 ASP の詳細についてはします。NET の検証コントロールをチェック アウト[ASP.NET のフォーム検証](http://www.4guysfromrolla.com/webtech/090200-1.shtml)、[詳細に分析する ASP.NET 2.0 の検証コントロール](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)、および[検証サーバー コントロールのチュートリアル](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp)[W3Schools](http://www.w3schools.com/)します。

ユーザー インターフェイスを作成したら、ページの宣言型マークアップに次のようなる必要があります。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample8.aspx)]

完全なユーザー インターフェイスで、次のタスクに新しいレコードを挿入する、`GuestbookComments`時にテーブル、`PostCommentButton`をクリックします。 これは、さまざまな方法で実現できます: ボタンの ADO.NET コードを記述できます`Click`イベント ハンドラーは SqlDataSource コントロールをページに追加、構成することができます、`InsertCommand`を呼び出してその`Insert`からメソッド、`Click`イベントハンドラー。またはゲストブックの新しいコメントを挿入するために割り当てられた中間層を作成してそのからこの機能を実行することも、`Click`イベント ハンドラー。 手順 3 で、SqlDataSource を使用するところからは、ここでの ADO.NET コードを使用してみましょう。

> [!NOTE]
> Microsoft SQL Server データベースからデータをプログラムでアクセスするために使用する ADO.NET クラスにある、`System.Data.SqlClient`名前空間。 ページの分離コード クラスにこの名前空間をインポートする必要があります (つまり、 `Imports System.Data.SqlClient`)。


イベント ハンドラーを作成、`PostCommentButton`の`Click`イベントと、次のコードを追加します。

[!code-vb[Main](storing-additional-user-information-vb/samples/sample9.vb)]

`Click`ユーザーが指定したデータが有効であるかをチェックしてイベント ハンドラーを起動します。 そうでない場合は、レコードを挿入する前に、イベント ハンドラーが終了します。 提供されたデータが有効である場合、現在ログオンしているユーザーの`UserId`値が取得されに格納されている、`currentUserId`ローカル変数。 指定する必要がありますので、この値が必要な`UserId`にレコードを挿入するときの値`GuestbookComments`します。

次に、接続文字列が、`SecurityTutorials`データベースがから取得した`Web.config`と`INSERT`SQL ステートメントを指定します。 A`SqlConnection`オブジェクトが、作成され開かれます。 次に、`SqlCommand`オブジェクトが構築されで使用されるパラメーターの値、`INSERT`クエリが割り当てられています。 `INSERT`ステートメントを実行して、接続が閉じられたとします。 イベント ハンドラーの最後に、`Subject`と`Body`テキスト ボックスの`Text`プロパティが、ポストバック間でユーザーの値が永続化しないようにクリアします。

このページで、ブラウザーでテストしてみてください。 このページがであるため、`Membership`フォルダーはアクセスできない匿名の訪問者にします。 そのため、(まだいる) 場合は最初にログオンする必要があります。 値を入力、`Subject`と`Body`テキスト ボックス をクリックし、`PostCommentButton`ボタンをクリックします。 これにより、新しいレコードに追加する`GuestbookComments`します。 、ポストバックの件名と本文の指定したが、テキスト ボックスからワイプされます。

クリックした後、`PostCommentButton`ボタンがありますに視覚的なフィードバックがない、コメント、ゲストブックを追加します。 手順 5 で行っては既存のゲストブック コメントを表示するには、このページを更新する必要があります。 ためには、単に追加されたコメントは適切な視覚的なフィードバックを提供する、コメントの一覧に表示されます。 ここでは、確認の内容を調べることで、ゲストブック コメントが保存される、`GuestbookComments`テーブル。

図 17 の内容を示しています、`GuestbookComments`表の後、2 つのコメントが残されています。


[![GuestbookComments テーブル内のゲストブックのコメントを確認できます。](storing-additional-user-information-vb/_static/image50.png)](storing-additional-user-information-vb/_static/image49.png)

**図 17**: ゲストブックのコメントを表示、`GuestbookComments`テーブル ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image51.png))。


> [!NOTE]
> ユーザーを含む可能性のあるゲストブックのコメントを挿入しようとしました。 – HTML – ASP.NET などの危険なマークアップがスローされます、`HttpRequestValidationException`します。 この例外がスローされた理由と危険性のある値を送信するユーザーを許可する方法の詳細についてを参照してください、[要求の検証に関するホワイト ペーパー](../../../../whitepapers/request-validation.md)します。


## <a name="step-5-listing-the-existing-guestbook-comments"></a>手順 5: 既存のゲストブックのコメントを一覧表示します。

コメントにアクセスしたユーザーを離れることに加え、`Guestbook.aspx`ページは、ゲストブックの既存のコメントを表示できる必要があります。 これを実現するという名前の ListView コントロールを追加`CommentList`ページの下部にします。

> [!NOTE]
> ListView コントロールは ASP.NET バージョン 3.5 に追加されました。 非常にカスタマイズ可能な柔軟なレイアウトで項目の一覧を表示しながら組み込みの編集、挿入、削除、ページング、および並べ替え GridView のような機能を引き続き提供は設計されています。 ASP.NET 2.0 を使用している場合は、DataList または Repeater コントロールの代わりに使用する必要があります。 ListView の使用に関する詳細については、次を参照してください。 [Scott Guthrie](https://weblogs.asp.net/scottgu/)のブログ エントリ「 [asp: ListView コントロール](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)、筆者の記事と[ListView コントロールにデータを表示する](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)します。


ListView のスマート タグを開き、データ ソースのドロップダウン リストから新しいデータ ソースにコントロールをバインドします。 手順 2. で説明したようにこれが、データ ソース構成ウィザードが起動します。 [データベース] アイコンを選択し、結果として得られる SqlDataSource という名前を`CommentsDataSource`、[ok] をクリックします。 次に、選択、`SecurityTutorialsConnectionString`接続がドロップダウン リストから文字列を [次へ] をクリックします。

この時点で手順 2. で指定したデータ クエリを選択して、`UserProfiles`ドロップダウン リストからテーブルし、返される列を選択する (戻るは図 9 を参照してください)。 今回は、ただし、するバックアップからのレコードだけでなくをプルする SQL ステートメントを作成する`GuestbookComments`、校閲者の出身地も、ホームページが、署名、およびユーザー名。 そのため、「カスタム SQL ステートメントまたはストアド プロシージャを指定する」オプション ボタンを選択し、[次へ] をクリックします。

「定義カスタム ステートメントまたはストアド プロシージャ」画面が表示されます。 クエリをグラフィカルに作成、クエリ ビルダーのボタンをクリックします。 クエリを実行するテーブルを指定することを要求することで、クエリ ビルダーを開始します。 選択、 `GuestbookComments`、 `UserProfiles`、および`aspnet_Users`テーブルし、[ok] をクリックします。 これにより、すべての 3 つのテーブルがデザイン画面に追加されます。 間での外部キー制約があるので、 `GuestbookComments`、 `UserProfiles`、および`aspnet_Users`テーブル、クエリ ビルダーに自動的に`JOIN`s これらのテーブル。

残っているは返される列を指定します。 `GuestbookComments`テーブルの選択、 `Subject`、`Body`と`CommentDate`の列は返された場合、 `HomeTown`、 `HomepageUrl`、および`Signature`からの列、`UserProfiles`テーブルです戻って`UserName`から。`aspnet_Users`. また、追加する"`ORDER BY CommentDate DESC`"の末尾に、`SELECT`最も最近の投稿が最初に返されるようにクエリを実行します。 これらの選択を行った後、クエリ ビルダー インターフェイスは図 18 でスクリーン ショットのようになります。


[![構築クエリ GuestbookComments、ユーザー、および aspnet_Users テーブルを結合します。](storing-additional-user-information-vb/_static/image53.png)](storing-additional-user-information-vb/_static/image52.png)

**図 18**:、構築されたクエリ`JOIN`s、 `GuestbookComments`、 `UserProfiles`、および`aspnet_Users`テーブル ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image54.png))。


クエリ ビルダー ウィンドウを閉じて、「定義カスタム ステートメントまたはストアド プロシージャ」画面に戻るには、ok をクリックします。 クエリのテスト ボタンをクリックして、クエリの結果を表示することがあります、「テスト クエリ」画面に進みの横にあるをクリックします。 準備ができたら、データ ソース構成ウィザードを完了するには、[完了] をクリックします。

手順 2 で、関連付けられている DetailsView コントロールのデータ ソース構成ウィザードが完了すると`Fields`に含める、BoundField によって返される各列のコレクションが更新された、`SelectCommand`します。 ListView がただしは変更されません。引き続き、そのレイアウトを定義する必要があります。 ListView のレイアウトは、その宣言型マークアップまたはのスマート タグの"ListView の構成"オプションから、手動で構築できます。 通常、マークアップを手動で定義しますが、最も自然なメソッドを使用します。

最終的に、次を使用して`LayoutTemplate`、 `ItemTemplate`、および`ItemSeparatorTemplate`ListView コントロールの。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample10.aspx)]

`LayoutTemplate`のマークアップを定義、コントロールによって生成されるときに、 `ItemTemplate` SqlDataSource によって返される各項目を表示します。 `ItemTemplate`の結果として得られるマークアップが配置されます、`LayoutTemplate`の`itemPlaceholder`コントロール。 加え、 `itemPlaceholder`、`LayoutTemplate`制限 (既定値) のページごとに 10 だけのゲストブック コメントを表示する ListView、DataPager コントロールを含み、ページング インターフェイスをレンダリングします。

マイ`ItemTemplate`で各ゲストブック コメントの内容が表示されます、`<h4>`サブジェクトの下に設置されている本文を持つ要素。 本文を表示するための構文がによって返されるデータを受け取ることに注意してください、`Eval("Body")`データ バインド ステートメントでは、文字列に変換し、置換改行位置で、`<br />`要素。 この変換は、HTML での空白は無視されますので、コメントを送信するときに入力された改行を表示するために必要です。 ユーザーの署名は、斜体、彼のホーム ページ、日付と時刻のコメントが行われたコメントを残した相手のユーザー名へのリンク、ユーザーのホーム町ごとの後に、本文の下に表示されます。

ブラウザーでページを表示する時間がかかります。 ここに表示される手順 5. でゲストブックを追加したコメントを参照してください。


[![Guestbook.aspx ここでは、ゲストブックのコメントを表示します。](storing-additional-user-information-vb/_static/image56.png)](storing-additional-user-information-vb/_static/image55.png)

**図 19**:`Guestbook.aspx`ゲストブックのコメントが表示されます ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image57.png))。


新しいコメント、ゲストブックを追加してみてください。 クリックすると、`PostCommentButton`ボタン、ページがポストバックしコメントは、データベースに追加されますが、ListView コントロールは、新しいコメントを表示するのには更新されません。 これは、いずれかで解決できます。

- 更新、`PostCommentButton`ボタンの`Click`イベント ハンドラーを呼び出す、ListView コントロールのため`DataBind()`をデータベースに新しいコメントを挿入した後にメソッドまたは
- ListView コントロールの設定`EnableViewState`プロパティを`False`します。 このアプローチは、コントロールのビューステートを無効にすると、ポストバックのたびに基になるデータにバインドする必要がありますので機能します。

このチュートリアルからダウンロード可能なチュートリアルの web サイトは、両方の方法を示しています。 ListView コントロールの`EnableViewState`プロパティを`False`、ListView にデータをプログラムによって再バインドするために必要なコードがである、`Click`イベント ハンドラーをコメント アウトされていますが、します。

> [!NOTE]
> 現在、`AdditionalUserInfo.aspx`ページで、ユーザーを表示して、ホーム町、ホーム ページ、および署名の設定を編集できます。 更新すると良い`AdditionalUserInfo.aspx`ゲストブックのユーザーのコメントに、ログ記録を表示します。 つまりを調べると、自分の情報を変更する、だけでなく、ユーザーがアクセスできる、`AdditionalUserInfo.aspx`ページを参照するどのようなゲストブックのコメントをこれまでで彼女が行われます。 ままを演習として興味を持った読者にします。


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>手順 6: ホーム町、ホーム ページ、および署名のためのインターフェイスを含める CreateUserWizard コントロールをカスタマイズします。

`SELECT`で使用されるクエリ、`Guestbook.aspx`ページ使用して、`INNER JOIN`間で関連レコードを結合して、 `GuestbookComments`、 `UserProfiles`、および`aspnet_Users`テーブル。 場合にレコードがないユーザー`UserProfiles`ため、コメントは、ListView で表示されませんがゲストブックのコメント、`INNER JOIN`のみが返されます`GuestbookComments`レコードに一致するレコードがある場合に`UserProfiles`と`aspnet_Users`します。 ユーザーがレコード手順 3 で説明したように、`UserProfiles`彼女を表示またはで自分の設定を編集することはできません、`AdditionalUserInfo.aspx`ページ。

言う、設計が意思決定は、メンバーシップ システム内のすべてのユーザー アカウントは、一致することが重要に記録、`UserProfiles`テーブル。 対応するレコードに追加するには、意図した`UserProfiles`CreateUserWizard を通じて新しいメンバーシップ ユーザーのアカウントが作成されるたびにします。

説明したように、 [*ユーザー アカウントを作成する*](creating-user-accounts-vb.md)チュートリアルでは、新しいメンバーシップ ユーザーのアカウントには、CreateUserWizard コントロールが作成された後に発生させる、 [ `CreatedUser`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). このイベントのイベント ハンドラーを作成、作成したばかりのユーザーのユーザー Id を取得してにレコードを挿入し、`UserProfiles`の既定値を持つテーブル、 `HomeTown`、`HomepageUrl`と`Signature`列。 さらに、CreateUserWizard コントロールのインターフェイスを追加するテキスト ボックスをカスタマイズすることでユーザーにこれらの値を要求することができます。

最初に新しい行を追加する方法を見てみましょう、`UserProfiles`テーブルに、`CreatedUser`既定値を持つイベント ハンドラー。 次に、新しいユーザーのホーム町、ホーム ページ、および署名を収集する追加のフォーム フィールドを含める CreateUserWizard コントロールのユーザー インターフェイスをカスタマイズする方法が表示されます。

### <a name="adding-a-default-row-touserprofiles"></a>既定の行を追加します。`UserProfiles`

[*ユーザー アカウントを作成する*](creating-user-accounts-vb.md) CreateUserWizard コントロールを追加しましたチュートリアル、`CreatingUserAccounts.aspx`ページで、`Membership`フォルダー。 コントロールをレコードの追加、CreateUserWizard を確保するために`UserProfiles`テーブル ユーザー アカウントの作成時に、CreateUserWizard コントロールの機能を更新する必要があります。 このような変更ではなく、 `CreatingUserAccounts.aspx`  ページで、代わりに新しい CreateUserWizard コントロールを追加してみましょう`EnhancedCreateUserWizard.aspx`ページし、このチュートリアルでは、ある変更を行います。

開く、 `EnhancedCreateUserWizard.aspx` Visual Studio でのページおよび CreateUserWizard コントロールをツールボックスからページにドラッグします。 CreateUserWizard コントロールの設定`ID`プロパティを`NewUserWizard`します。 説明したように、 [*ユーザー アカウントを作成する*](creating-user-accounts-vb.md)チュートリアル, CreateUserWizard の既定のユーザー インターフェイスには、必要な情報の利用者が求められます。 この情報が提供されていると、コントロール内部的に新しいユーザー アカウントを作成、メンバーシップ フレームワークに 1 行のコードを記述することがなく。

CreateUserWizard コントロールは、そのワークフローの中にイベントの数を生成します。 CreateUserWizard コントロールを最初に発生させる訪問者は、要求情報を提供し、フォームを送信する、その[`CreatingUser`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx)します。 作成プロセス中に問題がある場合、 [ `CreateUserError`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx)が発生します。 ただし、ユーザーが正常に作成する場合は、次に、 [ `CreatedUser`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx)が発生します。 [*ユーザー アカウントを作成する*](creating-user-accounts-vb.md)チュートリアルのイベント ハンドラーを作成した、`CreatingUser`イベント、先頭または末尾の空白とを指定されたユーザー名が含まれないようにしますが、パスワードに、ユーザー名は任意の場所に表示されませんでした。

内の行を追加するには、`UserProfiles`テーブルのイベント ハンドラーの作成に必要な作成したばかりのユーザーの`CreatedUser`イベント。 時間によって、`CreatedUser`イベントが発生した、ユーザー アカウントは既に有効にすると、アカウントのユーザー Id 値を取得するため、メンバーシップ フレームワークで作成されています。

イベント ハンドラーを作成、`NewUserWizard`の`CreatedUser`イベントと、次のコードを追加します。

[!code-vb[Main](storing-additional-user-information-vb/samples/sample11.vb)]

追加したばかりのユーザー アカウントのユーザー Id を取得することによって上記のコード開始。 使用してこの情報は、`Membership.GetUser(username)`に関する特定のユーザー、および使用して情報を返すメソッド、`ProviderUserKey`ユーザー Id を取得するプロパティ。 CreateUserWizard コントロールでユーザーが入力したユーザー名を利用し、 [ `UserName`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx)します。

次に、接続文字列がから取得した`Web.config`と`INSERT`ステートメントを指定します。 必要な ADO.NET オブジェクトがインスタンス化し、コマンドを実行します。 コードを割り当てます、 [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx)インスタンスを`@HomeTown`、`@HomepageUrl`と`@Signature`パラメーターは、データベースの挿入の影響は`NULL`の値を`HomeTown`、 `HomepageUrl`、および`Signature`フィールド。

参照してください、`EnhancedCreateUserWizard.aspx`ブラウザーでページし、新しいユーザー アカウントを作成します。 その後、Visual Studio に戻りの内容を確認して、`aspnet_Users`と`UserProfiles`(図 12 に行った) などのテーブルします。 新しいユーザー アカウントを表示する必要があります`aspnet_Users`と対応する`UserProfiles`行 (で`NULL`値`HomeTown`、 `HomepageUrl`、および`Signature`)。


[![新しいユーザー アカウントとユーザー レコードが追加されました](storing-additional-user-information-vb/_static/image59.png)](storing-additional-user-information-vb/_static/image58.png)

**図 20**: ユーザー アカウントの新規と`UserProfiles`レコードが追加されています ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image60.png))。


訪問者が、新しいアカウント情報を指定して「ユーザーの作成」ボタンをクリックした、ユーザー アカウントの作成、および行に追加した後、`UserProfiles`テーブル。 CreateUserWizard を表示し、その`CompleteWizardStep`、成功メッセージと続行ボタンが表示されます。 [続行] ボタンをクリックすると、ポストバック、アクションは実行されませんでスタックしているユーザーを離れること、`EnhancedCreateUserWizard.aspx`ページ。

CreateUserWizard コントロールを使用して、[続行] ボタンがクリックされたときにユーザーに送信する URL を指定します[`ContinueDestinationPageUrl`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)します。 設定、`ContinueDestinationPageUrl`プロパティを"~/Membership/AdditionalUserInfo.aspx"。 これにより、新しいユーザーを`AdditionalUserInfo.aspx`、場所、表示して設定を更新します。

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>新しいユーザーのホーム町、ホーム ページ、および署名の入力を求めるを CreateUserWizard のインターフェイスのカスタマイズ

CreateUserWizard コントロールの既定のインターフェイスは、単純なアカウントの作成シナリオのユーザー名、パスワード、および電子メールのような唯一のコア ユーザー アカウント情報を収集する必要があれば十分です。 しかし、自分のアカウントを作成する際に、自分の出身地、ホーム ページ、および署名を入力するビジターを要求したい場合でしょうか。 サインアップの詳細情報を収集する、CreateUserWizard コントロールのインターフェイスをカスタマイズすることができますし、この情報を使用することがあります、`CreatedUser`イベント ハンドラーを基になるデータベースに追加のレコードを挿入します。

CreateUserWizard コントロールは、一連の順序を定義するページの開発者が可能なコントロールである ASP.NET のウィザード コントロールを拡張`WizardSteps`します。 ウィザード コントロールは、アクティブなステップをレンダリングし、により、これらの手順を移動するビジター ナビゲーション インターフェイスを提供します。 ウィザード コントロールは、いくつかの簡単な手順に長いタスクを分割するために最適です。 ウィザード コントロールの詳細については、次を参照してください。[ステップ バイ ステップのユーザー インターフェイスを作成する ASP.NET 2.0 ウィザード コントロールに](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)します。

CreateUserWizard コントロールの既定のマークアップは、2 つ定義`WizardSteps`:`CreateUserWizardStep`と`CompleteWizardStep`します。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample12.aspx)]

最初の`WizardStep`、 `CreateUserWizardStep`、ユーザー名、パスワード、電子メール、およびなどの入力を求めるインターフェイスをレンダリングします。 彼女が示すように、訪問者は、この情報を提供し、「ユーザーの作成」をクリックすると、 `CompleteWizardStep`、成功メッセージと続行ボタンを示しています。

追加のフォームのフィールドを含める、CreateUserWizard コントロールのインターフェイスをカスタマイズするには、ことと、以下のことができます。

- <strong>1 つまたは複数を新規作成</strong><strong>`WizardStep`</strong><strong>、追加のユーザー インターフェイス要素を格納する s</strong>します。 新しい追加`WizardStep`CreateUserWizard をクリックして、"追加/削除`WizardStep`s"を起動するスマート タグからのリンク、`WizardStep`コレクション エディター。 そこから追加、削除、または、ウィザードの手順の順序を変更することができます。 これは、このチュートリアルを使用するアプローチです。

- <strong>変換、</strong><strong>`CreateUserWizardStep`</strong><strong>が編集可能に</strong><strong>`WizardStep`</strong><strong>します。</strong> これにより置き換えられます、`CreateUserWizardStep`同等な`WizardStep`をマークアップに一致するユーザー インターフェイスを定義する、 `CreateUserWizardStep`' s。 変換することで、`CreateUserWizardStep`に、`WizardStep`コントロールの位置を変更または追加のユーザー インターフェイス要素をこのステップを追加できます。 変換する、`CreateUserWizardStep`または`CompleteWizardStep`が編集可能に`WizardStep`コントロールのスマート タグからのリンクを「完全なステップをカスタマイズする」や「ユーザーの作成のカスタマイズ ステップ」をクリックします。

- **上記の 2 つのオプションの組み合わせを使用します。**

内から、「ユーザーの作成」ボタンがクリックされたときに、CreateUserWizard コントロールはそのユーザー アカウントの作成プロセスを実行します。 1 つの重要な点に留意するのはその`CreateUserWizardStep`します。 追加があってもかまいません`WizardStep`秒後に、`CreateUserWizardStep`かどうか。

カスタムを追加するときに`WizardStep`CreateUserWizard コントロール、カスタムの追加のユーザー入力を収集するために`WizardStep`前に、または後に配置することができます、`CreateUserWizardStep`します。 前に来る場合、`CreateUserWizardStep`カスタムから追加のユーザー入力を収集し`WizardStep`利用、`CreatedUser`イベント ハンドラー。 ただし場合、カスタム`WizardStep`の後に続く`CreateUserWizardStep`時間によって、カスタム`WizardStep`が表示されます、新しいユーザー アカウントが既に作成されて、`CreatedUser`イベントが既に発生しています。

図 21 は、ワークフローを示しています。 ときに、追加した`WizardStep`の前に、`CreateUserWizardStep`します。 追加のユーザー情報が、時間によって収集されたため、`CreatedUser`イベントが発生するすべての更新は、`CreatedUser`をこれらの入力を取得し、それらを使用してイベント ハンドラー、`INSERT`ステートメントのパラメーターの値 (なく`DBNull.Value`).


[![CreateUserWizard ワークフロー、CreateUserWizardStep の直前に追加の wizardstep.](storing-additional-user-information-vb/_static/image62.png)](storing-additional-user-information-vb/_static/image61.png)

**図 21**:、CreateUserWizard ワークフローときに、追加`WizardStep`Precedes、 `CreateUserWizardStep` ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image63.png))。


場合、カスタム`WizardStep`配置*後*、 `CreateUserWizardStep`、自分の出身地、ホームページ、または署名を入力する機会があるユーザーに提供する前に、ただし、ユーザー アカウントの作成プロセスが発生します。 このような場合は、この追加情報をデータベースに挿入される、ユーザー アカウントが作成されて、図 22 に示すように必要があります。


[![CreateUserWizard ワークフロー、CreateUserWizardStep 後に関しては追加の wizardstep.](storing-additional-user-information-vb/_static/image65.png)](storing-additional-user-information-vb/_static/image64.png)

**図 22**:、CreateUserWizard ワークフローときに、追加`WizardStep`後は、 `CreateUserWizardStep` ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image66.png))。


図 22 に示すように、ワークフローの待機にレコードを挿入する、`UserProfiles`手順 2. が完了後するまでテーブルします。 訪問者は、手順 1. の後、ブラウザーを閉じ場合、ただし、私たちに達している、ユーザー アカウントが作成されましたに追加されたレコードがない状態`UserProfiles`します。 1 つの回避策は持つレコードが存在する`NULL`に挿入された値を既定または`UserProfiles`で、 `CreatedUser` (手順 1 の後に発生) するイベント ハンドラー、および手順 2 が完了したらこの記録し更新します。 これにより、`UserProfiles`ユーザーが登録プロセスの途中で終了した場合でも、ユーザー アカウントのレコードが追加されます。

このチュートリアルを作成しましょう新しい`WizardStep`以降後に発生した、`CreateUserWizardStep`する前に、`CompleteWizardStep`します。 みましょうで wizardstep の配置および構成されている最初に取得し、そこでコードについて説明します。

CreateUserWizard コントロールのスマート タグから選択して、"追加/削除`WizardStep`s"が表示されます、`WizardStep`コレクション エディター ダイアログ。 新しい追加`WizardStep`で、設定、`ID`に`UserSettings`、その`Title`「設定」してその`StepType`に`Step`。 後になるようにし、配置、 `CreateUserWizardStep` ("Sign Up Your の新しいアカウントの") と前に、 `CompleteWizardStep` (「完了」)、図 23 に示すようにします。


[![CreateUserWizard コントロールに新しい wizardstep を追加します。](storing-additional-user-information-vb/_static/image68.png)](storing-additional-user-information-vb/_static/image67.png)

**図 23**: 新規追加`WizardStep`CreateUserWizard コントロールを ([フルサイズの画像を表示する をクリックします](storing-additional-user-information-vb/_static/image69.png))。


閉じるには、ok をクリックして、`WizardStep`コレクション エディター ダイアログ。 新しい`WizardStep`の上昇 CreateUserWizard コントロールの更新された宣言型マークアップによって明らかです。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample13.aspx)]

新しいメモ`<asp:WizardStep>`要素。 新しいユーザーのホーム町、ホーム ページ、およびここに署名を収集するユーザー インターフェイスを追加する必要があります。 宣言構文内で、またはデザイナーを使用、このコンテンツを入力することができます。 デザイナーを使用するには、デザイナーでの手順を参照するスマート タグでドロップダウン リストから「設定の」ステップを選択します。

> [!NOTE]
> CreateUserWizard コントロールの更新プログラムのスマート タグのドロップダウン リストからステップを選択[`ActiveStepIndex`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx)、開始ステップのインデックスを指定します。 このドロップダウン リストを使用して、デザイナーで「設定の」ステップを編集する場合がユーザーの初回アクセス時にこの手順が表示されるように「記号を新しいアカウントの」に設定することを確認するため、`EnhancedCreateUserWizard.aspx`ページ。


という名前の 3 つのテキスト ボックス コントロールを含む「設定の」ステップ内のユーザー インターフェイスを作成`HomeTown`、 `HomepageUrl`、および`Signature`します。 このインターフェイスを構築するには後、CreateUserWizard の宣言型マークアップは次のようになります。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample14.aspx)]

さあ、ブラウザーからこのページを参照してください、出身地、ホーム ページ、および署名の値を指定する、新しいユーザー アカウントを作成します。 完了した後、`CreateUserWizardStep`メンバーシップ フレームワークでは、ユーザー アカウントを作成してくださいおよび`CreatedUser`に新しい行を追加します。 イベント ハンドラーが実行`UserProfiles`、データベースが`NULL`値`HomeTown`、 `HomepageUrl`、と。`Signature`. ホーム町、ホーム ページ、および署名の入力した値は使用されません。 最終的に新しいユーザー アカウントには、`UserProfiles`レコードを`HomeTown`、 `HomepageUrl`、および`Signature`指定するフィールドがまだ必要です。

ユーザーが入力したホーム町、honepage、および署名の値を取得し、適切な更新を「設定」手順の後にコードを実行する必要があります`UserProfiles`レコード。 ユーザーは、ウィザードの手順の間で移動するたびにコントロールをウィザードの[`ActiveStepChanged`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx)が起動します。 このイベントと更新プログラムのイベント ハンドラーを作成することができます、 `UserProfiles` 「設定の」ステップが完了したときにテーブルです。

CreateUserWizard のイベント ハンドラーを追加`ActiveStepChanged`イベントと、次のコードを追加します。

[!code-vb[Main](storing-additional-user-information-vb/samples/sample15.vb)]

上記のコードは、"Complete"ステップに達しましただけかどうかによって開始します。 「設定の」ステップの直後に「完了」手順が発生したため、訪問者に達すると「完了」ステップ「設定の」ステップが彼女に終了したことを意味します。

このような場合は、内のテキスト ボックス コントロールをプログラムで参照する、`UserSettings WizardStep`します。 使用してこの情報は、`FindControl`メソッドをプログラムで参照する、 `UserSettings WizardStep`、し、内から、テキスト ボックスを参照するには、もう一度、`WizardStep`します。 実行する準備ができました、テキスト ボックスを参照していますと、`UPDATE`ステートメント。 `UPDATE`ステートメントには同数のパラメーターとして、`INSERT`内のステートメント、`CreatedUser`イベントのハンドラーが、ここでは、ユーザーが指定したホーム町、ホーム ページ、および署名の値を使用します。

場所でのこのイベント ハンドラーに次を参照してください。、`EnhancedCreateUserWizard.aspx`ブラウザーでページとホーム町、ホーム ページ、および署名の値を指定する新しいユーザー アカウントを作成します。 新しいアカウントの作成後にリダイレクトする必要があります、 `AdditionalUserInfo.aspx`  ページで、だけが入力したホーム町、ホーム ページ、および署名情報が表示されます。

> [!NOTE]
> 当社の web サイトの訪問者が新しいアカウントを作成する 2 つのページが現在:`CreatingUserAccounts.aspx`と`EnhancedCreateUserWizard.aspx`します。 Web サイトのサイト マップおよびログイン ページを指す、 `CreatingUserAccounts.aspx`  ページが、`CreatingUserAccounts.aspx`ページがホーム町、ホーム ページ、および署名については、ユーザー プロンプトが表示されませんしに対応する行は追加されません`UserProfiles`します。 そのため、更新、`CreatingUserAccounts.aspx`この機能を提供するためのページまたはページを更新するサイト マップおよびログインを参照する`EnhancedCreateUserWizard.aspx`の代わりに`CreatingUserAccounts.aspx`します。 後者のオプションを選択する場合に更新することを確認して、`Membership`フォルダーの`Web.config`匿名ユーザーへのアクセスを許可するためのファイル、`EnhancedCreateUserWizard.aspx`ページ。


## <a name="summary"></a>まとめ

このチュートリアルでは、メンバーシップ フレームワーク内のユーザー アカウントに関連付けられているデータのモデリング手法について説明しました。 具体的には、一対多リレーションシップを一対一のリレーションシップを共有するデータだけでなく、ユーザー アカウントのユーザーと共有エンティティをモデル化について説明しました。 さらに、この関連情報を表示、挿入、および、SqlDataSource コントロールとその他のユーザーを使用していくつかの例では、更新がどのようにした ADO.NET コードを使用します。

このチュートリアルを完了すると、ユーザー アカウント、参照してください。 以降、次のチュートリアルで注目をロールに変換されます。 今後は、ロール フレームワークに注目するはいくつかのチュートリアルは、新しいロールを作成する方法を参照してください。 どのような役割を決定する、ユーザーが所属する方法と、ユーザーにロールを割り当てる方法、ロールベースの承認を適用します。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [アクセスして、ASP.NET 2.0 のデータの更新](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 2.0 ウィザード コントロール](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [ASP.NET 2.0 ウィザード コントロールのステップ バイ ステップのユーザー インターフェイスを作成します。](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [カスタム データ ソース コントロールのパラメーターを作成します。](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [CreateUserWizard コントロールをカスタマイズします。](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [DetailsView コントロールのクイック スタート](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [ListView コントロールでデータの表示](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [詳細に分析する ASP.NET 2.0 の検証コントロール](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [データ挿入の編集と削除](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [ASP.NET のフォーム検証](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [カスタムのユーザー登録情報の収集](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [ASP.NET 2.0 のプロファイル](http://www.odetocode.com/Articles/440.aspx)
- [Asp: ListView コントロール](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [ユーザー プロファイルのクイック スタート](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>執筆者紹介

Scott Mitchell、複数の受け取ります書籍の著者と、4GuysFromRolla.com の創設者では、1998 年から、Microsoft Web テクノロジに取り組んできました。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は *[Sams 教える自分で ASP.NET 2.0 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* します。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別に感謝しています.

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)します。

> [!div class="step-by-step"]
> [前へ](user-based-authorization-vb.md)
