## DevOps Thực Chiến: Từ Security đến DevSecOps

### 1. DevOps không chỉ là Docker hay K8s

Nhiều người nghĩ DevOps là học một đống tool như Docker, Terraform, Kubernetes. Thực ra, cốt lõi của DevOps là quy trình và văn hóa. 

Với dân Security, bạn đã quen với việc phòng thủ chiều sâu (Defense in Depth). Việc học DevOps giúp bạn chủ động xây dựng hệ thống an toàn ngay từ đầu (Shift-left security), thay vì đợi Dev code xong rồi mới pentest.

**Xóa bỏ "Silo" (Sự cô lập)**
Trong Security, team SOC tìm ra lỗi, đẩy cho Dev sửa, Dev lại bảo lỗi do Ops cấu hình. DevOps sinh ra để giải quyết việc đùn đẩy này.
Thay vì hỏi "Ai làm hỏng?", hãy hỏi "Lỗ hổng quy trình nào để xảy ra lỗi này?". Nó giống hệt cách chúng ta làm *Incident Response* và *Post-mortem*. 

---

### 2. Linux: Kỹ năng bắt buộc

90% server hiện nay chạy Linux. Các tool pentest hay SIEM của bạn cũng chạy trên Linux. Nếu chỉ dừng ở mức gõ vài lệnh cơ bản, bạn sẽ không thể tự động hóa hay hardening hệ thống được. Ở môi trường server, Terminal là số 1, quên giao diện GUI đi.

**Hiểu quá trình Boot (Khởi động)**
BIOS -> MBR -> GRUB -> Kernel -> Init -> Runlevel. 
Hiểu luồng này, bạn sẽ biết các loại Bootkit/Rootkit ẩn náu ở đâu, và Secure Boot hoạt động thế nào.

**Tối ưu và Hardening hệ thống**
Linux mặc định chạy được, nhưng chưa chắc đã bảo mật và tối ưu. Bạn sẽ cần biết cách phân quyền chuẩn, tắt các module thừa, cấu hình SELinux/AppArmor, và giới hạn syscall để thu hẹp attack surface (bề mặt tấn công).

> Học Linux không phải học vẹt lệnh, mà là hiểu cách hệ điều hành quản lý tài nguyên để cấu hình nó bảo mật và tối ưu hơn.
