# Báo cáo Day 13 — NguyenManhDuc, 2A202603153

> **Cách dùng mẫu này:** đổi tên file thành `<MSSV>_<HoTen>.md` (họ tên không dấu, viết liền), ví dụ `2A202600123_NguyenVanAn.md`. Đổi tên, đừng chép thành file thứ hai.
>
> Xoá mọi dấu TODO và dòng gợi ý (dòng bắt đầu bằng `>`). Mỗi con số lấy từ **bảng A** hoặc **phiếu tự kiểm** trong `submission/`, kèm đơn vị (m, °). Ảnh để trong `report/individual/images/` (nộp LMS trong file zip, không commit); trong báo cáo chỉ ghi **tên file ảnh**. Báo cáo này nằm trên repo public: không chép biển số, chữ trên biển hiệu hay biển chỉ đường, tên đường, địa danh; nhắc tới bạn khác thì ghi MSSV, không ghi họ tên. Cả báo cáo ~2 trang.

## 1. Kết luận trước
- D13-2A202603153, s01f17  sequence_01, s01f47, s02f22, s02f53, s03f04, s03f51, s04f06, s04f31, s05f19, s05f36 

- Win11, CVAT 2.74.1



- "Tôi đã vẽ 65 cuboid trên 10 frame."

- "Frame ít nhất là s01f47, s02f53, s03f51 với 5 hộp, vì những xe còn lại khó quan sát." 

- "`selfcheck` cờ 1/65 cuboid; tôi tin nhất ở L, yếu nhất ở W


## 2. Đọc frame (Bước 2)
Hai frame đọc kỹ. Ảnh: `t1-<frame>-3d`, `t1-<frame>-top` (ảnh nhìn từ trên xuống, có đánh dấu phía trước, +x, +y).

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| s01f17 | Phía trước ở cạnh trên; +x sang trái, +y xuống dưới | Phẳng, thấp hơn LiDAR ~1,7 m | Car 40m | Nhiều car cùng chiều và 1 xe bus bên trái | Phía sau là car, bên trái là car và bus | Có lẽ là car |
| s01f47 | Phía trước ở cạnh trên; +x sang trái, +y xuống dưới | Phẳng, thấp hơn LiDAR ~1,7 m | Car 20m | Nhiều car cùng chiều |  Phía sau là car, bên trái là car | Có lẽ là car |

Chỉ camera cho biết: (1) màu sơn và kiểu dáng xe để phân loại class — ví dụ `[xe buýt màu đỏ bên trái ở s01f17]`; (2) chữ, biển báo, đèn tín hiệu, đèn xi-nhan — những thứ không có hình dạng 3D để LiDAR nhận ra; (3) tư thế người: đang ngồi trên xe hay đang dắt bộ, có hành khách hay không — ví dụ `[người ngồi trên xe máy ở s01f47]`.


Chỉ LiDAR cho biết: (1) khoảng cách chính xác tới vật theo mét (ảnh không có chiều sâu); (2) kích thước thật dài × rộng × cao của vật, không phụ thuộc khoảng cách hay góc nhìn; (3) những vật nằm ngoài khung của cả 7 camera (góc mù giữa các ống kính) hoặc phía sau xe — ví dụ [xe con phía sau-phải chỉ thấy trên point cloud, không có trong cam01]; ngoài ra LiDAR còn cho độ dốc và cao độ mặt đường so với cảm biến.

Tám frame lướt:

| Frame | Phía trước ở cạnh nào của ảnh nhìn từ trên xuống | Điều lạ (nếu có) |
|---|---|---|
| s02f22 | -y | |
| s02f53 | -y | |
| s03f04 | -y | |
| s03f51 | -y | |
| s04f06 | -y | |
| s04f31 | -y | |
| s05f19 | -y | |
| s05f36 | -y | |

## 3. Ba cuboid có hướng dẫn (Bước 3)
Ảnh: `t2-a-car`, `t2-b-bus-truck`, `t2-c-ped-cyclist`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | s01f17, 1 | Car | Có | Có | Có | -1.016,4.283,1.828 |
| (b) xe buýt hoặc xe tải | s01f17, 6 | Bus | Có | Có | Có | 9.589,2.655,3.078 |
| (c) người đi bộ hoặc xe hai bánh | s02f22, 7 | Có | Có | Có | Có | 0.567,0.570,1.507 |

- Vòng hiệu chuẩn không được tổ chức

## 4. Bốn hiện tượng chất lượng (Bước 4)
Nguyên nhân ghi là **giả thuyết**. Frame mượn thì ghi rõ nguồn: task chung, hoặc MSSV của bạn cho mượn.

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | s02f22 | Cây che xe máy | Vẫn thấy đủ trên point cloude | Không |
| 2. Thưa ở xa | s01f17 | chùm tia LiDAR loe rộng theo khoảng cách nên mật độ điểm giảm mạnh ở 30–50 m | Không thấy rõ | Bỏ qua |
| 3. Phản xạ thấp / ít điểm bất thường | s05f19 | Khoảng cách | Thấy 1 vài điểm | Vẫn gán |
| 4. Điểm lạ | s0f51 | có lẽ là người lái xe máy bị hàng rào che | chỉ thấy trên point cloud | không gán |

## 5. Số từ cuboid của tôi (Bước 5)


| Cuboid (frame, idx, class) | z | h | z đáy = z − h/2 | dist_m | r = √(x²+y²+z²) | yaw_deg | Nhận xét (lơ lửng / chìm / chạm đường) |
|---|---|---|---|---|---|---|---|
| s04f06 #3 Car | −0,813 | 1,774 | −1,700 | 3,66 | 3,74 | 270,5 | Chạm đường (lệch 0,00 m so với −1,7 m) |
| s01f17 #6 Bus | −0,248 | 3,078 | −1,787 | 28,56 | 28,56 | 272,3 | Chạm đường (lệch −0,09 m)
| s01f17 #5 Car | −0,446 | 1,449 | −1,170 | 47,16 | 47,16 | 268,1 | Lơ lửng 0,53 m — vật ở 47 m, điểm quá thưa nên đáy ước sai |

## (a) Gần nhất và xa nhất

> Cuboid __gần nhất__ là `s04f06 #3` (Car): `dist_m = 3,66 m`, `r = √(x²+y²+z²) = 3,74 m` (r lớn hơn dist_m vì tâm hộp nằm thấp hơn LiDAR ~0,8 m). Cuboid __xa nhất__ trong vùng được chấm là `s01f17 #5` (Car): `dist_m = 47,16 m`, `r = 47,16 m` — gần bằng `dist_m` vì `z` chỉ −0,45 m nên đóng góp của z không đáng kể. Hai hộp `s01f17 #7` (68,71 m) và `#8` (77,22 m) vượt 50 m nên không được chấm, tôi không tính vào đây.

## (b) So kích thước với bảng 4.3

> Người đi bộ `s02f22 #7` (Pedestrian): `l × w × h = 0,57 × 0,57 × 1,51 m`, nằm trong khoảng người lớn của bảng 4.3 (0,4–0,9 / 0,5–0,9 / 1,50–1,95), chỉ `h` sát mép dưới vì người ở xa ~19 m nên phần đầu chỉ còn vài điểm. Xe con `s01f17 #1` (Car): `4,28 × 1,83 × 1,50 m`, nằm trong khoảng sedan/hatchback (3,8–5,0 / 1,70–1,90 / 1,40–1,60), không chiều nào ngoài khoảng. Cả hai hộp đều được kiểm ở Top/Side/Front và đối chiếu ảnh camera.

## (c) yaw cùng chiều / ngược chiều

> Xe cùng chiều phía trước `s04f31 #1` (Car, 8,26 m) có `yaw_deg = 270,5°` — lệch 0,5° so với 270°, đúng hướng (vạch đỏ ngắn chỉ về phía trước xe, trùng hướng −y). Xe ngược chiều `s05f36 #6` (Car, 9,53 m) có `yaw_deg = 93,2°` — lệch 3,2° so với 90°, đúng hướng ngược chiều. Cả hai đều đối chiếu ảnh camera để xác định đầu xe trước khi xoay yaw.


Mật độ theo khoảng cách (4 cuboid ở các `dist_m` khác nhau):

| Cuboid (frame, idx, class) | dist_m | Số điểm (`points_in`) |
|---|---|---|
| s04f06 #3 Car | 3,66 | 6813 |
| s05f19 #3 Car | 10,55 | 1442 |
| s02f53 #1 Car | 20,04 | 282 |
| s05f36 #4 Car | 43,24 | 59 |

Xa gấp khoảng __2,9 lần__ (3,66 m → 10,55 m) thì số điểm còn khoảng __1/5__ (6813 → 1442); tới 20,04 m còn 282 điểm (__~1/24__ so với mốc gần nhất); tới 43,24 m chỉ còn 59 điểm (__~1/115__). Như vậy cứ mỗi 10 m số điểm giảm còn khoảng 1/4–1/5, tức giảm gần theo bình phương khoảng cách. So với đường cong ở slide (xe con: ~1968 điểm ở 0–10 m, 718 ở 10–20 m, 214 ở 20–30 m, 72 ở 30–40 m, 19 ở 60–80 m), xu hướng của tôi __cùng dạng__ (giảm mạnh rồi chững lại) nhưng __cao hơn__ ở các bin gần

- `s02f22`: hai hộp `#3` (Pedestrian) và `#4` (Cyclist) trùng nhau trên cùng một người đang lái xe → __xoá hộp Pedestrian__, giữ một hộp __Cyclist__ bao cả người và xe, nâng chiều cao từ 0,92 m lên 1,44 m (hết cả 3 cờ: `height ngoài khoảng`, `chồng lên hộp`, `quá chật`).

- `s04f31`: cùng lỗi — gộp `#6` (Cyclist) và `#7` (Pedestrian) thành __một hộp Cyclist__ 2,05 × 0,79 × 1,70 m, xoá hộp còn lại.

- `s01f47` (Car): nâng đáy từ __−2,26 m__ lên __−1,75 m__ (trước đó chìm 0,47 m).

- `s02f53` (Car): hạ đáy từ __−0,90 m__ xuống __−1,75 m__ (trước đó lơ lửng 1,05 m).

- `s05f36` (Car): hạ đáy từ __−1,32 m__ xuống __−1,76 m__ và thu chiều rộng từ __2,50 m__ về __2,30 m__. Ngoài ra `s05f19 #7` được đổi class từ __Car__ sang __Truck__ vì trên ảnh camera là xe bán tải/van chở hàng, chiều rộng thu từ 2,50 m về 2,10 m.

- Giữ nguyên 1 cảnh báo: s03f51 #5 (Car) — đáy thấp hơn mặt đường 0,48 m. Tôi giữ vì khi kiểm ở Side thấy bánh xe chạm mặt đường; đoạn đường quanh hộp dốc lên nên công cụ ước mặt đường cao hơn thực tế, hạ hộp theo cảnh báo sẽ làm hộp chìm vào đường.



## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc

(a)
1. Vì sao nhãn 3D đo bằng mét, không phải pixel

Model phải dự đoán vị trí và kích thước của vật trong không gian thật để ra quyết định (khoảng cách phanh, khoảng trống để chuyển làn), nên nhãn cần ở đơn vị mét trong hệ toạ độ LiDAR: mỗi vật có đúng một nhãn duy nhất (x, y, z, l, w, h, yaw) không phụ thuộc ảnh chụp từ camera nào. Nếu dùng pixel, cùng một chiếc xe ở gần và ở xa sẽ cho hai nhãn khác nhau, và model không thể học được kích thước hay khoảng cách thật.

2. Vì sao vật thưa điểm cần quy tắc riêng

Nếu bỏ qua mọi vật thưa điểm, model chỉ học được vật ở gần và mất khả năng phát hiện vật ở xa — đúng vùng cần phát hiện sớm nhất khi lái; ngược lại nếu vẽ bừa từ vài điểm rác thì nhãn sai sẽ dạy model kích thước và vị trí sai. Quy tắc "chỉ vẽ khi thấy khoảng 15–20% thể tích, còn lại ghi LOW_VISIBILITY" giữ cho nhãn vừa đủ tin cậy vừa không bị thiên lệch theo khoảng cách.

(b)
1. Cline, cách tăng độ sáng trong CVAT, sửa lỗi PowerShell, soát cấu trúc báo cáo

(c)

1. Không có