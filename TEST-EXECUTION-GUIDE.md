# Unit Test Framework & Execution Guide

## Overview

This document provides complete instructions for running unit and UI tests for all 8 payment applications (4 Android + 4 iOS).

---

## ANDROID TEST EXECUTION

### 1. Unit Tests Setup

#### Prerequisites
- Android Studio 4.1+
- JDK 11+
- Android SDK (API 21+)
- Gradle 8.0+

#### Test Framework
- **JUnit 4**: Core unit testing
- **Espresso**: UI/Instrumentation tests
- **Mockito**: Object mocking (optional)

#### Dependencies in `build.gradle`

```gradle
dependencies {
    // Testing
    testImplementation 'junit:junit:4.13.2'
    testImplementation 'androidx.test:core:1.5.0'
    androidTestImplementation 'androidx.test.ext:junit:1.1.5'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.5.1'
    androidTestImplementation 'androidx.test.espresso:espresso-intents:3.5.1'
}
```

### 2. Running Unit Tests

#### Method 1: Via Android Studio
1. Open project in Android Studio
2. Right-click on test file in `app/src/test/java`
3. Select **Run > PaymentAppTests**
4. View results in Run window

#### Method 2: Via Terminal (JUnit Tests)

```bash
# Run all unit tests
./gradlew test

# Run specific test class
./gradlew test --tests PaymentAppTests

# Run specific test method
./gradlew test --tests PaymentAppTests.testOTPGeneration

# Run with detailed output
./gradlew test --tests PaymentAppTests -i

# Generate test report
./gradlew test --tests PaymentAppTests
# Report: app/build/reports/tests/testDebug/index.html
```

#### Method 3: Via Terminal (Security Tests)

```bash
# Run all security tests
./gradlew test --tests SecurityIntegrationTest

# Run encryption tests only
./gradlew test --tests EncryptionHelperTest

# Run session manager tests
./gradlew test --tests SessionManagerTest
```

### 3. Running Instrumentation/UI Tests

UI tests require a connected device or emulator.

#### Prerequisites
- Physical Android device OR
- Android Emulator (API 21+)
- Device/emulator connected: `adb devices`

#### Method 1: Via Android Studio
1. Connect device/emulator
2. Right-click test file in `app/src/androidTest/java`
3. Select **Run > PaymentAppUITest**
4. Tests execute on device/emulator

#### Method 2: Via Terminal

```bash
# Connect device or start emulator
adb devices

# Run all UI tests
./gradlew connectedAndroidTest

# Run specific UI test class
./gradlew connectedAndroidTest -Pandroid.testInstrumentationRunnerArguments.class=com.example.paymentapp.test.PaymentAppUITest

# Run with detailed logging
./gradlew connectedAndroidTest -i --stacktrace

# Generate coverage report
./gradlew connectedAndroidTest createDebugAndroidTestCoverageReport
```

### 4. Test Coverage Analysis

#### Generate Coverage Report
```bash
# For unit tests
./gradlew test jacocoTestReport

# For UI tests
./gradlew connectedAndroidTest createDebugAndroidTestCoverageReport

# View coverage report
# Report location: app/build/reports/jacoco/index.html
```

#### JaCoCo Plugin Configuration in `build.gradle`

```gradle
plugins {
    id 'jacoco'
}

jacoco {
    toolVersion "0.8.8"
}

task jacocoTestReport(type: JacocoReport) {
    reports {
        xml.enabled = true
        csv.enabled = false
        html.enabled = true
    }
}
```

#### Coverage Requirements
- **Overall**: 70% code coverage
- **Security Classes**: 90% coverage (EncryptionHelper, SessionManager, BiometricHelper)
- **OTPGenerator**: 85% coverage
- **UI Tests**: All critical user flows

### 5. Test Execution for All Android Apps

Execute tests for each app separately:

```bash
# MoMoPayApp tests
cd MoMoPayApp
./gradlew test

# ZaloPayApp tests
cd ../ZaloPayApp
./gradlew test

# ViettelPayApp tests
cd ../ViettelPayApp
./gradlew test

# VCBPayApp tests
cd ../VCBPayApp
./gradlew test

# Run UI tests for all
./gradlew connectedAndroidTest
```

### 6. Test Output Examples

#### Successful Test Output
```
PaymentAppTests > testOTPGeneration PASSED (45ms)
PaymentAppTests > testOTPValidation PASSED (12ms)
PaymentAppTests > testOTPExpiration PASSED (18ms)
SessionManagerTest > testAccountLockout PASSED (35ms)

== Test Summary ==
✓ 4 passed
✗ 0 failed
⊗ 0 skipped
Time: 110ms
Coverage: 75.2%
```

#### Failed Test Output
```
EncryptionHelperTest > testTamperingDetection FAILED
java.lang.AssertionError: Expected failure on tampered data
    at com.example.paymentapp.test.EncryptionHelperTest.testTamperingDetection(EncryptionHelperTest.java:95)
```

---

## iOS TEST EXECUTION

### 1. Unit Tests Setup

#### Prerequisites
- Xcode 13.0+
- Swift 5.5+
- macOS 11.0+
- iOS 14.0+ target

#### Test Framework
- **XCTest**: Native iOS testing framework
- **XCUITest**: UI automation testing

#### Test Target Configuration

1. Create test target in Xcode:
   - File > New > Target > Unit Testing Bundle
   - Name: `PaymentAppTests`
   - Language: Swift

2. Add to test target's Build Phases:
   - Link Binary With Libraries: Add app framework
   - Copy Bundle Resources: Add test resources

### 2. Running Unit Tests

#### Method 1: Via Xcode

```
1. Product > Scheme > Edit Scheme
2. Select Test action
3. Select test targets to run
4. Product > Test (⌘U)
5. View results in Test Navigator
```

#### Method 2: Via Terminal

```bash
# Run all unit tests
xcodebuild test -scheme PaymentApp -configuration Debug -destination 'platform=iOS Simulator,name=iPhone 14'

# Run specific test class
xcodebuild test -scheme PaymentApp -destination 'platform=iOS Simulator,name=iPhone 14' -testProductName PaymentAppTests

# Run specific test method
xcodebuild test -scheme PaymentApp -destination 'platform=iOS Simulator,name=iPhone 14' -only-testing PaymentAppTests/OTPGeneratorTests/testOTPGeneration

# Verbose output
xcodebuild test -scheme PaymentApp -destination 'platform=iOS Simulator,name=iPhone 14' -verbose
```

### 3. Running UI Tests

#### Simulator or Device Required

```bash
# Ensure simulator is running
xcrun simctl list devices

# Run UI tests on simulator
xcodebuild test -scheme PaymentApp -destination 'platform=iOS Simulator,name=iPhone 14' -testProductName PaymentAppUITests

# Run on physical device
xcodebuild test -scheme PaymentApp -destination 'platform=iOS,name=iPhone 14 Pro' -testProductName PaymentAppUITests

# Build and test together
xcodebuild build-for-testing -scheme PaymentApp -destination 'platform=iOS Simulator,name=iPhone 14'
xcodebuild test-without-building -scheme PaymentApp -destination 'platform=iOS Simulator,name=iPhone 14' -testProductName PaymentAppTests
```

### 4. Test Coverage Analysis

#### Generate Coverage Report

```bash
# Enable code coverage in Xcode
1. Edit Scheme > Test > Options > Code Coverage: ON

# Run tests with coverage
xcodebuild test -scheme PaymentApp -destination 'platform=iOS Simulator,name=iPhone 14' -enableCodeCoverage YES

# View coverage in Xcode
# Report Navigator > Click test run > Coverage tab
```

#### Coverage Requirements
- **Overall**: 70% code coverage
- **Security Classes**: 90% coverage
- **OTPGenerator**: 85% coverage
- **UI Tests**: All critical user flows (100%)

### 5. Test Execution for All iOS Apps

```bash
# MoMoPayApp-iOS
xcodebuild test -scheme MoMoPayApp-iOS -destination 'platform=iOS Simulator,name=iPhone 14'

# ZaloPayApp-iOS
xcodebuild test -scheme ZaloPayApp-iOS -destination 'platform=iOS Simulator,name=iPhone 14'

# ViettelPayApp-iOS
xcodebuild test -scheme ViettelPayApp-iOS -destination 'platform=iOS Simulator,name=iPhone 14'

# VCBPayApp-iOS
xcodebuild test -scheme VCBPayApp-iOS -destination 'platform=iOS Simulator,name=iPhone 14'

# Run all in sequence with loop
for app in MoMoPayApp ZaloPayApp ViettelPayApp VCBPayApp; do
  echo "Testing $app-iOS..."
  xcodebuild test -scheme $app-iOS -destination 'platform=iOS Simulator,name=iPhone 14'
done
```

### 6. Test Output Examples

#### Successful Test Output (Xcode Console)
```
Test Suite 'OTPGeneratorTests' started at 2024-01-10 10:30:45
OTPGeneratorTests.testOTPGeneration (0.045s) - PASSED
OTPGeneratorTests.testOTPValidation (0.012s) - PASSED
OTPGeneratorTests.testOTPExpiration (0.018s) - PASSED

Test Suite 'OTPGeneratorTests' finished at 10:30:47
Tests run: 3, Passed: 3, Failed: 0, Skipped: 0
Overall Duration: 0.075s
```

#### Failed Test Output
```
SessionManagerTests.testSessionTimeout FAILED
Error: AssertionError at SessionManagerTests.swift:45
Expected: true, Actual: false
Session should be invalid after 30 minutes of inactivity
```

---

## CROSS-PLATFORM TEST EXECUTION

### Complete Test Suite Run

#### All Tests at Once
```bash
# Android: Run all tests for all apps
cd ../
./gradlew test connectedAndroidTest

# iOS: Run all tests for all apps
xcodebuild test -scheme MoMoPayApp-iOS -destination 'platform=iOS Simulator,name=iPhone 14'
xcodebuild test -scheme ZaloPayApp-iOS -destination 'platform=iOS Simulator,name=iPhone 14'
xcodebuild test -scheme ViettelPayApp-iOS -destination 'platform=iOS Simulator,name=iPhone 14'
xcodebuild test -scheme VCBPayApp-iOS -destination 'platform=iOS Simulator,name=iPhone 14'
```

#### Test Summary Report
```
╔════════════════════════════════════════╗
║      PAYMENT APP TEST SUMMARY          ║
╠════════════════════════════════════════╣
║ Platform   │ App           │ Status     ║
├────────────┼───────────────┼────────────┤
║ Android    │ MoMoPayApp    │ ✓ PASSED   ║
║ Android    │ ZaloPayApp    │ ✓ PASSED   ║
║ Android    │ ViettelPayApp │ ✓ PASSED   ║
║ Android    │ VCBPayApp     │ ✓ PASSED   ║
├────────────┼───────────────┼────────────┤
║ iOS        │ MoMoPayApp    │ ✓ PASSED   ║
║ iOS        │ ZaloPayApp    │ ✓ PASSED   ║
║ iOS        │ ViettelPayApp │ ✓ PASSED   ║
║ iOS        │ VCBPayApp     │ ✓ PASSED   ║
╠════════════════════════════════════════╣
║ Total: 8/8 ✓ | Coverage: 74% ✓        ║
╚════════════════════════════════════════╝
```

---

## TEST CATEGORIES & COVERAGE

### 1. Unit Tests (40% of total)
- **OTP Generator Tests** (8 methods)
  - Generation: 6-digit format ✓
  - Validation: Correct/incorrect input ✓
  - Expiration: 5-minute timeout ✓
  - Boundary: Edge cases ✓

- **Session Manager Tests** (7 methods)
  - Session start/validity ✓
  - Timeout: 30 minutes ✓
  - Failed attempts: Count & reset ✓
  - Lockout: 5 attempts → 15 min block ✓

- **Encryption Tests** (7 methods)
  - Encrypt/decrypt round-trip ✓
  - Empty string handling ✓
  - Tampering detection ✓
  - Long string support ✓
  - Different IVs per encryption ✓

- **Biometric Tests** (3 methods)
  - Availability check ✓
  - Type detection ✓
  - Callback handling ✓

### 2. Security Tests (25% of total)
- Account credential encryption ✓
- Balance information protection ✓
- Session token handling ✓
- API payload encryption ✓
- Sensitive data masking ✓

### 3. UI Tests (20% of total)
- **Login Flow Tests** (5 methods)
  - Screen display ✓
  - Empty field validation ✓
  - Weak password detection ✓
  - OTP transition ✓
  - Error handling ✓

- **Transfer Flow Tests** (6 methods)
  - Screen elements ✓
  - Empty recipient validation ✓
  - Invalid amount handling ✓
  - Successful transfer ✓
  - Confirmation flow ✓

- **Biometric Auth Tests** (3 methods)
  - Button display ✓
  - Fallback to password ✓
  - Skip functionality ✓

### 4. Integration Tests (15% of total)
- Complete MFA sequence ✓
- Authentication with timeout ✓
- Lockout after failures ✓
- Secure token storage ✓
- Multi-platform consistency ✓

---

## TROUBLESHOOTING

### Android Issues

#### Issue: Tests fail with "Cannot resolve symbol"
**Solution:**
```bash
./gradlew clean build
# Then retry tests
```

#### Issue: Emulator not found
**Solution:**
```bash
# List available emulators
emulator -list-avds

# Start specific emulator
emulator -avd Pixel_6_API_30 &

# Check connected devices
adb devices
```

#### Issue: Test timeout
**Solution:**
```gradle
android {
    testOptions {
        unitTests.all {
            testLogging {
                events "passed", "skipped", "failed"
            }
            timeout = '60s'
        }
    }
}
```

### iOS Issues

#### Issue: Xcode test target not found
**Solution:**
```bash
# List schemes
xcodebuild -list -project PaymentApp.xcodeproj

# Create scheme if missing:
# Xcode > Product > Scheme > Manage Schemes > + > Add
```

#### Issue: Simulator not responding
**Solution:**
```bash
# Reset simulator
xcrun simctl erase all

# Boot simulator fresh
xcrun simctl boot <device-udid>
```

#### Issue: Code signing issues
**Solution:**
```bash
# Reset signing identities
security delete-keychain ~/Library/Keychains/login.keychain-db
security create-keychain -p "" ~/Library/Keychains/login.keychain-db

# Restart Xcode
killall Xcode
```

---

## CONTINUOUS INTEGRATION SETUP

### GitHub Actions (Optional)

Create `.github/workflows/tests.yml`:

```yaml
name: Tests

on: [push, pull_request]

jobs:
  android-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-java@v3
        with:
          java-version: '11'
      - run: ./gradlew test
      - run: ./gradlew connectedAndroidTest

  ios-tests:
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v3
      - run: |
          for app in MoMoPayApp ZaloPayApp ViettelPayApp VCBPayApp; do
            xcodebuild test -scheme $app-iOS
          done
```

---

## BEST PRACTICES

1. **Always run tests before committing code**
   ```bash
   ./gradlew test  # Android
   xcodebuild test -scheme PaymentApp  # iOS
   ```

2. **Maintain >70% code coverage**
   - Review coverage reports regularly
   - Add tests for new features
   - Aim for 90% on security classes

3. **Keep tests isolated**
   - Use mocks for external dependencies
   - Clear state between tests
   - Avoid hardcoded test data

4. **Use descriptive test names**
   - `testOTPGenerationProduces6Digits` (good)
   - `testOTP` (bad)

5. **Document expected behavior**
   - Add comments explaining test purpose
   - Include assertions for all checks
   - Reference requirements/docs

---

## Test Maintenance Schedule

- **Weekly**: Run full test suite
- **Before Release**: 100% test coverage
- **Monthly**: Review and update test cases
- **Quarterly**: Security test audit

---

## Next Steps After Testing

✅ **All tests passing?** → Ready for deployment
→ Follow DEPLOYMENT.md for app store submission

❌ **Tests failing?** → Debug and fix code
→ Add new tests for uncovered scenarios
→ Retry until 100% pass rate

