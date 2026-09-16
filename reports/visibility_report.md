# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 16.04 khớp có v > 0 mỗi người
- Tổng: v=2 342 | v=1 107 | v=0 27

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 23 | 5 | 0 | 18% |
| 1 | left_eye | 19 | 9 | 0 | 32% |
| 2 | right_eye | 21 | 7 | 0 | 25% |
| 3 | left_ear | 10 | 18 | 0 | 64% |
| 4 | right_ear | 15 | 13 | 0 | 46% |
| 5 | left_shoulder | 27 | 1 | 0 | 4% |
| 6 | right_shoulder | 27 | 1 | 0 | 4% |
| 7 | left_elbow | 23 | 5 | 0 | 18% |
| 8 | right_elbow | 24 | 4 | 0 | 14% |
| 9 | left_wrist | 20 | 8 | 0 | 29% |
| 10 | right_wrist | 18 | 9 | 1 | 32% |
| 11 | left_hip | 22 | 5 | 1 | 18% |
| 12 | right_hip | 24 | 3 | 1 | 11% |
| 13 | left_knee | 19 | 6 | 3 | 21% |
| 14 | right_knee | 24 | 1 | 3 | 4% |
| 15 | left_ankle | 14 | 5 | 9 | 18% |
| 16 | right_ankle | 12 | 7 | 9 | 25% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
