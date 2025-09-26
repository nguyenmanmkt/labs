# 🧪 Lab 7: Task Automation trên Windows

### 🎯 Mục tiêu
- Làm quen với Task Scheduler.  
- Tự động chạy một script PowerShell theo lịch.  
- Thực hành tạo job backup thư mục.  

### 🔧 Yêu cầu
- Máy tính Windows 10/11.  
- Quyền Administrator.  
- Có thư mục `C:\LabAutomation\Source` chứa file cần backup.  

---

### 📝 Các bước thực hiện

#### 1. Tạo Script PowerShell Backup
Tạo file `backup.ps1` với nội dung:
```powershell
# Script backup thư mục Source sang D:\Backup
$source = "C:\LabAutomation\Source"
$destination = "D:\Backup"

# Tạo folder Backup nếu chưa có
if (!(Test-Path $destination)) {
    New-Item -ItemType Directory -Path $destination
}

# Copy toàn bộ file từ Source sang Backup
Copy-Item -Path $source\* -Destination $destination -Recurse -Force

Write-Output "Backup completed at $(Get-Date)" | Out-File -Append D:\Backup\backup_log.txt
```

Chạy thử script:
```powershell
.ackup.ps1
```
➡ Kiểm tra trong `D:\Backup` đã có file và log.

---

#### 2. Tạo Task với Task Scheduler
1. Mở **Task Scheduler** (`taskschd.msc`).  
2. **Create Basic Task** → đặt tên: `Daily Backup`.  
3. Chọn **Trigger**: Daily (mỗi ngày 9:00 AM).  
4. **Action**: Start a Program  
   - Program/script: `powershell.exe`  
   - Add arguments:  
     ```powershell
     -ExecutionPolicy Bypass -File "C:\LabAutomation\backup.ps1"
     ```  
5. Hoàn tất và lưu Task.  

---

#### 3. Kiểm tra và Thử nghiệm
- Vào Task Scheduler → chọn Task → **Run** để chạy thử.  
- Mở thư mục `D:\Backup` để kiểm tra file có được copy không.  
- Xem log `backup_log.txt` để xác nhận thời gian chạy.  

---

### ❓ Bài tập
1. Chỉnh sửa script để:  
   - Tạo folder mới theo ngày: `D:\Backup\2025-09-18`.  
   - Copy file vào đó thay vì ghi đè.  

2. Tạo thêm 1 Task khác:  
   - Tự động dọn file rác trong thư mục `C:\Temp` mỗi tuần.  

