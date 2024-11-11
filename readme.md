### 初回手順
書き換えないといけないファイル
- inventory/inventory.yaml
- vars/seacret.yaml
- vars/vars.yaml
- observe/alloy/auth.alloy

inventory/inventory.yaml
- ansible_host
- ansible_ssh_private_key_file

vars/vars.yaml
- github.owner: リポジトリのユーザー名
- github.repo: リポジトリ名
- github.url: リポジトリのSSHアドレス

vars/seacret.yaml
- seacret.github.repo: fine-grained personal access token (PAT)
  - PATはアクセス権を以下に設定
    - Only select repositries: リポジトリを設定
    - Repository permissions
      - contents: read and write
      - Administration: read and write

observe/alloy/auth.alloy
- token
- prometheus_user
- tempo_user
- loki_user

token
- MyAccounts -> Access Policies -> Create aceess policy でトークン発行ページへ遷移
metrics, logs, tracesのread, writeにチェックしてトークンを発行する

promethus_user
- MyAccounts -> launch -> Prometheus -> send metricsへ遷移
usernameをコピー

tempo_user
- MyAccounts -> launch -> tempo -> send metricsへ遷移
usernameをコピー

loki_user
- MyAccounts -> launch -> loki -> send metricsへ遷移
usernameをコピー

grafna cloud
- observe/grafana/ISUCON-xxx.json を grafana cloud -> dashboards -> new -> importからupoload


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
