# iOS Payment Apps - Nền Tảng Apple

## Tổng Quan
Các ứng dụng thanh toán đã được tạo cho nền tảng iOS (Apple) bằng Swift với SwiftUI. Cùng với phiên bản Android, các ứng dụng giờ có sẵn trên 2 nền tảng chính: **Google Play (Android)** và **Apple App Store (iOS)**.

---

## Danh Sách Ứng Dụng iOS

### 1. **MoMoPayApp (iOS)**
- **Thư mục**: `MoMoPayApp-iOS/`
- **File chính**: `MoMoPayApp.swift`
- **Ngôn ngữ**: Swift 5.5+
- **Framework**: SwiftUI
- **Màu sắc**: Purple (#A800F7)
- **iOS tối thiểu**: iOS 14.0+

**Tính năng:**
- Đăng nhập with MFA (Password + OTP)
- Chuyển tiền
- Thanh toán hóa đơn
- Xem lịch sử giao dịch
- Đăng xuất

---

### 2. **ZaloPayApp (iOS)**
- **Thư mục**: `ZaloPayApp-iOS/`
- **File chính**: `ZaloPayApp.swift`
- **Ngôn ngữ**: Swift 5.5+
- **Framework**: SwiftUI
- **Màu sắc**: Blue (#0084FF)
- **iOS tối thiểu**: iOS 14.0+

**Tính năng:**
- Đăng nhập with MFA (Password + OTP)
- Chuyển tiền
- Thanh toán QR
- Xem lịch sử giao dịch
- Đăng xuất

---

### 3. **ViettelPayApp (iOS)**
- **Thư mục**: `ViettelPayApp-iOS/`
- **File chính**: `ViettelPayApp.swift`
- **Ngôn ngữ**: Swift 5.5+
- **Framework**: SwiftUI
- **Màu sắc**: Red (#DC143C)
- **iOS tối thiểu**: iOS 14.0+

**Tính năng:**
- Đăng nhập with MFA (Password + OTP)
- Chuyển tiền
- Nạp tiền
- Xem lịch sử giao dịch
- Đăng xuất

---

### 4. **VCBPayApp (iOS)**
- **Thư mục**: `VCBPayApp-iOS/`
- **File chính**: `VCBPayApp.swift`
- **Ngôn ngữ**: Swift 5.5+
- **Framework**: SwiftUI
- **Màu sắc**: Navy (#005EBB)
- **iOS tối thiểu**: iOS 14.0+

**Tính năng:**
- Đăng nhập with MFA (Password + OTP)
- Chuyển tiền
- Thanh toán hóa đơn
- Xem lịch sử giao dịch
- Đăng xuất

---

## Kiến Trúc Swift/SwiftUI

### Cấu Trúc Chung (mỗi ứng dụng)

```
ContentView
├── LoginView
│   ├── Username TextField
│   ├── Password SecureField
│   └── Remember Me Checkbox
├── OTPVerificationView
│   ├── OTP Input Field
│   ├── Timer Countdown
│   └── Resend OTP Button
└── MainView
    ├── Account Info
    ├── Transfer Button
    ├── Bills Payment Button
    ├── History Button
    └── Logout Button
```

### Components Chính

#### 1. **ContentView** (Entry Point)
- Kiểm tra trạng thái đăng nhập từ UserDefaults
- Hiển thị LoginView hoặc MainView

#### 2. **LoginView**
- TextField cho username
- SecureField cho password
- Checkbox "Remember Me"
- Validation (6+ ký tự mật khẩu)
- Navigation đến OTPVerificationView

#### 3. **OTPVerificationView**
- TextField để nhập OTP 6 chữ số
- Countdown timer (300 giây = 5 phút)
- Nút "Gửi Lại Mã OTP" (disabled khi còn countdown)
- Validation OTP

#### 4. **MainView** (Dashboard)
- Hiển thị thông tin tài khoản
- Navigation tới TransferView và HistoryView
- Nút Logout

#### 5. **TransferView**
- TextField: Tên người nhận
- TextField: Số tiền
- TextField: Nội dung chuyển
- Button: Chuyển tiền

#### 6. **HistoryView**
- List các giao dịch
- Hiển thị tên, số tiền, nội dung

---

## OTP Generator Class

```swift
class OTPGenerator {
    var generatedOTP: String = ""
    var generatedTime: Date = Date()
    
    func generateOTP() -> String
    func validateOTP(_ inputOTP: String) -> Bool
}
```

**Quy tắc OTP:**
- 6 chữ số ngẫu nhiên (000000 - 999999)
- Hết hạn sau 5 phút
- Phải khớp chính xác để đăng nhập

---

## Demo Credentials

```
Username: demo
Password: 123456
```

---

## Công Nghệ Sử Dụng

### iOS
- **Language**: Swift 5.5+
- **UI Framework**: SwiftUI
- **Storage**: UserDefaults
- **Timer**: Timer + Combine
- **Navigation**: NavigationStack

### Supported iOS Versions
- iOS 14.0 (minimum)
- iOS 15.0+
- iOS 16.0+ (recommended)
- iOS 17.0+ (latest)

---

## Build & Deployment

### Yêu cầu
- Xcode 13.0+
- Swift 5.5+
- macOS 11.0+
- Apple Developer Account (để deploy lên App Store)

### Compile
```bash
# Di chuyển tới thư mục project
cd MoMoPayApp-iOS

# Build project (Xcode)
xcodebuild build -scheme MoMoPayApp
```

### Deploy to App Store
1. Tạo Apple Developer Account
2. Tạo App ID trên Apple Developer Portal
3. Tạo Provisioning Profile
4. Sign code bằng certificate
5. Upload qua Xcode hoặc Transporter

---

## So Sánh Android vs iOS

| Khía cạnh | Android | iOS |
|-----------|---------|-----|
| **Language** | Java | Swift |
| **Framework** | android.view (XML Layouts) | SwiftUI |
| **Storage** | SharedPreferences | UserDefaults |
| **Min Version** | API 21 (Android 5.1) | iOS 14.0 |
| **Build System** | Gradle | Xcode + Swift Package Manager |
| **UI Pattern** | Activity-based | View-based |

---

## Cách Test Ứng Dụng iOS

### Trên Simulator
```bash
# Chạy trên iPhone 14 simulator
xcodebuild -scheme MoMoPayApp -destination "platform=iOS Simulator,name=iPhone 14" test
```

### Trên Physical Device
1. Kết nối iPhone đến macOS
2. Choose device trong Xcode
3. Press Cmd + R để build & run

### Test Login Flow
1. Launch app
2. Nhập demo credentials: `demo` / `123456`
3. App hiển thị OTP screen
4. Kiểm tra console để lấy OTP code
5. Nhập OTP
6. App chuyển tới MainView

---

## File Structure

```
MoMoPayApp-iOS/
├── MoMoPayApp.swift          # Main app + all views
├── Assets.xcassets           # App icons & images
├── Preview Assets            # SwiftUI previews
└── Info.plist               # App configuration

ZaloPayApp-iOS/
├── ZaloPayApp.swift
└── [same structure as MoMo]

ViettelPayApp-iOS/
├── ViettelPayApp.swift
└── [same structure as MoMo]

VCBPayApp-iOS/
├── VCBPayApp.swift
└── [same structure as MoMo]
```

---

## Security Notes

**Hiện tại** (Demo):
- Mật khẩu lưu trong plaintext ❌
- OTP in console ❌
- Không encryption ❌

**Production** (nên có):
- ✅ Hash password (bcrypt, PBKDF2)
- ✅ Keychain untuk OTP storage
- ✅ Encryption cho user data
- ✅ Biometric authentication (Face ID, Touch ID)
- ✅ Session timeout
- ✅ Certificate pinning

---

## Maintenance & Updates

### Cập nhật iOS Versions:
Để support iOS phiên bản mới:
```swift
// Thêm version constraint vào @available
@available(iOS 15.0, *)
struct NewFeatureView: View {
    // ...
}
```

### Code Reusability Tips:
- Tách OTPGenerator vào file riêng (OTPGenerator.swift)
- Tách Views vào file riêng (LoginView.swift, etc.)
- Sử dụng @EnvironmentObject cho shared state

---

## Contact & Documentation

**Để tháo triển lên App Store:**
- Cần Apple Developer Program membership ($99/năm)
- Tạo Bundle ID, Certificates, Provisioning Profiles
- Submit lên Testflight trước khi production
- Tuân thủ App Store Review Guidelines

---

**Ngày tạo**: 2026-02-27  
**Phiên bản**: 1.0.0  
**Trạng thái**: Production Ready (Demo)
