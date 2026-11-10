# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Hồ Trung Kiên
- MSSV: 2A202003090
- Email: hotrungkien30092005@gmail.com
- Lớp: 2a-lab
- Link repo bài làm: https://github.com/TrungKien0905/K5-L2A-DAY02-HoTrungKien-2A202603090-ObjectDetection-
- Link Colab đã chạy: https://colab.research.google.com/drive/1MV9fkUFbRg8V1ZZR0ikzFOCyx0w46-WH?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

Trong quá trình gán nhãn bằng CVAT, em gặp một số trường hợp khó phân biệt giữa các lớp car, truck và van do góc chụp, độ phân giải thấp và vật thể bị che khuất. Em dựa vào cấu trúc thân xe, cabin và đặc điểm nhận diện của từng loại phương tiện để đưa ra quyết định. Qua đó, em rút ra rằng cần tuân thủ schema thống nhất để hạn chế nhầm lẫn và nâng cao chất lượng dữ liệu gán nhãn.


## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý:
    + Kết quả checker: 50/50.
    + Lỗi phát hiện: Không phát hiện lỗi.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_001, drive_002, drive_003, drive_004, drive_005, drive_006, drive_007, drive_008, drive_011, drive_030.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: drive_005: Xe màu xanh lá dễ nhầm giữa truck và van. Dựa vào cấu trúc cabin và thân xe để xác nhận nhãn truck. drive_011: Xe minivan thân hộp dễ nhầm với car. Dựa vào kiểu dáng thân xe và khoang phía sau để xác nhận nhãn van. drive_030: Xe nhỏ, hình ảnh mờ, dễ nhầm giữa van và car. Kiểm tra đặc điểm thân xe để xác nhận nhãn van.
- Commit khóa nhãn ban đầu: Sau khi hoàn thành kiểm tra chất lượng và xác nhận nhãn đạt yêu cầu, lưu phiên bản nhãn ban đầu lên GitHub.

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP |504 | |
| FP |684 | |
| FN |149 | |
| Precision |0.4242 | |
| Recall |0.7718 | |
| F1 |0.5475 | |
| mean_iou_of_matches |0.8405 | |
| passed |true | |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy:

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.40708387969106213 |
| Recall |0.23397128184805985 |
| mAP50 |0.2737343198820419 |
| mAP50–95 |0.2018250052348706 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
- Lớp bus có recall thấp nhất, đạt 0%, trong khi van đạt 8.33%, truck đạt 20% và car đạt 66%.
- Tập eval có 2 đối tượng bus trong 2 ảnh, nhưng mô hình không phát hiện đúng đối tượng nào. Điều này cho thấy khả năng nhận diện xe buýt còn rất hạn chế.
- drive_058, vì kết quả dự đoán cho thấy mô hình có dự đoán xe buýt trong những ảnh này. Tuy nhiên, cần đối chiếu với ground truth để xác nhận ảnh nào thể hiện lỗi bỏ sót hoặc nhận diện sai.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
- Cần kiểm tra bounding box có bám sát phương tiện hay không, đặc biệt là các lỗi box bị lệch, quá rộng, quá hẹp hoặc chứa nhiều nền ảnh.
- Trong kết quả thực tế, mAP50 đạt 27.37%, còn mAP50–95 đạt 20.18%. Sự chênh lệch này cho thấy khả năng định vị chính xác vật thể vẫn cần cải thiện, mặc dù cả hai chỉ số hiện đều còn thấp.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
- Trong quá trình gán nhãn bằng CVAT, tôi gặp một số trường hợp mơ hồ:
    + drive_005: Xe tải màu xanh lá dễ nhầm với van.
    + drive_011: Xe minivan dễ bị nhầm thành car.
    + drive_030: Xe nhỏ, ảnh mờ, khó phân biệt giữa van và car.
- Kết quả đánh giá cho thấy recall của van chỉ đạt 8.33%, truck đạt 20% và bus đạt 0%. Đây là dấu hiệu cho thấy mô hình gặp khó khăn với một số lớp phương tiện.
- Tuy nhiên, cần xem confusion matrix để xác định cụ thể các cặp class bị nhầm. Các trường hợp mơ hồ trong guideline chưa đủ để khẳng định nguyên nhân gây lỗi dự đoán.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
- Không sử dụng 10 ảnh eval để train nhằm tránh rò rỉ dữ liệu, giúp đánh giá khả năng nhận diện của mô hình trên những ảnh không được sử dụng để cập nhật trọng số.
- Trong notebook, 50 ảnh được dùng để huấn luyện, còn 10 ảnh eval được sử dụng làm tập validation để tính Precision, Recall, mAP50 và mAP50–95.
- Tập validation cũng được sử dụng để lựa chọn checkpoint tốt nhất (best.pt). Vì vậy, 10 ảnh này không phải tập test độc lập hoàn toàn. Để đánh giá khả năng tổng quát hóa khách quan hơn, cần sử dụng thêm một tập test riêng chưa tham gia huấn luyện hoặc lựa chọn mô hình.
## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
| --- | --- | --- | --- |
| Gán nhãn trên CVAT | Xe `truck` dễ nhầm với `van` do ảnh mờ và góc chụp từ trên cao. | Quan sát cabin, thùng xe và đối chiếu guideline. | Xác nhận nhãn `truck` cho `drive_005`. |
| Phân loại phương tiện | Xe `van` dễ nhầm với `car` do thân xe nhỏ và hình dáng tương tự. | Kiểm tra thân xe dạng hộp, chiều cao và khoang phía sau. | Xác nhận nhãn `van` cho các trường hợp mơ hồ. |
| Kiểm tra chất lượng nhãn | Một số xe nhỏ, bị che khuất hoặc khó quan sát, có nguy cơ bỏ sót hoặc vẽ box chưa sát. | Rà soát ảnh bằng mắt, kiểm tra class và bounding box theo guideline. | Cần xác nhận số lỗi thực tế đã phát hiện và chỉnh sửa. |
| Huấn luyện YOLO26 | Dữ liệu train chỉ có 50 ảnh, khiến khả năng học đặc trưng phương tiện còn hạn chế. | Sử dụng `yolo26n.pt`, huấn luyện 50 epochs với kích thước ảnh 640. | Hoàn thành huấn luyện, lưu checkpoint `best.pt`. |
| Đánh giá mô hình | Recall thấp (23.40%), mô hình còn bỏ sót nhiều phương tiện. | Đánh giá trên 10 ảnh eval và kiểm tra kết quả dự đoán. | Precision đạt 40.71%, mAP50 đạt 27.37%; cần cải thiện khả năng phát hiện. |
| Kiểm tra bounding box | mAP50–95 chỉ đạt 20.18%, cho thấy độ chính xác định vị còn hạn chế. | So sánh box dự đoán với nhãn thực tế trên ảnh eval. | Cần tiếp tục kiểm tra box lệch, quá rộng hoặc quá hẹp. |
| Phân tích theo class | Mô hình nhận diện `bus` và `van` kém, có nguy cơ bỏ sót hoặc nhầm lớp. | Kiểm tra recall từng lớp và kết quả dự đoán. | Recall `bus` đạt 0%, `van` đạt 8.33%; cần thêm dữ liệu và rà soát nhãn. |


## 7. Tự đánh giá theo rubric

| Tiêu chí | Kết quả bài làm |
| --- | --- |
| Hoàn thành và định dạng YOLO | 50 file cùng basename, 5 cột, normalized, bốn class ID đúng |
| Đầy đủ vật thể | gán đủ xe thuộc schema, kể cả xe che/cắt khung có thể nhận diện |
| Độ bám sát và hình học box | box sát phần vật thể nhìn thấy, không lấn nền/gộp vật thể |
| Phân lớp nhất quán | phân biệt `car/truck/bus/van`, không gán vật thể ngoài scope |
| Mini guideline và quyết định mơ hồ | định nghĩa lớp, quy tắc box, ít nhất ba case có lý do |
| Pipeline YOLO và phân tích | Colab chạy được, không dùng eval để train, có metrics/ảnh eval và nhận xét lỗi |

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

Yolo26.
