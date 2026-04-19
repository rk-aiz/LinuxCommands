# Linux コマンドリファレンス

> 初級〜中上級者向け。実用的な使用例とオプションをまとめたリファレンス。

---

## 目次

1. [ファイル操作](#ファイル操作)
2. [パーミッション](#パーミッション)
3. [テキスト編集・閲覧](#テキスト編集閲覧)
4. [検索](#検索)
5. [プロセス管理](#プロセス管理)
6. [ディスク・ネットワーク](#ディスクネットワーク)
7. [アーカイブ](#アーカイブ)
8. [システム管理](#システム管理)

---

## ファイル操作

### `ls` — ディレクトリの内容を一覧表示

```bash
ls -la               # 隠しファイルを含む詳細表示
ls -lh /var/log      # サイズを KB/MB 単位で表示
ls -lt               # 更新日時の新しい順にソート
```

| オプション | 意味 |
|-----------|------|
| `-l` | 詳細(long)形式 |
| `-a` | 隠しファイルを含む |
| `-h` | サイズを人間が読める形式 |
| `-t` | 時刻順でソート |
| `-r` | 逆順でソート |

---

### `cd` — ディレクトリの移動

```bash
cd ~       # ホームディレクトリへ
cd ..      # 一つ上(親ディレクトリ)へ
cd -       # 直前のディレクトリに戻る
```

---

### `cp` — ファイル・ディレクトリのコピー

```bash
cp file.txt backup.txt          # ファイルをコピー
cp -r src/ dst/                 # ディレクトリを再帰コピー
cp -i file.txt dest/            # 上書き前に確認
```

| オプション | 意味 |
|-----------|------|
| `-r` | 再帰コピー(ディレクトリ) |
| `-i` | 上書き確認 |
| `-p` | パーミッション/タイムスタンプ保持 |
| `-v` | 進捗を表示 |

---

### `mv` — ファイルの移動・リネーム

```bash
mv old.txt new.txt              # リネーム
mv *.log /var/logs/             # ディレクトリへ移動(グロブ使用)
mv -i file.txt dest/            # 上書き前に確認
```

| オプション | 意味 |
|-----------|------|
| `-i` | 上書き確認 |
| `-n` | 上書きしない |
| `-v` | 詳細表示 |

---

### `rm` — ファイル・ディレクトリの削除

```bash
rm file.txt                     # ファイル削除
rm -rf dir/                     # ディレクトリを再帰削除 ⚠ 確認なし
rm -i *.log                     # 1つずつ確認しながら削除
```

> ⚠ `rm -rf` は取り消しができません。特に `sudo rm -rf /` などは絶対に実行しないこと。

| オプション | 意味 |
|-----------|------|
| `-r` | 再帰削除 |
| `-f` | 強制削除(確認なし) |
| `-i` | 削除前に確認 |
| `-v` | 詳細表示 |

---

### `ln` — リンクを作成

```bash
ln -s /usr/local/bin/python3 /usr/bin/python   # シンボリックリンク
ln file.txt hardlink.txt                        # ハードリンク
```

| オプション | 意味 |
|-----------|------|
| `-s` | シンボリックリンク |
| `-f` | 既存ファイルを上書き |

---

## パーミッション

### `chmod` — パーミッションを変更

```bash
chmod +x script.sh              # 実行権限を付与
chmod 755 script.sh             # 数値指定 (rwxr-xr-x)
chmod -R 644 /var/www/html/     # ディレクトリ以下すべてに適用
```

**数値表現の早見表:**

| 数値 | パーミッション | 意味 |
|------|--------------|------|
| `7` | `rwx` | 読み書き実行 |
| `6` | `rw-` | 読み書き |
| `5` | `r-x` | 読み実行 |
| `4` | `r--` | 読み取りのみ |
| `0` | `---` | なし |

よく使う組み合わせ:
- `755` → スクリプト・ディレクトリ (所有者は全権、他は読み実行)
- `644` → 設定ファイル・HTMLファイル (所有者は読み書き、他は読み取り)
- `600` → 秘密鍵など (所有者のみ読み書き)

| オプション | 意味 |
|-----------|------|
| `-R` | 再帰適用 |
| `u/g/o/a` | user/group/other/all |
| `+/-` | 権限追加/削除 |

---

### `chown` — 所有者・グループを変更

```bash
chown alice file.txt                        # 所有者変更
chown alice:developers file.txt             # 所有者とグループを同時変更
sudo chown -R www-data:www-data /var/www/   # Webサーバーのファイルに多用
```

| オプション | 意味 |
|-----------|------|
| `-R` | 再帰適用 |
| `user:group` | 所有者:グループ の形式 |
| `-v` | 変更内容を表示 |

---

## テキスト編集・閲覧

### `nano` — シンプルなターミナルテキストエディタ

```bash
nano /etc/hosts                         # ファイルを開く
nano newfile.txt                        # 新規作成
sudo nano /etc/nginx/nginx.conf         # root権限で設定ファイルを編集
```

**キーバインド:**

| ショートカット | 動作 |
|--------------|------|
| `Ctrl+O` | 保存 (Write Out) |
| `Ctrl+X` | 終了 |
| `Ctrl+W` | 検索 |
| `Ctrl+K` | 行を切り取り |
| `Ctrl+U` | 貼り付け |
| `Ctrl+G` | ヘルプ |

---

### `cat` — ファイルの内容を表示・結合

```bash
cat /etc/os-release                 # 内容表示
cat -n file.txt                     # 行番号付きで表示
cat file1.txt file2.txt > merged.txt  # ファイルを結合
```

---

### `less` — ファイルをページ送りで閲覧

```bash
less /var/log/syslog                # ファイルを閲覧
dmesg | less                        # 長い出力をページ送りで確認
less +/error /var/log/auth.log      # 特定のキーワードで開始
```

**操作:**

| キー | 動作 |
|------|------|
| `Space` / `b` | 次ページ / 前ページ |
| `/ <word>` | 検索 |
| `n` / `N` | 次 / 前のマッチ |
| `G` | 末尾へ移動 |
| `q` | 終了 |

---

## 検索

### `grep` — テキストパターンを検索

```bash
grep 'error' /var/log/syslog       # ファイルを検索
grep -r 'TODO' ./src/              # ディレクトリを再帰検索
grep -n 'config' settings.conf     # 行番号付きで表示
ps aux | grep nginx                # パイプと組み合わせる
grep -i 'Warning' /var/log/app.log # 大文字小文字を無視
```

| オプション | 意味 |
|-----------|------|
| `-r` | 再帰検索 |
| `-n` | 行番号表示 |
| `-i` | 大文字小文字を無視 |
| `-v` | マッチしない行を表示 |
| `-E` | 拡張正規表現(ERE) |
| `-l` | ファイル名のみ表示 |

---

### `find` — 条件を指定してファイルを検索

```bash
find /home -name '*.txt'                  # 名前で検索
find . -mtime -7                          # 7日以内に変更されたファイル
find / -size +100M -type f                # 100MB以上のファイル
find . -name '*.log' -exec rm {} \;      # 検索結果を削除
find . -type f -not -name '*.git'        # .git 以外のファイル
```

| オプション | 意味 |
|-----------|------|
| `-name` | ファイル名パターン |
| `-type f/d` | ファイル / ディレクトリ |
| `-mtime N` | N日前に変更 |
| `-size` | サイズ条件 |
| `-exec` | 見つかったファイルにコマンド実行 |
| `-not` | 条件の否定 |

---

## プロセス管理

### `ps` — 実行中のプロセスを一覧表示

```bash
ps aux                     # 全プロセスを表示
ps aux | grep python       # 特定プロセスを絞り込む
ps axjf                    # ツリー形式で表示
```

| オプション | 意味 |
|-----------|------|
| `a` | 全ユーザーのプロセス |
| `u` | ユーザー形式で表示 |
| `x` | 端末なしプロセスも含む |
| `f` | フォレスト(ツリー)形式 |

---

### `top` — CPU・メモリ使用状況をリアルタイム表示

```bash
top                        # 起動
top -u alice               # 特定ユーザーのプロセスのみ
htop                       # 拡張版(別途インストール要)
```

**操作キー:**

| キー | 動作 |
|------|------|
| `q` | 終了 |
| `k` | プロセス終了 |
| `M` | メモリ使用量順 |
| `P` | CPU使用率順 |
| `1` | CPUコア個別表示 |

---

### `kill` — プロセスにシグナルを送信

```bash
kill 1234               # 正常終了(SIGTERM)
kill -9 1234            # 強制終了(SIGKILL)
pkill nginx             # プロセス名で終了
killall python3         # 同名プロセスをすべて終了
```

| シグナル | 意味 |
|---------|------|
| `-15` / `SIGTERM` | 正常終了(デフォルト) |
| `-9` / `SIGKILL` | 強制終了 |
| `-1` / `SIGHUP` | 設定再読み込みに使われることも |

---

## ディスク・ネットワーク

### `df` — ディスクの使用量を表示

```bash
df -h               # 人間が読みやすい形式
df -h /var          # 特定ディレクトリ
df -hT              # ファイルシステムタイプも表示
```

---

### `du` — ディレクトリ・ファイルのサイズを計測

```bash
du -sh .                                    # 現在のディレクトリの合計
du -h --max-depth=1 /var                    # 1階層下のサイズ内訳
du -h /home | sort -rh | head -20          # 大きい順に上位20件
```

| オプション | 意味 |
|-----------|------|
| `-s` | 合計のみ |
| `-h` | 人間向け単位 |
| `--max-depth=N` | 深さ制限 |
| `-a` | ファイル単位で表示 |

---

### `ssh` — リモートサーバーへのセキュアな接続

```bash
ssh user@192.168.1.10                       # 基本接続
ssh -p 2222 user@server.example.com        # ポート指定
ssh -i ~/.ssh/mykey.pem user@host          # 秘密鍵を指定
```

| オプション | 意味 |
|-----------|------|
| `-p` | ポート指定 |
| `-i` | 秘密鍵ファイル |
| `-L` | ローカルポートフォワード |
| `-v` | デバッグ出力 |

---

### `scp` — SSH経由でファイルをコピー

```bash
scp file.txt user@host:/tmp/               # ローカル → リモート
scp user@host:/var/log/app.log .           # リモート → ローカル
scp -r ./project user@host:~/             # ディレクトリごとコピー
```

---

### `curl` — HTTPリクエストやファイルダウンロード

```bash
curl https://example.com                                            # GETリクエスト
curl -O https://example.com/file.zip                              # ファイルをダウンロード
curl -X POST \
  -H 'Content-Type: application/json' \
  -d '{"key":"val"}' \
  https://api.example.com/                                         # POSTリクエスト
curl -I https://example.com                                        # ヘッダーのみ取得
```

| オプション | 意味 |
|-----------|------|
| `-O` | 元のファイル名で保存 |
| `-o` | 出力先ファイル指定 |
| `-X` | HTTPメソッド指定 |
| `-H` | ヘッダー追加 |
| `-d` | リクエストボディ |
| `-I` | ヘッダーのみ取得 |
| `-s` | サイレントモード |

---

## アーカイブ

### `tar` — ファイルのアーカイブ(圧縮・展開)

```bash
tar -czvf archive.tar.gz ./dir/     # gz圧縮アーカイブを作成
tar -xzvf archive.tar.gz            # 展開
tar -tzvf archive.tar.gz            # 内容確認(展開しない)
tar -xzvf archive.tar.gz -C /tmp/  # 展開先ディレクトリを指定
```

**オプションの覚え方:** `c`reate / e`x`tract / `t`est + `z`=gzip / `j`=bzip2 + `v`erbose + `f`ile

| オプション | 意味 |
|-----------|------|
| `-c` | アーカイブ作成 |
| `-x` | 展開 |
| `-t` | 内容一覧 |
| `-z` | gzip圧縮 |
| `-j` | bzip2圧縮 |
| `-v` | 詳細表示 |
| `-f` | ファイル名指定 |

---

## システム管理

### `systemctl` — systemd サービスの管理

```bash
systemctl status nginx                      # サービスの状態確認
sudo systemctl start nginx                  # 起動
sudo systemctl stop nginx                   # 停止
sudo systemctl restart nginx               # 再起動
sudo systemctl reload nginx                # 設定再読み込み
sudo systemctl enable nginx                # 自動起動を有効化
sudo systemctl disable nginx               # 自動起動を無効化
systemctl list-units --type=service        # サービス一覧
```

---

### `journalctl` — systemd ログの閲覧

```bash
journalctl -n 50                                            # 直近50行
journalctl -u nginx                                         # サービスのログ
journalctl -f                                               # リアルタイム追跡
journalctl --since '2024-01-01' --until '2024-01-02'       # 時刻範囲指定
journalctl -p err                                           # エラーログのみ
```

| オプション | 意味 |
|-----------|------|
| `-u` | ユニット絞り込み |
| `-n` | 表示行数 |
| `-f` | リアルタイム追跡 |
| `-p` | 優先度フィルタ(err/warning など) |
| `--since/--until` | 時刻範囲指定 |

---

### `crontab` — 定期実行タスクの管理

```bash
crontab -e          # cronを編集
crontab -l          # 一覧を確認
```

**cron式の書式:**

```
分  時  日  月  曜日  コマンド
*   *   *   *   *     実行するコマンド
```

**よく使うパターン:**

```bash
0 2 * * *    /usr/local/bin/backup.sh        # 毎日午前2時
*/5 * * * *  /usr/local/bin/monitor.sh       # 5分ごと
0 0 * * 0    /usr/local/bin/weekly.sh        # 毎週日曜0時
0 9 1 * *    /usr/local/bin/monthly.sh       # 毎月1日午前9時
```

---

*最終更新: 2025年*
