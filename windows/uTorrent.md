# Windows Final Lab — Tổng hợp

## Môi trường & yêu cầu
- VM Windows 11 trên Hyper-V (4 vCPU, 8GB RAM, 70GB disk).  
- IP tĩnh trong dải lớp, DNS: `1.1.1.1`.  
- Cài đặt phần mềm: **uTorrent**.  

---

## 1) Phát hiện tiến trình
- Ghi lại: **tên tiến trình, PID, %CPU, %Memory** tại thời điểm phát hiện **uTorrent** chạy.  

---

## 2) Dừng tiến trình (tạm thời)
- Dừng tiến trình **uTorrent**.  
- Ghi trạng thái sau khi dừng (có còn chạy lại hay không trong **2 phút quan sát**).  

---

## 3) Xác định cơ chế tự khởi động
- Kiểm tra xem tiến trình được khởi động lại bằng: **Scheduled Task, Service, hay Registry Run key**.  
- Ghi rõ cơ chế bạn tìm thấy.  

---

## 4) Ngăn chặn tiến trình khởi động lại
- Thực hiện hành động để ngăn tiến trình chạy lại:  
  - Disable Task / Stop + Disable Service / Remove Run Key.  
- Ghi rõ hành động đã thực hiện.  

---

## 5) Event Viewer / Log
- Nêu ít nhất **2 event/log** liên quan đến hành vi của tiến trình.  
- Ghi: **Event ID, Source, thời gian, tóm tắt message**.  

---

## 6) Task Automation (script) — bắt buộc
- Viết 1 script **auto_stop.ps1** thực hiện:  
  - Dừng tiến trình **uTorrent**.  

---

### 🎯 Đầu ra nộp
- Báo cáo ngắn (1–2 trang) kèm kết quả.  
- File `auto_stop.ps1`.  
