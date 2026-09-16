# Mini guideline - nhóm: Bài cá nhân  |  người gán: Châu Nguyễn Tri Vũ (MSSV: 2A202602101)  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Ước lượng tâm khớp hông tại vị trí mấu chuyển lớn xương đùi (ngay dưới cạp quần/thắt lưng); gán `v=1` nếu mặc áo dài trùm qua hông, gán `v=2` nếu nhìn rõ nếp gấp đùi | Hông không có bề mặt da nhìn thấy trực tiếp khi mặc đồ; căn theo mốc giải phẫu vận động để chiều dài xương đùi và thân không bị lệch |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nếu thấy được vành tai hoặc gốc tai thì chọn `v=2`; nếu mũ bảo hiểm hoặc tóc che khuất hoàn toàn lỗ tai nhưng vẫn định vị được dựa trên trục mắt - mũi thì chọn `v=1` và chấm ước lượng | Đảm bảo tính nhất quán giữa bằng chứng nhìn thấy trực tiếp và ước lượng có căn cứ thị giác |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Toàn bộ các khớp nằm ngoài mép ảnh (đầu gối, cổ chân) bắt buộc chọn `v=0` (Outside) và không đặt tọa độ | Khớp đã văng ra ngoài khung hình thì không đoán mò, tránh làm nhiễu không gian toạ độ khi huấn luyện model |
| Cổ tay nằm sau tay lái / sau thân mình | Ước lượng điểm giao giữa cẳng tay và bàn tay, chọn `v=1` (Occluded) | Hướng cẳng tay và bàn tay nắm giữ vẫn cung cấp đầy đủ thông tin hình học để xác định tâm cổ tay |
| Hai người chồng lên nhau | Hoàn thiện dứt điểm từng người; người ở sau bị che bộ phận nào thì bộ phận đó gắn `v=1` tại vị trí giải phẫu ước lượng | Tránh lỗi nhầm xương giữa hai người và tránh việc bỏ sót điểm của người bị che |
| Người nhỏ đến mức nào thì không gán nữa | Mọi người có thể phân biệt được đầu và thân trong 20 ảnh core đều phải gán đủ 17 điểm | Bộ dữ liệu core 20 ảnh đã được ban tổ chức chuẩn hóa để mọi người đều đủ điều kiện gán nhãn |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_04.jpg`, người thứ `1` (nữ lái xe cào cào bên phải), khớp `left_knee, right_knee, left_ankle, right_ankle`

- Mơ hồ ở chỗ nào: Mép dưới bức ảnh cắt ngang qua vùng đùi/hông, không nhìn thấy chân nhưng người vẫn đứng ở vùng trung tâm ảnh.
- Bạn quyết thế nào: Chọn `v = 0` (Outside) cho toàn bộ đầu gối và cổ chân cả hai bên.
- Vì sao: Khớp đã hoàn toàn văng ra ngoài khung hình (out of frame) của camera, không thể coi là bị vật thể che khuất (occluded).
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu chọn `v = 1` và chấm bừa vào mép dưới ảnh, model sẽ học sai rằng đầu gối người có thể nằm ngay sát hông hoặc mép ảnh là một phần cơ thể.

### Ca 2 - ảnh `train_02.jpg`, người thứ `1` (người đi xe đạp), khớp `left_elbow, right_elbow`

- Mơ hồ ở chỗ nào: Người lái quay lưng về phía sau theo góc chéo, tay phải co lại gần ba lô, tay trái vươn ra phía trước cầm ghi-đông, cùi chỏ bị che một phần bởi góc nhìn nghiêng.
- Bạn quyết thế nào: Chấm tâm khớp tại điểm giao trục bắp tay và cẳng tay, gán `v = 2` cho khớp nhìn rõ và `v = 1` nếu bị khuất góc nhìn.
- Vì sao: Trục xương hai phần tay vẫn tạo thành góc rõ ràng, cho phép định vị chính xác cùi chỏ giải phẫu.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu bỏ qua không chấm hoặc đánh `v = 0`, model sẽ mất khả năng dự đoán tư thế tay khi lái xe ở góc nhìn 3/4 từ phía sau.

### Ca 3 - ảnh `train_10.jpg`, người thứ `1` (người áo đỏ trên xe máy), khớp `left_knee, right_knee, left_ankle, right_ankle`

- Mơ hồ ở chỗ nào: Người lái cúi rạp trên đầu xe máy trong phòng, góc chụp cận cảnh chỉ lấy từ thắt lưng trở lên.
- Bạn quyết thế nào: Gán hông `v = 1` (do bị áo hoodie và bình xăng che), còn toàn bộ gối và cổ chân gán `v = 0`.
- Vì sao: Phần hông vẫn nằm trong phạm vi ảnh (ngay phía trên yên xe), còn chân hoàn toàn nằm ngoài khung ảnh phía dưới và hai bên.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu để chân `v = 1`, model sẽ cố gắng đoán vị trí chân ở những nơi hoàn toàn không có thông tin hình ảnh, gây ảo giác (hallucination) khi dự đoán pose.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `64%` / đối chiếu `42%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Do guideline ban đầu chưa làm rõ tiêu chuẩn tai bị tóc hoặc quai mũ che một phần (che bao nhiêu % diện tích thì chuyển sang `v=1`).
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Chỉ cần thấy được gốc tai hoặc vành tai thì giữ `v=2`; nếu lỗ tai và cấu trúc chính bị che hoàn toàn thì mới đánh dấu `v=1`.
