# 🧪 Lab 3: Quản lý User & Permissions trên Windows

### 🎯 Mục tiêu
- Hiểu cách tạo và quản lý user trong Windows.  
- Phân quyền NTFS cho thư mục (Read, Write, Full Control).  
- Thử nghiệm quyền truy cập giữa các user khác nhau.  

### 🔧 Yêu cầu
- Máy tính Windows 10/11 (phiên bản Pro để có nhiều tùy chọn Local Users & Groups).  
- Có ít nhất **2 user account**: Administrator (mặc định) + 1 user mới tạo.  

---

### 📝 Các bước thực hiện

#### 1. Tạo User mới
**Cách 1: Dùng CMD**
```cmd
net user student /add
net user student 123456
```

**Cách 2: Dùng GUI**
1. Mở **Control Panel → User Accounts → Manage another account**  
2. Chọn **Add a new user** → nhập tên → đặt password  

---

#### 2. Thêm User vào nhóm Administrators
**CMD**
```cmd
net localgroup administrators student /add
```

**GUI**
1. Mở **Computer Management → Local Users and Groups → Users**  
2. Chuột phải `student` → **Properties → Member Of → Add → Administrators**  

---

#### 3. Phân quyền NTFS cho thư mục
1. Tạo thư mục `D:\TestPermissions`  
2. Chuột phải thư mục → **Properties → Security**  
3. Thêm user `student`:  
   - **Read & Execute** → chỉ đọc  
   - **Modify** → cho phép chỉnh sửa  
   - **Full Control** → toàn quyền  

---

#### 4. Kiểm tra phân quyền
1. Đăng nhập bằng user `student`  
2. Mở thư mục `D:\TestPermissions`  
3. Thử:  
   - Tạo file → nếu chỉ có **Read** thì sẽ bị từ chối  
   - Chỉnh sửa file → nếu có **Modify** thì làm được  
   - Xóa file → chỉ khi có **Full Control**  

---

### ❓ Bài tập
1. Tạo thêm 2 user: `guest1` và `guest2`  
2. Phân quyền:  
   - `guest1`: chỉ **Read**  
   - `guest2`: **Modify**  
3. Đăng nhập và kiểm tra kết quả  
4. Viết bảng so sánh quyền của từng user  
