# Review nhãn - bài cá nhân

- Người gán: Cù Đức Quang
- Người review kỹ thuật: Cù Đức Quang (self-review)
- Ngày: 2026-09-16
- Hình thức: self-review; không có bài của bạn cùng nhóm để so sánh.

## Phạm vi kiểm tra

Đã kiểm 20 ảnh, 29 skeleton bằng `check_pose_labels.py`, ảnh vẽ từ `visualize_pose.py`, visibility report và kết quả đối chiếu gold. Checker đạt định dạng với 0 lỗi chặn nộp và 5 cảnh báo heuristic; năm cảnh báo đã được soi lại trên ảnh, còn evaluator gold không phát hiện `dao_trai_phai`, `nham_nguoi` hay `xoa_khop_bi_che` sau rework.

## Các lỗi đã sửa

| Ảnh | Người | Khớp | Phát hiện | Cách sửa |
| --- | --- | --- | --- | --- |
| `train_04` | #1, bên trái | `left_wrist` | Điểm thuộc sai vùng tay/người lân cận. | Chuyển về bàn tay của chính người đó và chọn `v=1`. |
| `train_15` | #1 trong export cuối, bên trái | `left_elbow` | Lệch nhẹ 62 px so với gold. | Đưa lên khuỷu tay trên cánh tay đang hướng tới tay lái. |
| `train_15` | #1 trong export cuối, bên trái | `left_wrist` | Lệch nhẹ 48 px so với gold. | Đưa lên bàn tay trên tay lái. |

## Visibility và guideline

Không có thư mục nhãn peer nên không thể tạo bảng `%v=1` của hai người; số liệu so sánh không được tự dựng. Self-review cho thấy `left_ear` 66% và `right_ear` 52% là hai khớp bị che nhiều nhất. `GUIDELINE_MINI.md` đã chốt: tai bị tóc/mũ che nhưng vẫn nằm trong ảnh phải đặt điểm giải phẫu và dùng `v=1`, không dùng Outside.

## Kết luận

Sau rework, mean OKS với gold là 0.9476, OKS@0.50 và OKS@0.75 đều là 1.000; 29/29 người được ghép, không thiếu và không thừa. Bài đủ điều kiện kỹ thuật để nộp. Hạn chế duy nhất của mục review là không có peer reviewer vì bài được làm cá nhân.
