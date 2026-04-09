# Cá nhân Reflection: Dự án Vinmec chatbot

### Tư duy Thiết kế & Hệ thống (Spec & Architecture)
**Xây**** ****dựng**** Use Cases (4 Paths):** Không chỉ dừng lại ở luồng chính, việc thiết kế 4 nhánh tương tác giúp bao quát toàn bộ hành trình bệnh nhân, từ lúc bắt đầu mô tả triệu chứng đến khi nhận gợi ý chuyên khoa.
Xây dựng kiến trúc hệ thống:
Với yêu cầu "Augmentation" và giới hạn "3 vòng hội thoại", nên sử dụng LangGraph để kiểm soát luồng (State Management) chặt chẽ hơn là để Agent tự chạy tự do.
### 2. Chiến lược Mock Data (Kiến tạo dữ liệu mô phỏng)
Để bản Demo đạt độ tin cậy cao, Mock Data được thiết kế dựa trên 3 trụ cột thực tế:
**Đa**** ****dạng**** ****kịch**** ****bản****:** Bao gồm **Happy Path** (thành công), **Low Confidence** (mơ hồ - cần hỏi thêm), và **Red Flag** (cấp cứu - ưu tiên cao nhất).
**Tính**** ****bản**** ****địa**** ****hóa**** (UX Writing):** Sử dụng ngôn ngữ tiếng Việt đời thường (*"**đau** **lâm** **râm**", "**vã** **mồ** **hôi** **hột**"*) và các lỗi chính tả phổ biến để thử thách khả năng xử lý ngôn ngữ tự nhiên (NLP) của Prompt.
**Ánh**** ****xạ**** ****thực**** ****tế**** (Mapping)**
Cấu trúc một bản ghi Mock Data

**Insight:** Trong y tế, **Precision** quan quan trọng hơn sự phỏng đoán. Hệ thống nên được thiết kế để ưu tiên nói *"**Tôi** **không** **chắc** **và** **cần** **hỏi** **thêm**"* thay vì đưa ra kết luận sai với độ tự tin cao.
### 
### Đánh giá độ tin cậy


Accuracy & Confidence (Độ chính xác và Sự tự tin)
**Tỷ**** ****lệ**** ****phân**** khoa ****đúng**** ****ngay**** ****lần**** ****đầu**** (First-time Triage Accuracy):** Mục tiêu của nhóm bạn là **≥ 85%**. Chỉ số này đo bằng số ca AI gợi ý đúng chuyên khoa so với kết quả khám thực tế của bác sĩ.
**Tỷ**** ****lệ**** ****yêu**** ****cầu**** ****hỗ**** ****trợ**** (Escalation Rate):** Tỷ lệ các cuộc hội thoại mà AI không đủ tự tin (Confidence Score dưới ngưỡng) và phải chuyển cho nhân viên điều phối. Một hệ thống tin cậy là hệ thống biết nói "Tôi không chắc" thay vì đoán bừa.
**Precision (****Độ**** ****chính**** ****xác****) vs. Recall (****Độ**** bao ****phủ****):** Trong y tế, nhóm bạn đã xác định **Precision ****quan**** ****trọng**** ****hơn**. Thà AI thừa nhận chưa chắc chắn còn hơn đưa ra gợi ý sai với độ tự tin cao.
Safety Metrics (Chỉ số An toàn - Quan trọng nhất) - Độ tin cậy trong y tế gắn liền với tính mạng con người.
**Tỷ**** ****lệ**** ****bỏ**** ****sót**** ca ****khẩn**** ****cấp**** (False Negative for Red Flags):** Đây là metric "sinh tử". Tỷ lệ các ca cấp cứu (đau ngực, khó thở...) mà AI không nhận diện được để chuyển ngay sang Emergency. Chỉ số này **bắt**** ****buộc**** ****phải**** ****bằng**** 0%**.
**Tỷ**** ****lệ**** ****kích**** ****hoạt**** Safety Layer:** Đo lường mức độ hoạt động của các "rule cứng" độc lập với LLM để đảm bảo hệ thống luôn có lưới an toàn.

### 4. Phương pháp Kiểm thử độ tin cậy (Testing Strategies)
Để xác định Agent có thực sự hoạt động ổn định hay không, mình đề xuất 4 phương pháp kiểm thử:
**Red Teaming (****Kiểm**** ****thử**** ****xâm**** ****nhập**** ****nội**** dung):** Giả lập các tình huống bệnh nhân cung cấp thông tin sai lệch hoặc cố tình gây nhiễu để xem AI có giữ đúng Policy và Safety Layer không.
**Backtesting**** (****Kiểm**** ****thử**** ****hồi**** ****cứu****):** Sử dụng dữ liệu lịch sử từ các ca khám thực tế tại Vinmec (đã ẩn danh) để đối chiếu kết quả phân khoa của AI với kết quả của bác sĩ.
**A/B Testing ****trên**** Prompt:** Thử nghiệm các biến thể Prompt khác nhau để tìm ra cấu trúc giúp AI nhận diện "Red Flag" nhạy nhất.
**Human-in-the-loop (****Đánh**** ****giá**** ****chuyên**** ****gia****):** Mời các bác sĩ/điều dưỡng đánh giá các câu hỏi làm rõ (Clarifying Questions) của AI xem có thực sự mang tính chuyên môn hay không.

### Hướng dẫn sử dụng cho Demo:
**Kiểm**** ****chứng**** Safety Layer:** Bạn hãy lấy mẫu số **1, 4, 15** để demo. AI của bạn bắt buộc phải nhận diện được nhãn Emergency và đưa ra thông báo khẩn cấp thay vì hỏi nhiều.
**Demo Clarifying Questions:** Dùng mẫu số **5 ****hoặc**** 12**. Đây là những ca mơ hồ, AI cần thể hiện khả năng hỏi 1-2 câu ngắn để thu hẹp phạm vi chuyên khoa (Augmentation).
**Xử**** ****lý**** ****ngôn**** ****ngữ**** ****tự**** ****nhiên****:** Tôi đã dùng các từ như "vã mồ hôi hột", "đau lâm râm", "ợ chua"... để bạn kiểm tra xem Prompt có hiểu đúng ngữ cảnh y tế phổ thông không.
