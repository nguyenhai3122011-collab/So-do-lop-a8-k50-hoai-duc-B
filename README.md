# Sơ đồ lớp — GitHub Pages

Ứng dụng web tĩnh để quản lý chỗ ngồi trong lớp: kéo thả, đổi tên học sinh, tìm kiếm, xáo trộn, hoàn tác, in và xuất dữ liệu. Dự án không cần backend hay bước build, nên có thể đưa thẳng lên **GitHub Pages**.

## Điểm nổi bật

- **Tự đọc `latest.json`** (hoặc `lastest.json` nếu đổi tên lại) khi mở trang; đây là dữ liệu mẫu/bố cục gốc của lớp. Nếu không tải được file, ứng dụng hiển thị popup **“Không tải được sơ đồ, vui lòng tạo sơ đồ mới.”**
- Kéo thả trên máy tính; trên điện thoại có thể **chạm chỗ thứ nhất rồi chạm chỗ đích** để đổi chỗ.
- Giao diện co giãn theo màn hình. Với sơ đồ nhiều cột trên điện thoại, khu vực sơ đồ cuộn ngang để tên vẫn dễ đọc.
- Tự lưu bản đang làm vào `localStorage`. Nếu cập nhật `latest.json` và triển khai lại, ứng dụng nhận dữ liệu gốc mới. Muốn đưa sơ đồ mới lên repository: **Xuất JSON**, đổi tên thành `latest.json`, rồi commit/push file đó lên GitHub.
- **Xuất ảnh/PDF luôn đồng nhất trên mọi thiết bị**: ảnh được dựng trong khung cố định kiểu desktop nên tải về từ điện thoại hay máy tính đều cho kết quả giống hệt nhau, luôn nền trắng (kể cả khi đang dùng giao diện tối).
- **Chọn kiểu bố cục trước khi tải**: bấm *Ảnh PNG* hoặc *PDF A4* sẽ hiện hộp thoại tùy chọn — hiện/ẩn số thứ tự chỗ ngồi, nhãn lối đi (mặc định để trống), nhãn bàn giáo viên, nhãn cửa ra vào (để trống để ẩn khu đó) và dòng chú thích cuối sơ đồ. Lựa chọn được ghi nhớ cho lần sau.
- Chọn **giao diện sáng hoặc tối** tại nút góc phải trên cùng; lựa chọn được ghi nhớ trong trình duyệt.
- Thêm/sửa/xóa học sinh, tìm kiếm danh sách, thay đổi số hàng/cột/lối đi, điều chỉnh cỡ bàn ghế.
- Hoàn tác/làm lại (`Ctrl/Cmd + Z`, `Ctrl/Cmd + Y`), xuất JSON, CSV, PNG, PDF A4 ngang và in trực tiếp.

## Chạy trên máy

Do ứng dụng cần đọc file JSON, hãy chạy qua một web server thay vì mở trực tiếp `index.html`:

```bash
python3 -m http.server 8000
```

Sau đó truy cập: `http://localhost:8000`

## Đưa lên GitHub Pages

1. Tạo repository mới trên GitHub, ví dụ `so-do-lop`.
2. Đẩy toàn bộ các file trong thư mục này lên nhánh `main`.
3. Vào **Settings → Pages** của repository.
4. Trong **Build and deployment**, chọn **Deploy from a branch**.
5. Chọn nhánh `main`, thư mục `/(root)`, rồi bấm **Save**.
6. Sau ít phút, GitHub cung cấp đường dẫn dạng `https://<tai-khoan>.github.io/so-do-lop/`.

## Cập nhật dữ liệu lớp

Mở file [`latest.json`](./latest.json) và chỉnh các trường sau:

```json
{
  "title": "10A8",
  "rows": 6,
  "cols": 9,
  "aisleCols": [4],
  "seats": ["Tên học sinh", "", "…"]
}
```

- `aisleCols` đếm từ **0**: `[4]` nghĩa là cột thứ 5 là lối đi.
- `seats` được ghi theo thứ tự từ trái sang phải, từ trên xuống dưới; số phần tử nên bằng `rows × cols`.
- Ô thuộc lối đi luôn được để trống.
- Có thể dùng nút **Xuất JSON** sau khi chỉnh trên giao diện, sau đó đổi tên file tải về thành `latest.json` để dùng làm bản mẫu mới.

> Lưu ý: ứng dụng thử lần lượt `latest.json` rồi `lastest.json`, nên đặt tên file nào trong hai cái cũng được. Nếu đổi thành tên khác thì phải sửa lại đường dẫn trong `app.js`.

## Cấu trúc dự án

```text
├── index.html       # Giao diện ứng dụng
├── styles.css       # Thiết kế responsive + chế độ in
├── app.js           # Logic quản lý sơ đồ
├── latest.json      # Sơ đồ được nạp mặc định
└── README.md        # Hướng dẫn sử dụng & triển khai
```

## Ghi chú về xuất ảnh/PDF

Tính năng xuất PNG/PDF tải `html2canvas` và `jsPDF` từ CDN khi cần, vì vậy cần có Internet ở thời điểm xuất. Các tính năng quản lý sơ đồ, lưu cục bộ, JSON, CSV và in vẫn hoạt động mà không cần hai thư viện này.
