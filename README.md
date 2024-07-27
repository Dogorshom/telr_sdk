# Telr SDK for Flutter 🚀 

Telr SDK allows you to easily integrate the Telr payment gateway into your Flutter applications.

![alt text](https://i.ibb.co/nnYVHd6/Telr-XFlutter.png)

### Platform Compatibility 

The Telr SDK supports the following platforms:
- **iOS📱**
- **Android📱**
- **Web🌐**

## Features 🫱🏼‍🫲🏻

- Simple and easy-to-use widget for initiating payment processes.
- Callback functions for handling payment success, failure, cancellation, and errors.
- Supports both light and dark modes.
- Customizable environments for development and production.

## Installation ⚙️

Add the following dependency to your `pubspec.yaml` file:

```yaml
dependencies:
  telr_sdk: latestVersion
```

## Telr Order Model Parameters 📖

### Required Parameters

- **order** (TelrOrderDetails, required): Object containing the order details.
  - **cartId** (String, required): The Cart ID or Order ID, unique identifier for the order.
  - **test** (String, required): Test mode indicator, 0 = Live, 1 = Test, default is `1`.
  - **amount** (String, required): The amount of the order.
  - **currency** (String, required): The currency of the order.
  - **description** (String, required): The description of the order.
  - **tranType** (String?, optional): Transaction type. Optional, configured default in merchant admin is used if not set.
- **returnUrls** (TelrReturnUrls, required): Object containing the return URLs.
  - **authorised** (String, required): The authorised URL.
  - **declined** (String, required): The declined URL.
  - **cancelled** (String, required): The cancelled URL.
- **method** (String?, optional): Method name, default is `create`.
- **store** (int, required): Store ID.
- **authKey** (String, required): Authentication key that provided by Telr integration team.

### Optional Parameters

- **framed** (int?): Framed value for display options:
  - 0 - Standard full screen payment page.
  - 1 - Framed payment page.
  - 2 - Framed payment page as the initial display but can break out to a full page display if required.

## Usage 🗠

Just use `Navigator.push` normally and navigate to `TelrWidget` and let the SDK do the rest!

Here's a basic example of how to use the TelrWidget in your Flutter application:

```dart
import 'dart:developer';
import 'package:flutter/material.dart';

void main() async {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Telr Pay Demo',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Color.fromARGB(255, 17, 86, 19)),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'Flutter Telr Pay Demo'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});
  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.white,
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        crossAxisAlignment: CrossAxisAlignment.center,
        children: <Widget>[
          Center(
            child: Text(
              'You are paying 100 SAR',
              style: Theme.of(context).textTheme.titleLarge?.copyWith(color: Colors.black),
            ),
          ),
          const SizedBox(height: 20),
          ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => TelrWidget(
                      telrOrderModel: TelrOrderModel(
                          order: TelrOrderDetails(
                              cartId: "123", test: "1", amount: "amount", currency: "SAR", description: "description"),
                          returnUrls: TelrReturnUrls(
                              authorised: "https://sayaratech.com/success",
                              declined: "https://sayaratech.com/failed",
                              cancelled: "https://sayaratech.com/cancelled"),
                          store: 1234,
                          authKey: "mykey1234",
                          framed: 0,
                          method: "create"),
                      isDarkMode: true,
                      onPaymentCancel: () {
                        log('Payment Cancel: from the on payment cancel callback');
                        ScaffoldMessenger.of(context).showSnackBar(
                          const SnackBar(
                            content: Text('You have canceled the payment'),
                            // backgroundColor: Colors.red,
                          ),
                        );
                      },
                      onPaymentSuccess: () {
                        log('Payment Success: from the on payment success callback');
                        ScaffoldMessenger.of(context).showSnackBar(
                          const SnackBar(
                            content: Text('You have successfully paid with Telr'),
                            // backgroundColor: Colors.green,
                          ),
                        );
                      },
                      onPaymentFailure: () {
                        log('Payment Failure: from the on payment failure callback');
                        ScaffoldMessenger.of(context).showSnackBar(
                          const SnackBar(
                            content: Text('Payment failed, please try again later'),
                            // backgroundColor: Colors.red,
                          ),
                        );
                      },
                      onPaymentError: () {
                        log('Payment Error: from the on payment error callback');
                        ScaffoldMessenger.of(context).showSnackBar(
                          const SnackBar(
                            content: Text('An error occurred while processing your payment'),
                            // backgroundColor: Colors.red,
                          ),
                        );
                      },
                    ),
                  ),
                );
              },
              child: const Text('Pay Now With Telr'))
        ],
      ),
    );
  }
}

```

### Support

If you encounter any issues or have questions, please contact our support team at [support@telr.com](mailto:support@telr.com).

### Contributions

Contributions to the SDK are welcome! You can open an issue or submit a pull request on GitHub if you have any improvements or bug fixes.

---

For more details, please refer to the [official documentation](https://docs.telr.com/reference/get-started).
