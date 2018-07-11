# <a name="how-to-buildrun-secure-user-data-sample"></a>セキュリティで保護されたユーザー データのサンプルをビルド/実行する方法

* Secret Manager ツールを使用したパスワードを設定します。

  `dotnet user-secrets set SeedUserPW <pw>`

* データベースを更新します。

    `dotnet ef database update`

* プロジェクトで SSL を有効にします。
