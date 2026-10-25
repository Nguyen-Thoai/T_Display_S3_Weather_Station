# 🚀 Hướng Dẫn Nạp Firmware ESP32-S3 Qua Arduino IDE

## 🛠️ Bước 1: Cài Đặt Hỗ Trợ Board ESP32 Trong Arduino IDE

Arduino IDE **chưa hỗ trợ sẵn** ESP32-S3. Bạn cần thêm gói board từ Espressif.

| Thao tác | Đường dẫn |
|----------|-----------|
| 🪟 Windows | `File → Preferences` |
| 🍎 macOS | `Arduino IDE → Settings` |

**Tại mục `Additional boards manager URLs`, dán link sau:**

```
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
```

> 💡 **Mẹo:** Nếu đã có link khác trong ô này, hãy ngăn cách bằng dấu phẩy `,` rồi mới dán link mới.

Nhấn **OK** để lưu lại. ✅

---

## 📦 Bước 2: Cài Đặt Gói Board ESP32

1. Vào **`Tools → Board → Boards Manager...`**
2. Gõ `esp32` vào ô tìm kiếm
3. Tìm gói **`esp32 by Espressif Systems`**
4. Nhấn **Install** và đợi vài phút ⏳

> ⚠️ **Lưu ý:** Nên cài phiên bản **(3.x)** để có hỗ trợ đầy đủ cho ESP32-S3. Nếu gặp lỗi biên dịch, có thể thử hạ xuống bản ổn định hơn.

---

## 🔌 Bước 3: Kết Nối Và Chọn Board, Port

### 🔗 Kết nối phần cứng
Cắm board ESP32-S3 vào máy tính bằng **cáp USB có chức năng truyền dữ liệu** (data cable).  
> ❌ Cáp sạc (charge-only) sẽ **không** nạp được code.

### 🎯 Chọn Board & Port
- **`Tools → Board → esp32`** → chọn đúng dòng board:
  - `ESP32S3 Dev Module` — phổ biến nhất
- **`Tools → Port`** → chọn cổng COM mà board đang kết nối

> 💡 **Mẹo nhận diện Port:** Rút cáp USB → cổng đó biến mất khỏi danh sách → cắm lại → cổng xuất hiện lại. Đó chính là cổng của board bạn.

---

## ⚙️ Bước 4: Cấu Hình Thông Số Quan Trọng

⚠️ **Đây là bước quan trọng nhất** — cấu hình sai sẽ dẫn đến nạp lỗi hoặc board không chạy. Kiểm tra kỹ các mục sau trong menu **`Tools`**:

| Thông số | Giá trị khuyến nghị | Ghi chú |
|----------|---------------------|---------|
| **USB CDC On Boot** | `Enabled` | Bật để board tự nhận lệnh nạp qua USB, không cần nhấn nút thủ công |
| **USB Mode** | `Hardware CDC and JTAG` | Chế độ ổn định nhất, dùng cổng USB native |
| **Flash Size** | Theo board (8MB / 16MB) | Xem datasheet board nếu không chắc |
| **PSRAM** | `OPI PSRAM` | Bật nếu board có PSRAM — **đừng để `Disabled`** |
| **Upload Speed** | `921600` | Nạp nhanh; nếu lỗi, hạ về `460800` hoặc `115200` |
| **Partition Scheme** | `Default 16MB` | Điều chỉnh nếu dùng nhiều bộ nhớ |

> 🔎 **Kiểm tra nhanh:** Nếu board là ESP32-S3 **N16R8** → Flash `16MB`, PSRAM `OPI PSRAM`.

---

## 🚀 Bước 5: Nạp Code (Upload)

1. 📂 Mở sketch ví dụ: **`File → Examples → 01.Basics → Blink`**
2. ✅ Nhấn **Verify** để kiểm tra cú pháp
3. ⬆️ Nhấn **Upload** để nạp code
4. 🎉 Chờ thông báo `Done uploading` — board sẽ tự chạy chương trình mới

> 💡 **Sau khi nạp:** Nếu board không chạy, thử nhấn nút **RESET (RST)** trên board một lần.

---

## 📚 Bước 6: Thêm Thư Viện Trong Library

Để sử dụng **Library** đi kèm, bạn cần thay thế các thư viện cần thiết

> 📦 **Thay thế thư viện cũ:** Nếu trong Library đã có thư viện trùng tên, hãy **xóa bản cũ** trước khi cài bản mới để tránh xung đột.

---

## 🔧 Xử Lý Sự Cố Thường Gặp

### ❌ Lỗi: `Failed to connect to ESP32-S3` / `No serial data received`
**Nguyên nhân:** Board không vào được chế độ nạp (download mode).

**Cách khắc phục — vào chế độ nạp thủ công:**
1. **Giữ nút `BOOT`** trên board
2. **Nhấn và nhả nút `RESET`** (vẫn giữ `BOOT`)
3. **Thả nút `BOOT`** ra
4. Nhấn **Upload** lại trên Arduino IDE

### ❌ Không tìm thấy cổng COM
- ✅ Kiểm tra cáp USB là **data cable** (không phải charge-only)
- ✅ Đảm bảo **USB CDC On Boot** đã đặt `Enabled`
- ✅ Thử cổng USB khác trên máy (tránh hub USB)
- ✅ Cài driver: `CP210x` hoặc `CH340` tùy board

### ❌ Nạp thành công nhưng board không chạy
- Nhấn nút **RESET (RST)** trên board
- Kiểm tra lại `PSRAM` và `Flash Size` có đúng với board không
- Mở **Serial Monitor** (baud `115200`) để xem log khởi động

### ❌ Lỗi biên dịch: `xxx.h: No such file or directory`
- Cài thiếu thư viện → quay lại **Bước 6**
- Hoặc chọn sai **Board** trong `Tools → Board`

---

<div align="center">

**Made by Thoai for the ESP32 community**

</div>