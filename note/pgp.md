## PGPのメモ
- http://www.wakayama-u.ac.jp/~takehiko/pgp.html
```
# 公開鍵のエクスポート（ASCII形式）
gpg -a --export test@example.com > gpgkey.pub
# インポート
gpg --import gpgkey.pub
# フィンガープリントの確認
gpg --fingerprint test@example.com
# 暗号化
gpg -o targetfile.encrypted -r hikalium@hikalium.com --encrypt targetfile
# 復号
gpg -o targetfile --decrypt targetfile.encrypted
```

