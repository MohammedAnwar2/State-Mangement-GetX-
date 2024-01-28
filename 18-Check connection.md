# controller
```dart
abstract class NetworkController extends GetxController {
  Future<void> initConnectivity();
  _updateConnectionStatus(ConnectivityResult result);
}

class ImpNetworkController extends NetworkController {
  static ImpNetworkController instance = Get.find();
  final connectionStatus = 0.obs;
  final Connectivity _connectivity = Connectivity();
  late StreamSubscription<ConnectivityResult> _connectivitySubscription;
  @override
  void onInit() {
    super.onInit();
    initConnectivity();
    _connectivitySubscription =
        _connectivity.onConnectivityChanged.listen(_updateConnectionStatus);
  }

  @override
  void onClose() {
    _connectivitySubscription.cancel();
    super.onClose();
  }

  // Platform messages are asynchronous, so we initialize in an async method.
  Future<void> initConnectivity() async {
    late ConnectivityResult result;
    // Platform messages may fail, so we use a try/catch PlatformException.
    try {
      result = await _connectivity.checkConnectivity();
    } on PlatformException catch (e) {
      Get.snackbar("error", 'Couldn\'t check connectivity status $e');
    }
    return _updateConnectionStatus(result);
  }

  _updateConnectionStatus(ConnectivityResult result) {
    switch (result) {
      case ConnectivityResult.wifi:
        connectionStatus.value = 1;
        break;
      case ConnectivityResult.mobile:
        connectionStatus.value = 2;
        break;
      case ConnectivityResult.none:
        connectionStatus.value = 0;
        break;
      default:
        Get.snackbar("Network error", 'Failed to get network connection');
    }
  }
}

```

# Binding
```dart
class AppBinding extends Bindings {
  @override
  void dependencies() {
  Get.lazyPut(() => ImpNetworkController(), fenix: true);
  }
}

```
# main function
```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Get.putAsync(() async => SettingServices().init());
  HomeBinding().dependencies();//----------------------------------<<<<
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  MyApp({Key? key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      debugShowCheckedModeBanner: false,
      initialBinding: HomeBinding(),
      getPages: route,
    );
  }
}

```
# Home Page
```dart
class HomePage extends StatelessWidget {
  const HomePage({super.key});
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
          title: const Text(
        "Check connection",
        style: TextStyle(color: Colors.black),
      )),
      body: Center(
        child: CustomContainer(
            widget: Obx(() =>
                ImpNetworkController.instance.connectionStatus.value == 1 ||
                        ImpNetworkController.instance.connectionStatus.value == 2
                    ? Text("Connected")
                    : Text("Note Connected"))),
      ),
    );
  }
}

/*
CustomContainer(
            widget: Obx(() =>
                NetworkController.instance.connectionStatus.value == 0
                    ? Text("not connected ")
                    : Text("connected ")))
*/
```
# Simple Example
# https://github.com/MohammedAnwar2/connection_theme_localization_project.git

