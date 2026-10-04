# AI_USAGE

Khai phạm vi sử dụng AI (Copilot/CLI assistant) cho bài nộp này.

## AI đã hỗ trợ

- Chạy các lệnh setup môi trường (`venv`, `pip install -r requirements.txt`).
- Chạy `scripts/verify_lite.py`, `scripts/generate_data_lite.py`,
  `scripts/generate_ai_data.py`, `pytest`, `scripts/run_all.py`.
- Convert 8 file `notebooks/*.py` sang `.ipynb` bằng `jupytext` và thực thi
  toàn bộ (`jupyter nbconvert --execute`) để lưu output — nội dung code trong
  notebook là **mã có sẵn của đề bài**, AI không chỉnh sửa logic bài tập.
- Sao chép notebook đã chạy vào `submission/notebooks/`, chụp screenshot bằng
  chứng (`submission/screenshots/`) từ bản HTML render của notebook đã thực thi.
- Soạn thảo các file báo cáo: `INFO.md`, `REFLECTION.md`, `AI_USAGE.md` này —
  tổng hợp số liệu từ output thực tế của notebook, không bịa số liệu.

## Việc học viên tự làm (không giao cho AI)

- Fork repo đề bài về tài khoản GitHub cá nhân và đổi tên fork đúng mẫu
  (`K4-Track02-Day18-TranHoangDuyAnh-2A202602558-Lakehouse-Lab`).
- Đọc và hiểu nội dung từng notebook, đối chiếu kết quả với
  [RUBRIC.md](../docs/RUBRIC.md) trước khi nộp.
- Mở Pull Request về repo đề bài, điền mô tả PR và gửi liên kết nộp bài qua
  kênh của lớp (AI không có quyền thao tác trên tài khoản GitHub của học viên).
- Đọc lại `REFLECTION.md` và xác nhận nội dung phản ánh đúng suy nghĩ cá nhân.

## Ghi chú

AI không được dùng để tạo ra số liệu giả hay bỏ qua bất kỳ `assert` nào trong
notebook; toàn bộ output trong `submission/notebooks/` là kết quả thực thi
thật trên máy, không chỉnh sửa tay.
