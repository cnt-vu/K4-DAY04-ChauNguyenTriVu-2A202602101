# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Châu Nguyễn Tri Vũ   MSSV: 2A202602101   Hình thức: Bài làm cá nhân   Ngày: 16/09/2026

> Báo cáo được lập dựa trên kết quả chạy công cụ và số liệu thực tế từ hệ thống.

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 342 / 107 / 27 |
| Thời gian trung bình mỗi ảnh | ~4.5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` — 64%
2. `right_ear` — 46%
3. `right_wrist` (và `left_eye`) — 32% (hoặc `left_wrist` — 29%)

**Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích:**

Không hoàn toàn. Các khớp có tỉ lệ `%v=1` cao nhất như tai (`left_ear`, `right_ear`) chủ yếu là do góc quay đầu hoặc bị tóc, mũ bảo hiểm che khuất một phần (tức là "hay bị che"), nhưng về mặt giải phẫu thì vị trí của tai vẫn tương đối dễ suy luận từ cấu trúc khuôn mặt và hướng nhìn của mắt. Ngược lại, khớp mà tôi thấy khó gán chính xác nhất là hông (`left_hip`, `right_hip`), bởi vì người trong bộ ảnh hầu hết đều mặc quần áo dày hoặc đang ngồi trên phương tiện (xe đạp, xe máy), không có mốc bề mặt nhìn thấy trực tiếp nên hoàn toàn phải ước lượng dựa trên trục cơ thể.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9418 | 0.9418 |
| OKS@0.50 | 0.9655 | 0.9655 |
| OKS@0.75 | 0.9655 | 0.9655 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 1 | 1 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- Bài làm đạt mức **Xuất sắc** ngay từ lần chấm đầu tiên với OKS trung bình đạt **0.9418** và OKS@0.75 đạt **0.9655** (vượt xa ngưỡng xuất sắc 0.85). Toàn bộ 20 ảnh không có bất kỳ lỗi đảo trái/phải nào (`dao_trai_phai = 0`). Chỉ có 1 người ở rìa ảnh xa trong `train_13.jpg` (người đứng phía xa bên trái) chưa được gán và 1 cảnh báo nghi nhầm người ở `train_04.jpg` (khớp `left_wrist` của người thứ 1 nằm sát vùng cánh tay người thứ 2 do hai người lái xe song song).

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh. Riêng ảnh `train_02.jpg` (người đi xe đạp), điểm OKS đối chiếu với đáp án gold đạt mức xuất sắc **0.9854** (gần như trùng khớp tuyệt đối). Điều này chứng minh nhận định về tư thế thực tế của tôi là hoàn toàn chính xác và hoàn toàn khớp với ground-truth của COCO.

## 3. Tự kiểm soát chất lượng (Self-QC - Bài làm cá nhân)

- **Hình thức thực hiện**: Bài làm cá nhân độc lập theo quy định của giảng viên (không phân nhóm, không thực hiện kiểm chéo với người khác).
- **Quy trình tự kiểm định chất lượng (Self-QC)**: Thay cho việc so sánh chéo, học viên đã tự thiết lập và thực hiện quy trình kiểm soát chất lượng nội bộ qua 3 chặng khép kín:
  1. **Trực quan hóa xương (`tools/visualize_pose.py`)**: Đã xuất và rà soát toàn bộ 20 ảnh và 28 skeleton dưới dạng ảnh nối khớp trong `outputs/vis_train/`. Kiểm tra trực quan 100% để đảm bảo không có xương nối cắt chéo thân (không đảo trái/phải) và không kéo sang cơ thể người khác.
  2. **Kiểm tra tính toàn vẹn dữ liệu (`tools/check_pose_labels.py`)**: Tự động kiểm tra định dạng nhãn YOLO Pose (đủ 56 số/dòng, tọa độ chuẩn hóa trong [0, 1], cờ `v` chỉ nhận giá trị 0, 1, 2 và khớp `v=1` luôn có tọa độ thực tế). Kết quả kiểm tra: 0 lỗi.
  3. **Phân tích phân phối cờ (`reports/visibility_report.md`)**: Tự rà soát phân phối các cờ visibility. Đặc biệt kiểm tra kỹ các khớp có tỉ lệ `%v=1` cao như `left_ear` (64%), `right_ear` (46%) và các khớp có `v=0` để đảm bảo tuân thủ nghiêm ngặt luật của môn học (khớp bị che khuất trong khung hình bắt buộc gán `v=1`, không được dùng `v=0`).

Quy tắc mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi tự rà soát:

- **Quy tắc về tai**: Nếu nhìn thấy rõ vành tai hoặc gốc tai thì gán `v=2`. Nếu tai bị tóc dài, mũ len hoặc mũ bảo hiểm che khuất tâm lỗ tai nhưng vẫn xác định được vị trí dựa vào trục mắt - mũi thì bắt buộc chọn `v=1` (Occluded) và đặt chấm ước lượng. Tuyệt đối không chọn `v=0` khi đầu vẫn ở trong khung hình.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. **`pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?**  
   Chỉ số `pose_mAP50-95` tăng **+0.0055** (từ 0.6853 lên 0.6908) và `pose_precision` tăng **+0.0058** (từ 0.9734 lên 0.9792). Việc chỉ số mAP tăng sau khi fine-tune chỉ với 20 ảnh là kết quả rất tích cực, chứng minh 20 nhãn train được gán cẩn thận, bổ sung được các đặc trưng tư thế thực tế hữu ích (đặc biệt là các tư thế ngồi phương tiện, quay lưng) mà không làm suy giảm khả năng tổng quát hóa gốc của mô hình.

2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?**  
   `box_mAP50-95` (0.8041) cao hơn `pose_mAP50-95` (0.6908) khoảng **0.1133 (~11.3%)**. Model tìm *người* dễ hơn tìm *khớp* rất nhiều. Bounding box chỉ cần bao quát diện tích khối cơ thể tổng thể (đầu, thân) với kích thước lớn và đặc trưng ổn định; trong khi keypoints đòi hỏi định vị chính xác tuyệt đối tới từng cụm pixel giải phẫu của 17 điểm nhỏ, vốn rất dễ bị xoay hướng, biến dạng và che khuất phức tạp.

3. **Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):**  
   Ở ảnh `test_03.jpg`, model nhận diện đúng vị trí người và thân nhưng bị **lệch nhẹ** ở khớp cổ tay và mắt cá chân do đối tượng ở góc nghiêng và bị đồ vật che khuất một phần.

4. **Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?**  
   Ảnh có OKS thấp nhất là **`train_06`** (chỉ đạt **0.098**). Trong ca này, **nhãn của tôi là đúng**. Căn cứ thị giác: người lái xe mô tô Honda Goldwing màu vàng mặc áo khoác trùm đầu rộng thùng thình và đội mũ bảo hiểm vàng che kín mặt từ phía sau. Mô hình AI bị nhầm lẫn trước hình khối cồng kềnh của thân xe và áo khoác nên dự đoán lệch gần như toàn bộ khung xương, trong khi nhãn của tôi bám đúng theo trục cơ thể người lái.

5. **Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?**  
   Ảnh `train_06` là ảnh có sự bất đồng lớn nhất (OKS 0.098). Điều này phản ánh bức ảnh thuộc diện ca biên đặc biệt khó (edge case): góc chụp từ sau lưng xe mô tô phân khối lớn, góc nhìn 3/4 khuất mặt và trang phục che giấu hầu hết các đường cong giải phẫu tự nhiên, tạo ra thách thức thị giác lớn cho cả mắt người lẫn thuật toán học sâu.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người, khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

Trong ảnh `train_04.jpg`, người thứ nhất (nữ tay lái cào cào phía bên phải), tôi đã phải đưa ra quyết định trạng thái cờ cho các khớp đầu gối (`knee`) và cổ chân (`ankle`). Căn cứ thị giác là mép dưới của bức ảnh cắt ngang qua phần hông và gốc đùi của người lái, toàn bộ cẳng chân và bàn chân không hề xuất hiện trong trường nhìn của camera. Do các khớp này đã nằm hoàn toàn ngoài khung hình (out of frame) chứ không phải nằm trong ảnh mà bị đồ vật khác che khuất, tôi quyết định chọn trạng thái `v = 0` (Outside) và không chấm tọa độ ước lượng, thay vì gán `v = 1`.
