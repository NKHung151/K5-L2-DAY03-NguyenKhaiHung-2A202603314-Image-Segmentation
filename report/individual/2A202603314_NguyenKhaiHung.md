# Báo cáo thực hành Day 3 — Nguyễn Khải Hùng

- Mã học viên theo lớp: 2A202603314
- Ngày / CVAT local: 12/11/2026, CVAT chạy local
- Công cụ đã dùng: Polygon và Brush trong CVAT; ở Easy có dùng Snap to contour / Intelligent scissors để bám biên.

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: ảnh `000000181542`, người phụ nữ mặc áo dài trắng ở phía trước, tôi vẽ bằng Polygon.
- Class và quy tắc tôi dùng để chọn biên: class `person`. Tôi chỉ vẽ phần nhìn thấy, dừng mask ở chỗ người này chạm vào xe máy hoặc người đứng cạnh, không đoán phần bị che.
- Nếu dùng gợi ý sau đó: không dùng gợi ý tự động cho Medium, tất cả các object còn lại tôi vẽ tay bằng Polygon và Brush.
- Một quyết định gán nhãn của tôi: xe máy bị người ngồi che thành nhiều mảng rời, tôi coi đó là một vật nên gộp thành một object `motorcycle`, không tách mỗi mảng thành một object.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: easy_semantic, ảnh thứ 2, vùng cây (vegetation) và tòa nhà.
- Lỗi thuộc loại: sai thứ tự lớp (z-order), dẫn đến phủ sai vùng.
- Bằng chứng tôi nhìn thấy: tôi vẽ một mảng `sky` lớn rồi vẽ cây và nhà đè lên. Khi giải nén bản export và xem lại mask, mảng sky lại nằm đè lên cây, nên cây bị mất một phần. Tôi nhận ra nguyên nhân là thứ tự lớp trong CVAT chứ không phải do tôi vẽ sai biên.
- Quy tắc và hành động sửa: với semantic, mỗi pixel chỉ thuộc một lớp nên lớp nằm trên sẽ thắng. Tôi đưa `sky` xuống dưới cùng bằng "To background", rồi để các lớp building, vegetation, sidewalk, road nằm trên nó.
- Sau sửa đã Save và export lại chưa? Rồi, tôi export lại và xem lại thì cây không còn bị sky che.

Chưa có điểm tự đánh giá để ghi ở mục này.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| Easy, ảnh có ô tô trên đường | Tô cả ô tô vào `road` vì ô tô đứng trên mặt đường, hoặc để trống ô tô vì class của Easy không có `car` | Easy chỉ có 5 class, không có xe. Vẽ ô tô vào `road` thì mask đường sẽ sai | Tôi để trống phần ô tô, chỉ tô vùng đường nhìn thấy |
| Medium, ảnh `000000181542`, xe máy bị người che | Mỗi mảng nhìn thấy là một xe máy riêng, hoặc cả các mảng là một xe | Hướng dẫn nói một vật bị che có thể có các vùng nhìn thấy rời nhau nhưng vẫn là một instance. Lúc đầu tôi vẽ một xe thành ba polygon | Tôi gộp thành một object bằng Brush cho mỗi xe máy, kết quả có 4 xe máy |
| Hard, ảnh `000000460147`, làn đường bên trái | Làn trái là `road` vì có vạch kẻ và xe chạy, hoặc là `sidewalk` | Ranh road–sidewalk theo chức năng và bó vỉa (mini-sheet), không chỉ theo màu; làn này nằm sau dải cây phân cách nên khó xác định | Câu hỏi cho coach: làn bên trái này nên là `road` hay `sidewalk`? Trong bài nộp tôi gán `sidewalk` |
