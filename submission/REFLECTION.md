# REFLECTION

Trong "Top 5 Lakehouse Anti-Patterns", anti-pattern mình thấy dễ vướng nhất là
**small-files problem**: liên tục ghi lô nhỏ (streaming/micro-batch) mà không
compact định kỳ. NB2 và NB6 đo trực tiếp điều này — hàng trăm file nhỏ ban đầu,
sau `OPTIMIZE` + `Z-ORDER` giảm hơn 10 lần số file và tăng tốc truy vấn hơn chục
lần nhờ file-skipping tốt hơn.

Hệ thống mình quan tâm là pipeline log LLM-observability giống NB4: mỗi request
sinh một bản ghi nhỏ, nếu ingest trực tiếp theo từng request mà không batch sẽ
tạo hàng triệu file Parquet li ti trong vài ngày. Hậu quả là driver/metadata
server phải liệt kê quá nhiều file cho mỗi query, chi phí list tăng, và các
job Gold (p50/p95/cost theo ngày) chạy ngày càng chậm dù dữ liệu không lớn.

Cách phòng tránh: ghi vào buffer/bronze theo micro-batch time-based, chạy
`OPTIMIZE`/compaction theo lịch (ví dụ cuối ngày), và dùng Z-ORDER trên cột hay
lọc (ví dụ `ts`, `model`) để maintenance không chỉ gom file mà còn hỗ trợ
file-skipping cho truy vấn Gold.

Phạm vi dùng AI: xem `AI_USAGE.md`.
