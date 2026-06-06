# create-ark-genesis.sh

`ark-island.service` をベースに、Genesis マップ用の systemd サービス (`ark-Genesis.service`) を新規作成するスクリプトです。

---

## 概要

- `ark-island.service` の設定（セッション名・パスワード・プラットフォーム・MOD）をそのまま引き継ぎます
- マップIDだけ `Genesis_WP` に差し替えた `ark-Genesis.service` を自動生成します
- 自動起動（enable）は行いません。必要に応じて手動で設定してください

---

## 前提条件

- `ark-island.service` が `/etc/systemd/system/` に存在すること
- root 権限で実行すること

---

## 使い方

```bash
# 実行権限を付与（初回のみ）
chmod +x create-ark-genesis.sh

# 実行
sudo ./create-ark-genesis.sh
```

実行すると `ark-island.service` から以下の設定を自動で読み取り、確認画面を表示します。

| 項目 | 内容 |
|------|------|
| セッション名 | island.service から引き継ぎ |
| パスワード | island.service から引き継ぎ |
| プラットフォーム | island.service から引き継ぎ（未設定時は `PC`） |
| MOD | island.service から引き継ぎ |
| マップID | `Genesis_WP`（固定） |

`ark-Genesis.service` がすでに存在する場合は上書き確認が入ります。

---

## サービス作成後の操作

**1. 起動済みのサービスを停止する（例）**
```bash
sudo systemctl stop ark-island.service
```

**2. Genesis サーバーを起動する**
```bash
sudo systemctl start ark-Genesis.service
```

**起動ログの確認**
```bash
tail -F /home/steam/island-ShooterGame.log
```

**自動起動を有効にする場合（任意）**
```bash
sudo systemctl enable ark-Genesis.service
```

---

## 生成されるサービスファイル

`/etc/systemd/system/ark-Genesis.service`

`ark-island.service` をコピーし、以下の箇所のみ書き換えます。

- `Description` → `ARK Ascended - Genesis Server`
- `ExecStart` のマップID → `Genesis_WP`
