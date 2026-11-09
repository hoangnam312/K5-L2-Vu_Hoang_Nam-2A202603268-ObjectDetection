# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Vũ Hoàng Nam
- MSSV: 2A202603268
- Email: 26ai.namvh3@vinuni.edu.vn
- Lớp: 
- Link repo bài làm: https://github.com/hoangnam312/K5-L2-Vu_Hoang_Nam-2A202603268-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1N6inkcWflctce4pgqFn3bFu6M5J1eB28?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50 file YOLO
- Kết quả checker và lỗi đã xử lý: chạy `python tools/check_yolo_labels.py` cho kết quả **ĐẠT cấu trúc: 50/50 file nhãn hợp lệ**. Lần chạy đầu bị lỗi sai format do copy cả folder export vào trong
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: `drive_001`, `drive_003`, `drive_005`, `drive_010`, `drive_015`, `drive_017`, `drive_020`, `drive_025`, `drive_030`, `drive_040`.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: đã kiểm tra sự đầy đủ của xe, mỗi xe một box, độ bám sát box và class theo mini guideline; trong 10 ảnh rà không phát hiện lỗi cần sửa thêm. Các trường hợp xe bị che hoặc ở xa được giữ box theo phần nhìn thấy và chỉ gán class khi có đủ căn cứ nhận diện.
- Commit khóa nhãn ban đầu: **`79ec16d` — `add label`**.

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 0 | — |
| FP | 727 | — |
| FN | 653 | — |
| Precision | 0.0000 | — |
| Recall | 0.0000 | — |
| F1 | 0.0000 | — |
| mean_iou_of_matches | 0.0000 | — |
| passed | `false` | — |

Dẫn tới [`outputs/annotation_evaluation.json`](../../outputs/annotation_evaluation.json). Evaluator ghép box cùng class ở ngưỡng IoU `0.50`; kết quả chưa đạt hai ngưỡng yêu cầu là F1 `>= 0.80` và mean IoU `>= 0.70`. Trong repository hiện chưa có bản kết quả sau rework hoặc commit sửa nhãn tương ứng, nên chưa ghi nhận thay đổi sau sửa. Không kết luận nguyên nhân chỉ từ FP/FN; cần đối chiếu trực tiếp từng ảnh, class và vị trí box với gold trước khi quyết định rework.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy:

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.3156193471665181 |
| Recall | 0.30664893617021277 |
| mAP50 | 0.2739647516429376 |
| mAP50–95 | 0.20063156304965743 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
Lớp có recall thấp nhất: Lớp bus có recall bằng 0.0 (thấp nhất trong tất cả các lớp, tiếp theo là truck với 0.2 và van với 0.25).
Ảnh minh họa:
Trong kết quả chạy dự đoán (best_model.predict), ảnh drive_053.jpg được dự đoán có 3 buses, ảnh drive_058.jpg dự đoán có 2 buses, và ảnh drive_060.jpg dự đoán có 1 bus.
Tuy nhiên, thực tế lớp bus chỉ có đúng 2 thực thể (instances) trên toàn bộ tập eval nhưng mô hình không nhận diện chính xác thực thể thực tế nào (recall = 0) hoặc đoán sai lệch hoàn toàn. 

2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
Nếu mAP50 cao nhưng mAP50-95 thấp, điều đó chứng tỏ mô hình rất tốt trong việc phát hiện ra có đối tượng ở vùng đó (nhận diện đúng class và vị trí đại khái), nhưng khung bao (bounding box) vẽ ra bị lệch, quá rộng, hoặc quá hẹp so với nhãn chuẩn (Ground Truth), không đáp ứng được các tiêu chuẩn IoU khắt khe hơn (như IoU >= 0.75 hoặc 0.90).

3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
Có sự nhầm lẫn lớn hoặc không thống nhất trong việc phân biệt giữa các lớp có hình dáng tương đồng như car vs van, hoặc van vs truck/bus.
Dữ kiện:
Số lượng instances của car cực kỳ áp đảo (94 instances trên tổng số 113) và có mAP50 rất tốt (0.738).
Trong khi đó, các lớp như van (12 instances, mAP50 = 0.115), truck (5 instances, mAP50 = 0.235), và bus (2 instances, mAP50 = 0.007) có kết quả rất tệ.
Ở log predict, ví dụ ảnh drive_056.jpg mô hình tìm thấy tới 15 cars, 1 bus, 3 vans. Việc gán nhãn thủ công ban đầu giữa người và người (hoặc giữa các ảnh train) có thể đã vi phạm guideline phân loại: ví dụ một số xe SUV cỡ lớn hoặc xe bán tải (pick-up) lúc thì được gán là car, lúc thì gán là van hoặc truck. Sự mất cân bằng dữ liệu nghiêm trọng kèm theo việc áp dụng guideline không nhất quán khiến mô hình bị thiên lệch (bias) nặng nề về lớp car.

4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
Vì sao không dùng để train:
Để tránh hiện tượng quá khớp (overfitting) và đảm bảo tính khách quan khi đánh giá năng lực tổng quát hóa (generalization) của mô hình. Nếu đưa 10 ảnh eval vào train, mô hình chỉ đơn giản là "học vẹt" các đặc điểm của 10 ảnh này. Lúc đó, các chỉ số đo lường (mAP) đạt được sẽ cực kỳ cao nhưng hoàn toàn vô giá trị khi đem mô hình áp dụng vào thực tế trên các hình ảnh mới chưa từng thấy.
Vai trò validation của chúng trong notebook:
Định hướng dừng sớm (Early Stopping) và lưu checkpoint tốt nhất: Trong quá trình train, sau mỗi epoch mô hình sẽ kiểm tra độ lỗi trên 10 ảnh eval này để chọn ra file weight tốt nhất (best.pt).
Cung cấp phản hồi (Feedback Loop): Giúp người phát triển biết được mô hình đang học tốt hay xấu sau 50 epochs để điều chỉnh siêu tham số (hyperparameters) hoặc cải thiện chất lượng bộ dữ liệu nhãn.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Self-QC nhãn YOLO | Lần đầu chạy bị lỗi vì copy cả folder ouptu vào | Copy lại và chạy lại | Kết quả đạt **50/50 file nhãn hợp lệ**; đây là lỗi thao tác, không phải lỗi cấu trúc nhãn. |
| Gold QC annotation | Các box không được ghép với gold ở ngưỡng IoU `0.50`; kết quả là TP = 0, FP = 727, FN = 653. | Đọc `outputs/annotation_evaluation.json` và giữ nguyên kết quả; không sửa gold hoặc suy đoán nguyên nhân chỉ từ FP/FN. | F1 = 0 và mean IoU = 0, chưa đạt gate. Còn thiếu bước đối chiếu trực tiếp từng ảnh để xác định box/class nào cần rework. |
| Đối chiếu với gold train | Khi đối chiếu, nhận thấy gold train có trường hợp thiếu đối tượng, bounding box bị lệch và nhãn class không đúng so với ảnh quan sát được. | Không xử lý thêm và giữ nguyên cách gán nhãn cũ, vì không được sửa gold train hoặc thay đổi dữ liệu chuẩn. | Giữ nguyên bộ nhãn hiện tại; kết quả gold QC vẫn được báo cáo đúng theo file evaluator, không tự điều chỉnh để làm thay đổi điểm số. |
| Train/evaluate YOLO | Metrics cuối trên 10 ảnh eval thấp: precision `0.3156`, recall `0.3066`, mAP50 `0.2740`, mAP50-95 `0.2006`. | Giữ nguyên split train/eval, dùng cấu hình đã lưu trong `assets/yolo26_runs/vehicle_annotation/args.yaml` với `yolo26n`, 50 epochs và ảnh kích thước 640. | Pipeline đã tạo được log, biểu đồ và ảnh prediction; chất lượng model còn thấp, có thể liên quan đến số lượng dữ liệu ít, mất cân bằng class và chất lượng annotation.|

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

- **Định dạng YOLO và self-QC:** đã có 50 file nhãn đúng basename; checker đạt 50/50.
- **Mini guideline và quyết định mơ hồ:** đã có trong [`GUIDELINE_MINI.md`](../../GUIDELINE_MINI.md), gồm định nghĩa bốn class, quy tắc box và ba case mơ hồ.
- **Gold-set annotation QC:** đã chạy evaluator và có [`outputs/annotation_evaluation.json`](../../outputs/annotation_evaluation.json), nhưng kết quả chưa đạt: F1 `0.0000`, mean IoU `0.0000`, `passed = false`. Chưa có vòng rework được ghi nhận.
- **Đầy đủ vật thể, độ bám sát box và phân lớp:** checker chỉ kiểm tra cấu trúc/toạ độ, không chứng minh chất lượng nội dung. Kết quả gold QC không đạt nên ba tiêu chí này chưa thể tự nhận hoàn thành.
- **Pipeline YOLO và phân tích:** notebook đã chạy và repo có metrics, log train, biểu đồ đánh giá cùng 10 ảnh prediction trong `report/individual/assets/`. Đã giữ riêng train/eval; tuy nhiên model có metrics thấp 

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

- **ChatGPT/Codex:** hỗ trợ đọc cấu trúc repo, kiểm tra nội dung báo cáo, diễn giải thông báo checker/evaluator và soạn thảo các mục báo cáo. Các số liệu trong báo cáo được đối chiếu lại với file trong repo, không dùng AI để tạo hoặc thay thế nhãn YOLO.
- **Python và các script của repo:** dùng `tools/check_yolo_labels.py` để kiểm tra cấu trúc 50 file nhãn và dùng kết quả trong `outputs/annotation_evaluation.json` để ghi phần gold QC.
- **Google Colab/Ultralytics:** dùng để train và đánh giá YOLO; các tham số được ghi lại từ `report/individual/assets/yolo26_runs/vehicle_annotation/args.yaml`.
- **Tài liệu tham khảo:** `RULES.md`, `CHECKPOINTS.md`, `GUIDE.md`, `SUBMISSION.md`, `RUBRIC.md`, `GUIDELINE_MINI.md` và notebook `notebooks/day2_annotation_to_yolo.ipynb`. Không sử dụng nguồn ngoài repo và không đính kèm thông tin bí mật hoặc gold labels.
