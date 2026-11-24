# KimDong IOC — gateway phòng học

- Phần cứng: **KD-GW-HW-v1.2**, mỗi phòng học 1 gateway
- Profile: `profile.json` — **NHÁP, chờ thầy duyệt** (thiết bị đề xuất theo phòng học điển hình)
- Khu: Môi trường lớp · An ninh – an toàn · Thiết bị điện · Điện năng
- Dashboard: KimDong IOC Center (kdioc.ecosmartkd.click)
- Tải 220V (đèn, quạt trần, máy chiếu, ổ cắm) đóng cắt qua contactor; relay KD-GW chỉ đóng cuộn hút
- Camera IP đẩy RTSP thẳng về server IOC; gateway gửi sự kiện để dashboard mở đúng camera

## Lấy mã nguồn

Repo này dùng phần cứng chung qua submodule `kdgw-hardware/` (ghim đúng bản KD-GW-HW-v1.2, vỏ hộp r2):

```bash
git clone --recurse-submodules <url>/kdgw-kimdong-ioc.git
cd kdgw-kimdong-ioc
```

## Sinh lại vỏ hộp và tài liệu sau khi sửa `profile.json`

```bash
python3 kdgw-hardware/platform/make_project.py .          # kiểm tra nhãn + xuất STL vào enclosure/stl/
python3 kdgw-hardware/platform/make_project.py . --no-stl # chỉ kiểm tra nhãn (nhanh)
python3 kdgw-hardware/platform/make_ports_pdf.py .        # PDF bảng cổng
python3 kdgw-hardware/platform/draw_walls.py .            # ảnh nhãn các vách
python3 kdgw-hardware/platform/draw_lids.py .             # ảnh nắp chữ nổi
python3 kdgw-hardware/platform/render_preview.py enclosure/enclosure.scad enclosure/xem_truoc "Tên dự án"
```

Cần: Python 3, OpenSCAD, Pillow, reportlab, cairosvg (xvfb-run để chụp ảnh 3D trên máy không có màn hình).

Cập nhật lên bản phần cứng mới (khi có): `cd kdgw-hardware && git fetch --tags && git checkout <tag mới>`, rồi đổi `hardware` trong `profile.json` và sinh lại.
