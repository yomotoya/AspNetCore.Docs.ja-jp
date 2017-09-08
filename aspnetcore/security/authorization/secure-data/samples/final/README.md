# <a name="how-to-buildrun-secure-user-data-sample"></a>ユーザーのセキュリティで保護されたデータ サンプルをビルド/実行する方法

* シークレット マネージャー ツールを使用してパスワードを設定します。

  `dotnet user-secrets set SeedUserPW <pw>`

* データベースを更新します。

    `dotnet ef database update`
