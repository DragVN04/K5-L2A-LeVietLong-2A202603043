# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Lê Việt Long
- MSSV: 2A202603043
- Email: longtk292@gmail.com / 26ai.longlv3@vinuni.edu.vn
- Lớp: Khóa V - Level 2-3
- Link repo bài làm: https://github.com/DragVN04/K5-L2A-LeVietLong-2A202603043
- Link Colab đã chạy:

## 2. Schema và mini guideline

Trong bài lab này, em thực hiện gán nhãn các đối tượng trong ảnh bằng bounding box theo định dạng YOLO. Mỗi đối tượng được xác định bằng class và tọa độ bounding box.
Em sử dụng file GUIDELINE_MINI.md để ghi lại các quy tắc gán nhãn.

Trong quá trình thực hiện, em gặp một số trường hợp cần chú ý:
1. Đối tượng bị mờ: Một số vật thể trong ảnh không có ranh giới rõ ràng nên việc xác định bounding box khá khó khăn. Em cần cân nhắc vùng thực sự thuộc đối tượng thay vì chỉ lấy phần rõ nhất.
2. Đối tượng bị che khuất: Có trường hợp các vật thể nằm chồng lên nhau hoặc bị che mất một phần. Điều này gây khó khăn khi xác định kích thước và giới hạn của bounding box.
3. Đối tượng nhỏ hoặc ở xa: Một số vật thể có kích thước nhỏ, khó phân biệt class và dễ bị gán nhãn không chính xác.

Điều em nhận thấy là việc gán nhãn không chỉ đơn giản là vẽ box quanh vật thể mà còn cần áp dụng thống nhất các quy tắc của guideline.

Phần em muốn được làm rõ thêm là cách xử lý đối tượng bị che khuất nhiều, vật thể quá mờ và trường hợp nào nên bỏ qua thay vì cố gắng gán nhãn.

## 3. Annotation và self-QC

- Số file nhãn train: 50 file.
- Kết quả checker và lỗi đã xử lý: Chưa bổ sung kết quả chạy checker cuối cùng.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: Cần bổ sung danh sách sau khi đối chiếu lại quá trình kiểm tra.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: 
- Commit khóa nhãn ban đầu: 

Trong quá trình annotation, em gặp khó khăn chủ yếu ở việc xác định đúng đối tượng cần gán và kích thước bounding box.

Một số ảnh có nhiều đối tượng nhỏ hoặc bị che khuất nên em chưa chắc chắn về cách đặt box. Khi kiểm tra lại kết quả, em nhận thấy số lượng nhãn không khớp còn khá lớn.

Em đã thực hiện rà soát và chỉnh sửa một số ảnh, chủ yếu tập trung vào việc kiểm tra các box có khả năng bị thừa hoặc chưa đúng. Sau đó, em tiếp tục chạy công cụ đánh giá để xem kết quả có cải thiện hay không.

## 4. Gold QC và rework

Sau khi hoàn thành phần gán nhãn, em sử dụng evaluate_annotations.py để so sánh kết quả với bộ Gold Ground Truth.

| Chỉ số              | Trước sửa | Sau sửa nếu có |
| ------------------- | --------: | -------------: |
| TP                  |    457    |    450         |
| FP                  |    576    |    487         |
| FN                  |    196    |      203       |
| Precision           |    0.4424 |    0.4803      |
| Recall              |   0.6998  |    0.6891      |
| F1                  |    0.5421 |  0.5660        |
| mean_iou_of_matches |    0.8424 |    0.8454      |
| passed              |     false |      false     |

Quá trình rework:

Trong quá trình sửa nhãn, em tập trung kiểm tra những ảnh có nhiều bounding box không khớp với Gold.
Một số ảnh có thay đổi về số lượng nhãn giữa các lần đánh giá:

drive_018: Giảm từ 10 xuống 5 box.
drive_021: Giảm từ 12 xuống 8 box.
drive_028: Giảm từ 19 xuống 12 box.
drive_036: Giảm từ 15 xuống 6 box.
drive_042: Giảm từ 10 xuống 5 box.

Em sử dụng các chỉ số FP, FN và số lượng matches để xác định những ảnh cần ưu tiên kiểm tra. Tuy nhiên, các chỉ số này không đủ để kết luận chính xác từng box sai vì lý do gì.

Sau khi chỉnh sửa, số lượng False Positive giảm từ 576 xuống 487. Precision và F1 có cải thiện, nhưng Recall giảm nhẹ.

Nhận xét kết quả:

Kết quả cuối cùng của em đạt F1 = 0.5660 và Mean IoU = 0.8454.

Mean IoU đã vượt yêu cầu 0.7, cho thấy những bounding box được match có độ trùng khớp tương đối tốt với Gold.

Tuy nhiên, F1 vẫn chưa đạt yêu cầu 0.8. Số lượng FP và FN còn lớn, chứng tỏ kết quả gán nhãn vẫn còn nhiều điểm cần cải thiện.

Em nhận thấy khó khăn lớn nhất không chỉ nằm ở việc vẽ box chính xác mà còn ở việc xác định đúng những đối tượng cần được gán nhãn theo guideline.

Dù đã thực hiện nhiều lần chỉnh sửa, kết quả vẫn chưa đạt yêu cầu. Đây là phần em cần tiếp tục cải thiện trong quá trình học.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy:

| Chỉ số trên 10 ảnh eval | Giá trị |
| ----------------------- | ------: |
| Precision               |         |
| Recall                  |         |
| mAP50                   |         |
| mAP50–95                |         |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
| -------------- | ------------ | ----------- | ----------------- |
|                |              |             |                   |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
