```dart
import 'package:get/get.dart';
import '../Middleware/SuperMaget.dart';
import '../Pages/HomePage.dart';
import '../Pages/New Pages/login.dart';
import '../Pages/New Pages/signin.dart';
import '../Pages/page_1.dart';
import '../Pages/page_2.dart';

List<GetPage<dynamic>> route = [
  GetPage(
    name: AppRoute.HomePage,
    page: () => MyHomePage(),
  ),
  GetPage(name: AppRoute.Page1, page: () =>Page_1()),
  GetPage(name: AppRoute.Page2, page: () => Page_2()),
  GetPage(name: AppRoute.Login, page: () => const Login()),
  GetPage(name: AppRoute.Signin, page: () => const Signin()),
  GetPage(name: AppRoute.SuperMarket, page: () => const SuperMarket()),

];

class AppRoute {
  static const String HomePage = "/";
  static const String Page1 = "/page1";
  static const String Page2 = "/page2";
  static const String Login = "/Login";
  static const String Signin = "/Signin";
  static const String SuperMarket = "/SuperMarket";
}
```
