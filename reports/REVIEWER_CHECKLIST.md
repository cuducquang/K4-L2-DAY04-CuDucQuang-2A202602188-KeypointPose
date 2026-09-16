# Reviewer checklist - self-review kỹ thuật

Người gán: Cù Đức Quang

Người kiểm: Cù Đức Quang (self-review kỹ thuật)

Ngày: 2026-09-16

> Bài được làm cá nhân nên không có dữ liệu của bạn cùng nhóm. Checklist này ghi lại self-review bằng checker, visualization và gold; không thay thế một peer review nếu giảng viên bắt buộc có người kiểm khác.

| # | Mục kiểm | Đạt? | Ghi chú / ảnh |
| ---: | --- | :---: | --- |
| 1 | Mọi người có đủ 17 điểm | ☑ | 20 file, 29 skeleton; converter và checker đọc đủ. |
| 2 | Không có xương cắt chéo bất thường ở vai/hông | ☑ | Đã soi `outputs/vis_current`; gold không báo `dao_trai_phai`. |
| 3 | Không có xương kéo sang cơ thể khác | ☑ | Đã sửa `train_04` người #1 `left_wrist`; gold không còn `nham_nguoi`. |
| 4 | Khớp bị che dùng `v=1` và có điểm | ☑ | Tổng `v=1`: 129; các ca mũ/tay lái được giữ điểm. |
| 5 | `v=0` chỉ dùng cho khớp ra ngoài ảnh | ☑ | Checker còn 5 cảnh báo heuristic, đã soi lại; gold không báo `xoa_khop_bi_che`. |
| 6 | Không dùng Hidden | ☑ | Export chỉ có visibility 0/1/2 theo COCO. |
| 7 | COCO Keypoints có 51 số cho mỗi người | ☑ | `annotations/coco_keypoints/person_keypoints_default.json`: 29 annotation. |
| 8 | YOLO Pose có 56 số mỗi dòng, `kpt_shape: [17, 3]` | ☑ | Checker báo **ĐẠT định dạng**. |
| 9 | Visibility report đã nộp và đã so hai bảng | ☑* | Report đã nộp; phần so peer không áp dụng vì bài cá nhân. |
| 10 | Các ca không rõ được ghi trong guideline | ☑ | Ba ca cụ thể trong `GUIDELINE_MINI.md`, có ảnh minh họa. |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | 0 lỗi chặn nộp; có 5 cảnh báo không chặn và đã review. |

## Lỗi tìm được và đã sửa

| Ảnh | Người | Khớp | Lỗi | Sửa thế nào |
| --- | --- | --- | --- | --- |
| `train_04` | #1, bên trái | `left_wrist` | Điểm bị kéo về vùng tay lái/người lân cận (`nham_nguoi`). | Kéo về bàn tay của chính người đó theo chuỗi shoulder-elbow-wrist, giữ `v=1`. |
| `train_15` | #1 trong export cuối, bên trái | `left_elbow` | Lệch xuống dưới cẳng tay 62 px so với gold. | Kéo lên vị trí khuỷu tay trên cánh tay đang hướng về tay lái. |
| `train_15` | #1 trong export cuối, bên trái | `left_wrist` | Lệch xuống dưới bàn tay 48 px so với gold. | Kéo lên bàn tay trên tay lái, giữ đúng chuỗi tay trái. |

## Kết luận

- Lỗi đáng chú ý nhất là khớp tay gần tay lái hoặc người lân cận dễ bị đặt theo vật thể thay vì theo chuỗi cánh tay.
- Đây chủ yếu là lỗi thao tác trong cảnh che khuất; guideline đã được làm rõ bằng quy tắc nối tiếp shoulder-elbow-wrist và visibility `v=1`.
