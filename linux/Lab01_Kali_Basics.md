# 🐉 Lab 0x01 — Kali Linux Basics (CLI)

## 🎯 Mục tiêu
- Di chuyển trong hệ thống file (pwd, ls, cd).
- Tạo/chỉnh sửa/xoá file & thư mục (touch, nano/vim, rm, mkdir).
- Quyền & sở hữu (chmod, chown, chgrp).
- Tìm kiếm & lọc (find, grep, cut, sort, uniq).
- Tiến trình & dịch vụ (ps, top, systemctl, journalctl).
- Mạng cơ bản (ip, ip route, ping, curl, nc).
- Gói phần mềm (apt).
- SSH & SCP.
- Nén/giải nén (tar, gzip).
- Viết shell script nhỏ (bash).

> Yêu cầu: Kali (VM hoặc bare metal), có quyền sudo.

---

## 0) Chuẩn bị môi trường (5’)
```bash
# Tạo sân tập tại ~/lab01
mkdir -p ~/lab01/{bin,logs,tmp,data}
cd ~/lab01
echo "hello kali" > data/hello.txt
```

Kiểm tra:
```bash
pwd && ls -la
```

---

## 1) Điều hướng & liệt kê (10’)
**Lệnh học:** `pwd`, `ls`, `cd`, `tree` (cài nếu thiếu)

Bài tập:
1. Di chuyển vào `~/lab01/data`, in đường dẫn hiện tại.
2. Liệt kê tất cả file ẩn theo dạng chi tiết (long listing).
3. Cài `tree` và hiển thị cây thư mục `~/lab01`.

Gợi ý:
```bash
cd ~/lab01/data
pwd
ls -la
sudo apt update && sudo apt install -y tree
tree ~/lab01
```

---

## 2) Tạo/chỉnh sửa/xoá (10’)
**Lệnh học:** `touch`, `cat`, `nano`/`vim`, `rm`, `mkdir`, `cp`, `mv`

Bài tập:
1. Tạo file `notes.txt` với 3 dòng bất kỳ.
2. Sao chép `hello.txt` thành `hello.bak`.
3. Di chuyển `notes.txt` sang thư mục `logs/` và đổi tên thành `notes-1.txt`.
4. Xoá file `hello.bak`.

Gợi ý:
```bash
cd ~/lab01
printf "line1\nline2\nline3\n" > notes.txt
cp data/hello.txt data/hello.bak
mv notes.txt logs/notes-1.txt
rm data/hello.bak
```

---

## 3) Quyền & sở hữu (10’)
**Lệnh học:** `chmod`, `umask`, `chown`, `chgrp`, `stat`

Bài tập:
1. Tạo script `bin/sayhi.sh` in ra “Hi, Kali!”.
2. Cấp quyền thực thi cho chủ sở hữu.
3. Xem chi tiết quyền bằng `stat`.

Gợi ý:
```bash
cat > ~/lab01/bin/sayhi.sh << 'EOF'
#!/usr/bin/env bash
echo "Hi, Kali!"
EOF

chmod u+x ~/lab01/bin/sayhi.sh
stat ~/lab01/bin/sayhi.sh
~/lab01/bin/sayhi.sh
```

---

## 4) Tìm kiếm & lọc dữ liệu (15’)
**Lệnh học:** `find`, `grep`, `wc`, `head`, `tail`, `cut`, `awk`, `sort`, `uniq`

Chuẩn bị log giả:
```bash
for i in {1..100}; do echo "INFO $(date +%T) item=$i" >> logs/app.log; done
for i in {1..10};  do echo "ERROR $(date +%T) code=500 item=$i" >> logs/app.log; done
```

Bài tập:
1. Đếm số dòng chứa `ERROR`.
2. Lấy 5 dòng cuối cùng của log.
3. Trích cột trạng thái (INFO/ERROR) và thống kê tần suất.

Gợi ý:
```bash
grep -c "ERROR" logs/app.log
tail -n 5 logs/app.log
cut -d' ' -f1 logs/app.log | sort | uniq -c
# hoặc:
awk '{print $1}' logs/app.log | sort | uniq -c
```

---

## 5) Quy trình & dịch vụ (10’)
**Lệnh học:** `ps aux`, `pgrep`, `top`/`htop`, `systemctl`, `journalctl`

Bài tập:
1. Liệt kê tiến trình chứa chuỗi “ssh”.
2. Kiểm tra trạng thái dịch vụ `ssh`.
3. Xem 20 dòng log gần nhất của `ssh`.

Gợi ý:
```bash
ps aux | grep ssh | grep -v grep
systemctl status ssh --no-pager
journalctl -u ssh -n 20 --no-pager
```

> Nếu `ssh` chưa có: `sudo apt install -y openssh-server && sudo systemctl enable --now ssh`

---

## 6) Mạng cơ bản (15’)
**Lệnh học:** `ip a`, `ip route`, `ping`, `dig`/`nslookup`, `curl`, `nc`

Bài tập:
1. Xem IP và default gateway.
2. Ping `8.8.8.8` và `google.com` (so sánh DNS vs ICMP).
3. Thử mở TCP port 8080 “listen” cục bộ và gửi dữ liệu test.

Gợi ý:
```bash
ip a
ip route
ping -c 3 8.8.8.8
ping -c 3 google.com
# Netcat test:
( while true; do echo "hello from server"; sleep 2; done ) | nc -l -p 8080 &
nc 127.0.0.1 8080
pkill -f "nc -l -p 8080"  # dọn dẹp
```

---

## 7) Quản lý gói (apt) (5’)
**Lệnh học:** `apt update`, `apt install`, `apt remove`, `apt search`

Bài tập:
- Tìm và cài `jq`, sau đó dùng `jq` định dạng JSON.

Gợi ý:
```bash
sudo apt update
apt search jq | head
sudo apt install -y jq
echo '{"k":"v","n":1}' | jq .
```

---

## 8) Nén/giải nén (5’)
**Lệnh học:** `tar`, `gzip`, `gunzip`

Bài tập:
1. Nén toàn bộ `~/lab01` thành `lab01.tar.gz`.
2. Giải nén ra thư mục `~/lab01_extract/`.

Gợi ý:
```bash
cd ~
tar -czf lab01.tar.gz lab01
mkdir -p lab01_extract && tar -xzf lab01.tar.gz -C lab01_extract
```

---

## 9) SSH & SCP (10’)
**Lệnh học:** `ssh`, `scp`

Bài tập (mô phỏng, nếu có server đích):
- Kết nối `ssh user@host`.
- Sao chép file `data/hello.txt` sang server đích: `/tmp/hello.txt`.

Gợi ý:
```bash
# thay user@host cho phù hợp
ssh user@host
scp ~/lab01/data/hello.txt user@host:/tmp/hello.txt
```

---

## 10) Shell scripting mini (15’)
**Lệnh học:** `bash`, biến, vòng lặp, điều kiện, `$#`, `$1`, `exit`

Bài tập:
- Viết script `bin/count_error.sh`:
  - Nhận tham số đường dẫn log.
  - In số dòng chứa `ERROR`. Nếu file không tồn tại → exit 1.

Gợi ý:
```bash
cat > ~/lab01/bin/count_error.sh << 'EOF'
#!/usr/bin/env bash
LOG="$1"
if [[ ! -f "$LOG" ]]; then
  echo "File not found: $LOG" >&2
  exit 1
fi
COUNT=$(grep -c "ERROR" "$LOG")
echo "ERROR count: $COUNT"
EOF

chmod +x ~/lab01/bin/count_error.sh
~/lab01/bin/count_error.sh ~/lab01/logs/app.log
```

---

## 11) Bonus: one-liners hữu ích (5’)
```bash
# Tìm file > 10MB trong /var (cần sudo):
sudo find /var -type f -size +10M -printf "%p %k KB\n" 2>/dev/null | sort -nr -k2 | head

# Kiểm tra cổng đang nghe:
sudo ss -tulpen

# Xem 10 tiến trình ngốn RAM nhất:
ps aux --sort=-%mem | head

# Lọc IP từ log:
grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' logs/app.log | sort -u
```

---

## ✅ Bài kiểm tra nhanh (tự chấm)
Chạy từng lệnh, nếu in **OK** là đạt.

```bash
# 1) Thư mục & file
test -d ~/lab01/bin && test -d ~/lab01/logs && echo "OK: dirs" || echo "FAIL: dirs"
test -x ~/lab01/bin/sayhi.sh && echo "OK: sayhi" || echo "FAIL: sayhi"

# 2) Log & grep
ERRS=$(grep -c ERROR ~/lab01/logs/app.log); [[ $ERRS -ge 10 ]] && echo "OK: grep" || echo "FAIL: grep"

# 3) Tar
test -f ~/lab01.tar.gz && echo "OK: tar" || echo "FAIL: tar"

# 4) Script
out=$(~/lab01/bin/count_error.sh ~/lab01/logs/app.log 2>/dev/null | awk '{print $3}')
[[ "$out" -ge 10 ]] && echo "OK: script" || echo "FAIL: script"
```

---

## 🧹 Dọn dẹp (tuỳ chọn)
```bash
rm -rf ~/lab01 ~/lab01_extract ~/lab01.tar.gz
```

---

## Gợi ý mở rộng (cho buổi sau)
- `sed`/`awk` nâng cao, `xargs`.
- `cron` & `at`.
- `systemd` unit tuỳ chỉnh (service/timer).
- `tcpdump`, `nmap`, `wireshark-cli` (tshark) trong mạng.
- `gpg` ký/giải mã cơ bản.
