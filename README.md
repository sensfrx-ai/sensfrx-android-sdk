# Sensfrx Android SDK

![Sensfrx Logo](https://sensfrx.ai/assets/image/footer-logo-white.png)

## Overview
Sensfrx Android SDK is a cutting-edge fraud detection solution designed to enhance mobile app security by collecting real-time Android device information and tracking application interactions. It effectively identifies fraudulent activities such as app cloning, network evasion, device rooting, and emulator-based attacks.

By leveraging advanced behavioral analytics and AI-driven risk assessment, Sensfrx SDK enables businesses to safeguard user accounts, prevent fake registrations, and mitigate online transaction fraud. Its lightweight and efficient implementation ensures minimal impact on app performance while providing robust security against evolving threats.

With support for a wide range of Android devices (Android 9+), Sensfrx SDK offers seamless integration into your mobile application, requiring only a few lines of code to start detecting and preventing fraud in real time. Whether you're securing fintech apps, ecommerce platforms, or any user-centric application, Sensfrx provides a powerful layer of defense against malicious actors.

[Know more about us](https://sensfrx.ai)

## Configuration Requirements
- **minSdkVersion:** 28
- **Permissions (Optional):** `INTERNET`, `ACCESS_FINE_LOCATION`, `ACCESS_COARSE_LOCATION`
- **Required Credentials:** Property ID and Property Secret [Sensfrx Dashboard](https://sensfrx.ai)
- **Detailed documentation:** For detailed documentation [Android SDK Integration Guide](https://docs.sensfrx.ai/en/integration/android-sdk).
- **Supported OS Version:** Android 9.0+ (Pie)

---

## Installation (Mobile Developer)

### Add SDK Dependency
Add the following dependency in your **app-level** `build.gradle`:

```gradle
dependencies {
    implementation 'ai.sensfrx:sensfrx-sdk:1.0.3'
}
```

For projects using **Gradle Version Catalog**, add:

```toml
[versions]
sensfrx = "1.0.3"

[libraries]
sensfrx-sdk = { module = "ai.sensfrx:sensfrx-sdk", version.ref = "sensfrx" }
```

Then include it in your dependencies:

```gradle
dependencies {
    implementation(libs.sensfrx.sdk)
}
```

### Maven Repository
Sensfrx SDK is published on Maven Central. Ensure your project includes the Maven repository:

```gradle
allprojects {
    repositories {
        mavenCentral()
    }
}
```

---

## Configuration (Mobile Developer)

### Initialize the SDK

In your application's main class (subclass of `Application`), initialize Sensfrx:

To Get Your Property Secret
- Register on the [Sensfrx Dashboard](https://client.sensfrx.ai/).
- After registration, log in to your dashboard.
- Generate your Property ID and Property Secret from the dashboard. These credentials are required to configure the SDK in your app.

#### sandboxMode Parameter
sandboxMode (boolean):
- true: Use this mode for testing. In sandbox mode, the SDK will simulate interactions and data without affecting your production environment.
- false: Set this mode to false when you are ready to deploy the app in a live production environment. This ensures the SDK works with real data and integrates with the live backend services.

#### Java
```java
public class MyApp extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        // Replace "3764534847133568:VGF9TR2qfOnGCEghl" with your actual Property Secret from the Sensfrx Dashboard
        // sandboxMode set to false indicates production mode
        Sensfrx.configure(context: this, sandboxMode: false, propertyAndSecrect:"3764534847133568:VGF9TR2qfOnGCEghl");  // false indicates production mode
    }
}
```

#### Kotlin
```kotlin
class MyApp : Application() {
    override fun onCreate() {
        super.onCreate()
        // Replace "3764534847133568:VGF9TR2qfOnGCEghl" with your actual Property Secret from the Sensfrx Dashboard
        // sandboxMode set to false indicates production mode
        Sensfrx.configure(context: this, sandboxMode: false, propertyAndSecrect:"3764534847133568:VGF9TR2qfOnGCEghl")  // false indicates production mode
    }
}
```

Make sure to replace `3764534847133568:VGF9TR2qfOnGCEghl` with the Property Secret you obtained from the [Sensfrx Dashboard](https://client.sensfrx.ai/).

---

## Event Tracking
Sensfrx provides multiple event-tracking mechanisms:


### Click Events
Monitor user interactions with the app by tracking button clicks or any other view clicks. Click events help us read the behavioral data of users, providing insights into how users interact with different elements of your app.

For Activity click events:
#### Java / Kotlin
```java/Kotlin
@Override
public boolean onTouchEvent(MotionEvent event) {
    Sensfrx.trackClickEvent(event);
    return true;
}
```

For Activity click events:
#### Java / Kotlin
```java/Kotlin
// Add this line in your Fragment onCreateView 
Sensfrx.trackClickEvent(view.getRootView());
```


### Transaction Event Tracking
The Transaction Event provides insights into user purchases and financial transactions. By monitoring transaction data, Sensfrx can help detect suspicious activities like payment fraud, stolen credit cards, and transaction manipulation, ensuring secure and reliable financial interactions.

Benefits of Tracking Transaction Events:
- Payment Fraud Detection: Identifies abnormal transaction patterns such as high-value transactions, unusual payment methods, or frequent payment failures.
- Risk Management: Helps businesses assess transaction risks and detect compromised accounts or fraudulent payment methods.
- Transaction Integrity: Ensures that only legitimate purchases are processed, protecting both users and merchants from fraud.


#### Java / Kotlin
```java/Kotlin
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

// Track the transaction event
Sensfrx.trackTransactionEvent(jsonObject)
```

### Transaction Success/Failure Handling
After initiating the payment process with your payment SDK, you can track whether the transaction was successful or failed by utilizing Sensfrx.trackTransactionEvent in your onPaymentSuccess and onPaymentError methods. This helps in tracking the transaction's outcome and improves fraud detection.

#### Java
```java
// Called when payment is successful
@Override
public void onPaymentSuccess(String s) {
    // Set the event to transaction_succeeded
    jsonObject.addProperty("ev", "transaction_succeeded");

    // Send the complete transaction details jsonrequest again with updated flag
    Sensfrx.trackTransactionEvent(jsonObject);
}

// Called when payment fails
@Override
public void onPaymentError(int i, String s) {
    // Set the event to transaction_failed
    jsonObject.addProperty("ev", "transaction_failed");

    // Send the complete transaction details jsonrequest again with updated flag
    Sensfrx.trackTransactionEvent(jsonObject);
}
```

#### Kotlin
```Kotlin
// Called when payment is successful
override fun onPaymentSuccess(s: String) {
    // Set the event to transaction_succeeded
    jsonObject.addProperty("ev", "transaction_succeeded")

    // Send the complete transaction details jsonrequest again with updated flag
    Sensfrx.trackTransactionEvent(jsonObject)
}

// Called when payment fails
override fun onPaymentError(i: Int, s: String) {
    // Set the event to transaction_failed
    jsonObject.addProperty("ev", "transaction_failed")

    // Send the complete transaction details jsonrequest again with updated flag
    Sensfrx.trackTransactionEvent(jsonObject)
}

```

### Track App State Events
Tracking the app's state allows you to monitor when your app is opened, closed, or moved to the background. This information is crucial for detecting unusual app usage patterns, improving user engagement, and strengthening security measures.



#### Java/Kotlin
```java/Kotlin
// Track the app state as OPEN when the app is launched
Sensfrx.trackAppEvent("OPEN");
```

Supported App States
- OPEN: Tracks when the app is launched.
- FOREGROUND: Detects when the app returns from the background.
- BACKGROUND: Detects when the app is minimized or sent to the background.
- CLOSED: Tracks when the app is fully closed.


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


Fore more details checkout the demo application or connect with our [support team](https://sensfrx.ai).

## Error Handling
To ensure smooth SDK integration, consider the following common issues:

- **Network Issues:** Ensure the app has internet access and is not blocked by firewalls.
- **Invalid Keys:** Verify the correct secret key.

Enable detailed logging using Android Studio Logcat for debugging.

---

## Analytics and Dashboard
After integrating Sensfrx into your app, you can view real-time fraud detection analytics, user behavior, and transaction logs directly on the Sensfrx Dashboard. This allows you to monitor suspicious activities and make data-driven decisions for fraud prevention.

Access your [Sensfrx Dashboard](https://sensfrx.ai)

---

## SDK Publication Details
- **Group ID:** ai.sensfrx
- **Artifact ID:** sensfrx-sdk
- **Version:** 1.0.3
- **Repository:** Maven Central
- **License:** Apache License 2.0
- **Project URL:** [https://sensfrx.ai](https://sensfrx.ai)

---

## Support
For assistance, contact our support team at [info@sensfrx.com](mailto:info@Sensfrx.com) or visit our developer portal.

## License
This project is licensed under the Apache License 2.0 - see the LICENSE file for details.

<p>
  <a href="https://sensfrx.ai" target="_blank">
    <img src="https://forthebadge.com/images/badges/built-with-love.svg" alt="Built with Love">
    <img src="https://forthebadge.com/images/badges/built-for-android.svg" alt="Built for Android">
  </a>
</p>


