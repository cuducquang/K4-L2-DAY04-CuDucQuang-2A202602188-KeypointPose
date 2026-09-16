# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.55 khớp có v > 0 mỗi người
- Tổng: v=2 322 | v=1 129 | v=0 42

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 6 | 2 | 21% |
| 1 | left_eye | 18 | 9 | 2 | 31% |
| 2 | right_eye | 19 | 8 | 2 | 28% |
| 3 | left_ear | 10 | 19 | 0 | 66% |
| 4 | right_ear | 14 | 15 | 0 | 52% |
| 5 | left_shoulder | 25 | 4 | 0 | 14% |
| 6 | right_shoulder | 26 | 3 | 0 | 10% |
| 7 | left_elbow | 22 | 5 | 2 | 17% |
| 8 | right_elbow | 23 | 6 | 0 | 21% |
| 9 | left_wrist | 20 | 6 | 3 | 21% |
| 10 | right_wrist | 17 | 10 | 2 | 34% |
| 11 | left_hip | 19 | 10 | 0 | 34% |
| 12 | right_hip | 22 | 7 | 0 | 24% |
| 13 | left_knee | 15 | 9 | 5 | 31% |
| 14 | right_knee | 19 | 5 | 5 | 17% |
| 15 | left_ankle | 16 | 4 | 9 | 14% |
| 16 | right_ankle | 16 | 3 | 10 | 10% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
