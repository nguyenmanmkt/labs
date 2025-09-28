# Lab Mạng & Firewall — RDP / WinRM / SMB 
## Môi trường & yêu cầu
- VM1 Windows 11 trên Hyper-V (6 vCPU, 8GB RAM, 60GB disk). (VM lưu ở ổ D\HyperV)
- VM2 Windows 11 trên Hyper-V (6 vCPU, 8GB RAM, 60GB disk). (VM lưu ở ổ D\HyperV)
- IP tĩnh trong dải lớp, DNS: `8.8.8.8`.  
- VM1 mở dịch vụ Remote Desktop (RDP), WinRM (Windows Remote Management) và SMB (D:\ShareFile).  
- Quyền Admin trên VM (để thay đổi firewall).

---

## 1) Kiểm tra dịch vụ & port VM1
- Liệt kê trạng thái service: **RDP (TermService)**, **WinRM (WinRM)**, **SMB (LanmanServer)**.  
- Liệt kê các port đang lắng nghe liên quan: **RDP (3389)**, **WinRM (5985 HTTP / 5986 HTTPS)**, **SMB (445, 139)**.  
- Ghi lại output (service status + netstat / Get-NetTCPConnection / Test-NetConnection).

---

## 2) Kiểm tra truy cập từ vm2 tới vm1
- Từ máy VM2 :  
  - Thử kết nối RDP -> ghi kết quả (connect success / fail).  
  - Thử WinRM (PowerShell Remoting) -> `Test-WsMan` hoặc `Enter-PSSession` -> ghi kết quả.  
  - Truy cập SMB "\\<IP_VM1>\ShareFile" -> ghi kết quả.  

---

## 3) Chặn port bằng Windows Firewall trên vm1
- Dùng Windows Firewall with Advanced Security tạo **Inbound Rule** để **block**:  
  - TCP 3389 (RDP)  
  - TCP 5985 / 5986 (WinRM)  
  - TCP 445 (SMB)  
- Ghi rõ rule bạn tạo (tên rule + port + scope).

---

## 4) Xác minh sau khi block
- Lặp bước 2 (từ vm2) và ghi lại kết quả: RDP / WinRM / SMB có còn truy cập được không.  
- Liệt kê firewall rules hiện thời liên quan (Get-NetFirewallRule + Get-NetFirewallPortFilter).

---

## 5) Task Automation (script) — bắt buộc
- Viết 1 script **block_ports.ps1** thực hiện:  
  - Tạo inbound firewall rules để block các port trên (3389, 5985, 5986, 445).  
  - Kiểm tra và xuất trạng thái (list các rule vừa tạo).  

---

## Đầu ra nộp
Sinh viên cần nộp các mục sau:

1. **Báo cáo ngắn (khoảng 1 trang, file .docx hoặc .pdf)**  
   - Liệt kê trạng thái của các service RDP, WinRM, SMB (trước khi block).  
   - Ghi kết quả test kết nối từ vm2 (trước khi block).  
   - Ghi kết quả test kết nối từ vm2 (sau khi block).  
   - Danh sách các Firewall Rule mà bạn đã tạo (tên rule, port bị block).  

2. **File script `block_ports.ps1`**  
   - Script này phải có lệnh tạo các firewall rule để block port 3389, 5985, 5986, 445.  
   - Script cũng cần có lệnh kiểm tra lại rule sau khi tạo (ví dụ `Get-NetFirewallRule`).  

3. **Ảnh chụp màn hình (2 ảnh)**  
   - Ảnh 1: Kết quả `netstat` hoặc `Get-NetTCPConnection` trước khi block (cho thấy port đang mở).  
   - Ảnh 2: Màn hình khi bạn thử kết nối RDP sau khi block và thấy kết nối thất bại.  

