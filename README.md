# Sensfrx Android SDK
# ![Sensfrx Logo](https://sensfrx.ai/assets/image/footer-logo-white.png)  

## Overview
Sensfrx Android SDK is an advanced fraud detection toolkit for mobile apps. It collects real-time device signals and behavioral data to detect fraudulent environments and risky user activity. With AI-powered threat analysis, Sensfrx enables proactive defense without impacting app performance.

👉 Supports **Android 9.0+ (API 28+)**, and integrates in minutes with just a few lines of code.

🔒 Ideal for fintech, ecommerce, and high-risk apps that require robust protection from fraud.

📘 [Learn more](https://sensfrx.ai)

---

## 🛠 Configuration Requirements

| Parameter             | Details                                                                 |
|-----------------------|-------------------------------------------------------------------------|
| `minSdkVersion`       | 28                                                                      |
| Permissions (optional)| `INTERNET`, `ACCESS_FINE_LOCATION`, `ACCESS_COARSE_LOCATION`           |
| Required Credentials  | Property ID and Secret from [Sensfrx Dashboard](https://client.sensfrx.ai/)     |
| Supported OS          | Android 9.0+ (Pie)                                                      |
| Docs                  | [Android SDK Integration Guide](https://docs.sensfrx.ai/en/integration/android-sdk) |

---

## 📦 Installation (Mobile Developer)

### Step 1: Add SDK Dependency

**Gradle**
```gradle
dependencies {
    implementation 'ai.sensfrx:sensfrx-sdk:1.0.3'
}
```

**Gradle Version Catalog (Optional)**
```toml
[versions]
sensfrx = "1.0.3"

[libraries]
sensfrx-sdk = { module = "ai.sensfrx:sensfrx-sdk", version.ref = "sensfrx" }
```

```gradle
dependencies {
    implementation(libs.sensfrx.sdk)
}
```

### Step 2: Ensure Maven Central is Included
```gradle
allprojects {
    repositories {
        mavenCentral()
    }
}
```

---

## ⚙️ SDK Configuration

### Initialize Sensfrx SDK

In your `Application` class:

#### 🔐 Get Property Secret
1. Register on [Sensfrx Dashboard](https://client.sensfrx.ai/)  
2. Login → Generate **Property ID** and **Property Secret**

### `sandboxMode` Explained

| Value   | Purpose                             |
|---------|-------------------------------------|
| `true`  | Testing mode (no real data sent)    |
| `false` | Production mode (real event tracking) |

#### Java
```java
public class MyApp extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        Sensfrx.configure(context: this, sandboxMode: false, propertyAndSecrect: "YOUR_PROPERTY_SECRET");
    }
}
```

#### Kotlin
```kotlin
class MyApp : Application() {
    override fun onCreate() {
        super.onCreate()
        Sensfrx.configure(context = this, sandboxMode = false, propertyAndSecrect = "YOUR_PROPERTY_SECRET")
    }
}
```

---

## Event Tracking

### 🖱 Click Events  
Track UI interaction patterns by monitoring user taps across the app. These interactions help Sensfrx assess behavioral consistency and identify anomalies like automated clicks, bots, or synthetic environments.

👉 Optional but recommended: To enable behavioral-based fraud signals (e.g., user interaction rate, tap rhythm, input inconsistencies), you must explicitly call the click tracking API.

❗Click events are not auto-tracked. You need to manually add the event on each screen where behavior analysis is needed.

Add this to the root view or inside each screen or fragment where you want to capture taps
#### Activity  
```java
@Override
public boolean onTouchEvent(MotionEvent event) {
    Sensfrx.trackClickEvent(event);
    return true;
}
```

#### Fragment  
```java
// Add in onCreateView:
Sensfrx.trackClickEvent(view.getRootView());
```

---

### 💳 Transaction Events  
Capture transaction-level activity with complete context such as payment method, items purchased, billing and shipping details. This allows Sensfrx to evaluate transaction legitimacy based on device and behavioral signals.

```kotlin
val jsonObject = JsonObject().apply {
    addProperty("transaction_id", "TXN123456")
    addProperty("transaction_type", "purchase")
    addProperty("email", "dummy@example.com")
    addProperty("first_name", "John")
    addProperty("last_name", "Doe")
    addProperty("username", "john_doe123")
    addProperty("payment_mode", "Credit Card")
    addProperty("payment_provider", "Visa")
    addProperty("card_fullname", "John Doe")
    addProperty("card_bin", "411111")
    addProperty("card_expire", "12/2026")
    addProperty("card_last", "1234")
    addProperty("cvv", "999")
    addProperty("phone_no", "9998887777")
    addProperty("transaction_amount", "1500")
    addProperty("transaction_currency", "INR")

    val itemsArray = JsonArray().apply {
        val item1 = JsonObject().apply {
            addProperty("item_id", "101")
            addProperty("item_name", "Wireless Headphones")
            addProperty("item_category", "Electronics")
            addProperty("item_quantity", "1")
            addProperty("item_price", "1200")
            addProperty("item_url", "https://example.com/item101")
        }
        val item2 = JsonObject().apply {
            addProperty("item_id", "102")
            addProperty("item_name", "USB-C Charger")
            addProperty("item_category", "Accessories")
            addProperty("item_quantity", "1")
            addProperty("item_price", "300")
            addProperty("item_url", "https://example.com/item102")
        }
        add(item1)
        add(item2)
    }
    add("items", itemsArray)

    addProperty("shipping_cost", "50")
    addProperty("shipping_country", "India")
    addProperty("shipping_state", "Maharashtra")
    addProperty("shipping_city", "Mumbai")
    addProperty("shipping_zip", "400001")
    addProperty("shipping_phone", "9998887777")
    addProperty("shipping_fullname", "John Doe")
    addProperty("shipping_method", "Express")

    addProperty("billing_country", "India")
    addProperty("billing_state", "Maharashtra")
    addProperty("billing_city", "Mumbai")
    addProperty("billing_zip", "400001")
    addProperty("billing_phone", "9998887777")
    addProperty("merchant_name", "Gadget World")
    addProperty("merchant_category", "Electronics")
    addProperty("merchant_id", "M123456")
    addProperty("merchant_country", "India")

    addProperty("ev", "attempt_succeeded")
    addProperty("card_type", "Visa")
    addProperty("tax_amount", "100")
    addProperty("discount_amount", "50")
    addProperty("invoice_id", "INV98765")
    addProperty("d_f", "device_fingerprint_abc123")
    addProperty("request_token", "token_xyz789")
    addProperty("user_id", "user_987654")
}

Sensfrx.trackTransactionEvent(jsonObject)
```

---

### ✅ Payment Success & ❌ Failure Tracking
After initiating the payment process with your payment SDK, you can track whether the transaction was successful or failed by utilizing Sensfrx.trackTransactionEvent in your onPaymentSuccess and onPaymentError methods. This helps in tracking the transaction's outcome and improves fraud detection.

#### Java
```java
@Override
public void onPaymentSuccess(String s) {
    jsonObject.addProperty("ev", "transaction_succeeded");
    Sensfrx.trackTransactionEvent(jsonObject);
}

@Override
public void onPaymentError(int i, String s) {
    jsonObject.addProperty("ev", "transaction_failed");
    Sensfrx.trackTransactionEvent(jsonObject);
}
```

#### Kotlin
```kotlin
override fun onPaymentSuccess(s: String) {
    jsonObject.addProperty("ev", "transaction_succeeded")
    Sensfrx.trackTransactionEvent(jsonObject)
}

override fun onPaymentError(i: Int, s: String) {
    jsonObject.addProperty("ev", "transaction_failed")
    Sensfrx.trackTransactionEvent(jsonObject)
}
```

---

### 📱 App State Events  
Track transitions such as app open, background, foreground, or closed. This data supports risk detection based on abnormal app usage behavior.

```kotlin
Sensfrx.trackAppEvent("OPEN") // or FOREGROUND / BACKGROUND / CLOSED
```

---

## 🔐 Backend Integration (For Backend Developers)

After you've secured the app and tracked user behaviors, you’ll need to handle user authentication on the backend. Sensfrx provides helper functions to collect tokens and fingerprints which you can send to your server during login and registration events.

### 🔑 Login Event

## Overview

Use this endpoint to report login events (`login_failed` or `login_succeeded`) to Sensfrx for fraud analysis and risk scoring.

---

## 📍 Endpoint

POST /v1/login/android

Host: https://m.sensfrx.ai

---

## 📄 Headers

| Key             | Value                       |
|----------------|-----------------------------|
| Authorization  | `Basic <base64(<api_key>:<secret>)>` |
| Content-Type   | `application/json`          |
| package   | `<package name>`          |

---

## 📦 Request Body

```json
{
  "ev": "login_succeeded",
  "uID": "USER_ID",
  "dID": "REQUEST_TOKEN_FROM_SDK",
  "d_f": "DEVICE_FINGERPRINTING_FROM_SDK",
  "uex": {
    "email": "user@example.com",
    "username": "username_here"
  }
}
```
## Sample PHP Implementation

```
<?php

$endpoint = "https://m.sensfrx.ai/v1/login/android";
$auth = base64_encode("your_api_key:your_secret");

$data = [
    "ev" => "login_succeeded",
    "uID" => "15",
    "dID" => "REQUEST_TOKEN_FROM_SDK",
    "d_f" => "DEVICE_FINGERPRINTING_FROM_SDK",
    "uex" => [
        "email" => "admin15@yopmail.com",
        "username" => "admin15"
    ]
];

$headers = [
    "Authorization: Basic $auth",
    "Content-Type: application/json",
    "package: package_name"
];

$ch = curl_init($endpoint);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($data));
$response = curl_exec($ch);
curl_close($ch);

echo $response;
?>
```

### 📝 Registration Event

## Overview

Use this endpoint to report Registration events (`register_failed` or `register_succeeded`) to Sensfrx for fraud analysis and risk scoring.

---

## 📍 Endpoint

POST /v1/register/android

Host: https://m.sensfrx.ai

---

## 📄 Headers

| Key             | Value                       |
|----------------|-----------------------------|
| Authorization  | `Basic <base64(<api_key>:<secret>)>` |
| Content-Type   | `application/json`          |
| package   | `<package name>`          |

---

## 📦 Request Body

```json
{
  "ev": "register_succeeded",
  "dID": "REQUEST_TOKEN_FROM_SDK",
  "d_f": "DEVICE_FINGERPRINTING_FROM_SDK",
  "rfs": {
    "uID": "USER_ID",
    "email": "user@example.com",
    "name": "username_here",
    "phone": "phone_here",
  }
}
```
## Sample PHP Implementation

```
<?php

$endpoint = "https://m.sensfrx.ai/v1/register/android";
$auth = base64_encode("your_api_key:your_secret");

$data = [
    "ev" => "register_succeeded",
    "dID" => "REQUEST_TOKEN_FROM_SDK",
    "d_f" => "DEVICE_FINGERPRINTING_FROM_SDK",
    "uex" => [
        "uID" => "15",
        "email" => "admin15@yopmail.com",
        "name" => "admin15"
    ]
];

$headers = [
    "Authorization: Basic $auth",
    "Content-Type: application/json",
    "package: package_name"
];

$ch = curl_init($endpoint);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($data));
$response = curl_exec($ch);
curl_close($ch);

echo $response;
?>
```
---

## 📬 Support

📧 [info@sensfrx.com](mailto:info@sensfrx.com)  

---

## 🪪 License

Licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0)

<p>
  <a href="https://sensfrx.ai" target="_blank">
    <img src="https://forthebadge.com/images/badges/built-for-android.svg" alt="Built for Android">
  </a>
</p>
