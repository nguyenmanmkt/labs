# 🧑‍💻 Lab 0x02 — User & Permission Management (Kali Linux)

## 🎯 Mục tiêu
- Quản lý tài khoản người dùng (`useradd`, `passwd`, `deluser`, `su`).
- Quản lý nhóm (`groupadd`, `usermod`, `groups`).
- Hiểu quyền file (rwx, UID/GID, sticky bit).
- Thực hành `chmod`, `chown`, `chgrp`, `umask`.
- Sử dụng `sudo` và `/etc/sudoers`.
- Làm quen **ACL** (`getfacl`, `setfacl`).
- Tạo kịch bản kiểm soát quyền file thực tế.

> ⚙️ Môi trường: Kali Linux, quyền `sudo`, không cần internet.

---

## 0️⃣ Chuẩn bị
```bash
mkdir -p ~/lab02/{data,scripts,logs}
cd ~/lab02
echo "secret info" > data/flag.txt
```

Kiểm tra quyền ban đầu:
```bash
ls -l data/
# -rw-r--r--  kali kali 12 flag.txt
```

---

## 1️⃣ Tạo & quản lý user
**Lệnh học:** `useradd`, `passwd`, `id`, `whoami`, `su`, `deluser`

Bài tập:
1. Tạo user `alice` và `bob`.
2. Đặt mật khẩu cho họ (tạm thời là `123`).
3. Đăng nhập thử sang user `alice`.
4. Kiểm tra `id` và thư mục home.

Gợi ý:
```bash
sudo useradd -m alice
sudo useradd -m bob
echo "123" | sudo passwd --stdin alice 2>/dev/null || echo "123" | sudo passwd alice
sudo su - alice
whoami && pwd && id
exit
```

---

## 2️⃣ Tạo nhóm & gán user vào nhóm
**Lệnh học:** `groupadd`, `usermod -aG`, `groups`

Bài tập:
1. Tạo nhóm `projectx`.
2. Thêm `alice` và `bob` vào nhóm này.
3. Kiểm tra họ có trong nhóm không.

Gợi ý:
```bash
sudo groupadd projectx
sudo usermod -aG projectx alice
sudo usermod -aG projectx bob
groups alice
groups bob
```

---

## 3️⃣ Quyền truy cập file cơ bản (rwx)
**Lệnh học:** `chmod`, `ls -l`, `stat`

Bài tập:
1. Cho phép **nhóm projectx** đọc file `flag.txt`.
2. Cấm người khác (`others`) truy cập.
3. Xác nhận bằng `ls -l`.

Gợi ý:
```bash
sudo chown kali:projectx data/flag.txt
sudo chmod 640 data/flag.txt
ls -l data/flag.txt
```

> Giải thích: `640` = rw- (owner), r-- (group), --- (others).

---

## 4️⃣ Thực hành phân quyền sâu hơn
**Lệnh học:** `chmod +x`, `umask`, `setuid`, `sticky bit`

Bài tập:
1. Tạo script `scripts/test.sh` in `whoami`.
2. Cho phép mọi người chạy.
3. Thay đổi umask → tạo file mới thử.

Gợi ý:
```bash
echo '#!/bin/bash' > scripts/test.sh
echo 'whoami' >> scripts/test.sh
chmod +x scripts/test.sh
ls -l scripts/test.sh

umask 077
touch data/private.txt
ls -l data/private.txt
```

---

## 5️⃣ Quyền sudo
**Lệnh học:** `visudo`, `/etc/sudoers.d/`

Bài tập:
1. Cho phép user `alice` chạy lệnh `apt update` mà không cần mật khẩu.
2. Kiểm tra bằng `sudo -l`.

Gợi ý:
```bash
echo "alice ALL=(ALL) NOPASSWD: /usr/bin/apt update" | sudo tee /etc/sudoers.d/alice
sudo su - alice
sudo -l
sudo apt update
exit
```

---

## 6️⃣ ACL (Access Control List)
**Lệnh học:** `getfacl`, `setfacl`, `setfacl -x`

Bài tập:
1. `flag.txt` chỉ nhóm `projectx` có quyền đọc.
2. Gán quyền đặc biệt cho `bob` được **ghi** (`rw`).
3. Xem ACL hiện tại.

Gợi ý:
```bash
sudo setfacl -m u:bob:rw data/flag.txt
getfacl data/flag.txt
```

> Bạn sẽ thấy thêm dòng:
> ```
> user:bob:rw-
> ```

Xoá quyền:
```bash
sudo setfacl -x u:bob data/flag.txt
```

---

## 7️⃣ Sticky Bit (cho thư mục chia sẻ)
**Lệnh học:** `chmod +t`

Bài tập:
1. Tạo thư mục `shared/` cho nhóm `projectx`.
2. Bật sticky bit để user không xoá file của người khác.

Gợi ý:
```bash
sudo mkdir -p /tmp/shared
sudo chown root:projectx /tmp/shared
sudo chmod 1770 /tmp/shared
ls -ld /tmp/shared
```

> `drwxrwx--T` → sticky bit bật.

---

## 8️⃣ Kiểm tra kết quả tự động (Quick Test)
```bash
# 1) Nhóm projectx tồn tại
getent group projectx && echo "OK: group" || echo "FAIL: group"

# 2) alice thuộc nhóm
groups alice | grep -q projectx && echo "OK: alice group" || echo "FAIL: alice group"

# 3) file flag.txt đúng quyền
stat -c "%a %G" ~/lab02/data/flag.txt | grep -q "640 projectx" && echo "OK: chmod" || echo "FAIL: chmod"

# 4) ACL cho bob tồn tại
getfacl ~/lab02/data/flag.txt | grep -q "user:bob" && echo "OK: acl" || echo "FAIL: acl"

# 5) Sticky bit
stat -c "%A" /tmp/shared | grep -q "T" && echo "OK: sticky" || echo "FAIL: sticky"
```

---

## 🧹 Dọn dẹp
```bash
sudo deluser alice --remove-home
sudo deluser bob --remove-home
sudo groupdel projectx
sudo rm -rf ~/lab02 /tmp/shared /etc/sudoers.d/alice
```

---

## 💡 Gợi ý mở rộng (cho Lab 0x03)
- PAM module & chính sách mật khẩu (`/etc/pam.d/`, `chage`).
- `login.defs`, `ulimit`, `limits.conf`.
- `sudo` nâng cao: alias, include.
- Audit quyền & user activity (`last`, `lastlog`, `history`).
- Rootkit check (rkhunter, chkrootkit).
