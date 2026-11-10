# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Ô tô con chủ yếu dùng chở người, thường có 4–7 chỗ, thân xe dạng sedan, hatchback, coupe hoặc SUV/crossover thông thường. | Sedan, hatchback, SUV gia đình; phân biệt với `van` ở kiểu thân xe và khoang chở người/hàng, với `bus` ở chức năng chuyên chở nhiều hành khách. |
| `truck` | Xe tải có kết cấu và mục đích chính là vận chuyển hàng hóa, thường có thùng hàng, sàn chở hàng hoặc đầu kéo. | Xe tải thùng, xe ben, xe bán tải có thùng hàng lộ rõ, đầu kéo; không gán xe tải thùng kín là `van` chỉ vì có khoang hàng kín. |
| `bus` | Xe được thiết kế chủ yếu để vận chuyển nhiều hành khách theo tuyến hoặc theo đoàn, gồm xe buýt, xe khách và xe đưa đón cỡ lớn. | Xe buýt đô thị, xe khách, xe đưa đón nhiều hàng ghế; nhận diện bằng thân xe dài, nhiều cửa sổ/hàng ghế và bố trí phục vụ hành khách. |
| `van` | Xe thân hộp, khoang cabin và khoang chở người/hàng liền khối, thường dùng chở hàng nhẹ hoặc nhóm hành khách nhỏ. | Minivan, xe van giao hàng, xe 9–16 chỗ dạng van; khác `truck` ở việc không có thùng tải tách biệt, khác `bus` ở kích thước/cấu trúc và công năng. |

## Quy tắc chung

- **Box bám sát vật thể nghĩa là:** Vẽ bounding box hình chữ nhật nhỏ nhất bao trọn phần phương tiện nhìn thấy được, không để dư nền quá nhiều; bao gồm các bộ phận thuộc xe còn nhìn thấy (thân, bánh, gương nếu rõ). Mỗi xe có một box riêng, không gộp nhiều xe vào một box. Không bao gồm bóng đổ hoặc vật thể khác.
- **Vật thể bị che khuất:** Nếu vẫn nhận diện được loại xe từ các đặc điểm còn nhìn thấy, gán nhãn và vẽ box theo phần nhìn thấy được; không tự suy đoán phần bị che để mở rộng box. Nếu không đủ thông tin xác định lớp, không gán nhãn và ghi lại để kiểm tra.
- **Vật thể bị cắt bởi rìa ảnh:** Vẫn gán nhãn nếu còn đủ dấu hiệu nhận diện; box chỉ nằm trong phạm vi ảnh và bám sát phần xe hiện ra, không kéo box ra ngoài ảnh.
- **Vật thể rất nhỏ/mơ hồ:** Chỉ gán nhãn khi có thể phân biệt đáng tin cậy một trong bốn lớp. Nếu xe quá nhỏ, nhòe hoặc không thể xác định `car`/`van`/`truck`/`bus`, bỏ qua và ghi nhận ca mơ hồ để self-QC; không đoán nhãn theo cảm tính.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_005` | Phương tiện màu xanh lá ở phía bên phải ảnh có kích thước tương đối nhỏ, phần thùng hàng phía sau không rõ ràng do góc chụp từ trên cao và chất lượng ảnh thấp. Vì vậy, dễ nhầm lẫn giữa truck và van. | Schema ưu tiên cấu trúc/chức năng kéo xe; xe bán tải móc kéo ở sau `truck`. |
| `drive_011` | Xe minivan thân hộp, chụp từ phía sau dễ bị nhầm là `car`. | Gán `van` nếu là xe thân hộp cỡ nhỏ, có phần thùng hàng nối liền phía sau. | Schema xác định `van` theo thân xe to và công năng chở nhiều hơn. |
| `drive_030` | Chiếc xe có kích thước nhỏ trong ảnh, bị mờ, một phần thân xe khó quan sát do góc chụp từ trên cao và có nhiều xe xung quanh. Vì vậy, phương tiện dễ bị nhầm lẫn giữa `van` và `car`. | Bỏ qua nếu không đủ đặc điểm nhận diện; nếu thấy rõ đặc trưng xe buýt hoặc thùng tải thì gán lớp tương ứng. | Schema xác định `van` vì thân xe có thùng hàng nối liền và có nhiều cửa sổ. |
