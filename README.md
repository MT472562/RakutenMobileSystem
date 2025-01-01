# Rakuten Mobile 使用量モニタリングツール

このツールは、楽天モバイルの使用量を取得し、現在の利用状況に基づいて適切なアドバイスを提供します。日々の利用可能量を計算し、現在の使用量に基づいて適切な利用を推奨します。また、LINEに通知を送る機能も含まれています。

## 必要環境

- Python 3.12+
- Selenium　headlessモード
- venv
- Google Chrome およびその対応するChromeDriver

## セットアップ

1. `config.json` ファイルを以下の形式で作成します。

```json
{
    "mail": "hogehoge@example.com",
    "password": "password",
    "lineUid": "Uehogehogehoge",
    "lineToken": "hogehogehoge"
}
```

### `config.json` の詳細

- **mail**: 楽天モバイルにログインするためのメールアドレス。
- **password**: 楽天モバイルにログインするためのパスワード。
- **lineUid**: LINE通知を受け取るユーザーID。
- **lineToken**: LINE通知を送信するためのアクセストークン。

## 実行方法

以下のコマンドを実行して、楽天モバイルの使用量を取得します。

```bash
python main.py nomal
```

```bash
python main.py over
```

## 1. `mode == "normal"` の場合

- **残りの使用量**と**今日使える量**に関するメッセージが表示されます。
- 今日までに使用した量 (`usage_amount`) を基に、目標値 (一日あたりの使用量) と比較して、今日使える量を計算します。
- 今日使える量がマイナス（目標を超過している場合）だと、その量と警告メッセージが表示されます。
- 今日使える量が問題なければ、その量が表示されます。

**例:**
今月利用できる残量は 15.0 GB です。 今日使える量は 0.5 GB です。 使用量 : 19.5GB/20GB

## 2. `mode == "over"` の場合

- **目標値を超えているかどうか**だけがチェックされ、超過している場合はその量を表示します。
- 目標値（`target_value`）が現在の使用量（`usage_amount`）を超えている場合、何も表示されません。
- 使用量が目標を超えている場合、超過量を表示します。

**例:**
使用量が目標値を超えています。 0.5 GB 超過しています。
定期実行の設定（crontab）
以下の手順でcrontabに定期実行の設定を追加できます。

crontabの編集を開始します。

bashCopycrontab -e

以下のような設定を追加します。

bashCopy# 毎日午前9時に通常モードで実行
0 9 * * * cd /path/to/your/script && /usr/bin/python3 main.py normal

# 毎日午後6時に超過チェックモードで実行
0 18 * * * cd /path/to/your/script && /usr/bin/python3 main.py over
注意:

/path/to/your/script は実際のスクリプトのディレクトリパスに置き換えてください。
/usr/bin/python3 は実際のPythonインタプリタのパスに置き換えてください。
cronの実行環境によってはPythonの依存パッケージのパスが通っていない場合があります。その場合は、仮想環境のPythonを指定するか、フルパスで指定してください。


## 注意事項

- `config.json` の情報は第三者に漏れないよう注意してください。
- 初回実行時にGoogle Chromeが必要です。また、対応するバージョンのChromeDriverをシステムに設定してください。
- LINE通知機能を有効にするには、LINE Developersコンソールで適切にアクセストークンを取得してください。

## ライセンス

このツールはMITライセンスのもとで提供されます。
