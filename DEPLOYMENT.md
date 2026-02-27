# 📦 Deployment Guide - Google Play & Apple App Store
## Hướng Dẫn Triển Khai Ứng Dụng Thanh Toán Lên Cửa Hàng Ứng Dụng

**Phiên bản**: 1.0.0 | **Ngày cập nhật**: 2026-02-27 | **Trạng thái**: Production Ready

---

## 🎯 **Quick Start Summary**

| Bước | Google Play | Apple App Store |
|------|-------------|-----------------|
| **Tài khoản** | Developer Account ($25) | Developer Program ($99/năm) |
| **Thời gian** | 48-72 giờ xem xét | 24-48 giờ xem xét |
| **File build** | .aab (Android App Bundle) | .ipa (via Xcode archive) |
| **Chứng chỉ** | Keystore (.jks) | Certificate + Provisioning Profiles |

---

## 📱 **GOOGLE PLAY STORE - Android Deployment**

### **Phase 1: Setup Google Play Developer Account**

#### **Step 1.1: Create Account**
```bash
1. Go to https://play.google.com/console
2. Click "Create account"
3. Login with Google account (create one if needed)
```

#### **Step 1.2: Developer Registration**
```
1. Accept Google Play Developer Distribution Agreement
2. Pay one-time fee: $25 USD
3. Complete developer profile:
   - Developer name (company name)
   - Contact email
   - Phone number
   - Developer website
4. Accept merchant agreement
5. Add payment method (credit card)
```

**Timeframe**: 15-30 minutes

---

### **Phase 2: Create Signing Certificate (Keystore)**

#### **Step 2.1: Generate Keystore**

**On Windows (cmd):**
```bash
keytool -genkey -v -keystore momo_release.keystore -keyalg RSA -keysize 2048 -validity 10000 -alias momo_key
```

**On macOS/Linux:**
```bash
keytool -genkey -v -keystore momo_release.keystore -keyalg RSA -keysize 2048 -validity 10000 -alias momo_key
```

**Interactive Prompts:**
```
Enter keystore password: (choose strong password: Min 6 chars)
Re-enter password: (confirm)
What is your first and last name? 
  → Hoang Nguyen

What is the name of your organizational unit?
  → Payment Apps Division

What is the name of your organization?
  → VietnamPay Inc.

What is the name of your City or Locality?
  → Ho Chi Minh City

What is the name of your State or Province?
  → Ho Chi Minh

What is the two-letter country code for this unit?
  → VN

Is CN=Hoang Nguyen, OU=Payment Apps, O=VietnamPay, L=HCM, ST=HCM, C=VN correct?
  → yes

Enter key password for <momo_key>:
  → (press Enter to use keystore password)
```

**Output**: `momo_release.keystore` file (~2 KB)

**⚠️ IMPORTANT**: 
- Save this file in a **SECURE location**
- Store password in **password manager**
- **NEVER commit to Git**
- Backup on **external drive**
- **Keep for 10 years** (validity period)

#### **Step 2.2: View Certificate Details**
```bash
keytool -list -v -keystore momo_release.keystore -alias momo_key
```

Note the **SHA-1 fingerprint** (for future reference)

**Timeframe**: 5-10 minutes

---

### **Phase 3: Build Release APK/AAB**

#### **Step 3.1: Configure Gradle Signing**

Create/Edit `MoMoPayApp/gradle.properties`:
```properties
MOMO_RELEASE_STORE_FILE=momo_release.keystore
MOMO_RELEASE_STORE_PASSWORD=your_strong_password
MOMO_RELEASE_KEY_ALIAS=momo_key
MOMO_RELEASE_KEY_PASSWORD=your_strong_password
```

**Or** Edit `MoMoPayApp/app/build.gradle`:
```gradle
android {
    ...
    signingConfigs {
        releaseConfig {
            keyAlias 'momo_key'
            keyPassword 'your_strong_password'
            storeFile file('momo_release.keystore')
            storePassword 'your_strong_password'
        }
    }

    buildTypes {
        release {
            signingConfig signingConfigs.releaseConfig
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}
```

#### **Step 3.2: Build App Bundle (AAB) - RECOMMENDED**

```bash
cd MoMoPayApp

# Build release AAB
./gradlew bundleRelease

# Output: app/build/outputs/bundle/release/app-release.aab (~4-8 MB)
```

**Why AAB instead of APK?**
- Smaller download size (users get optimized version)
- Google Play auto-generates optimized APKs per device
- Required for Google Play (since August 2021)

#### **Step 3.3: Build APK (if needed for testing)**

```bash
# Build release APK
./gradlew assembleRelease

# Output: app/build/outputs/apk/release/app-release.apk (~12-15 MB)
```

**Timeframe**: 2-5 minutes per build

---

### **Phase 4: Configure Google Play Console**

#### **Step 4.1: Create App**
```
1. Google Play Console → "Create app"
2. App name: "MoMo Pay"
3. Category: Finance & Monetization
4. Content rating: High maturity (due to financial data)
5. Accept declarations
```

#### **Step 4.2: Set Up App Store Listing**
```
App details → Tab: "Store listing"
├── Short description (80 chars max)
│   └─ "Fast Money Transfer with AI-Powered Security"
├── Full description (4000 chars)
│   └─ (write marketing copy)
├── Screenshots (5-8 required)
│   └─ Add HD screenshots (1080x1920px)
├── Feature graphic (1024x500px)
│   └─ Eye-catching banner
├── Video preview (optional)
│   └─ 30-second demo video
└── Icon (512x512px)
    └─ App icon at high res
```

#### **Step 4.3: Privacy Policy**
```
App details → Tab: "Policy"
├── Privacy policy URL
│   └─ https://yoursite.com/privacy.md
├── Terms of service (optional)
│   └─ https://yoursite.com/tos.md
└── Store listing
    └─ Review all content
```

**Sample Privacy Policy Template:**
```markdown
# Privacy Policy - MoMo Pay

## Information We Collect
- Authentication data (encrypted)
- Transaction history
- Device identifiers

## How We Use It
- To process transactions
- To improve security
- To comply with regulations

## Data Protection
- End-to-end encryption
- Secure server storage
- No third-party sharing without consent

Last updated: February 27, 2026
```

#### **Step 4.4: Content Rating Questionnaire**
```
App content rating → Complete questionnaire
├── Category: Financial services
├── Violence: None
├── Sexual content: None
├── Profanity: Rare
├── Behavioral: Gambling (select No)
└── Submit
    → System generates age rating
```

Typical result: **3+ years age rating**

**Timeframe**: 20-30 minutes

---

### **Phase 5: Upload & Publish**

#### **Step 5.1: Ready for Review**
```
Testing on Google Play → Tab: "Internal testing"
1. Click "Create new release"
2. Select app bundle (.aab file)
   └─ Click "Browse files" → select app-release.aab
3. Review bundle details
4. Add release notes:
   "Version 1.0.0 - Initial launch"
5. Click "Save"
```

#### **Step 5.2: Test Release (Optional)**
```
1. Add testers (Google Groups or email list)
2. Share test URL with testers
3. Collect feedback
4. Fix issues if needed
5. When ready: promote to Production
```

#### **Step 5.3: Promote to Production**
```
Releases → Tab: "Production"
1. Click "Create new release"
2. Select your tested app bundle
3. Write release notes
4. Set release date (recommended: immediate)
5. Click "Review release"
   → Google Play validates app
   → Shows warning (if any)
6. Click "Start rollout"
   → 100% of users get it
```

#### **Step 5.4: Monitor After Publishing**
```
Dashboard
├── Ratings & reviews (watch for 1-stars)
├── Crashes & ANRs (monitor stability)
├── Downloads (track adoption)
└── User feedback (fix bugs quickly)
```

**Timeframe**: 48-72 hours for Google to review and publish

---

### **Phase 6: Troubleshooting Google Play Issues**

#### **Common Rejection Reasons**

| Issue | Solution |
|-------|----------|
| "Invalid APK file" | Re-sign APK, check keystore password |
| "Permissions abuse" | Remove unnecessary permissions from manifest |
| "Privacy policy missing" | Add privacy policy URL in store listing |
| "App crashes on launch" | Test on emulator, check logcat |
| "Billing required" | Remove test IAP, comply with billing terms |

#### **Check Errors**
```bash
# Validate APK/AAB signature
jarsigner -verify -verbose -certs app-release.aab

# View manifest
unzip -c app-release.aab AndroidManifest.xml | strings | grep -A 5 "<uses-permission"
```

---

## 🍎 **APPLE APP STORE - iOS Deployment**

### **Phase 1: Apple Developer Program Enrollment**

#### **Step 1.1: Prerequisites**
```
✓ Apple ID (free)
✓ Credit card for $99/year fee
✓ Enrolled device (Mac) with Xcode
✓ Minimum macOS version
```

#### **Step 1.2: Enroll in Developer Program**
```
1. Go to https://developer.apple.com
2. Click "Account" → "Membership"
3. Click "Enroll now"
4. Choose "Individual" or "Organization"
5. Complete enrollment process
6. Pay $99 USD annual fee
7. Accept software agreements (long!)
8. Verify account via email
```

**Timeframe**: 24-48 hours (Apple verification)

---

### **Phase 2: Create Certificates & Profiles**

#### **Step 2.1: Certificate Signing Request (CSR)**

**In Xcode:**
```
Xcode → Preferences → Accounts
└─ Select Apple ID
  └─ Download Manual Profiles
    └─ (if needed, manual CSR generation)

OR - Using Keychain:
Keychain Access → Certificate Assistant → Request Certificate
├── User email: (your Apple ID)
├── Common name: "MoMo App Certificate"
├── Request: "Saved to disk"
└─ Save as: MoMoApp.certSigningRequest
```

#### **Step 2.2: Create Certificates in Developer Portal**

```
Developer.apple.com → Certificates → iOS App Development
1. Click "+" button
2. Select "iOS App Development"
3. Click "Continue"
4. Upload .certSigningRequest from above
5. Download certificate → MoMoApp.cer
6. Double-click to install in Keychain
```

**Types of Certificates Needed:**
- **Development**: For testing on devices
- **Distribution**: For App Store submission
- **Push Notification Service**: For notifications (optional)

#### **Step 2.3: Register App ID**

```
Developer.apple.com → Identifiers → App IDs
1. Click "+" button
2. Register App ID
   ├── App name: "MoMo Pay"
   ├── Bundle ID: com.momo.paymentapp
   └── Capabilities: Push Notifications (if used)
3. Save
4. Repeat for ZaloPay, ViettelPay, VCB Pay
```

**Bundle ID Pattern:**
```
com.momo.paymentapp
com.zalopay.paymentapp
com.viettel.paymentapp
com.vcb.paymentapp
```

#### **Step 2.4: Create Provisioning Profiles**

```
Developer.apple.com → Profiles → Development
1. Click "+" button
2. Select "iOS App Development"
3. Click "Continue"
4. Select App ID: com.momo.paymentapp
5. Select Certificates: (your development cert)
6. Select Devices: (your iPhone/iPad)
7. Name profile: "MoMo Dev Profile"
8. Download → MoMoDevProfile.mobileprovision
```

**Repeat for Distribution:**
```
Developer.apple.com → Profiles → Distribution
1. Select "iOS App Store"
2. Select App ID & Certificate
3. Name: "MoMo App Store Profile"
4. Download → MoMoDistProfile.mobileprovision
```

**Timeframe**: 15-20 minutes per certificate/profile

---

### **Phase 3: Configure Xcode Signing**

#### **Step 3.1: Import Signing Assets**

Double-click downloaded files:
- `MoMoApp.cer` (auto-installs to Keychain)
- `MoMoDevProfile.mobileprovision`
- `MoMoDistProfile.mobileprovision`

#### **Step 3.2: Configure Signing in Xcode**

```
Xcode → MoMoPayApp (project)
├─ Select "MoMoPayApp" target
├─ Tab: "Signing & Capabilities"
├─ Team: (select your Apple Team)
├─ Bundle Identifier: com.momo.paymentapp
├─ Development provisioning: MoMoDevProfile
└─ Distribution provisioning: MoMoDistProfile
```

**Verify Signature:**
```bash
# Check signing identity
codesign -dv ../MoMoPayApp.app

# Result should show team ID and certificate
```

**Timeframe**: 5-10 minutes

---

### **Phase 4: Test on Physical Device**

#### **Step 4.1: Connect iPhone**

```
1. Connect iPhone via USB to Mac
2. Trust the Mac on iPhone
   → Settings → General → Device Management
3. Xcode recognizes device
   → Shows in device selector
```

#### **Step 4.2: Build & Run on Device**

```bash
# Xcode GUI:
1. Select device from device dropdown
2. Click "Play" button (Cmd + R)
3. Wait for build (~1-2 minutes)
4. App installs & launches on device
5. Test manually

# Or via terminal:
xcodebuild -project MoMoPayApp.xcodeproj \
  -scheme MoMoPayApp \
  -destination 'id=<device-id>' \
  clean build test
```

#### **Step 4.3: Manual Testing Checklist**
```
□ Login with demo credentials works
□ OTP verification displays correctly
□ OTP countdown timer works (5 min)
□ Biometric prompt appears
□ Transfer screen loads
□ History shows past transactions
□ Logout clears session
□ Session timeout after 30 min inactivity
□ Account locks after 5 failed logins
□ No crashes on any screen
```

**Timeframe**: 20-30 minutes

---

### **Phase 5: Create App Store Connect Listing**

#### **Step 5.1: Create App Record**

```
App Store Connect → Apps → "My Apps"
1. Click "+"button
2. Create New App
   ├── Platform: iOS
   ├── App name: "MoMo Pay"
   ├── Primary language: Vietnamese
   ├── Bundle ID: com.momo.paymentapp
   ├── SKU: MOMOPAY001 (unique identifier)
   └── User Access: Full Access
3. Create
```

#### **Step 5.2: Upload Metadata**

```
App Store Connect → App Information
├── App Name: "MoMo Pay"
├── Subtitle: "Secure Money Transfer"
├── Category: Finance
├── Privacy Policy: https://yoursite.com/privacy
├── Support URL: https://support.yoursite.com
└── Marketing URL: https://yoursite.com
```

#### **Step 5.3: Add App Store Listing**

```
App Store Connect → Localizations → Vietnamese
├── App Name: "MoMo Pay"
├── Subtitle: "Chuyển tiền an toàn"
├── Description: (write compelling description)
├── Keywords: "payment, money, transfer, security"
├── Support Notes: "Hỗ trợ iOS 14.0+"
└── Review Notes: "Demo account: demo / 123456"
```

#### **Step 5.4: Add Screenshots**

```
Screenshots (5-8 required, specs: 1080x1920px)
├── Screenshot 1: Login screen
├── Screenshot 2: OTP verification
├── Screenshot 3: Dashboard
├── Screenshot 4: Transfer screen
├── Screenshot 5: Transaction history
├── (Optional: Add captions/overlays)
└── Ensure all show demo data only
```

#### **Step 5.5: Add App Icon & Preview

```
App Icon (required)
├── Size: 1024 x 1024 px
├── Format: PNG
├── No transparency
└─ Should show app logo clearly

Preview Video (optional)
├── Format: MP4
├── Duration: 15-30 seconds
├── Show user flow: Login → Transfer → History
```

**Timeframe**: 30-45 minutes

---

### **Phase 6: Build & Archive for App Store**

#### **Step 6.1: Create Release Build**

```bash
# Clean
xcodebuild clean

# Build for Release
xcodebuild -scheme MoMoPayApp \
  -configuration Release \
  -sdk iphoneos \
  -arch arm64
```

#### **Step 6.2: Create Archive**

```bash
# Create archive for submission
xcodebuild -scheme MoMoPayApp \
  -configuration Release \
  -archivePath MoMoPayApp.xcarchive \
  archive
```

**Or via Xcode GUI:**
```
Product → Archive
→ Wait for build complete
→ Organizer opens
→ Select latest archive
```

#### **Step 6.3: Export for App Store**

```bash
# Export for distribution
xcodebuild -exportArchive \
  -archivePath MoMoPayApp.xcarchive \
  -exportPath ./exports \
  -exportOptionsPlist exportOptions.plist
```

**exportOptions.plist content:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>method</key>
    <string>app-store</string>
    <key>signingStyle</key>
    <string>automatic</string>
    <key>teamID</key>
    <string>YOUR_TEAM_ID</string>
    <key>stripSwiftSymbols</key>
    <true/>
</dict>
</plist>
```

**Output**: `.ipa` file in exports folder

**Timeframe**: 3-5 minutes

---

### **Phase 7: Submit to App Store**

#### **Step 7.1: Upload via Xcode**

```bash
# Validate
xcodebuild -validateapp \
  -file MoMoPayApp.ipa \
  -username your_apple_id@icloud.com \
  -password app_specific_password

# Upload
xcodebuild -uploadapp \
  -file MoMoPayApp.ipa \
  -username your_apple_id@icloud.com  
  -password app_specific_password
```

**Or via Xcode GUI:**
```
Organizer → Select Archive → Distribute App
→ Select "App Store Connect"
→ Select Team
→ Select Release Signing Style
→ Upload
```

#### **Step 7.2: Submit for Review**

```
App Store Connect → Version Release
1. Select Version 1.0
2. Review all information
3. Select "Automatic Release" or schedule date
4. Click "Submit for Review"
5. Get confirmation email
```

#### **Step 7.3: Wait for Apple Review**

```
Status tracking:
├── Submitted: Pending review
├── In Review: Apple team reviewing (12-48 hours)
├── Approved: Ready to release
└── Available on App Store: Live for users!

Check progress:
→ App Store Connect → TestFlight / Review
→ Email notifications when status changes
```

**Timeframe**: 24-48 hours for review

---

### **Phase 8: Post-Submission (When Approved)**

#### **Step 8.1: Release to Users**

```
App Store Connect → Version Release
└─ Click "Release on App Store"
   → May take 1-2 hours to appear in store
```

#### **Step 8.2: Promote & Monitor**

```
First 48 hours critical:
✓ Monitor crash reports
✓ Read user reviews (respond to negatives)
✓ Check ratings (aim for 4.0+)
✓ Watch download numbers
✓ Fix any bugs found in production
```

#### **Step 8.3: Plan Updates**

```
For version 1.1:
- Increment build number in Xcode
- Update version string
- Add new features or bug fixes
- Go through submission process again
```

---

## 📊 **Deployment Comparison Matrix**

| Aspect | Google Play | Apple App Store |
|--------|-------------|-----------------|
| **Account Cost** | $25 (one-time) | $99/year |
| **Setup Time** | 1-2 days | 3-5 days |
| **Review Time** | 48-72 hours | 24-48 hours |
| **Rejection Rate** | ~5-10% | ~20-30% |
| **User Base** | ~3 billion devices | ~1.8 billion devices |
| **Market Share** | ~70% (worldwide) | ~25% (worldwide) |
| **Age Rating** | IARC (automatic) | Manual questionnaire |
| **Update Rollout** | Gradual (% based) | All at once |
| **Refund Policy** | 48-hour window | 14-day window |

---

## ✅ **Pre-Launch Checklist**

### **Development**
- [ ] All features tested locally
- [ ] No console errors or crashes
- [ ] Biometric works on devices
- [ ] OTP generates & validates correctly
- [ ] Session timeout works as expected
- [ ] Account lockout after 5 attempts
- [ ] Data encryption working
- [ ] App signing certificate valid

### **Store Listings**
- [ ] Compelling description (localized)
- [ ] 5+ high-quality screenshots (localized)
- [ ] Privacy policy & ToS links valid
- [ ] App icon 1024x1024 (no transparency)
- [ ] Support email configured
- [ ] Contact info up to date

### **Store Compliance**
- [ ] No demo accounts in production
- [ ] OTP not visible in screenshots
- [ ] No personal data exposed
- [ ] App crashes fixed
- [ ] Permissions justified in description
- [ ] Age rating appropriate

### **Security**
- [ ] Keystore/signing cert backed up offline
- [ ] App Store passwords in password manager
- [ ] API endpoints HTTPS only
- [ ] Demo creds not leaked in code
- [ ] Git history cleaned of secrets
- [ ] ProGuard/obfuscation enabled

### **Final QA**
- [ ] Sign complete user flow end-to-end
- [ ] Test on minimum supported OS version
- [ ] Test on latest OS version
- [ ] Test on different screen sizes
- [ ] Test offline behavior (if applicable)
- [ ] Battery consumption reasonable
- [ ] Data usage reasonable

---

## 🚨 **Common Pitfalls to Avoid**

```
❌ Don't:
- Use hardcoded API keys
- Include test accounts visible to users
- Request unnecessary permissions
- Store sensitive data unencrypted
- Forget to backup signing certificates
- Submit without privacy policy
- Make breaking changes without warning
- Ignore user reviews & crash reports
- Use outdated dependencies
- Leave debug logs in release build

✅ Do:
- Use environment variables for config
- Hide demo accounts from production
- Request only needed permissions
- Use encryption for sensitive data
- Backup certs to external secure drive
- Have comprehensive privacy policy
- Use semantic versioning
- Respond to reviews professionally
- Update dependencies regularly
- Strip debug logs from release builds
```

---

## 📞 **Support Resources**

- **Google Play**: https://play.google.com/console/support
- **Apple Developer**: https://developer.apple.com/support/
- **App Store Connect**: https://appstoreconnect.apple.com
- **Xcode Documentation**: https://developer.apple.com/xcode/download/
- **Android Studio Help**: https://developer.android.com/studio

---

## 🎯 **Next Steps After Launch**

1. **Monitor Analytics** - Track user behavior, crashes, errors
2. **Gather Feedback** - Read reviews, respond to users
3. **Plan Updates** - Feature requests, bug fixes
4. **A/B Testing** - Test UI changes with subset of users
5. **Security Updates** - Patch vulnerabilities promptly
6. **Localization** - Expand to more languages/regions
7. **Marketing** - App store optimization (ASO)
8. **Backend Integration** - Connect to real payment APIs

---

**Checklist Version**: 1.0.0  
**Last Updated**: February 27, 2026  
**Status**: Ready for Production Deployment  

🚀 **Good luck with your app launch!**
