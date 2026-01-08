# クイックリファレンス

## 接続方法

### MBP / iPhoneからサーバーにSSH
```bash
ssh youruser@home-server
```
※ Tailscaleが起動していれば、どこからでも繋がる

### サーバーでClaude Codeを使う
```bash
ssh youruser@home-server
cd ~/development/（プロジェクト名）

claude            # 新規セッション開始
claude --continue # 直前のセッションを再開
claude --resume   # 過去のセッション一覧から選んで再開
```

### ワンライナーで直接セッション再開
```bash
ssh -t youruser@home-server "claude --continue"
```
※ Blink Shellの履歴に入れておくと便利

## Blink Shell（iPhone）

| 操作 | ジェスチャー |
|------|-------------|
| 新しいタブ | 2本指でタップ |
| タブを閉じる | 2本指で上にスワイプ |
| タブ切り替え | 左右にスワイプ |

## 各ツールの状態確認・操作

### Tailscale

| 操作 | コマンド |
|------|----------|
| 状態確認 | `tailscale status` |
| 接続 | `sudo tailscale up` |
| 切断 | `sudo tailscale down` |

iPhone: Tailscaleアプリを起動してトグルON

### Syncthing

#### サーバー側
| 操作 | コマンド |
|------|----------|
| 状態確認 | `systemctl status syncthing@youruser` |
| 再起動 | `sudo systemctl restart syncthing@youruser` |
| ログ確認 | `journalctl -u syncthing@youruser -f` |

#### MBP側
| 操作 | コマンド |
|------|----------|
| 状態確認 | `brew services list` |
| 再起動 | `brew services restart syncthing` |
| WebUI | `open http://localhost:8384` |

### SSH

#### MBP側の設定（~/.ssh/config）
```
Host home-server
  HostName 100.x.x.x
  User youruser
```
※ HostNameは `tailscale status` で確認したサーバーのIPアドレス

初回接続時にSSH鍵をエージェントに登録：
```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

#### サーバー側
| 操作 | コマンド |
|------|----------|
| 状態確認 | `systemctl status ssh` |
| 再起動 | `sudo systemctl restart ssh` |

## Tailscale IPアドレス

| デバイス | IP |
|----------|-----|
| home-server | 100.x.x.x |
| MBP | 100.x.x.x |
| iPhone | 100.x.x.x |

※ `tailscale status` で確認

## よく使うパス

| 場所 | パス |
|------|------|
| 同期フォルダ（サーバー） | `~/development` |
| 同期フォルダ（MBP） | `~/development` |
| Syncthing設定（サーバー） | `~/.config/syncthing` |

## トラブル時

### 同期が止まった
```bash
# サーバー
sudo systemctl restart syncthing@youruser

# MBP
brew services restart syncthing
```

### SSHが繋がらない
```bash
# Tailscale確認
tailscale status

# IPで直接試す
ssh youruser@100.x.x.x
```

### サーバー再起動
```bash
sudo reboot
```
※ 再起動後、Tailscale/Syncthing/SSHは自動起動する
