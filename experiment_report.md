# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-XXXX
**Name:** (Dien ten cua ban)
**Date:** (Dien ngay thuc hien)

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario                          | Agent Response              | Accuracy (1-10) | Notes                                                                                                                                                                       |
| --------------------------------- | --------------------------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Clean Data (`processed_data.csv`) | (Ghi cau tra loi cua Agent) | 7               | AI chưa giải thích tại sao lại chọn option laptop làm best choice, AI dựa vào đâu. Price chỉ là giá mắc nhất thôi chứ chưa chắc giá tiền đó có phù hợp với chất lượng không |
| Garbage Data (`garbage_data.csv`) | (Ghi cau tra loi cua Agent) | 2               | AI just print out highest price but that price is invalid, no valiation                                                                                                     |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

(Viet nhan xet cua ban o day — it nhat 50 tu)

Agent trả lời sai khi dùng Garbage Data vì nó hoàn toàn phụ thuộc vào logic đơn giản “lọc theo category rồi lấy giá trị lớn nhất”, mà không có bất kỳ cơ chế kiểm tra chất lượng dữ liệu nào. Khi dữ liệu bị nhiễu, kết quả cũng bị kéo sai theo.
Các vấn đề thường gặp như duplicate IDs có thể làm Agent hiểu nhầm bản ghi hoặc chọn nhầm dòng dữ liệu. Sai kiểu dữ liệu (ví dụ price là string thay vì number) có thể khiến so sánh idxmax() hoạt động không đúng hoặc cho kết quả ngẫu nhiên. Outliers như “Nuclear Reactor giá 999999” sẽ chi phối toàn bộ quyết định vì thuật toán max luôn ưu tiên giá trị cực trị. Null values cũng làm giảm độ tin cậy khi lọc hoặc so sánh, thậm chí gây lỗi hoặc loại bỏ mất dữ liệu quan trọng.
Vì vậy, vấn đề không nằm ở model mà nằm ở data pipeline: thiếu cleaning, thiếu validation và thiếu rule để loại bỏ dữ liệu phi thực tế.

---

## 3. Ket luan

**Quality Data > Quality Prompt?** (Dong y hay khong? Giai thich ngan gon.)

Đồng ý vì prompt có hay đến đâu mà chất lượng đầu bị sai thì AI vẫn sẽ đưa ra kết quả sai
