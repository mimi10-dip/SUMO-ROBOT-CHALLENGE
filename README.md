# SUMO-ROBOT-CHALLENGE

## Cấu trúc thư mục

```
├── firmware/       
│   ├── src/        
│   ├── include/    
│   └── lib/        
├── hardware/       
│   ├── schematic/
│   └── wiring/
├── mechanical/       
│   ├── cad/            
├── docs/            
│   ├── report/
│   ├── images/
│   └── videos/
├── README.md
└── LICENSE
```

## Tính năng chính

* **Autonomous Mode:** Hệ thống tự động quyết định hành vi thời gian thực mà không cần sự can thiệp từ xa.
* **FSM:** Thuật toán firmware điều khiển phân cấp rõ ràng 4 trạng thái: Khởi động trễ (5s) $\rightarrow$ Tìm kiếm xoay tròn $\rightarrow$ Tấn công tốc độ cao $\rightarrow$ Dò biên thoát hiểm.
* **Anti-Fall Edge Detection:** Kích hoạt phản xạ lùi và chuyển hướng khẩn cấp trong vài mili-giây ($ms$) khi cụm cảm biến hồng ngoại phát hiện vạch biên Dohyo trắng.
* **Định vị đối thủ bằng Radar Siêu âm:** Quét và khóa mục tiêu di động trong phạm vi $< 60\text{ cm}$ bằng cảm biến HC-SR04.
* **Open-Chassis:** Khung mica phẳng tối giản giúp linh hoạt phân bổ lại trọng tâm cơ khí (CG) hướng trước, tăng áp lực pháp tuyến lên cặp lốp cao su nhằm tối đa hóa lực ma sát đẩy đối thủ.
* **Đóng gói điện năng an toàn:** Hệ nguồn Pin Li-ion 2S ($7.4\text{V} - 8.4\text{V}$) được thiết kế đóng gói cách điện toàn phần, xả dòng lớn trực tiếp nuôi mạch động lực cầu H L298N mà không gây sụt áp vi điều khiển.

---

## Thông Số Kỹ Thuật Thực Tế (Specifications)
| Thông số | Quy định giải đấu | Kết quả thực tế của Robot | Trạng thái |
| :--- | :---: | :---: | :---: |
| **Kích thước chân đế** | $\le 12 \times 12\text{ cm}$ | $12 \times 12\text{ cm}$ | Đạt chuẩn |
| **Tổng khối lượng** | $\le 700\text{ g}$ | $685\text{ g}$ | Đạt chuẩn |
| **Điện áp đầu vào** | $\le 8.4\text{ V}$ | $7.4\text{ V}$ (Danh định) / $8.4\text{ V}$ (Sạc đầy) | Đạt chuẩn |
| **Khởi động trễ** | Đúng $5\text{ giây}$ | Cấu hình cứng qua hàm `delay(5000)` | Đạt chuẩn |
| **Vỏ bảo vệ cực pin** | Bắt buộc | Đóng gói bọc cách điện toàn phần | Đạt chuẩn |

---

## Danh mục linh kiện 
Hệ thống phần cứng của Robot Sumo được cấu trúc và tối ưu hóa từ các linh kiện tiêu chuẩn nhằm đáp ứng các ràng buộc về dòng xả hiệu suất cao và phản xạ thời gian thực:

| STT | Thiết bị / Linh kiện | Số lượng | Mô tả chức năng & Thông số kỹ thuật |
| :---: | :--- | :---: | :--- |
| **1** | **Arduino Uno R3** | 01 | Khối xử lý trung tâm (Brain); chịu trách nhiệm thu thập dữ liệu cảm biến và thực thi thuật toán Máy trạng thái (FSM). |
| **2** | **Mô-đun cầu H L298N** | 01 | Mạch điều khiển động cơ; chịu dòng tải cao, tích hợp tản nhiệt nhôm để xử lý dòng ghì kẹt (Stall Current) khi va chạm. |
| **3** | **Động cơ DC giảm tốc (TT Motor)** | 02 | Động cơ giảm tốc vỏ vàng cốt nhựa, dải điện áp $6\text{V} - 9\text{V}$, tốc độ $120 - 200\text{ RPM}$ cung cấp mô-men xoắn lớn. |
| **4** | **Bánh xe cao su** | 02 | Bánh xe tương thích trục chữ I của TT Motor, lốp cao su mềm tăng ma sát bám đường trên sàn đấu Dohyo. |
| **5** | **Cảm biến siêu âm HC-SR04** | 01 | Cảm biến đo khoảng cách bằng sóng siêu âm; đặt ở mặt trước để quét radar tìm kiếm đối thủ trong phạm vi $< 60\text{ cm}$. |
| **8** | **Pin sạc Li-ion 18650** | 01 Cặp | Cấu hình nguồn nối tiếp (2S), điện áp danh định $7.4\text{ V}$ (Max $8.4\text{ V}$), dòng xả ổn định nuôi mạch động lực. |
| **9** | **Đế giữ pin Li-ion 2S** | 01 | Hộp đựng pin chuyên dụng; được gia cố màng bọc bảo vệ cách điện và quấn băng keo cách điện toàn phần để che cực trần. |
| **10** | **Dây nguồn chuyển đổi Jack DC** | 01 | Dây cáp chuyển đổi từ nguồn đầu ra của pin để cấp vào Jack nguồn tròn DC hoặc chân `VIN` ổn áp của Arduino. |
| **11** | **Dây cáp kết nối (Jumpers)** | 20 | Hệ thống dây bus nối mạch (Đực-Đực / Cái-Cái / Đực-Cái) để truyền dẫn tín hiệu logic và nguồn nuôi. |
| **12** | **Khung gầm Mica phẳng** | 01 | Tấm nền nhựa Mica cách điện được chế tạo thủ công, cấu trúc gầm mở (Open-chassis) giúp linh hoạt tối ưu hóa trọng tâm. |

---


## Thành viên 

Tên 
-Hoàng Thị Ngọc Ánh 
-Trần Thị Ngọc Diệp 


## License
Xem file [LICENSE](LICENSE).
