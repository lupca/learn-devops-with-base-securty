Hôm nay ta sẽ học cách cấu hình port và bảo mật kết nối để chống hacker brute-force.

Chuẩn bị một máy ảo (VMware, VirtualBox, hoặc Cloud VM) để thực hành.

---

## Ngày 2: Network Cơ bản & Cấu hình SSH Key

### 1. Network dưới góc nhìn Security

Hãy ôn lại 3 khái niệm mạng cốt lõi:
* **IP Address**: IP Public phơi ra Internet (attack surface lớn), IP Private dùng trong nội bộ (an toàn hơn). 
* **Port**: Mỗi port mở là một attack vector. SSH (22), HTTP (80), HTTPS (443). **Nguyên tắc**: Zero Trust - Chỉ mở những port thực sự cần thiết.
* **Subnet**: Gom các server nhạy cảm (như Database) vào Subnet riêng, không cấp IP Public (network segmentation).

### 2. Tại sao phải dùng SSH Key?

Xác thực bằng mật khẩu (Password) là điểm yếu lớn nhất vì dễ bị đoán (Brute-force) hoặc lộ lọt.

**SSH Key** dùng mã hóa bất đối xứng (Asymmetric Cryptography):
* **Public Key (Ổ khóa)**: Lưu trên Server. Ai xem cũng được, không cần bảo mật.
* **Private Key (Chìa khóa)**: Lưu trên máy bạn. Cần bảo vệ nghiêm ngặt.

Khi đăng nhập, Server gửi một challenge, máy bạn dùng Private Key để giải. Khớp thì vào, không cần nhập mật khẩu. Hacker có quét ra port 22 cũng không thể brute-force.

---

### 3. Thực hành: Tạo Key và Hardening Server

Mở Terminal trên **máy cá nhân (Laptop)** của bạn. Không gõ trên server ở bước này.

#### Bước 1: Tạo cặp khóa

```bash
ssh-keygen -t ed25519 -C "my-security-key"
```
* `ed25519`: Thuật toán EdDSA mới, an toàn và ngắn hơn RSA.
* Khi được hỏi tạo `passphrase` (mật khẩu bảo vệ Private Key), khuyên bạn nên nhập để có thêm defense in depth (nếu mất laptop, hacker vẫn cần passphrase mới dùng được key).

#### Bước 2: Kiểm tra khóa
```bash
ls -la ~/.ssh
```
Bạn sẽ thấy 2 file:
* `id_ed25519`: **Private Key**. Giữ bí mật tuyệt đối.
* `id_ed25519.pub`: **Public Key**. Xem nội dung bằng lệnh `cat ~/.ssh/id_ed25519.pub` và copy chuỗi này lại.

#### Bước 3: Cài Public Key lên Server

Đăng nhập vào Server (bằng mật khẩu, lần cuối cùng). Gõ:
```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
```
*Giải thích:* Tạo thư mục `.ssh`, set quyền `700` (chỉ user hiện tại được truy cập). Mở file `authorized_keys` bằng trình soạn thảo nano.

**Dán chuỗi Public Key** vào file này. Lưu lại (`Ctrl + O`, `Enter`), thoát (`Ctrl + X`).

Phân quyền chặt cho file khóa (nếu phân quyền sai, SSH sẽ coi đây là rủi ro bảo mật và từ chối kết nối):
```bash
chmod 600 ~/.ssh/authorized_keys
```

#### Bước 4: Tắt đăng nhập bằng mật khẩu

Đây là bước hardening bắt buộc. Tắt password auth để loại bỏ hoàn toàn vector brute-force.

Mở file config của SSH:
```bash
sudo nano /etc/ssh/sshd_config
```
Tìm dòng `PasswordAuthentication yes` và sửa thành `PasswordAuthentication no`. (Nếu có dấu `#` ở đầu thì xóa đi).

Lưu file và restart lại dịch vụ SSH:
```bash
sudo systemctl restart sshd
```

#### Bước 5: Kiểm tra kết quả

Gõ `exit` để thoát khỏi Server. Từ Laptop, kết nối lại:
```bash
ssh user@IP_cua_server
```
Bạn sẽ vào thẳng server, không bị hỏi mật khẩu server nữa (nếu có đặt passphrase ở Bước 1, nó sẽ hỏi passphrase của file key).

---

**Câu hỏi tư duy:**
Nếu máy tính bạn hỏng, mất file Private Key, và Server đã tắt đăng nhập bằng mật khẩu. Bạn sẽ làm thế nào để vào lại Server? *(Gợi ý: Tìm hiểu khái niệm Console Access của các nhà cung cấp Cloud hoặc cơ chế Break-glass account).*
