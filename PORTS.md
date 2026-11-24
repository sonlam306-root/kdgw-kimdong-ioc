# KimDong IOC – gateway phòng học — bảng cổng KD-GW

Phần cứng: **KD-GW-HW-v1.2** (không sửa). Dự án: `kdioc`. Trạng thái profile: **đã duyệt (24/11/2026)**.

| Cổng | Loại | Nhãn trên vỏ | Khu | Thiết bị | Nguồn | Ghi chú |
|---|---|---|---|---|---|---|
| AI1 | analog | ĐỘ ỒN | Môi trường lớp | Cảm biến âm thanh MAX4466 (độ ồn lớp) | 3V3 | Đặt giữa lớp, xa loa |
| AI2 | analog | DÒNG | Điện năng | Kẹp dòng SCT-013-030 (dòng tổng phòng) + mạch bias 1.65V | 3V3 | Dự phòng khi chưa lắp đồng hồ RS485 |
| AI3 | analog | GAS | An ninh – an toàn | MQ-2 (khói / gas) | 5V (rail) + chia áp 10k/20k | Ngõ ra qua cầu chia áp trước khi vào AI3 |
| AI4 | analog | DP | — | Trống | — |  |
| AI5 | analog | DP | — | Trống | — |  |
| AI6 | analog | DP | — | Trống | — |  |
| OW | 1wire | T°TỦ | Thiết bị điện | DS18B20 trong tủ điện / tủ mạng phòng | 3V3 | Cảnh báo tủ quá nhiệt |
| LOAD | hx711 | DP | — | Trống | — |  |
| DI1 | digital_in | 1 | An ninh – an toàn | PIR đầu lớp (có người) | 5V | PIR ngõ ra NPN/relay |
| DI2 | digital_in | 2 | An ninh – an toàn | PIR cuối lớp | 5V | Kết hợp DI1 để tự tắt đèn quạt khi phòng trống |
| DI3 | digital_in | 3 | An ninh – an toàn | Công tắc từ cửa chính | — | Tiếp điểm khô |
| DI4 | digital_in | 4 | An ninh – an toàn | Công tắc từ cửa sau / cửa sổ | — |  |
| DI5 | digital_in | 5 | An ninh – an toàn | Đầu báo khói (ngõ relay khô) | 12V riêng | Báo về IOC + bật còi |
| DI6 | digital_in | 6 | An ninh – an toàn | Nút gọi khẩn cấp (bàn giáo viên) | — | Nút nấm có khoá |
| DI7 | digital_in | 7 | Thiết bị điện | Công tắc chế độ TAY / TỰ ĐỘNG | — | Ở chế độ TAY gateway không tự tắt thiết bị |
| DI8 | digital_in | 8 | Thiết bị điện | Tiếp điểm phụ contactor đèn (xác nhận đã đóng) | — | Phát hiện contactor kẹt |
| DI-F | pulse_in | DP | — | Trống (có thể đếm người ra vào bằng cảm biến hồng ngoại) | — |  |
| RS485 | modbus_rtu | ĐIỆN NĂNG | Điện năng | Bus Modbus RTU: đồng hồ điện PZEM-016 (địa chỉ 1) + mặt công tắc cảm ứng KD-Touch (địa chỉ 2, xem repo kdgw-touch-panel) | nguồn riêng 220V | 9600 baud, cáp mạng: cặp A/B + 12V/GND cấp cho mặt cảm ứng; điện trở cuối đường 120Ω ở thiết bị xa nhất |
| PWR1 | pwm_12v | QUẠT | Môi trường lớp | Quạt hút 12V (chạy theo CO2) | 12V PWM |  |
| PWR2 | pwm_12v | DP | — | Trống | — |  |
| SERVO | servo | DP | — | Trống | — |  |
| I2C | i2c | I2C | Môi trường lớp | Hub I2C: SCD40 CO2 + nhiệt + ẩm (0x62), BH1750 ánh sáng (0x23), OLED 0.96" ở cửa (0x3C) | 3V3 | Dây I2C ≤ 1.5 m |
| RLY1 | relay | ĐÈN DÃY 1 | Thiết bị điện | Contactor đèn dãy cửa sổ | cuộn 220V | Relay COM–NO đóng cuộn hút contactor |
| RLY2 | relay | ĐÈN DÃY 2 | Thiết bị điện | Contactor đèn dãy giữa | cuộn 220V |  |
| RLY3 | relay | ĐÈN BẢNG | Thiết bị điện | Contactor đèn bảng | cuộn 220V |  |
| RLY4 | relay | QUẠT 1-2 | Thiết bị điện | Contactor quạt trần 1–2 | cuộn 220V |  |
| RLY5 | relay | QUẠT 3-4 | Thiết bị điện | Contactor quạt trần 3–4 | cuộn 220V |  |
| RLY6 | relay | MÁY CHIẾU | Thiết bị điện | Ổ cấp nguồn máy chiếu / tivi | cuộn 220V | Tắt sau giờ học |
| RLY7 | relay | Ổ CẮM GV | Thiết bị điện | Ổ cắm bàn giáo viên | cuộn 220V |  |
| RLY8 | relay | CÒI | An ninh – an toàn | Còi + đèn báo 12V | 12V | Khói, gọi khẩn |

Cổng chưa dùng: không có

## Mạng, camera, nguồn

- **LAN:** Switch tầng → server IOC (Pi 5 / VPS) → dashboard kdioc.ecosmartkd.click; mỗi phòng 1 gateway, mã phòng đặt trong cấu hình (VD: A-1.03)
- **công tắc:** Mặt công tắc cảm ứng ở cửa lớp (dự án kdgw-touch-panel) nối RS485 cùng bus đồng hồ điện; chạm → gateway đảo relay đèn/quạt, LED trên mặt hiện trạng thái thật. Gateway hỏng → dùng công tắc TAY–0–TỰ ĐỘNG trong tủ điện. Đã chốt lắp công tắc TAY–0–TỰ ĐỘNG cho từng contactor.
- **camera:** Camera IP của phòng đẩy RTSP thẳng về server IOC, không qua gateway; gateway chỉ gửi sự kiện (có người, cửa mở, khói) để dashboard mở đúng camera
- **power:** Adapter 12V 5A đặt trong tủ điện phòng. Đèn, quạt, máy chiếu 220V đóng cắt qua relay → khởi động từ (contactor) có CB riêng; relay chỉ đóng cuộn hút contactor
