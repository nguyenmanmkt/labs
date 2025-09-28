# Lab Mạng & Firewall — RDP / WinRM / SMB (Ngắn gọn)

## Môi trường & yêu cầu
- VM Windows 11 trên Hyper-V (4 vCPU, 8GB RAM, 70GB disk).  
- IP tĩnh trong dải lớp, DNS: `1.1.1.1`.  
- Cho phép Remote Desktop, WinRM (Windows Remote Management) và SMB share sẵn bật ban đầu (giảng viên cấu hình).  
- Quyền Admin trên VM (để thay đổi firewall).

---

## 1) Kiểm tra dịch vụ & port
- Liệt kê trạng thái service: **RDP (TermService)**, **WinRM (WinRM)**, **SMB (LanmanServer)**.  
- Liệt kê các port đang lắng nghe liên quan: **RDP (3389)**, **WinRM (5985 HTTP / 5986 HTTPS)**, **SMB (445, 139)**.  
- Ghi lại output (service status + netstat / Get-NetTCPConnection / Test-NetConnection).

---

## 2) Kiểm tra truy cập từ client
- Từ máy client trong cùng LAN:  
  - Thử kết nối RDP -> ghi kết quả (connect success / fail).  
  - Thử WinRM (PowerShell Remoting) -> `Test-WsMan` hoặc `Enter-PSSession` -> ghi kết quả.  
  - Truy cập SMB share (`\\<IP>\Shared`) -> ghi kết quả.  

---

## 3) Chặn port bằng Windows Firewall
- Dùng Windows Firewall with Advanced Security tạo **Inbound Rule** để **block**:  
  - TCP 3389 (RDP)  
  - TCP 5985 / 5986 (WinRM)  
  - TCP 445 (SMB)  
- Ghi rõ rule bạn tạo (tên rule + port + scope).

---

## 4) Xác minh sau khi block
- Lặp bước 2 (từ client) và ghi lại kết quả: RDP / WinRM / SMB có còn truy cập được không.  
- Liệt kê firewall rules hiện thời liên quan (Get-NetFirewallRule + Get-NetFirewallPortFilter).

---

## 5) Task Automation (script) — bắt buộc
- Viết 1 script **block_ports.ps1** thực hiện:  
  - Tạo inbound firewall rules để block các port trên (3389, 5985, 5986, 445).  
  - Kiểm tra và xuất trạng thái (list các rule vừa tạo).  

---

## Đầu ra nộp
- Báo cáo ngắn (1 trang): service status, kết quả test trước & sau block, tên các firewall rules bạn tạo.  
- File `block_ports.ps1`.  
- 2 ảnh chụp màn hình: kết quả `netstat`/`Get-NetTCPConnection` trước và screenshot thử kết nối RDP (fail) sau khi block.
