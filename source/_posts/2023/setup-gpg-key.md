---
title: 构建 GPG Key 体系
comments: true
categories: [笔记]
tags: [GPG, Mac]
mathjax: false
cc_license: true
date: 2023-02-24 14:47:58
updated: 2023-02-27 15:30:00
---

构建 GPG Key 签名、加密、认证体系。

<!--more-->

本文主要参考资料：[^tutorial][^zhihu][^docs]

## 安装 GPG

使用 brew 安装 GPG：

```sh
❯ brew install gpg

❯ gpg --version | head -n 1
gpg (GnuPG) 2.4.0
```

## 生成 GPG Keys

### Master Key

Master Key 是代表身份的 Root of Trust，只用于 Certify，永不过期：

```sh
❯ gpg --quick-generate-key "Name <test@test.com>" ed25519 cert never
```

完成后会显示生成的 Key 信息：

```
pub   ed25519 2023-02-24 [C]
      B5F1B8552CC0C2E78C46018313EBCB9202A2197C
uid                      Name <test@test.com>
```

中间的字符串就是这个 Key 的 Fingerprint 或者 ID，需要临时保存备用：

```sh
export KEYID=B5F1B8552CC0C2E78C46018313EBCB9202A2197C
```

此时可以继续添加其他的身份信息：

```sh
❯ gpg --quick-add-uid $KEYID "Name <test2@test.com>"
❯ gpg --quick-set-primary-uid $KEYID "Name <test@test.com>"
```

### Sub Keys

在 Master Key 下面继续创建 3 个 Sub Keys，分别用于 Sign、Encrypt、Authenticate，有效期为一年。

```sh
❯ gpg --quick-add-key $KEYID ed25519 sign 1y
❯ gpg --quick-add-key $KEYID cv25519 encr 1y
❯ gpg --quick-add-key $KEYID ed25519 auth 1y
```

## 防止 GPG 私钥丢失

生成所有的 Key 以后，需要防止私钥丢失，例如文件系统损坏。

### 备份整个 GnuPG 目录

为了方便起见，可以将整个 GnuPG 目录一起备份到外部存储，在需要使用的时候一起挂载。

1. 使用如下命令进行第一次备份：

   ```sh
   ❯ cp -rp ~/.gnupg /path/to/your/external/backup/gnupg-backup
   ```

2. 在需要的时候，挂载备份并使用：

   ```sh
   ❯ export GNUPGHOME=/path/to/your/external/backup/gnupg-backup
   ❯ gpg --list-secret-keys
   ```

3. 在备份上完成改动后，将改动写回本地目录：

   ```sh
   ❯ gpg --export | gpg --homedir ~/.gnupg --import
   ❯ unset GNUPGHOME
   ```

### 备份 Master Key

如果不愿意备份整个目录，也可以单独备份私钥，在丢失的时候手动导入。

使用如下命令导出公钥、私钥和吊销凭证：

```sh
❯ gpg -ao /external/backup/master-public.asc --export $KEYID
❯ gpg -ao /external/backup/master-secret.asc --export-secret-keys $KEYID
❯ gpg -ao /external/backup/master-revoke.asc --gen-revoke $KEYID
```

使用如下命令导入公钥、私钥：

```sh
❯ gpg --import /external/backup/master-public.asc
❯ gpg --import /external/backup/master-secret.asc
```

因为 Master Key 十分重要，可以保存一份物理备份：

```sh
❯ brew install paperkey
❯ gpg --export-secret-keys $KEYID | paperkey -o ~/Downloads/master-secret.txt
```

上面的命令会将私钥保存为方便 OCR 或手动输入的格式，打印出来保存即可。

### 备份 Sub Keys

首先查询 Sub Key 的 Fingerprint：

```sh
❯ gpg --list-keys --with-subkey-fingerprints
❯ export SUBKEY_ID=...
```

使用如下命令导出指定 Key 的公钥、私钥：

```sh
❯ gpg -ao subkey-public.asc --export $SUBKEY_ID
❯ gpg -ao subkey-secret.asc --export-secret-subkeys $SUBKEY_ID
```

此时就可以将 Sub Key 分发到其他机器使用，导入命令同上。

当然，最安全的做法是将密钥存放在智能卡上使用，例如下面提到的 YubiKey。

## 防止 GPG 私钥被盗

安全备份后，需要防止本地保存的私钥泄漏。

### 删除 Master Key 私钥

除非特殊情况[^subkeys]，一般不需要使用 Master Key 的私钥和吊销证书，因此不应该保存在本地。

首先需要找到 Master Key 对应的 keygrip：

```sh
❯ gpg --with-keygrip --list-key $KEYID
pub   ed25519 2023-02-24 [C]
      B5F1B8552CC0C2E78C46018313EBCB9202A2197C
      Keygrip = 58DE5F5E3722DC2DC0D80D073EC3D8CCC548A2D8
uid           [ultimate] Name <test@test.com>
sub   ed25519 2023-02-24 [S] [expires: 2024-02-24]
      Keygrip = C61611FBF43F47F1F3DD2787F1B803F65C2803BA
sub   cv25519 2023-02-24 [E] [expires: 2024-02-24]
      Keygrip = 30FF2CA83C1DCF68A04438B6987A85BE45D6D6FA
sub   ed25519 2023-02-24 [A] [expires: 2024-02-24]
      Keygrip = D56EA0C7623C2EDC93285C2C4B452826C3BB3F03
```

删除 pub 下面第一个 keygrip 对应的私钥文件：

```sh
❯ export KEYGRIP=58DE5F5E3722DC2DC0D80D073EC3D8CCC548A2D8
❯ rm ~/.gnupg/private-keys-v1.d/$KEYGRIP.key
```

此时列出私钥，Master Key 左侧显示 `sec#` 即表示删除成功：

```sh
❯ gpg --list-secret-keys $KEYID
sec#  ed25519 2023-02-24 [C]
      B5F1B8552CC0C2E78C46018313EBCB9202A2197C
uid           [ultimate] Name <test@test.com>
ssb   ed25519 2023-02-24 [S] [expires: 2024-02-24]
ssb   cv25519 2023-02-24 [E] [expires: 2024-02-24]
ssb   ed25519 2023-02-24 [A] [expires: 2024-02-24]
```

创建 Master Key 的时候会自动附带吊销证书，同样需要删除：

```sh
❯ rm ~/.gnupg/openpgp-revocs.d/$KEYID.rev
```

### 保护 Sub Keys 私钥

对于需要经常使用的 Sub Keys，他们的私钥应该存放在其他物理设备，例如 YubiKey 上面[^yubikey]。

首先进入编辑界面：

```sh
❯ gpg --edit-key $KEYID
```

因为有三个 Sub Key，需要依次进行以下步骤：

1. 选择某一个 Key：`key 1`
2. 把它的私钥转移到 YubiKey：`keytocard`
3. 根据 Key 的类型选择 Signature / Encryption / Authentication Key.

此时使用以下命令，会发现本地的私钥变成了指向其他地方的 `shadowed-private-key`：

```sh
❯ strings ~/.gnupg/private-keys-v1.d/*.key
```

## 常见使用场景

### 签名 Git Commit

首先找到用于签名的 Sub Key：

```sh
❯ gpg --list-keys --with-subkey-fingerprints
❯ export SIGN_KEY_ID=...
```

而后将这个 ID 记入本地的 Git 设置：

```sh
❯ git config --global user.signingkey $SIGN_KEY_ID
```

最后在 GitHub 的设置中添加 GPG 公钥：

```sh
❯ gpg --export --armor $KEYID
```

如果遇到特殊情况需要吊销之前用过的 Key，直接在 GitHub 中删除公钥会导致历史 Commit 显示为 Unverified。为了避免这个问题，需要在删除后，重新上传被吊销的同一个公钥：

```sh
❯ gpg --output revoke.asc --gen-revoke $KEYID
❯ gpg --import revoke.asc
❯ gpg --export --armor $KEYID
```

之后，历史 Commit 就会显示 Verified，只有点进去会告知 Revoke。



[^tutorial]: https://www.linux.com/news/protecting-code-integrity-pgp-part-1-basic-pgp-concepts-and-tools/
[^zhihu]: https://zhuanlan.zhihu.com/p/481900853
[^docs]: https://www.gnupg.org/documentation/manuals/gnupg/OpenPGP-Key-Management.html
[^subkeys]: https://wiki.debian.org/Subkeys
[^yubikey]: https://leanhe.dev/posts/2021.08.04.1/

