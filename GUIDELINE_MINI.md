# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
<<<<<<< HEAD
| `car` | Phương tiện giao thông chở người, thường có 4 bánh, thiết kế thân xe kín, kích thước nhỏ hoặc trung bình| Sedan, SUV, hatchback, coupe... |
=======
| `car` | Là phương tiện giao thông chở người, thường có 4 bánh, thiết kế thân xe kín, kích thước nhỏ hoặc trung bình| Sedan, SUV, hatchback, coupe... |
>>>>>>> 7a57c91 (update guideline mini, labels train)
| `truck` | Phương tiện vận tải hàng hóa có khoang chở hàng riêng biệt hoặc thùng hàng phía sau cabin| Xe tải thùng, xe tải ben, xe đầu kéo|
| `bus` | Phương tiện vận tải hành khách có kích thước lớn, khoang hành khách rộng và nhiều cửa sổ| Xe buýt nội thành, xe khách liên tỉnh, xe buýt mini|
| `van` | Phương tiện có thân xe dạng hộp, khoang phía sau cabin được thiết kế để chở người hoặc hàng hóa, thường nhỏ hơn xe buýt và xe tải| Minivan, passenger van, cargo van...|

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Sát với biên ngoài của phương tiện, Không lấy quá nhiều vùng nền, Không cắt mất những bộ phận nhìn thấy được của phương tiện, Không bao gồm bóng đổ hoặc vùng đường xung quanh, Mỗi phương tiện là một bounding box riêng biệt
- Vật thể bị che khuất: Nếu phương tiện bị che khuất một phần bởi phương tiện khác hoặc vật cản, vẫn tiến hành gán nhãn nếu nhận diện được loại phương tiện, Bounding Box chỉ bao phủ phần phương tiện thực sự nhìn thấy được, Không dự đoán hoặc vẽ thêm những phần bị che khuất, Nếu không đủ thông tin để xác định loại phương tiện, không tự ý đoán class.
- Vật thể bị cắt bởi rìa ảnh: Nếu phương tiện nằm một phần ngoài khung hình, vẫn gán nhãn nếu phần nhìn thấy đủ để nhận diện, Bounding Box giới hạn tại biên ảnh, Không mở rộng Bounding Box ra ngoài phạm vi hình ảnh.
- Vật thể rất nhỏ/mơ hồ: Chỉ gán nhãn khi có thể xác định phương tiện và class với mức độ tin cậy hợp lý, Nếu vật thể quá nhỏ, không thể phân biệt được loại phương tiện, không tự ý gán class, Không gán nhãn cho các vật thể chỉ có hình dạng tương tự phương tiện nhưng không đủ bằng chứng, Ưu tiên tính nhất quán thay vì phỏng đoán.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_008` | Xe tải đang chở hàng hóa cồng kềnh, kích thước hàng hóa nhô ra ngoài phần thùng xe| Gán nhãn truck và vẽ Bounding Box bao phủ toàn bộ xe tải cùng phần hàng hóa đang được vận chuyển| Xác định đối tượng cần nhận diện là phương tiện vận tải thực tế khi đang tham gia giao thông. Hàng hóa gắn liền với hoạt động vận chuyển được tính vào phạm vi Bounding Box để đảm bảo nhất quán trong các trường hợp tương tự|
| `drive_024` | Xe car nằm sát mép ảnh, một phần thân xe nằm ngoài khung hình| Gán car, box giới hạn trong phạm vi ảnh| Tuân thủ quy tắc truncation|
| `drive_028` | Một phương tiện ở xa, kích thước nhỏ, khó phân biệt giữa car và van| Không gán nếu không đủ thông tin xác định class| Tuân thủ quy tắc small/ambiguous objects|
