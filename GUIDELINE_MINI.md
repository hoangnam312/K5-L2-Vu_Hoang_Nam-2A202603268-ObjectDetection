# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | là xe ô tô bình thường | xe nhỏ, không chia khoang |
| `truck` | là xe ô tô chở hàng, thường chia thành hai phần: khoang lái, và khoang thùng chở hàng | có khoang thùng không mui phía sau |
| `bus` | xe to, dài, chở khác | to, dài hơn hẳn các xe khác |
| `van` | là xe ô tô chở hàng | dài hơn xe car, không chia khoang |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: box không được khoanh thừa, ngoài phạm vi vật thể
- Vật thể bị che khuất: vật thể hiển thị không đầy đủ, bị che khuất bởi một vật thể khác
- Vật thể bị cắt bởi rìa ảnh: vật thể hiển thị không đầy đủ ro không nằm trọn vẹn trên khung hình, thường nằm ở rìa ảnh
- Vật thể rất nhỏ/mơ hồ: vật thể ở xa, không nhìn rõ, xác định được loại, phạm vi

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001` | chiếc xe ô tô ở làn trái đi sát dải phân cách, dải phân cách là một hàng cây cao khiến cho xe bị che mất nửa dưới | chỉ khoanh phần xe nhìn thấy được | Vật thể bị che khuất |
| `drive_003` | hàng xe bên trái sếp dài từ giữa ảnh đến cuối ảnh, các xe càng về xa nhỏ dần và bị che bởi các xe phía trước, chỉ thấy mỗi chỏm đầu xe | chỉ khoang các xe ở gần, các, xe thấy rõ | Vật thể rất nhỏ/mơ hồ,Vật thể bị cắt bởi rìa ảnh, Vật thể bị che khuất |
| `drive_017` | hàng xe đỗ trên vỉa hè, không tham gia vào giao thông| không khoang vùng | vẫn gán nhãn |
