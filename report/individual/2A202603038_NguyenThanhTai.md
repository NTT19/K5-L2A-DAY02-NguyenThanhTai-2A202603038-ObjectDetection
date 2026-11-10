# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Nguyễn Thành Tài
- MSSV: 2A202603038
- Email: thanhtai5223@gmail.com
- Lớp: 2A202603038
- Link repo bài làm: https://github.com/NTT19/K5-L2A-DAY02-NguyenThanhTai-2A202603038-ObjectDetection.git
- Link Colab đã chạy: https://colab.research.google.com/drive/1BGZtBpXtQnl6AUkRg8LFmvtS55hywkC0?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train:
- Kết quả checker và lỗi đã xử lý:
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
- Commit khóa nhãn ban đầu:

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 413| |
| FP | | 311|
| FN | | 240|
| Precision | 0.5704| |
| Recall | 0.6325| |
| F1 | 0.5999| |
| mean_iou_of_matches | 0.8629| |
| passed | false| |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy:

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.3156193471665181|
| Recall | 0.30664893617021277|
| mAP50 | 0.2739647516429376|
| mAP50–95 | 0.20063156304965743|

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa? 
 - drive_059.jpg có một đối tượng được dự đoán là truck với confidence 0.54 có Recall thấp nhất.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
 - Khi mAP50 cao nhưng mAP50-95 thấp, model có khả năng nhận diện đúng đối tượng nhưng độ chính xác định vị bounding box chưa tốt. Các box có thể bị lệch vị trí, sai kích thước hoặc bao phủ thừa, thiếu một phần đối tượng. Ở IoU = 0.5, các box vẫn có thể được xem là dự đoán đúng, nhưng khi tăng ngưỡng IoU, độ chính xác không còn đáp ứng yêu cầu. Điều này cho thấy model cần cải thiện khả năng localization.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
 - Một vấn đề đáng chú ý là sự nhầm lẫn giữa các lớp Car và Van. Ví dụ, trong ảnh drive_051 và drive_054, model dự đoán một số phương tiện là Van với confidence cao. Cần đối chiếu Ground Truth để xác định phương tiện đó có thực sự là Van hay bị nhầm với Car. Ngoài ra, các phương tiện lớn trong drive_056 cũng cần được kiểm tra để phân biệt Van, Bus và Truck
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
 - Mặc dù 10 ảnh eval đã có Ground Truth labels, chúng không được sử dụng để huấn luyện nhằm bảo đảm tính khách quan trong quá trình đánh giá model.
 - Tập train gồm 50 ảnh được sử dụng để model học đặc trưng của các phương tiện như Car, Bus, Truck và Van thông qua quá trình cập nhật trọng số.
 - Tập eval gồm 10 ảnh được giữ riêng để kiểm tra khả năng tổng quát hóa (Generalization) của model trên dữ liệu không được sử dụng trực tiếp để cập nhật trọng số.
 - Nếu đưa ảnh eval vào train, model có thể học thuộc đặc điểm của những ảnh này, dẫn đến Data Leakage và làm cho kết quả đánh giá không còn phản ánh khách quan khả năng hoạt động trên dữ liệu mới.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
|Annotation dataset | Số lượng nhãn Student chưa khớp với bộ Gold. Lần kiểm tra ban đầu có 48 file nhãn Student trong khi Gold có 50 file.| Sử dụng Python để kiểm tra số lượng file .txt, đối chiếu tên ảnh và nhãn tương ứng.| Xác định được số lượng file còn thiếu. Cần hoàn thiện đủ 50 nhãn train trước khi chạy pipeline chính thức.|

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.
Đã thực hiện các bước chính trong quy trình xây dựng mô hình Object Detection bằng YOLO26, từ chuẩn bị dữ liệu, kiểm tra annotation đến quan sát kết quả dự đoán trên tập eval. Tuy nhiên, vẫn cần hoàn thiện việc xác nhận đủ 50 nhãn train hợp lệ, ghi nhận đầy đủ các chỉ số đánh giá và đối chiếu kết quả dự đoán với Ground Truth. Các nội dung chưa có kết quả kiểm chứng sẽ không được tự đánh giá là hoàn thành
## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
