---
title: "目的の文字列"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c96ed361-c382-4980-8933-800e740cfc38
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: cc33bcfab4945e6d6f9ca7e61edeff4d1837661a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="purpose-strings"></a>目的の文字列

<a name=data-protection-consumer-apis-purposes></a>

IDataProtectionProvider を消費するコンポーネントを一意に渡す必要があります*目的*CreateProtector メソッドのパラメーターです。 目的*パラメーター*はように、暗号化のコンシューマー間の分離を提供するルートの暗号化キーが同じ場合でも、データ保護システムのセキュリティに固有です。

コンシューマーは、目的を示す、そのコンシューマーに一意の暗号のサブキーを派生するルートの暗号化キーと共に、目的の文字列が使用されます。 これにより、アプリケーションでその他のすべての暗号コンシューマーからコンシューマーを排除します。 その他のコンポーネントは、そのペイロードを読み取ることではありませんし、他のコンポーネントのペイロードを読み取ることができません。 この分離は、コンポーネントに対する攻撃の実行不可能な場合の全体カテゴリも表示します。

![目的の図の例](purpose-strings/_static/purposes.png)

IDataProtector インスタンス A と B 上の図で**できません**ペイロードを読み取る互いののみ独自です。

目的の文字列は、秘密にするがありません。 単に適切に動作の他のコンポーネントがこれまでしないことを目的の文字列と同じ意味で一意である必要があります。

>[!TIP]
> 適切な原則として、この情報が競合することはありませんプラクティスと同様には、データ保護 Api を使用するコンポーネントの名前空間と型名を使用します。
>
>Minting ベアラー トークンを担当する contoso 社製コンポーネントは、その目的の文字列として Contoso.Security.BearerToken を使用できます。 または、目的の文字列として Contoso.Security.BearerToken.v1 を - さらに、使用可能性があります。 バージョン番号を追加することにより、将来のバージョンを目的に応じて Contoso.Security.BearerToken.v2 を使用する、さまざまなバージョン互いから完全に分離ペイロード移動限りなります。

CreateProtector する目的でパラメーターが文字列配列であるため、上記でしたが代わりに指定されて ["Contoso.Security.BearerToken"、"v1"] として。 これは、目的の階層を確立することができ、データ保護システムを使用するマルチ テナント シナリオの可能性を開きます。

<a name=data-protection-contoso-purpose></a>

>[!WARNING]
> コンポーネントは、目的のチェーンの入力の唯一のソースである信頼されていないユーザー入力を許可しません。
>
>たとえば、セキュリティで保護されたメッセージの格納を担当する Contoso.Messaging.SecureMessage コンポーネントを検討してください。 セキュリティで保護されたメッセージング コンポーネントが CreateProtector ([username]) を呼び出すかどうかは、悪意のあるユーザーを呼び出すコンポーネントを取得しようとするにユーザー名"Contoso.Security.BearerToken"を持つアカウントを作成する可能性があります CreateProtector(["Contoso.Security.BearerToken"])、したがって言い換えるペイロードをセキュリティで保護されたメッセージング システムを誤ってに原因とでしたと認識される認証トークンです。
>
>メッセージング コンポーネントを向上させる目的でチェーン CreateProtector になります (["Contoso.Messaging.SecureMessage"、"ユーザー: ユーザー名"])、適切な分離を提供します。

提供される分離および IDataProtectionProvider、IDataProtector、および目的の動作は次のとおりです。

* IDataProtectionProvider の特定のオブジェクトの CreateProtector メソッドは一意に作成した IDataProtectionProvider オブジェクトと、メソッドに渡されたする目的でパラメーターの両方に関連付けられている IDataProtector オブジェクトを作成します。

* 目的のパラメーターを null にするにはできません。 (場合は、配列としての目的を指定すると、つまり長さ 0 の配列にはできません、配列のすべての要素が null にする必要があります。)空の文字列目的は、技術的に可能ですは避けることを推奨。

* 2 つの目的の引数は、同じ順序では、(序数の比較子を使用して)、同じ文字列が含まれている場合にのみに相当します。 1 つの目的の引数は、対応する 1 つの要素の目的で配列と同じです。

* 等価の目的でパラメーターを持つ同等の IDataProtectionProvider オブジェクトから作成された場合にのみ、2 つの IDataProtector オブジェクトは等価です。

* 特定の IDataProtector オブジェクト Unprotect(protectedData) への呼び出しを返す元 unprotectedData 場合にのみ、protectedData: IDataProtector オブジェクトと等価の Protect(unprotectedData) を = です。

> [!NOTE]
> いくつかのコンポーネントが意図的に別のコンポーネントと競合する呼ばれる目的の文字列を選択、ケース マイクロソフトいない検討しています。 このようなコンポーネントは基本的と見なされる悪意のあると、このシステムは、悪意のあるコードは、ワーカー プロセス内で既に実行されているセキュリティ保証を提供するものではありません。
