# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-2A202600700
**Name:** Trần Minh Hoàng
**Date:** 2026-06-10

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | `Agent: Based on my data, the best choice is Laptop at $1200.` | 10 | Trả về đúng sản phẩm có giá cao nhất trong nhóm Electronics sau khi đã lọc và chuẩn hóa dữ liệu. |
| Garbage Data (`garbage_data.csv`) | `Agent: Based on my data, the best choice is Nuclear Reactor at $999999.` | 1 | Trả về sản phẩm rác/bất thường (outlier) do dữ liệu đầu vào không được kiểm định và lọc bỏ. |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Khi sử dụng `garbage_data.csv`, Agent đưa ra câu trả lời sai nghiêm trọng vì dữ liệu đầu vào chứa nhiều lỗi chất lượng chưa được xử lý:
1. **Outliers (Giá trị dị biệt):** Bản ghi "Nuclear Reactor" có giá trị $999,999 cực kỳ bất thường. Do Agent sử dụng hàm `.idxmax()` để tìm sản phẩm có giá trị lớn nhất nên đã trực tiếp chọn sản phẩm này mà không hề có cơ chế nhận biết đây là dữ liệu lỗi/không hợp lý.
2. **Duplicate IDs (Trùng lặp ID):** ID số 1 xuất hiện cả ở "Laptop" và "Banana", gây khó khăn cho việc định danh duy nhất và có thể gây lỗi logic khi truy vấn sâu.
3. **Wrong Data Types (Sai kiểu dữ liệu):** Giá trị của "Broken Chair" là "ten dollars" (dạng chuỗi thay vì số). Điều này khiến các phép so sánh số học hoặc chuyển đổi kiểu dữ liệu bị lỗi hoặc bỏ qua.
4. **Null values (Thiếu dữ liệu):** Bản ghi "Ghost Item" bị thiếu hoàn toàn Category và giá trị bằng 0, làm loãng dữ liệu huấn luyện hoặc ngữ cảnh của Agent.

Tóm lại, Agent chỉ thực hiện suy luận dựa trên dữ liệu được cung cấp. Nếu dữ liệu đầu vào là "rác" (Garbage In), kết quả đầu ra của Agent chắc chắn sẽ là "rác" (Garbage Out).

---

## 3. Ket luan

**Quality Data > Quality Prompt?** Đồng ý hoàn toàn.

Dù prompt có được thiết kế tối ưu, chi tiết hay thông minh đến đâu thì Agent vẫn phải dựa vào ngữ cảnh dữ liệu (Context) để đưa ra câu trả lời. Nếu dữ liệu cung cấp bị sai lệch, chứa lỗi định dạng hoặc thông tin nhiễu/outlier, Agent không thể tự suy luận ra thông tin đúng. Do đó, việc xây dựng một pipeline làm sạch dữ liệu (Data Pipeline Observability & Quality) là điều kiện tiên quyết và quan trọng hơn cả việc tối ưu câu lệnh (Prompt Engineering).
