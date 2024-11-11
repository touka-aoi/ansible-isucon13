### 初回手順
inventory/inventory.yaml内で設定してください
- ansible_host
- ansible_ssh_private_key_file

vars/vars.yaml内で設定してください
- github.owner: リポジトリのユーザー名
- github.repo: リポジトリ名

vars/seacret.yaml内で設定してください
- seacret.github.repo: fine-grained personal access token (PAT)
  - PATはアクセス権を以下に設定
    - Only select repositries: リポジトリを設定
    - Repository permissions
      - contents: read and write
      - Administration: read and write

下記を実行し環境構築を実行します

```bash
ansible-playbook -i ./inventory/inventory.yaml init.yaml
```

環境構築時には各種ツールが導入, 設定されます
- nginx
  - Jsonでログを出力する
- mysql
  - スロークエリログを出力する
- alloy
  - 監視情報を受け取るコレクターを作成
  - 問題: 使用していない時は停止しないとmytopで収集したプロセスデータが送られ続ける
- mytop
  - top情報をOpenTelemetoryで出力
  - repo: https://github.com/touka-aoi/mytop

### デプロイ
```
ansible-playbook -i ./inventory/inventory.yaml deploy.yaml
```

### ベンチ起動
```
ansible-playbook -i ./inventory/inventory.yaml run-bench.yaml
```

### まだ考えていないこと
- nginx, mysql関連の設定アップデート
- mysqlのテーブルアップデート
- mytopのserver別集計


### 参考文献
ansibleの作成: 
https://github.com/reactive-futakotamagawa/isu-isu-h-13/tree/main
