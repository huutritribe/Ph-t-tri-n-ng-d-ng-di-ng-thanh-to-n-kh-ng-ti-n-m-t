# README - 4 Ứng Dụng Thanh Toán Không Tiền Mặt Việt Nam

## 📱 Giới thiệu

Dự án này bao gồm 4 ứng dụng thanh toán di động (Mobile Payment App) cho 4 nền tảng thanh toán không tiền mặt lớn nhất Việt Nam:

### 1️⃣ **MoMo Pay** - Ứng dụng thanh toán MoMo
- **Mô tả**: Ứng dụng ví di động (Digital Wallet) phổ biến nhất Việt Nam
- **Tính năng chính**:
  - Chuyển tiền đến số điện thoại
  - Thanh toán hóa đơn (điện, nước, internet)
  - Kiểm tra số dư ví
  - Xem lịch sử giao dịch
- **Branding**: Màu tím (#A800F7) - Biểu tượng của MoMo
- **Gói ứng dụng**: `com.momo.paymentapp`

### 2️⃣ **ZaloPay** - Ứng dụng thanh toán ZaloPay
- **Mô tả**: Ứng dụng thanh toán qua Zalo với tích hợp mã QR
- **Tính năng chính**:
  - Chuyển tiền bằng Zalo ID
  - Thanh toán qua mã QR
  - Kiểm tra số dư
  - Lịch sử giao dịch
- **Branding**: Màu xanh (#0084FF) - Màu sắc chính thức
- **Gói ứng dụng**: `com.zalopay.paymentapp`

### 3️⃣ **Viettel Pay** - Ứng dụng thanh toán Viettel
- **Mô tả**: Hệ thống thanh toán của nhà mạng Viettel
- **Tính năng chính**:
  - Chuyển tiền chuyên nghiệp
  - Nạp bal ance vào tài khoản
  - Thanh toán hóa đơn
  - Kiểm tra số dư
- **Branding**: Màu đỏ (#DC143C) - Màu sắc Viettel
- **Gói ứng dụng**: `com.viettel.paymentapp`

### 4️⃣ **VCB Pay** - Ứng dụng thanh toán Vietcombank
- **Mô tả**: Ứng dụng thanh toán ngân hàng từ Vietcombank (VCB)
- **Tính năng chính**:
  - Chuyển tiền liên ngân hàng
  - Thanh toán hóa đơn (điện, nước, internet)
  - Kiểm tra số dư tài khoản
  - Xem lịch sử giao dịch chi tiết
- **Branding**: Màu xanh đậm (#005EBB) - Màu sắc Vietcombank
- **Gói ứng dụng**: `com.vcb.paymentapp`

---

## 🏗️ Cấu trúc dự án

```
Application Development/
├── MoMoPayApp/
│   ├── app/
│   │   ├── src/main/
│   │   │   ├── java/com/momo/paymentapp/
│   │   │   │   ├── MainActivity.java
│   │   │   │   ├── TransferActivity.java
│   │   │   │   └── HistoryActivity.java
│   │   │   └── res/
│   │   │       ├── layout/ (XML layouts)
│   │   │       ├── values/ (colors, strings, dimens)
│   │   │       └── drawable/ (icons & images)
│   │   └── build.gradle
│   └── AndroidManifest.xml
│
├── ZaloPayApp/
│   └── (Tương tự MoMoPayApp)
│
├── ViettelPayApp/
│   └── (Tương tự MoMoPayApp)
│
└── VCBPayApp/
    └── (Tương tự MoMoPayApp)
```

---

## 📋 Chi tiết các Activity trong mỗi ứng dụng

### **MainActivity** (Màn hình chính)
- Hiển thị logo ứng dụng
- Hiển thị số dư tài khoản / ví
- Nút chuyển tiền
- Nút tính năng khác (QR, nạp tiền, thanh toán)
- Nút kiểm tra số dư
- Nút xem lịch sử giao dịch

### **TransferActivity** (Gửi tiền)
- Nhập số điện thoại / ID / Số tài khoản
- Nhập số tiền cần chuyển
- Ghi chú / Mô tả
- Xác thực dữ liệu đầu vào
- Xử lý yêu cầu chuyển tiền (simulate)

### **HistoryActivity** (Lịch sử)
- Hiển thị danh sách giao dịch đã thực hiện
- Mỗi mục hiển thị: người nhận, số tiền, ngày giờ
- Dữ liệu mẫu để test giao diện

---

## 🛠️ Yêu cầu công nghệ
- **Ngôn ngữ**: Java
- **API tối thiểu**: Android 5.1 (API 21)
- **SDK mục tiêu**: Android 13 (API 33)
- **JDK**: 11+
- **Android Studio**: 4.2+

---

## 📦 Thư viện sử dụng
- `androidx.appcompat:appcompat:1.6.1`
- `com.google.android.material:material:1.9.0`
- `androidx.constraintlayout:constraintlayout:2.1.4`

---

## 🚀 Cách chạy ứng dụng

### 1. **Mở Android Studio**
   - File → Open → Chọn thư mục ứng dụng (ví dụ: MoMoPayApp)

### 2. **Đợi Gradle build**
   - Android Studio sẽ tự động build project

### 3. **Chạy ứng dụng**
   - Click Run → Select Device (Emulator hoặc Thiết bị thực)
   - Chọn tên ứng dụng → OK

### 4. **Test các tính năng**
   - Nhập thông tin chuyển tiền
   - Click các nút để kiểm tra logic
   - Toast messages sẽ hiển thị kết quả

---

## 💡 Tính năng nổi bật

✅ **Giao diện thân thiện** - UI/UX dễ sử dụng  
✅ **Validation** - Kiểm tra đầu vào dữ liệu  
✅ **Lịch sử giao dịch** - Quản lý lịch sử thanh toán  
✅ **Branding riêng** - Mỗi ứng dụng có màu sắc đặc trưng  
✅ **Tiếng Việt** - Giao diện hoàn toàn bằng Tiếng Việt  
✅ **Responsive Layout** - Thích ứng với nhiều kích thước màn hình  

---

## 📝 Ghi chú phát triển

### Các tính năng có thể mở rộng:
- Tích hợp API thực tế của các nền tảng
- Authentication/Login
- Biometric security (Fingerprint, Face ID)
- Push notifications
- Payment confirmation with OTP
- In-app card management
- Bill payment integration
- Banking integration

---

## 👨‍💻 Thiết kế Giao diện

Mỗi ứng dụng sử dụng:
- **LinearLayout** cho layout chính
- **EditText** cho nhập liệu
- **Button** cho hành động
- **ListView** cho danh sách
- **Standard Android components**

### Palettes:
- **MoMo**: Tím (#A800F7)
- **ZaloPay**: Xanh (#0084FF)
- **Viettel**: Đỏ (#DC143C)
- **VCB**: Xanh đậm (#005EBB)

---

## 📞 Hỗ trợ

Nếu có vấn đề khi biên dịch hoặc chạy ứng dụng:
1. Kiểm tra phiên bản Java (đảm bảo JDK 11+)
2. Đồng bộ Gradle files
3. Xóa thư mục `build` và build lại
4. Check manifest file có matching package name không

---

**Version**: 1.0  
**Ngày tạo**: 25/02/2026  
**Trạng thái**: Phát triển hoàn chỉnh 🎉
