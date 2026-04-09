**Reflection & ****Bài**** ****học**** ****từ**** Feedback**** ****các**** team ****khác**
Sau buổi review, tổng hợp và phân tích giải pháp của các nhóm để tối ưu hóa cho hệ thống AI Vinmec của mình:
**1. ****Nhóm**** 26: ****Vinmec**** Assistant (****Chuyên**** ****sâu**** ****về**** ****Hậu**** ****cần**** & ****Kỹ**** ****thuật****)**
**Điểm**** ****học**** ****tập****:** Nhóm này tập trung rất tốt vào **Persona** (người chuẩn bị đi khám) và giá trị thực tế là giúp bệnh nhân chuẩn bị đầy đủ giấy tờ, giảm thời gian tái khám.
**Về**** ****mặt**** ****kỹ**** ****thuật****:** Sử dụng **LangGraph** và **Kafka** để điều phối thông tin là một kiến trúc hiện đại. Đặc biệt là tư duy: *Ưu** **tiên** Database **trước**, **không** **có** **mới** **gọi** Tool* để tối ưu chi phí và tốc độ.
**Ứng**** ****dụng**** ****cho**** ****dự**** ****án**** ****của**** ****mình****:** * Bổ sung thêm thông tin "Cần chuẩn bị gì" (nhịn ăn, CCCD) vào kết quả phân khoa.
Áp dụng cơ chế **Guardrail ****cứng** cho ca đột quỵ (không phụ thuộc vào Tool/LLM để tránh độ trễ hoặc lỗi hệ thống).
**2. ****Nhóm**** 65: ****Trợ**** ****lý**** Y ****tế**** ****thông**** ****minh**** (****Chuyên**** ****sâu**** ****về**** ****Trải**** ****nghiệm**** & ****An**** ****toàn****)**
**Điểm**** ****học**** ****tập****:** Nhóm giải quyết rất tốt nỗi đau (pain point) của khách vãng lai phải đợi lâu bằng cách gợi ý danh mục khám kèm theo **% ****độ**** ****tự**** tin ****của**** LLM**.
**Về**** ****mặt**** an ****toàn****:** Khi gặp ca khẩn cấp (đau đầu nặng, ho lâu ngày), AI cung cấp ngay danh sách hướng dẫn sơ cứu trong khi gọi cấp cứu.
**Ứng**** ****dụng**** ****cho**** ****dự**** ****án**** ****của**** ****mình****:**
Tối ưu hóa **Confidence Score** trong kết quả phân khoa (như đã nêu trong Metric Accuracy).
Tăng cường kịch bản **Out-of-scope** (tình hình chiến sự...) để đảm bảo AI chỉ tập trung vào chuyên môn y tế, tránh lãng phí tài nguyên (latency của Gemini 1.5 Flash).
**3. ****Nhóm**** 11: V-TRIAGE (****Chuyên**** ****sâu**** ****về**** ****Điều**** ****hướng**** & ****Đa**** ****phương**** ****thức****)**
**Điểm**** ****học**** ****tập****:** Tập trung vào giải quyết bài toán quá tải tại bệnh viện. Điểm sáng là tính năng **Ghi**** ****âm**** ****giọng**** ****nói**** (Voice)** và hiển thị bản đồ đường đi/đặt lịch ngay lập tức.
**Về**** ****mặt**** ****xử**** ****lý**** ****tình**** ****huống****:** Xử lý tốt các ca "Con mèo cắn" hay "Đau ngực trái" bằng cách đưa ra các Option lựa chọn rõ ràng cho người dùng.
**Ứng**** ****dụng**** ****cho**** ****dự**** ****án**** ****của**** ****mình****:**
Bổ sung tính năng **Call-to-action (CTA)**: Sau khi AI gợi ý đúng khoa, hệ thống nên hiển thị nút "Đặt lịch ngay" hoặc "Chỉ đường đến khoa".
Mở rộng Mock Data cho các tình huống "Hybrids" (vừa là bệnh lý, vừa là tai nạn như mèo cắn) để kiểm tra độ nhạy của Safety Layer.

**Tổng**** ****kết**** ****bài**** ****học**** ****cá**** ****nhân**** **
Từ feedback của 3 nhóm trên, mình sẽ hiệu chỉnh dự án Vinmec AI của mình theo 3 trục chính:
**Tính**** ****ổn**** ****định**** (Reliability):** Áp dụng kiến trúc Guardrail của nhóm 26. Nếu phát hiện từ khóa "Cấp cứu", "Đột quỵ", hệ thống sẽ ngắt luồng suy nghĩ của LLM và kích hoạt ngay Workflow khẩn cấp.
**Tính**** ****minh**** ****bạch**** (Transparency):** Hiển thị % khả năng dự đoán (theo nhóm 65) để người dùng và nhân viên y tế có sự tin tưởng vào gợi ý của AI.
**Tính**** ****thực**** ****tế**** (Usability):** Không chỉ dừng lại ở phân khoa, mà phải hướng dẫn bệnh nhân "về mặt thủ tục" (nhịn ăn, giấy tờ) như nhóm 26 và 11 đã làm rất tốt.
