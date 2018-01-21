---
title: "目的の文字列"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core データ保護 Api で目的の文字列を使用する方法について説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: b1e95c9d0aa8195aa73fddfb97a4079e67a351bf
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="purpose-strings"></a>目的の文字列

<a name="data-protection-consumer-apis-purposes"></a>

コンポーネントから利用`IDataProtectionProvider`を一意に渡す必要があります*目的*パラメーターを`CreateProtector`メソッドです。 目的*パラメーター*はように、暗号化のコンシューマー間の分離を提供するルートの暗号化キーが同じ場合でも、データ保護システムのセキュリティに固有です。

コンシューマーは、目的を示す、そのコンシューマーに一意の暗号のサブキーを派生するルートの暗号化キーと共に、目的の文字列が使用されます。 これにより、アプリケーションでその他のすべての暗号コンシューマーからコンシューマーを排除します。 その他のコンポーネントは、そのペイロードを読み取ることではありませんし、他のコンポーネントのペイロードを読み取ることができません。 この分離は、コンポーネントに対する攻撃の実行不可能な場合の全体カテゴリも表示します。

![目的の図の例](purpose-strings/_static/purposes.png)

、上記の図で`IDataProtector`インスタンス A と B**できません**ペイロードを読み取る互いの、のみ独自です。

目的の文字列は、秘密にするがありません。 単に適切に動作の他のコンポーネントがこれまでしないことを目的の文字列と同じ意味で一意である必要があります。

>[!TIP]
> 適切な原則として、この情報が競合することはありませんプラクティスと同様には、データ保護 Api を使用するコンポーネントの名前空間と型名を使用します。
>
>Minting ベアラー トークンを担当する contoso 社製コンポーネントは、その目的の文字列として Contoso.Security.BearerToken を使用できます。 または、目的の文字列として Contoso.Security.BearerToken.v1 を - さらに、使用可能性があります。 バージョン番号を追加することにより、将来のバージョンを目的に応じて Contoso.Security.BearerToken.v2 を使用する、さまざまなバージョン互いから完全に分離ペイロード移動限りなります。

目的のパラメーターから`CreateProtector`文字列配列では、上記でした代わりとして指定されて`[ "Contoso.Security.BearerToken", "v1" ]`です。 これは、目的の階層を確立することができ、データ保護システムを使用するマルチ テナント シナリオの可能性を開きます。

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> コンポーネントは、目的のチェーンの入力の唯一のソースである信頼されていないユーザー入力を許可しません。
>
>たとえば、セキュリティで保護されたメッセージの格納を担当する Contoso.Messaging.SecureMessage コンポーネントを検討してください。 セキュリティで保護されたメッセージング コンポーネントを呼び出す場合`CreateProtector([ username ])`、悪意のあるユーザーを呼び出すコンポーネントを取得しようとするにユーザー名"Contoso.Security.BearerToken"を持つアカウントを作成する可能性があります、 `CreateProtector([ "Contoso.Security.BearerToken" ])`、誤ってしまい、セキュリティで保護されたメッセージング認証トークンとして認識でした言い換えるペイロードにシステムです。
>
>メッセージング コンポーネントを向上させる目的でチェーンがなります`CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`、適切な分離を提供します。

提供される分離との動作`IDataProtectionProvider`、 `IDataProtector`、その目的は次のとおり。

* 指定された`IDataProtectionProvider`オブジェクト、`CreateProtector`メソッドは、作成、`IDataProtector`両方にオブジェクトが一意に関連付けられている、`IDataProtectionProvider`メソッドに渡されたする目的でパラメーターを作成するオブジェクト。

* 目的のパラメーターを null にするにはできません。 (場合は、配列としての目的を指定すると、つまり長さ 0 の配列にはできません、配列のすべての要素が null にする必要があります。)空の文字列目的は、技術的に可能ですは避けることを推奨。

* 2 つの目的の引数は、同じ順序では、(序数の比較子を使用して)、同じ文字列が含まれている場合にのみに相当します。 1 つの目的の引数は、対応する 1 つの要素の目的で配列と同じです。

* 2 つ`IDataProtector`該当するショートカットからが作成される場合にのみ、オブジェクトが等しく`IDataProtectionProvider`と同等の目的でパラメーターを持つオブジェクト。

* 指定された`IDataProtector`オブジェクトへの呼び出し`Unprotect(protectedData)`元を返す`unprotectedData`場合にのみ`protectedData := Protect(unprotectedData)`同等の`IDataProtector`オブジェクト。

> [!NOTE]
> いくつかのコンポーネントが意図的に別のコンポーネントと競合する呼ばれる目的の文字列を選択、ケース マイクロソフトいない検討しています。 このようなコンポーネントは基本的と見なされる悪意のあると、このシステムは、悪意のあるコードは、ワーカー プロセス内で既に実行されているセキュリティ保証を提供するものではありません。
