---
title: Mobile Testing
parent: Specialized Testing
topic: Testing mobile-specific challenges
difficulty: specialist
created: 2026-08-05
tags:
  - career-path
  - quality-engineering
  - mobile-testing
  - ios
  - android
---

# Mobile Testing

> **Core Principle:** Mobile testing addresses unique challenges of mobile platforms: device fragmentation, network variability, touch interfaces, and platform-specific behaviors.

## What Mobile Testing Is

Mobile testing verifies:
- **Functionality:** App works correctly on mobile devices
- **Usability:** App is easy to use on small screens with touch
- **Performance:** App performs well with limited resources
- **Compatibility:** App works across devices and OS versions
- **Network:** App handles variable network conditions
- **Platform:** App follows platform conventions (iOS, Android)

## Mobile Testing Challenges

### 1. Device Fragmentation

**Android fragmentation:**
```
Screen sizes: 4" to 10"+
Resolutions: 720p to 4K
Aspect ratios: 16:9, 18:9, 19.5:9, 20:9
Android versions: 8.0 to 14 (API 26-34)
Manufacturers: Samsung, Google, Xiaomi, OnePlus, etc.
Custom UIs: One UI, MIUI, OxygenOS, etc.
Hardware: Varying RAM, CPU, GPU capabilities
```

**iOS fragmentation:**
```
Screen sizes: 4.7" to 6.7"
Resolutions: Retina, Super Retina XDR
iOS versions: iOS 15 to iOS 17
Devices: iPhone SE, iPhone 12-15 series
Hardware: A12 to A17 chips
```

**Testing strategy:**
- Test on top 10 devices by market share
- Include small and large screens
- Test oldest supported OS version
- Test on both physical devices and emulators
- Use cloud device farms for broader coverage

### 2. Network Variability

**Mobile network conditions:**
```
Network types:
- WiFi: Fast, stable, unlimited
- 5G: Very fast, low latency
- 4G/LTE: Fast, moderate latency
- 3G: Slow, high latency
- 2G/EDGE: Very slow, very high latency
- Offline: No connectivity

Network issues:
- Intermittent connectivity
- High latency (100ms to 2000ms)
- Packet loss (0% to 20%)
- Bandwidth throttling
- Network switching (WiFi to cellular)
```

### 3. Touch Interface

**Touch-specific challenges:**
```
Touch targets:
- Minimum 44x44 points (iOS)
- Minimum 48x48 dp (Android)
- Adequate spacing between targets

Gestures:
- Tap, long press
- Swipe (horizontal, vertical, diagonal)
- Pinch to zoom
- Rotate
- Multi-touch

Touch issues:
- Fat finger problem (hitting wrong target)
- Accidental touches
- Gesture conflicts
- Touch feedback (visual, haptic)
```

### 4. Platform Conventions

**iOS conventions:**
```
Navigation:
- Tab bar at bottom
- Navigation bar at top
- Back button in navigation bar
- Swipe from left edge to go back

UI patterns:
- Pull to refresh
- Segmented controls
- Action sheets
- Alerts with two buttons max

Interactions:
- Haptic feedback
- Smooth animations
- Rubber-banding at edges
```

**Android conventions:**
```
Navigation:
- Bottom navigation or navigation drawer
- App bar at top
- System back button
- Up button in app bar

UI patterns:
- Pull to refresh
- Floating action button (FAB)
- Bottom sheets
- Snackbars

Interactions:
- Ripple effects
- Material Design animations
- Overscroll glow
```

## Mobile Testing Strategy

### 1. Functional Testing

**Test cases:**
```python
# Using Appium for cross-platform mobile testing
from appium import webdriver
from appium.webdriver.common.appiumby import AppiumBy

class TestMobileApp:
    def setup(self):
        desired_caps = {
            'platformName': 'Android',
            'deviceName': 'Pixel 6',
            'app': '/path/to/app.apk',
            'automationName': 'UiAutomator2'
        }
        self.driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)
    
    def test_login_success(self):
        """Test successful login"""
        # Enter credentials
        self.driver.find_element(AppiumBy.ID, 'username').send_keys('testuser')
        self.driver.find_element(AppiumBy.ID, 'password').send_keys('password123')
        
        # Tap login button
        self.driver.find_element(AppiumBy.ID, 'login_button').click()
        
        # Verify logged in
        welcome = self.driver.find_element(AppiumBy.ID, 'welcome_message')
        assert 'Welcome, testuser' in welcome.text
    
    def test_offline_mode(self):
        """Test app behavior when offline"""
        # Turn off network
        self.driver.set_network_connection(0)  # Airplane mode
        
        # Try to load data
        self.driver.find_element(AppiumBy.ID, 'refresh_button').click()
        
        # Verify offline message
        message = self.driver.find_element(AppiumBy.ID, 'offline_message')
        assert 'No internet connection' in message.text
        
        # Restore network
        self.driver.set_network_connection(6)  # WiFi and data
```

### 2. Usability Testing

**Usability checklist:**
```
Layout:
□ Text readable without zooming
□ Touch targets large enough (44x44 pt / 48x48 dp)
□ Adequate spacing between interactive elements
□ Content fits screen without horizontal scrolling
□ Important content not hidden by keyboard

Navigation:
□ Easy to reach all features with one hand
□ Back navigation works correctly
□ Deep links work correctly
□ Orientation changes handled gracefully

Interactions:
□ Touch feedback immediate (< 100ms)
□ Gestures work reliably
□ No accidental touches
□ Loading states shown for long operations
□ Pull to refresh works

Accessibility:
□ Screen reader works (VoiceOver/TalkBack)
□ Dynamic Type / font scaling supported
□ Sufficient contrast
□ Touch alternatives for gestures
```

### 3. Performance Testing

**Mobile performance metrics:**
```
App startup time:
- Cold start: < 2 seconds
- Warm start: < 1 second

Frame rate:
- Target: 60 FPS
- Minimum: 30 FPS
- Jank: < 5% of frames > 16ms

Memory usage:
- iOS: < 200MB (varies by device)
- Android: < 256MB (varies by device)
- No memory leaks

Battery usage:
- < 5% per hour of active use
- Minimal background drain
- Efficient network usage

Network usage:
- Minimize data transfer
- Cache appropriately
- Compress images
- Use efficient protocols
```

**Performance testing tools:**
```bash
# Android - using adb
adb shell dumpsys meminfo com.example.app
adb shell dumpsys gfxinfo com.example.app

# iOS - using Instruments
# Xcode > Product > Profile > Time Profiler

# Cross-platform - using Firebase Performance
# Integrate Firebase Performance SDK
# Monitor in Firebase console
```

### 4. Compatibility Testing

**Device matrix:**
```
┌─────────────────┬──────────┬──────────┬──────────┬──────────┐
│ Device          │ Android  │ iOS      │ Tablet   │ Priority │
├─────────────────┼──────────┼──────────┼──────────┼──────────┤
│ iPhone 15 Pro   │          │ ✓        │          │ High     │
│ iPhone 13       │          │ ✓        │          │ High     │
│ iPhone SE       │          │ ✓        │          │ Medium   │
│ iPad Pro        │          │          │ ✓        │ Medium   │
│ Samsung S23     │ ✓        │          │          │ High     │
│ Pixel 8         │ ✓        │          │          │ High     │
│ Samsung A54     │ ✓        │          │          │ Medium   │
│ Xiaomi Redmi    │ ✓        │          │          │ Low      │
│ Samsung Tab S9  │          │          │ ✓        │ Low      │
└─────────────────┴──────────┴──────────┴──────────┴──────────┘
```

**Cloud device farms:**
- **BrowserStack:** Real devices, 3000+ device/OS combinations
- **Sauce Labs:** Real devices and emulators
- **Firebase Test Lab:** Android and iOS, free tier available
- **AWS Device Farm:** Real devices, pay per minute
- **Xamarin Test Cloud:** (now App Center)

### 5. Network Testing

**Network simulation:**
```python
# Using Charles Proxy to simulate network conditions
# Configure in mobile device WiFi settings

# Network profiles:
profiles = {
    '3G': {
        'bandwidth': '750 kbps',
        'latency': '200ms',
        'reliability': '95%'
    },
    '4G': {
        'bandwidth': '4 Mbps',
        'latency': '100ms',
        'reliability': '99%'
    },
    'Edge': {
        'bandwidth': '240 kbps',
        'latency': '500ms',
        'reliability': '90%'
    },
    'Offline': {
        'bandwidth': '0',
        'latency': 'N/A',
        'reliability': '0%'
    }
}

# Test scenarios:
test_cases = [
    'App loads with 3G network',
    'App handles network timeout gracefully',
    'App retries failed requests',
    'App works offline with cached data',
    'App syncs when network restored',
    'App handles network switching (WiFi to cellular)',
    'App shows loading indicators for slow operations'
]
```

## Mobile Testing Tools

### Automation Frameworks

| Tool | Platforms | Language | Best For |
|------|-----------|----------|----------|
| **Appium** | iOS, Android | Multi | Cross-platform |
| **XCUITest** | iOS | Swift/ObjC | iOS native |
| **Espresso** | Android | Java/Kotlin | Android native |
| **Detox** | iOS, Android | JavaScript | React Native |
| **Maestro** | iOS, Android | YAML | Simple flows |
| **Calabash** | iOS, Android | Ruby | BDD testing |

### Performance Tools

| Tool | Purpose |
|------|---------|
| **Firebase Performance** | Real-world performance monitoring |
| **Android Profiler** | CPU, memory, network profiling |
| **Instruments** | iOS performance profiling |
| **Charles Proxy** | Network debugging and simulation |
| **Flipper** | React Native debugging |

### Cloud Device Farms

| Service | Devices | Pricing |
|---------|---------|---------|
| **BrowserStack** | 3000+ real devices | Per minute |
| **Sauce Labs** | 900+ real devices | Per minute |
| **Firebase Test Lab** | Android + iOS | Free tier + paid |
| **AWS Device Farm** | Real devices | Per minute |
| **Kobiton** | Real devices | Per minute |

## Mobile Testing Checklist

### Functionality

- [ ] All features work on target devices
- [ ] App handles orientation changes
- [ ] App handles interruptions (calls, notifications)
- [ ] App handles backgrounding and foregrounding
- [ ] Push notifications work correctly
- [ ] Deep links work correctly
- [ ] App updates work correctly

### Usability

- [ ] Text readable on all screen sizes
- [ ] Touch targets large enough
- [ ] Gestures work reliably
- [ ] Touch feedback immediate
- [ ] Loading states shown
- [ ] Error messages clear and helpful
- [ ] Follows platform conventions

### Performance

- [ ] App startup time < 2 seconds
- [ ] Frame rate > 30 FPS (target 60)
- [ ] Memory usage within limits
- [ ] No memory leaks
- [ ] Battery usage acceptable
- [ ] Network usage efficient
- [ ] Images optimized

### Compatibility

- [ ] Works on target OS versions
- [ ] Works on target screen sizes
- [ ] Works on target devices
- [ ] Handles different aspect ratios
- [ ] Handles notches and cutouts
- [ ] Works with system font scaling

### Network

- [ ] Works on WiFi
- [ ] Works on 4G/5G
- [ ] Handles slow networks gracefully
- [ ] Works offline (if applicable)
- [ ] Handles network switching
- [ ] Retries failed requests
- [ ] Shows appropriate loading states

### Security

- [ ] Data encrypted at rest
- [ ] Network traffic encrypted (HTTPS)
- [ ] Sensitive data not logged
- [ ] Secure storage for credentials
- [ ] Certificate pinning (if needed)
- [ ] No sensitive data in screenshots

### Platform-Specific

**iOS:**
- [ ] Follows Human Interface Guidelines
- [ ] Supports Dynamic Type
- [ ] Works with VoiceOver
- [ ] Handles iPhone and iPad layouts
- [ ] Proper app state handling

**Android:**
- [ ] Follows Material Design guidelines
- [ ] Supports different screen densities
- [ ] Works with TalkBack
- [ ] Handles back button correctly
- [ ] Proper activity lifecycle handling

## Key Takeaways

1. **Test on real devices:** Emulators miss real-world issues
2. **Handle fragmentation:** Test on top devices by market share
3. **Test network conditions:** Mobile networks are unreliable
4. **Follow platform conventions:** iOS and Android have different patterns
5. **Automate wisely:** Some testing must be manual (usability, gestures)

## Related Topics

- [[01_Performance_Testing]]: Mobile performance testing
- [[04_Accessibility_Testing]]: Mobile accessibility
- [[05_API_Testing]]: Testing mobile app APIs

## Existing Vault Connections

- [[software-engineering-note/05_Software_Testing/12_Mobile_Testing]]: Mobile testing techniques
