# Telegram Shop Bot - Hướng Dẫn Chi Tiết

Bot Telegram bán tài khoản tự động với hệ thống thanh toán tích hợp SEPAY.

## 📋 Mục Lục
- [Giới thiệu](#giới-thiệu)
- [Yêu cầu hệ thống](#yêu-cầu-hệ-thống)
- [Hướng dẫn cài đặt từ đầu](#hướng-dẫn-cài-đặt-từ-đầu)
- [Cấu hình Bot](#cấu-hình-bot)
- [Cách chạy Bot](#cách-chạy-bot)
- [Hướng dẫn sử dụng cho Admin](#hướng-dẫn-sử-dụng-cho-admin)
- [Hướng dẫn sử dụng cho Khách hàng](#hướng-dẫn-sử-dụng-cho-khách-hàng)
- [Xử lý lỗi thường gặp](#xử-lý-lỗi-thường-gặp)

---

## 🎯 Giới thiệu

Bot này là một hệ thống bán hàng tự động trên Telegram, được thiết kế để:
- ✅ Bán tài khoản tự động (Netflix, Spotify, ChatGPT, v.v.)
- ✅ Tích hợp thanh toán qua ngân hàng (SEPAY API)
- ✅ Tự động giao hàng khi khách thanh toán
- ✅ Quản lý kho hàng, đơn hàng, thống kê doanh thu

---

## 💻 Yêu cầu hệ thống

### 1. **Node.js** (phiên bản 16 trở lên)

**Kiểm tra xem đã cài chưa:**
```bash
node --version
```

**Nếu chưa có, tải và cài đặt:**
- Truy cập: https://nodejs.org/
- Tải phiên bản **LTS** (khuyến nghị)
- Chạy file cài đặt và làm theo hướng dẫn
- Khởi động lại máy tính sau khi cài

### 2. **Git** (tùy chọn - để tải code)

**Kiểm tra:**
```bash
git --version
```

**Nếu chưa có:**
- Truy cập: https://git-scm.com/
- Tải và cài đặt
- Hoặc có thể tải code dưới dạng ZIP

### 3. **Trình soạn thảo văn bản**
- **Notepad++**: https://notepad-plus-plus.org/
- **Visual Studio Code**: https://code.visualstudio.com/ (khuyến nghị)

## Hướng dẫn cài đặt từ đầu

### Bước 1: Tải mã nguồn

**Cách 1: Dùng Git**
```bash
git clone <link-repository>
cd telegram-shop-bot
```

**Cách 2: Tải ZIP**
1. Tải file ZIP về máy
2. Giải nén vào thư mục bất kỳ
3. Mở Command Prompt (CMD) hoặc PowerShell
4. Gõ `cd` rồi kéo thả thư mục vào cửa sổ CMD

### Bước 2: Cài đặt thư viện

Mở CMD/PowerShell tại thư mục dự án và chạy:

```bash
npm install
```

**Giải thích:** Lệnh này sẽ tự động tải và cài đặt tất cả thư viện cần thiết:
- `node-telegram-bot-api`: Thư viện kết nối với Telegram
- `sql.js`: Thư viện quản lý cơ sở dữ liệu
- `dotenv`: Thư viện quản lý cấu hình bảo mật

**Chờ đợi:** Quá trình này mất khoảng 1-3 phút tùy tốc độ mạng.

---

## Cấu hình Bot

### Bước 1: Tạo Bot trên Telegram

1. **Mở Telegram và tìm @BotFather**
   - Gõ `@BotFather` vào thanh tìm kiếm
   - Nhấn vào bot có dấu tick xanh

2. **Tạo Bot mới**
   - Gửi lệnh: `/newbot`
   - Bot Father sẽ hỏi tên hiển thị: Gõ tên bot của bạn (ví dụ: "Shop Tài Khoản VIP")
   - Tiếp theo nhập username bot (phải kết thúc bằng `bot`): Ví dụ `shopvip_bot`

3. **Lưu Token**
   - BotFather sẽ gửi cho bạn một **Token** dài như này:
     ```
     1234567890:ABCdefGHIjklMNOpqrsTUVwxyz123456789
     ```
   - **QUAN TRỌNG:** Sao chép token này, không chia sẻ với ai!

### Bước 2: Lấy User ID của bạn

1. Tìm bot **@userinfobot** trên Telegram
2. Gửi bất kỳ tin nhắn nào
3. Bot sẽ trả về thông tin, tìm dòng `Id:` (ví dụ: `123456789`)
4. Sao chép số ID này

### Bước 3: Đăng ký SEPAY (Thanh toán tự động)

1. **Truy cập:** https://my.sepay.vn/
2. **Đăng ký tài khoản** với email và số điện thoại
3. **Xác thực tài khoản** qua email
4. **Liên kết ngân hàng:**
   - Vào phần "Ngân hàng"
   - Chọn "Thêm ngân hàng"
   - Nhập thông tin tài khoản ngân hàng của bạn
   - Xác thực theo hướng dẫn
5. **Lấy API Key:**
   - Vào "Cài đặt" > "API Key"
   - Tạo API Key mới
   - Sao chép API Key 

### Bước 4: Tạo file cấu hình `.env`

1. **Mở thư mục dự án**
2. **Tạo file mới tên `.env`** (chú ý dấu chấm ở đầu)
   - Trên Windows: Mở Notepad++, tạo file mới, Save As, đặt tên `.env`, chọn "All Files" ở phần Save as type

3. **Dán nội dung sau vào file `.env`:**

```env
# ============================================
# CẤU HÌNH BOT TELEGRAM
# ============================================

# Token bot lấy từ @BotFather (BẮT BUỘC)
BOT_TOKEN=1234567890:ABCdefGHIjklMNOpqrsTUVwxyz123456789

# ID Telegram của admin (có thể nhiều admin, cách nhau bởi dấu phẩy)
# Lấy ID từ @userinfobot (BẮT BUỘC)
ADMIN_IDS=123456789,987654321

# Username Telegram của admin chính (không có @)
# Ví dụ: nếu @shopowner thì chỉ ghi: shopowner
ADMIN_USER_NAME=shopowner

# Tên shop hiển thị trong bot
SHOP_NAME=Shop Tài Khoản VIP

# ============================================
# CẤU HÌNH THANH TOÁN SEPAY
# ============================================

# API Key từ SEPAY (https://my.sepay.vn/)
SEPAY_API_KEY=xxxxxxxxx....

# Số tài khoản ngân hàng nhận tiền
BANK_ACCOUNT=1234567890

# Tên ngân hàng (ví dụ: Vietcombank, Techcombank, MB Bank)
BANK_NAME=Vietcombank

# Tên chủ tài khoản (IN HOA, không dấu)
BANK_OWNER=NGUYEN VAN A

# Mã BIN ngân hàng (Lấy từ SEPAY sau khi liên kết)
# VCB=970436, TCB=970407, MB=970422, TP=970423, Agribank=970405
BANK_BIN=970436
```

4. **Thay thế các giá trị:**
   - Thay `1234567890:ABC...` bằng Token bot thật của bạn
   - Thay `123456789` bằng User ID thật của bạn
   - Thay `shopowner` bằng username Telegram của bạn
   - Thay các thông tin SEPAY và ngân hàng thành thông tin thật
   - Đổi tên shop theo ý bạn

5. **Lưu file** và đóng lại

**LƯU Ý:**
- Không chia sẻ file `.env` với ai (chứa thông tin bảo mật)
- Không upload file `.env` lên GitHub hay bất kỳ đâu
- Kiểm tra kỹ không có khoảng trắng thừa trong các giá trị

---

## Cách chạy Bot

### Cách 1: Dùng file start.bat (Đơn giản - Windows)

1. **Double-click** vào file `start.bat`
2. Cửa sổ CMD sẽ mở ra và bot tự động chạy
3. Khi thấy dòng `✅ Bot đã khởi động!` là thành công

### Cách 2: Dùng lệnh CMD/PowerShell

1. Mở CMD/PowerShell tại thư mục dự án
2. Chạy lệnh:
```bash
npm start
```

### Cách 3: Chạy ở chế độ phát triển (tự động restart khi có thay đổi)

```bash
npm run dev
```

### Kiểm tra Bot hoạt động

1. Mở Telegram
2. Tìm bot của bạn (theo username đã tạo)
3. Gửi lệnh `/start`
4. Nếu bot trả lời là **thành công** ✅

### Dừng Bot

- Nhấn **Ctrl + C** trong cửa sổ CMD
- Hoặc đóng cửa sổ CMD

## 👨‍💼 Hướng dẫn sử dụng cho Admin

Bot sử dụng giao diện menu button thay vì lệnh text, rất dễ sử dụng!

### 📌 Các lệnh Admin cơ bản

| Lệnh | Mô tả |
|------|-------|
| `/products` | ⚙️ Quản lý sản phẩm (thêm/sửa/xóa) |
| `/orders` | 📦 Xem đơn hàng gần đây |
| `/revenue` | 💰 Xem tổng doanh thu |
| `/stats` | 📊 Xem tồn kho |
| `/users` | 👥 Danh sách người dùng |
| `/broadcast` | 📣 Gửi thông báo tới tất cả user |
| `/clear` | 🗑️ Xóa tin nhắn (50 tin gần nhất) |
| `/myid` | 🔖 Xem User ID của bạn |

---

### 1️⃣ Quản lý sản phẩm (`/products`)

**Gửi lệnh:**
```
/products
```

Bot sẽ hiển thị **menu quản lý** với danh sách tất cả sản phẩm dạng button.

#### 🆕 Thêm sản phẩm mới

1. Nhấn nút **"➕ Thêm sản phẩm mới"**
2. Bot sẽ yêu cầu nhập thông tin theo format:
   ```
   Tên sản phẩm|Giá|Mô tả
   ```
3. **Ví dụ:** Gửi tin nhắn:
   ```
   Netflix Premium 1 Tháng|50000|Tài khoản Netflix Premium chất lượng cao, bảo hành 1 tháng
   ```
4. Bot sẽ tự động tạo sản phẩm và hiển thị menu chi tiết

**Lưu ý:**
- Phân cách bằng dấu `|` (gạch dọc)
- Giá phải là số nguyên (VNĐ)
- Mô tả có thể để trống hoặc viết chi tiết

---

#### ✏️ Sửa sản phẩm

1. Gửi `/products` → Chọn sản phẩm muốn sửa
2. Bot hiển thị menu chi tiết với các nút:
   - **✏️ Sửa tên**: Thay đổi tên sản phẩm
   - **💵 Sửa giá**: Thay đổi giá bán
   - **📝 Sửa mô tả**: Thay đổi mô tả
3. Nhấn nút tương ứng, nhập giá trị mới
4. Bot tự động cập nhật và hiển thị kết quả

---

#### 📦 Thêm stock (tài khoản vào kho)

1. Trong menu chi tiết sản phẩm, nhấn **"➕ Thêm stock"**
2. Bot yêu cầu gửi danh sách tài khoản
3. Gửi tin nhắn với format: **mỗi dòng 1 tài khoản**

**Ví dụ:**
```
netflix1@gmail.com|Pass123456
netflix2@gmail.com|Pass789012
netflix3@gmail.com|Pass345678
user1:password1
user2:password2
```

**Lưu ý:**
- Mỗi tài khoản một dòng
- Có thể là bất kỳ định dạng nào (email|pass, user:pass, v.v.)
- Bot sẽ lưu nguyên văn như bạn nhập
- Có thể paste hàng trăm tài khoản cùng lúc

Bot sẽ báo: `🎯 Đã thêm X tài khoản!`

---

#### 👁️ Xem stock hiện có

1. Trong menu chi tiết sản phẩm, nhấn **"👁️ Xem stock"**
2. Bot hiển thị:
   - Số lượng còn trong kho
   - Số lượng đã bán
   - Danh sách 10 tài khoản đầu tiên
3. Có thể:
   - **Xóa từng tài khoản**: Nhấn nút 🗑️ tương ứng
   - **Xóa tất cả stock**: Nhấn "🗑️ Xóa TẤT CẢ stock"

---

#### 🗑️ Xóa sản phẩm

1. Trong menu chi tiết sản phẩm, nhấn **"🗑️ Xóa sản phẩm"**
2. Bot yêu cầu xác nhận
3. Nhấn **"🗑️ Xóa luôn"** để xác nhận

**⚠️ Cảnh báo:** Xóa sản phẩm sẽ xóa luôn tất cả stock và không thể hoàn tác!

---

### 2️⃣ Xem đơn hàng (`/orders`)

**Gửi lệnh:**
```
/orders
```

Bot hiển thị **20 đơn hàng gần nhất** với:
- ✅ **Hoàn thành**: Đơn đã thanh toán và giao hàng
- ⏳ **Chờ thanh toán**: Đơn đang chờ khách chuyển khoản
- ❌ **Đã hủy/Hết hạn**: Đơn bị hủy hoặc quá 20 phút

Thông tin hiển thị:
- Mã đơn hàng
- Tên khách hàng
- Sản phẩm và số lượng
- Tổng tiền
- Thời gian tạo đơn

---

### 3️⃣ Xem doanh thu (`/revenue`)

**Gửi lệnh:**
```
/revenue
```

Bot hiển thị:
- 💵 **Tổng thu**: Tổng doanh thu từ tất cả đơn hoàn thành
- ✅ **Đơn hoàn thành**: Số lượng đơn đã giao hàng thành công
- 📦 **Sản phẩm**: Tổng số sản phẩm trong shop
- 🎯 **Tồn kho**: Tổng số tài khoản còn trong kho

---

### 4️⃣ Xem tồn kho (`/stats`)

**Gửi lệnh:**
```
/stats
```

Bot liệt kê **từng sản phẩm** với:
- ✅ Còn hàng: Hiển thị số lượng còn
- 🔴 Hết hàng: Hiển thị 0
- 📦 Tổng: Tổng số tài khoản còn trong tất cả sản phẩm

**Dùng để:** Kiểm tra nhanh xem sản phẩm nào cần bổ sung stock

---

### 5️⃣ Xem danh sách users (`/users`)

**Gửi lệnh:**
```
/users
```

Bot hiển thị:
- 📊 Tổng số người dùng
- Danh sách 50 user đầu tiên với:
  - Tên người dùng
  - User ID

**Dùng để:** Xem có bao nhiêu người đã dùng bot

---

### 6️⃣ Gửi thông báo (`/broadcast`)

**Cách 1: Gửi lệnh trống**
```
/broadcast
```
Bot sẽ yêu cầu nhập nội dung thông báo. Sau đó gửi nội dung.

**Cách 2: Gửi kèm nội dung luôn**
```
/broadcast Khuyến mãi 50% tất cả sản phẩm Netflix đến 5/2! 🎉
```

Bot sẽ:
1. Gửi tin nhắn cho **tất cả user** đã từng dùng bot
2. Báo cáo kết quả: Số user nhận được / Số user thất bại

**Tính năng:**
- Hỗ trợ văn bản, emoji
- Gửi tuần tự từng user
- Tự động bỏ qua user đã block bot

---

### 7️⃣ Xóa tin nhắn (`/clear`)

**Gửi lệnh:**
```
/clear
```

Bot sẽ xóa **50 tin nhắn gần nhất** trong chat hiện tại.

**Dùng để:** Dọn dẹp chat khi test bot hoặc có quá nhiều tin nhắn spam

---

### 💡 Tips cho Admin

1. **Quản lý nhanh:** Dùng `/products` để thao tác mọi thứ qua menu button, không cần nhớ lệnh
2. **Kiểm tra stock thường xuyên:** Dùng `/stats` để biết sản phẩm nào cần bổ sung
3. **Theo dõi đơn hàng:** Dùng `/orders` để xem đơn hàng mới
4. **Thông báo khuyến mãi:** Dùng `/broadcast` để tăng doanh số
5. **Backup dữ liệu:** Sao lưu file `data/shop.db` định kỳ để tránh mất dữ liệu

---

## 🛍️ Hướng dẫn sử dụng cho Khách hàng

Bot sử dụng giao diện menu button rất dễ sử dụng, chỉ cần nhấn và chọn!

### 📱 Bắt đầu sử dụng

**Bước 1:** Tìm bot trên Telegram (theo username đã tạo)

**Bước 2:** Gửi lệnh:
```
/start
```

Bot sẽ hiển thị:
- Lời chào mừng
- **Menu chính** với danh sách sản phẩm dạng button
- Mỗi button hiển thị: 🎁 Tên | Giá | 📦 Số lượng còn

---

### 🛒 Cách mua hàng (Chi tiết từng bước)

#### Bước 1: Chọn sản phẩm

Trong menu chính, **nhấn vào button sản phẩm** muốn mua.

Ví dụ: Nhấn `🎁 Netflix Premium ┃ 50,000 VND ┃ 📦5`

#### Bước 2: Chọn số lượng

Bot hiển thị menu chọn số lượng với:
- Các nút số lượng nhanh: `『1』` `『2』` `『3』` `『5』` `『10』`
- Nếu stock > 10: Có thêm nút `『MAX:xx』`
- Nếu muốn số lượng khác: Nhấn **"📝 Nhập số lượng khác"**

**Ví dụ:** Muốn mua 2 sản phẩm → Nhấn nút `『2』`

#### Bước 3: Thanh toán

Bot sẽ gửi:
- **Mã QR thanh toán** (quét là chuyển khoản tự động)
- **Thông tin chuyển khoản thủ công:**
  ```
  💳 THANH TOÁN ĐƠN #123
  ━━━━━━━━━━━━━━━━━━━━━
  
  🎁 Netflix Premium x2
  💰 Tổng: 100,000 VND
  
  🏦 THÔNG TIN CHUYỂN KHOẢN
  • NH: Vietcombank
  • STK: 1234567890
  • Chủ TK: NGUYEN VAN A
  • Nội dung: ABC123XYZ
  
  📲 Quét QR để thanh toán
  ⏳ Tự động xác nhận khi nhận tiền
  ⚠️ Đơn hết hạn sau 20 phút
  ```

**Các nút:**
- **🔄 Kiểm tra thanh toán**: Nhấn nếu đã chuyển khoản nhưng chưa nhận hàng
- **❌ Hủy đơn**: Hủy đơn hàng và quay lại menu

#### Bước 4: Chuyển khoản

**Cách 1: Quét QR (Khuyến nghị)**
1. Mở app ngân hàng
2. Chọn "Chuyển khoản" → "Quét QR"
3. Quét mã QR trong tin nhắn bot
4. Xác nhận chuyển khoản

**Cách 2: Chuyển khoản thủ công**
1. Mở app ngân hàng
2. Nhập thông tin:
   - Ngân hàng: Theo bot
   - Số tài khoản: Theo bot
   - Số tiền: **ĐÚNG SỐ TIỀN** bot yêu cầu
   - Nội dung: **SAO CHÉP CHÍNH XÁC** (ví dụ: `ABC123XYZ`)
3. Xác nhận chuyển khoản

**⚠️ LƯU Ý QUAN TRỌNG:**
- ✅ Chuyển khoản **ĐÚNG SỐ TIỀN** (không làm tròn, không thêm bớt)
- ✅ Ghi nội dung **CHÍNH XÁC** như bot cung cấp (KHÔNG sửa đổi, thêm chữ)
- ✅ Không ghi thêm: "mua tài khoản", "Netflix", v.v.
- ❌ Nếu sai nội dung → Bot không nhận diện được → Không tự động giao hàng

#### Bước 5: Nhận tài khoản

**Tự động (Khuyến nghị):**
- Sau khi chuyển khoản thành công
- Chờ **5-30 giây**
- Bot tự động kiểm tra giao dịch từ SEPAY
- Nếu khớp → Bot gửi tài khoản ngay lập tức

**Tin nhắn nhận được:**
```
✅ THANH TOÁN THÀNH CÔNG!
━━━━━━━━━━━━━━━━━━━━━

🎁 Netflix Premium x2

🔑 TÀI KHOẢN:
  1. netflix1@gmail.com|Pass123456
  2. netflix2@gmail.com|Pass789012

⚠️ Đổi mật khẩu ngay!
⛄ Cảm ơn bạn đã mua hàng!
🛒 Mua thêm? Gõ /menu
```

**Thủ công (Nếu chưa tự động):**
- Nhấn nút **"🔄 Kiểm tra thanh toán"**
- Bot sẽ kiểm tra lại giao dịch
- Nếu đã chuyển khoản đúng → Nhận tài khoản

**Nếu vẫn không nhận được:**
- Chờ thêm 1-2 phút (có thể ngân hàng chậm)
- Nhấn "🔄 Kiểm tra thanh toán" lại
- Liên hệ Admin (xem phần dưới)

---

### 👤 Xem hồ sơ cá nhân

Trong menu chính, nhấn nút **"👤 Hồ sơ"**

Bot hiển thị:
- 🆔 User ID của bạn
- ✨ Tên hiển thị
- 📧 Username Telegram
- 🛍️ Số đơn hoàn thành
- 💰 Tổng số tiền đã chi tiêu

---

### 📋 Xem lịch sử mua hàng

Trong menu chính, nhấn nút **"📋 Lịch sử"**

Bot hiển thị **10 đơn hàng gần nhất** với:
- ✅ **Thành công**: Đơn đã thanh toán và nhận hàng
- ⏳ **Chờ TT**: Đơn đang chờ thanh toán
- ⌛ **Hết hạn**: Đơn quá 20 phút chưa thanh toán
- ❌ **Đã hủy**: Đơn đã bị hủy

Mỗi đơn hiển thị:
- Mã đơn hàng (ví dụ: #123)
- Sản phẩm và số lượng
- Tổng tiền

**Lưu ý:** Bot chỉ lưu thông tin đơn hàng, không lưu lại tài khoản đã giao. Vui lòng **lưu tài khoản ngay khi nhận**!

---

### 💬 Liên hệ Admin

Nếu admin đã cấu hình username trong file `.env`:
- Trong menu chính sẽ có nút **"💬 Liên hệ Admin"**
- Nhấn vào sẽ mở chat với admin

Nếu không có nút:
- Liên hệ trực tiếp qua username Telegram của admin

---

### 🔄 Xem lại danh sách sản phẩm

Gửi lệnh:
```
/menu
```

Bot sẽ hiển thị lại menu sản phẩm.

---

### 💡 Tips cho khách hàng

1. **Lưu tài khoản ngay:** Bot chỉ gửi 1 lần, không gửi lại
2. **Chuyển khoản đúng nội dung:** Đảm bảo sao chép chính xác
3. **Đừng spam "Kiểm tra thanh toán":** Chờ 30 giây rồi nhấn 1 lần
4. **Đổi mật khẩu ngay:** Sau khi nhận tài khoản, đổi pass để bảo mật
5. **Không share tài khoản:** Mỗi tài khoản chỉ dùng cho 1 người
6. **Liên hệ admin nếu có vấn đề:** Đừng ngại hỏi nếu cần hỗ trợ

---

## 🔧 Xử lý lỗi thường gặp

### Lỗi 1: "Cannot find module 'xxx'"

**Nguyên nhân:** Chưa cài đặt thư viện

**Cách khắc phục:**
```bash
npm install
```

---

### Lỗi 2: "BOT_TOKEN is required"

**Nguyên nhân:** Chưa tạo file `.env` hoặc chưa điền BOT_TOKEN

**Cách khắc phục:**
1. Kiểm tra file `.env` có tồn tại không
2. Kiểm tra đã điền `BOT_TOKEN=...` chưa
3. Kiểm tra token có đúng format không (dài khoảng 45 ký tự)

---

### Lỗi 3: "ETELEGRAM: 401 Unauthorized"

**Nguyên nhân:** Token bot không đúng

**Cách khắc phục:**
1. Tạo lại bot mới trên @BotFather
2. Lấy token mới
3. Thay token mới vào file `.env`
4. Khởi động lại bot

---

### Lỗi 4: Bot không tự động giao hàng sau khi chuyển khoản

**Nguyên nhân:**
- Chưa cấu hình SEPAY đúng
- API Key không hợp lệ
- Chưa liên kết ngân hàng với SEPAY

**Cách khắc phục:**
1. Đăng nhập SEPAY: https://my.sepay.vn/
2. Kiểm tra ngân hàng đã liên kết chưa
3. Kiểm tra API Key còn hoạt động không
4. Kiểm tra BIN ngân hàng có đúng không
5. Thử chuyển khoản test nhỏ để kiểm tra

---

### Lỗi 5: "Port 3000 is already in use"

**Nguyên nhân:** Bot đang chạy rồi, bạn chạy lần 2

**Cách khắc phục:**
1. Tắt cửa sổ CMD đang chạy bot cũ
2. Hoặc mở Task Manager, tìm `node.exe` và End Task
3. Chạy lại bot

---

### Lỗi 6: Khách chuyển khoản nhưng không nhận được hàng

**Kiểm tra:**
1. Xem log trong CMD, có hiển thị giao dịch không?
2. Kiểm tra nội dung chuyển khoản có đúng không?
3. Kiểm tra số tiền có khớp không?
4. Dùng lệnh `/orders` để xem đơn hàng có được tạo không

**Cách xử lý thủ công:**
1. Admin dùng lệnh `/orders` xem danh sách đơn
2. Tìm đơn hàng của khách
3. Kiểm tra trạng thái
4. Nếu cần, gửi tài khoản thủ công cho khách

---

## 📞 Hỗ trợ

Nếu gặp vấn đề không giải quyết được:

1. Kiểm tra lại tất cả các bước cấu hình
2. Đọc kỹ thông báo lỗi trong CMD
3. Google thông báo lỗi để tìm giải pháp
4. Liên hệ developer qua GitHub Issues

---

## 📝 Ghi chú

- File cơ sở dữ liệu: `data/shop.db` (tự động tạo)
- Backup định kỳ file `shop.db` để tránh mất dữ liệu
- Nên chạy bot trên VPS/Server để hoạt động 24/7
- Có thể dùng PM2 để chạy bot tự động khởi động lại khi có lỗi

---

## 🌟 Tính năng nâng cao

### Chạy bot 24/7 trên VPS

**Bước 1:** Thuê VPS (Vultr, DigitalOcean, AWS...)

**Bước 2:** Cài đặt Node.js trên VPS
```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

**Bước 3:** Upload code lên VPS
```bash
git clone <repository-url>
cd telegram-shop-bot
npm install
```

**Bước 4:** Cài đặt PM2 (Process Manager)
```bash
npm install -g pm2
```

**Bước 5:** Chạy bot với PM2
```bash
pm2 start src/bot.js --name "shop-bot"
pm2 save
pm2 startup
```

Bot sẽ tự động khởi động lại khi VPS restart!

---

## ❓ FAQ - Câu hỏi thường gặp

### 1. Bot có miễn phí không?
Có, mã nguồn miễn phí. Chi phí phát sinh:
- VPS nếu muốn chạy 24/7 (từ $5/tháng)
- SEPAY miễn phí cho giao dịch nhỏ

### 2. Cần biết lập trình không?
Không cần! Chỉ cần làm theo hướng dẫn này.

### 3. Có thể bán sản phẩm vật lý không?
Có, nhưng phải tự xác nhận đơn hàng và giao hàng thủ công.

### 4. Có giới hạn số lượng sản phẩm không?
Không giới hạn.

### 5. Có thể thêm nhiều admin không?
Có, thêm User ID vào `ADMIN_IDS` cách nhau bởi dấu phẩy:
```env
ADMIN_IDS=123456789,987654321,555555555
```

### 6. Bot có hỗ trợ thanh toán khác ngoài SEPAY không?
Hiện tại chỉ hỗ trợ SEPAY. Có thể tùy chỉnh code để thêm gateway khác.

---

**🎉 Chúc bạn thành công với shop của mình!**
