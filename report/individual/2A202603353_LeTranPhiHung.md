# Báo cáo Day 13 — Lê Trần Phi Hùng, 2A202603353

## 1. Kết luận trước

MSSV: **2A202603353**  
Task: `D13-2A202603353` (job 27)  
Hệ điều hành: Windows. Trình duyệt: Google Chrome. CVAT: 2.74.1 (đối chiếu mục About của máy chủ CVAT local). MSSV bạn ngồi cạnh: chưa có trong project.

10 frame: `s01f23`, `s01f59`, `s02f24`, `s02f40`, `s03f03`, `s03f43`, `s04f09`, `s04f45`, `s05f20`, `s05f37`.

Tổng cộng có 66 cuboid. `s03f43` có ít cuboid nhất, với 5 cuboid (4 Car và 1 Cyclist); chưa có ghi chép để kết luận vì sao. Phiếu `selfcheck` hiện gắn cờ 0/66. Các hộp xe gần có nhiều điểm để đối chiếu; đáy hộp thiếu điểm mặt đường và hướng xe ở xa còn khó xác nhận.

Cách thao tác theo hướng dẫn CVAT:

- Xoay khung 3D để nhìn từ trên xuống; kéo chuột phải để dời khung và dùng con lăn để phóng to, thu nhỏ.
- Chọn Shape, nháy đúp vào vật để đặt hộp; sau đó kiểm tra và chỉnh ở Top, Side, Front.
- Bật Cuboid orientation để xem hướng hộp; dùng `preview` để đối chiếu với ảnh camera.

## 2. Đọc frame (Bước 2)

Hai frame đọc kỹ. Ảnh: `t1-s01f23-3d.png`, `t1-s01f23-top.png`, `t1-s01f59-3d.png`, `t1-s01f59-top.png`. Ảnh Top có đánh dấu phía trước, +x và +y.

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| s01f23 | Phía trước xe về trái (−Y), +X xuống dưới, +Y về phải | Mặt đường cục bộ quanh Car #2 và Bus #3 ước khoảng 1,76–1,78 m dưới LiDAR, từ z đáy và khoảng hở. Chưa xác định độ dốc toàn giao lộ | Car #8 là cuboid đã gán xa nhất, cách 50,32 m theo phương ngang và có 39 điểm; khả năng nhận dạng còn hạn chế | Car, Bus | Bus #3 phía sau-trái, Car #4 phía sau-phải | Chưa có ghi chép xác định cụm cụ thể |
| s01f59 | Phía trước xe về trái (−Y), +X xuống dưới, +Y về phải | Ước tính mặt đường cục bộ thấp khoảng 1,80–1,86 m, từ z đáy và khoảng hở của các hộp. Chưa đo độ dốc | Car #6, 15,93 m, xa nhất trong các Car đã gán | Car | Car #4 phía sau, Car #1 sát bên trái | Chưa có ghi chép xác định cụm cụ thể |

Camera cho thấy màu thân xe, tín hiệu đèn giao thông và ký hiệu/vạch làn đường. LiDAR cho biết khoảng cách theo mét, phân bố điểm quanh xe thu dữ liệu kể cả phía sau và hai bên, cũng như hình dạng và cao độ trong không gian 3D. Các nhận xét này dựa trên cặp frame `s01f23` và `s01f59`.

Tám frame lướt. Hướng dưới đây dùng cùng góc nhìn CVAT; trục +Y màu xanh lá ở bên phải nên phía trước xe (−Y) ở bên trái.

| Frame | Phía trước ở cạnh nào của ảnh nhìn từ trên xuống | Điều lạ (nếu có) |
|---|---|---|
| s02f24 | Cạnh trái (−Y) | Chưa có ghi chép |
| s02f40 | Cạnh trái (−Y) | Chưa có ghi chép |
| s03f03 | Cạnh trái (−Y) | Chưa có ghi chép |
| s03f43 | Cạnh trái (−Y) | Chưa có ghi chép |
| s04f09 | Cạnh trái (−Y) | Chưa có ghi chép |
| s04f45 | Cạnh trái (−Y) | Chưa có ghi chép |
| s05f20 | Cạnh trái (−Y) | Cụm điểm rời dưới dải mặt đường; xem mục 4 |
| s05f37 | Cạnh trái (−Y) | Chưa có ghi chép |

## 3. Ba cuboid có hướng dẫn (Bước 3)

Ảnh: `t2-a-car.png`, `t2-b-bus-truck.png`, `t2-c-ped-cyclist.png`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | s01f23 #2 | Car | 269,9°, gần hướng −Y | Chạm theo tự kiểm (khoảng hở 0,00 m) | Ảnh CVAT có đủ ba khung và ảnh camera | 4,544 × 1,972 × 1,504 m; nếu là sedan/hatchback thì rộng hơn mốc 1,90 m |
| (b) xe buýt hoặc xe tải | s01f23 #3 | Bus | 269,6°, gần −Y; cần đối chiếu đầu xe | Đáy thấp hơn mặt đường cục bộ 0,04 m | Ảnh CVAT có đủ ba khung và ảnh camera | 12,602 × 3,082 × 3,524 m; nếu là bus thường thì cả ba chiều vượt mốc trên, rõ nhất ở chiều rộng |
| (c) người đi bộ hoặc xe hai bánh | s03f43 #5 | Cyclist | 271,8°, gần −Y; cần đối chiếu hướng di chuyển | Đáy cao hơn mặt đường cục bộ 0,04 m | Ảnh CVAT có ba khung; cần xác nhận hướng và subtype bằng camera phù hợp | 1,484 × 0,738 × 1,243 m; dài và cao dưới khoảng tham chiếu của xe đạp và xe máy có người |

Job cá nhân CVAT ghi các cuboid đã gán. Kết quả hiệu chuẩn, kết luận chung của lớp và danh sách vật đã bỏ qua cần đối chiếu với ghi chép của buổi thực hành.

## 4. Bốn hiện tượng chất lượng (Bước 4)

Nguyên nhân dưới đây là **giả thuyết**. Các ảnh đều lấy từ task cá nhân.

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | s01f23 / `t4-1-bong-che.png` / task cá nhân | Bus có thể chắn tia LiDAR | Ảnh đánh dấu vùng nghi có bóng che phía sau Bus #3 theo hướng tia từ LiDAR | Đánh dấu occluded khi có vật bị che và đủ căn cứ để đặt hộp |
| 2. Thưa ở xa | s01f23 / `t4-2-thua-o-xa.png` / task cá nhân | Khoảng cách giữa các tia tăng theo tầm | Car #8 ở 50,32 m có 39 điểm | Kiểm tra phần hình dạng nhìn thấy; nếu không đủ căn cứ thì dùng LOW_VISIBILITY |
| 3. Phản xạ thấp / ít điểm bất thường | s05f20 / `t4-3-phan-xa-thap.png` / task cá nhân | Góc tới, vật liệu hoặc che khuất có thể làm giảm số điểm | Car #3 ở 20,98 m có 157 điểm, Car #5 ở 18,82 m có 468 điểm | Đối chiếu camera và điểm nhìn thấy trước khi quyết định kích thước hộp |
| 4. Điểm lạ | s05f20 / `t4-4-diem-la.png` / task cá nhân | Có thể là điểm phản xạ bất thường | Ảnh góc nhìn ngang đánh dấu một cụm điểm rời bên dưới dải mặt đường; chưa đo độ cao của cụm trên ảnh | Không gán cụm điểm rời thành một vật |

## 5. Số từ cuboid của tôi (Bước 5)

Nguồn: `submission/D13-2A202603353-bangA.csv`; số điểm lấy từ cột `points_in` của `submission/D13-2A202603353-tukiem.csv`. Mặt đường tham chiếu khoảng −1,7 m; z đáy lệch hơn 0,3 m là lơ lửng hoặc chìm.

| Cuboid (frame, idx, class) | z | h | z đáy = z − h/2 | dist_m | r = √(x²+y²+z²) | yaw_deg | Nhận xét (lơ lửng / chìm / chạm đường) |
|---|---|---|---|---|---|---|---|---|
| s01f23 #2 Car | −1,032 m | 1,504 m | −1,784 m | 4,29 m | 4,41 m | 269,9° | Chạm đường theo số đo cục bộ |
| s01f23 #3 Bus | −0,036 m | 3,524 m | −1,798 m | 13,80 m | 13,80 m | 269,6° | Chạm gần mặt đường, thấp hơn 0,04 m |
| s03f43 #5 Cyclist | −1,113 m | 1,243 m | −1,7345 m | 17,81 m | 17,85 m | 271,8° | Sát mặt đường, cao hơn 0,04 m |

**(a) Gần nhất và xa nhất.** Gần nhất là `s01f59` #1 Car (`dist_m` 2,91 m, r 3,08 m). Xa nhất là `s01f23` #8 Car (`dist_m` 50,32 m, r 50,34 m). Tâm hộp #8 vượt giới hạn 50 m theo phương ngang trong hướng dẫn 0,32 m.

**(b) So sánh kích thước với bảng 4.3.** Car `s01f23` #2 đo 4,544 × 1,972 × 1,504 m. Nếu đối chiếu sedan/hatchback, chiều rộng lớn hơn mốc 1,90 m, còn dài và cao trong khoảng tham chiếu. Cyclist `s03f43` #5 đo 1,484 × 0,738 × 1,243 m. So với xe máy có người, dài và cao thấp hơn khoảng, còn rộng nằm trong khoảng. So với xe đạp có người, dài và cao vẫn thấp hơn khoảng, rộng lớn hơn mốc 0,70 m. Cần kiểm subtype và mép hộp trước khi kết luận nguyên nhân.

**(c) Hướng xe.** `s01f23` #2 có y âm và yaw 269,9°, gần 270°. `s01f23` #8 có yaw 90,8°, gần 90° và hộp chỉ theo hướng ngược lại. Đầu xe #8 còn cần xác nhận bằng ảnh camera.

Mật độ theo khoảng cách (4 cuboid ở các `dist_m` khác nhau):

| Cuboid (frame, idx, class) | dist_m | Số điểm (`points_in`) |
|---|---|---|
| s01f23 #2 Car | 4,29 m | 3084 |
| s01f23 #6 Car | 19,36 m | 449 |
| s02f40 #2 Car | 23,48 m | 230 |
| s01f23 #8 Car | 50,32 m | 39 |

Từ 4,29 m đến 23,48 m, khoảng cách tăng 5,47 lần và số điểm còn khoảng 7,5%. Ba dải đầu có 3084, 449 và 230 điểm, cùng xu hướng giảm với các mốc 1968, 718 và 214 điểm trên slide.

Đối chiếu phiếu tự kiểm bản trước trong Git với bản hiện tại cho thấy đã chỉnh đáy `s02f24` #6 để khoảng hở giảm từ 0,44 m xuống 0,16 m, và chỉnh đáy `s03f43` #5 để giảm từ 0,36 m xuống 0,04 m. Phiếu hiện có 0 cờ. Các ô `ground_gap_m` trống ở `s04f09` #5, `s04f45` #5 và `s05f37` #5–#7 là những trường hợp thiếu số đo mặt đường.

Đổi kiểm: MSSV người kiểm chéo, hai số được đối chiếu và kết quả kiểm chéo cần lấy từ ghi chép của buổi thực hành.

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc

**(a) Ý nghĩa của nhãn.** Nhãn 3D đo bằng mét để model học kích thước và vị trí thực của vật. Vật thưa điểm cần quy tắc riêng vì một phần hình dạng có thể chưa quan sát được, khiến vị trí và kích thước hộp khó xác định.

**(b) Cách dùng AI.**

- Công cụ: Codex.
- Câu hỏi chính: cách thao tác CVAT trên Windows, dùng PowerShell và đóng gói ảnh.
- Cách dùng và giới hạn: AI được dùng để hỗ trợ thao tác kỹ thuật. Trong bản nháp trước, Codex cũng đã xem ảnh CVAT, đối chiếu số liệu và gợi ý nhận xét. RULES.md giới hạn AI ở phần hướng dẫn thao tác và giải thích khái niệm, đồng thời yêu cầu học viên tự đọc bảng A, điền số và viết nhận xét. Các phần từng được AI hỗ trợ cần được mình kiểm tra và viết lại trước khi nộp.

**(c) Điều còn chưa chắc.** Mình còn chưa chắc về hướng đầu xe ở xa và vị trí đáy của những hộp thiếu điểm mặt đường. Hiện chưa có ghi chép về lỗi phát hiện sau hạn.
