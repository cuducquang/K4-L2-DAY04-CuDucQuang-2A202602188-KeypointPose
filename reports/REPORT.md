# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Cù Đức Quang

MSSV: 2A202602188

Nhóm: Cá nhân (L2-L3 K4P1)

Ngày: 2026-09-16

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 322 / 129 / 42 |
| Thời gian trung bình mỗi ảnh | khoảng 8 phút (từ thời gian job CVAT 10:05-12:44, có gồm kiểm tra/rework) |

Ba khớp có `%v=1` cao nhất:

1. `left_ear` - 66%
2. `right_ear` - 52%
3. `right_wrist` - 34% (đồng hạng với `left_hip` - 34%)

Hai tai đúng là nhóm khó vì mũ bảo hiểm hoặc tóc che bề mặt giải phẫu, như hai người trong `train_04`; vị trí chỉ có thể suy ra từ mắt, đầu và mép mũ. `right_wrist` cũng khó trong cảnh lái xe vì bàn tay thường nằm sau tay lái hoặc thân người. `%v=1` cao phản ánh tần suất bị che, không nhất thiết là sai vị trí; vì vậy tôi vẫn đặt điểm dựa trên chuỗi shoulder-elbow-wrist.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.942 | 0.9476 |
| OKS@0.50 | 1.000 | 1.000 |
| OKS@0.75 | 1.000 | 1.000 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 1 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa các lần chạy:**

- `train_04`, người #1 bên trái, `left_wrist`: điểm cũ nằm về vùng tay lái/người lân cận; tôi kéo về bàn tay của chính người đó theo hướng `left_elbow` và giữ `v=1`.
- `train_15`, người #1 trong export cuối (bên trái), `left_elbow`: điểm lệch xuống dưới cẳng tay 62 px; tôi kéo lên vị trí khuỷu trên cánh tay đang hướng về tay lái.
- `train_15`, người #1 trong export cuối (bên trái), `left_wrist`: điểm lệch xuống dưới bàn tay 48 px; tôi kéo lên bàn tay trên tay lái, đúng chuỗi tay trái.

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh ở cả hai lần chấm. Tôi kiểm thêm bằng ảnh visualization: xanh là bên trái cơ thể, cam là bên phải, đặc biệt ở các cảnh có người quay lưng hoặc chồng nhau như `train_13`. Không có đường vai/hông cắt chéo bất thường sau lần kiểm cuối.

## 3. Kiểm chéo

Bạn cùng nhóm: Không có - bài được làm cá nhân.

Không có thư mục nhãn của người thứ hai nên không thể tạo hai hàng chênh `%v=1` mà không bịa số liệu:

| Khớp | Tôi | Peer | Lệch | Nguyên nhân |
| --- | ---: | ---: | ---: | --- |
| Không áp dụng | - | - | - | Không có peer dataset. |
| Không áp dụng | - | - | - | Self-review được ghi trong `reports/review_partner.md`. |

Luật bổ sung sau self-review: tai bị tóc hoặc mũ che mà đầu vẫn còn trong khung phải đặt điểm theo vị trí giải phẫu và dùng `v=1`; chỉ dùng `v=0` nếu chính vị trí tai đã ra ngoài mép ảnh. Quy tắc này kiểm chứng được bằng `train_04` và đã được thêm vào `GUIDELINE_MINI.md`.

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi cuối notebook

1. `pose_mAP50-95` tăng 0.0055, từ 0.6853 lên 0.6908. Mức tăng nhỏ cho thấy 20 ảnh có giúp model thích nghi thêm với cảnh người bị che bởi xe, mũ và người khác, nhưng chưa đủ để kết luận chất lượng tăng mạnh. `pose_mAP50` và recall không đổi nên phần cải thiện chủ yếu nằm ở độ chính xác định vị tại các ngưỡng chặt hơn.

2. Sau fine-tune, `box_mAP50-95` là 0.8041 còn `pose_mAP50-95` là 0.6908, chênh 0.1133. Model tìm người dễ hơn tìm 17 khớp vì bounding box chỉ cần bao đúng cơ thể, còn pose phải đặt đúng từng khớp và đúng trái/phải. Các cảnh tay sau tay lái, mũ che tai và nhiều người chồng nhau làm sai số keypoint tăng dù box vẫn đúng.

3. Ở `test_02`, model tạo một skeleton nhỏ ở mép trái trên vùng trời/mép mái dù không có người tương ứng. Đây là lỗi **trượt hẳn**, vì các điểm rơi vào nền thay vì chỉ lệch nhẹ quanh một khớp thật. Detection người chính ở bên phải vẫn hợp lý, nên lỗi này là một false positive cục bộ chứ không phải đảo trái/phải.

4. `train_15` có OKS model so với nhãn của tôi thấp nhất, bằng 0.552. Sau khi sửa `left_elbow` và `left_wrist`, nhãn của tôi đạt OKS 0.9119 so với gold cho người tương ứng và toàn ảnh không còn lỗi nặng. Vì thế nhãn của tôi gần bằng chứng gold hơn model; model bị ảnh hưởng bởi tay lái, bình xăng và phần tay bị che.

5. Không cùng ảnh: nhãn của tôi thấp nhất so với gold ở `train_13`, người #1, OKS 0.847; model thấp nhất so với nhãn của tôi ở `train_15`, OKS 0.552. `train_13` khó do ba cơ thể chồng lên nhau và một người nhỏ, nên người gán dễ lệch chuỗi chân. `train_15` lại khó cho model vì cánh tay giao với xe máy và tay lái; hai giá trị thấp đến từ hai kiểu che khuất khác nhau.

## 5. Một rule evidence đã dùng

Ở `train_04`, người #1 bên trái, tôi phải quyết định visibility cho `left_wrist`. Cổ tay không ra ngoài ảnh: cẳng tay vẫn hướng tới bàn tay nằm sau tay lái, nhưng bề mặt khớp bị vật thể và bàn tay che một phần. Vì còn căn cứ từ `left_elbow` và tay lái, tôi đặt điểm ước lượng tại bàn tay của chính người đó và chọn `v=1`. Nếu chọn `v=0`, khớp bị loại khỏi huấn luyện dù nó vẫn nằm trong khung; nếu kéo sang người bên cạnh thì tạo lỗi `nham_nguoi`.
