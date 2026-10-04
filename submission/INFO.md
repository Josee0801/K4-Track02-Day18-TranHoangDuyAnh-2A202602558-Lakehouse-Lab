# INFO

- **Họ tên:** Tran Hoang Duy Anh
- **MSSV:** 2A202602558
- **Mã bài:** K4-Track02-Day18
- **Đường chạy:** Lightweight (`deltalake` + `pyiceberg` + DuckDB + Polars), không dùng Spark cho NB1–NB4
- **Python:** 3.11.0 (venv `.venv`, tạo bằng `py -3.11 -m venv .venv`)
- **Hệ điều hành:** Windows (PowerShell), chạy trực tiếp bằng Python thay vì `make` (theo hướng dẫn "Chạy lightweight trên Windows PowerShell" trong README)

## Các bước đã thực hiện

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe scripts/verify_lite.py        # 9/9 PASS
.\.venv\Scripts\python.exe scripts/generate_data_lite.py  # Bronze 200K dòng cho NB4
.\.venv\Scripts\python.exe scripts/generate_ai_data.py    # corpus multimodal + agent traces cho NB7/NB8
.\.venv\Scripts\python.exe -m pytest                       # 24/24 PASS
.\.venv\Scripts\python.exe scripts/run_all.py              # 8/8 notebook PASS
```

Sau đó mở 8 notebook trong Jupyter Lab, chạy từ đầu đến cuối ("Run All") để giữ
output đầy đủ, rồi copy 8 `.ipynb` đã chạy vào `submission/notebooks/` theo đúng
hướng dẫn trong [SUBMISSION.md](../docs/SUBMISSION.md). (Ghi chú kỹ thuật: dùng
`jupytext` để convert `notebooks/*.py` sang `.ipynb` và `jupyter nbconvert --execute`
để chạy, cho kết quả tương đương thao tác Run All thủ công trong Jupyter Lab.)

## Kết quả kiểm tra

| Bước | Kết quả |
|---|---|
| `scripts/verify_lite.py` (smoke) | 9/9 PASS |
| `pytest` | 24/24 PASS |
| `scripts/run_all.py` (8 notebook headless) | 8/8 PASS, 47.5s |
| Chạy 8 `.ipynb` giữ output | 8/8 không có cell lỗi |

## Số liệu chính theo từng notebook (trích từ output đã chạy)

- **NB1:** `_delta_log/` có ≥ 2 commit JSON; ghi `age='thirty'` bị chặn bởi schema
  enforcement; cột `tier` được thêm qua `schema_mode="merge"`; DuckDB thấy 2 nhóm tier.
- **NB2:** speedup ≈ **12.9×**, pruning ratio ≈ **55.0×** (cả hai đều vượt ngưỡng ≥ 3× / ≥ 10×).
- **NB3:** MERGE 100K dòng trong 0.12s; RESTORE về v2 trong 0.03s; `history()` có
  **5 version** (gồm cả RESTORE); 0 dòng `score < 0` sau restore.
- **NB4:** Bronze 200K dòng → Silver dedup < Bronze; Gold phủ 7 ngày × 3 model,
  p50 ≤ p95, `cost_usd` dương, `error_rate` trong [0, 1].
- **NB5:** pruning ratio ≥ 5×; field ID của `latency_millis` giữ nguyên qua rename;
  ≥ 2 partition spec cùng tồn tại; toàn bộ dữ liệu vẫn đọc được.
- **NB6:** compaction giảm ≥ 10× số file; clustering skip ≥ 50%; 3 Delta orphan bị
  xoá; Iceberg expire còn 3 snapshot và dọn file không còn tham chiếu.
- **NB7:** amplification random-read = **200×**; vector int8 nhỏ hơn float32 ≥ 3×;
  recall@10 ≥ 0.80; topic fidelity ≥ 0.95; tái hiện được lifecycle bug (external
  index vẫn trả dữ liệu đã xoá).
- **NB8:** Silver partition theo `agent_version`; replay khớp số bước với version
  đã pin; 5 lượt `list_tables` chỉ đọc catalog 1 lần (cache); 4 bucket provenance
  + partition `UNCLASSIFIED`; xoá subject thành công khỏi version hiện tại.
