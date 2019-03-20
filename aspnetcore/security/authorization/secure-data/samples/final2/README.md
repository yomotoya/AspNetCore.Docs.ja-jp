---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210107"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a>セキュリティで保護されたユーザー データのサンプルをビルド/実行する方法

* Secret Manager ツールを使用したパスワードを設定します。

  `dotnet user-secrets set SeedUserPW <pw>`

* データベースを更新します。

  `dotnet ef database update`

* プロジェクトで HTTPS を有効にします。
