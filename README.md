[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=24112769&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** hoangtmhd@fpt.edu.vn
**Name:** Trần Minh Hoàng

---

## Mo ta

Bài Lab này xây dựng một ETL Pipeline tự động bằng Python để xử lý, làm sạch và chuẩn hóa dữ liệu sản phẩm từ file JSON sang CSV. Qua đó, tiến hành thử nghiệm tác động của chất lượng dữ liệu đối với câu trả lời của AI Agent (Stress Test):
- **Extract:** Đọc dữ liệu từ file `raw_data.json`.
- **Validate:** Loại bỏ dữ liệu rác (giá <= 0, thiếu nhóm ngành hàng).
- **Transform:** Chuẩn hóa nhóm ngành hàng thành Title Case, áp dụng giảm giá 10% (discounted_price) và thêm thuộc tính giám sát thời gian xử lý (`processed_at`).
- **Load:** Lưu trữ dữ liệu sạch vào `processed_data.csv`.

---

## Cach chay (How to Run)

### Prerequisites
```bash
pip install pandas pytest
```

### Chay ETL Pipeline
```bash
python solution.py
```

### Chay Agent Simulation (Stress Test)
```bash
python agent_simulation.py
```

---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline script
├── processed_data.csv       # Output cua pipeline
├── experiment_report.md     # Bao cao thi nghiem
└── README.md                # File nay
```

---

## Ket qua

- **Dữ liệu thô ban đầu:** 5 bản ghi.
- **Sau khi chạy pipeline:**
  - **Giữ lại:** 3 bản ghi hợp lệ (được chuẩn hóa và lưu vào `processed_data.csv`).
  - **Loại bỏ:** 2 bản ghi không hợp lệ (1 bản ghi có giá <= 0, 1 bản ghi bị thiếu Category).
- **Stress test Agent:** Agent đưa ra câu trả lời chính xác 100% khi dùng dữ liệu sạch, nhưng trả lời sai lệch hoàn toàn khi dùng dữ liệu rác chứa các giá trị dị biệt (Outliers).
