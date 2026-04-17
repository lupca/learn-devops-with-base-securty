## Thực Tế Nghề Nghiệp: Các Vai Trò & Kỷ Nguyên AI

### 1. Phân định các vai trò (Không ai là "Siêu nhân")

Một sai lầm phổ biến là nhồi nhét mọi thứ từ code, test, deploy đến dọn rác hệ thống vào một chức danh "DevOps". Thực tế trong các hệ thống lớn, vai trò được chia rất rõ, và dân Security có lợi thế ở hầu hết các mảng này:

* **Platform Engineering:** Xây dựng nền tảng tự phục vụ (self-service) cho Dev. Với góc nhìn Security, đây là nơi bạn thiết lập các **Guardrails** (chặn lỗi bằng policy tự động) thay vì làm **Gatekeeper** (chặn bằng quy trình review thủ công ở cuối).
* **SRE (Site Reliability Engineering):** Đảm bảo tính sẵn sàng của hệ thống. Giải quyết bài toán chịu tải, tối ưu và tự động hóa phục hồi. Công việc này dùng chung nhiều kỹ năng với **Incident Response**.
* **App Ops:** Chuyên biệt đưa một mảng ứng dụng cụ thể lên production.
* **DevSecOps:** Nhúng các công cụ kiểm tra bảo mật (SAST, DAST, SCA, Container Scan) trực tiếp vào CI/CD pipeline để nó chạy tự động mỗi khi có code mới.

> *Xác định rõ mình thuộc team nào và "blast radius" (tầm ảnh hưởng khi xảy ra lỗi) của mình đến đâu là viên gạch đầu tiên để làm việc chuyên nghiệp.*

---

### 2. Tránh "Bẫy" Tuyển Dụng & Áp Lực On-call

Dân Security (đặc biệt là làm SOC) rất hiểu cảm giác bị bào mòn bởi cảnh báo rác (Alert Fatigue). Vận hành hệ thống (Ops) cũng vậy. Trước khi nhận việc, hãy thẳng thắn hỏi nhà tuyển dụng:

* *"Dự án này là xây dựng luồng tự động hóa mới, hay chủ yếu đi dọn technical debt (cấu hình cũ nát)?"*
* *"Văn hóa On-call (trực sự cố) ra sao? Tỉ lệ cảnh báo rác (Noise-to-Signal ratio) của hệ thống monitor có cao không?"*

Phần lớn thời gian của DevSecOps là thiết lập hạ tầng và pipeline một lần, thời gian còn lại là audit, tối ưu và xử lý sự cố. Hãy tránh xa những nơi bào mòn sức khỏe bằng những ca trực đêm triền miên do không chịu tối ưu hệ thống cảnh báo.

---

### 3. Phỏng Vấn Kỷ Nguyên AI: Hết Thời "Thợ Gõ"

AI có thể viết một file Terraform hay manifest Kubernetes trong 3 giây. Trọng tâm tuyển dụng hiện tại không còn là "kiểm tra học thuộc cú pháp", mà là **Tư duy Kiến trúc và Bảo mật**:

1. **Hiệu đính (Audit) Code AI:** AI thường sinh ra code hạ tầng chạy được nhưng cực kỳ thiếu an toàn (mở full port 0.0.0.0/0, cấp quyền IAM `*`). Chỉ người nắm vững nguyên lý Security mới đủ khả năng rà soát và từ chối đoạn code đó.
2. **Bảo mật cho AI / Autonomous Agents:** Câu hỏi thiết kế hệ thống hiện đại: *"Làm sao thiết kế một sandbox cách ly an toàn, cấp quyền Least Privilege để một AI Agent có thể tự động đọc log và chạy script khắc phục sự cố trên production mà không làm lộ lọt Secret Keys?"*
3. **Giảm thiểu MTTR (Mean Time To Recovery):** Áp dụng tự động hóa để khoanh vùng (Containment) và phục hồi hệ thống ngay khi phát hiện tấn công hoặc sự cố, thay vì dùng sức người.

> *Trong kỷ nguyên tự động hóa, bạn không còn là thợ "vặn ốc" cấu hình. Bạn là người thiết kế quy trình, đặt ra giới hạn bảo vệ (Zero Trust, Least Privilege) và quản lý rủi ro.*
