# 🧪 Lab 2: Command Line (CMD & PowerShell) cơ bản

### 🎯 Mục tiêu
- Sử dụng CMD & PowerShell để thao tác file, mạng.  
- Viết script PowerShell đơn giản.  

### 🔧 Yêu cầu
- Máy tính Windows 10/11.  

### 📝 Các bước thực hiện

#### 1. CMD cơ bản
```cmd
dir                :: liệt kê file/thư mục
cd Documents       :: chuyển vào thư mục Documents
cd ..              :: quay lại thư mục trước
copy test.txt D:\ :: copy file sang ổ D
ipconfig           :: xem IP
ping google.com    :: kiểm tra kết nối mạng
```

#### 2. PowerShell cơ bản
```powershell
Get-Process                 # xem danh sách process
Get-Service                 # xem dịch vụ hệ thống
Get-Help Get-Process        # xem trợ giúp
```

#### 3. Viết Script PowerShell
```powershell
# Tạo 10 thư mục test
for ($i=1; $i -le 10; $i++) {
    New-Item -Path "C:\LabTest\Folder$i" -ItemType Directory
}
```

Chạy script:
```powershell
Set-ExecutionPolicy RemoteSigned -Scope Process
.\create_folders.ps1
```

### ❓ Bài tập
- Dùng CMD để tạo file `hello.txt` trong Documents.  
- Dùng PowerShell để liệt kê tất cả process có CPU > 10%.  
- Viết script tạo 5 file `.txt` trong mỗi folder vừa tạo.  
