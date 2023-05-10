- تشبه بشكل كبييير لل GetxController بحيث انها تشترك معها في ال life cycle لكن ليست مهمتها ان تكون اللوجك للصفحات بل مهمتها ببساطة شديده انها reachable اي تسهّل الوصول للمتغيرات والدوال التي بداخلها و تستخدم بالغالب عند عمل initialised لل Application 

- بمعنى أول ما نشغل ال Application ممكن نعمل initialised لل services الموجوده عندنا مثل
- 1-hive.
- 2-shared preference.
- 3-get storage.
- 4-more connection.


-Easy Access to Services: Once registered, you can access the service using Get.find() or Get.put() from anywhere in your app. This allows you to easily access the methods and properties of your service, making it convenient to perform tasks such as making API calls, managing state, handling data persistence, and more.
- باختصار : اي شيء أنا بحاجة انه يشتغل أول ما يشتغل ال application نستعمل ال GetxService 


---------------------------------------------------------------------GetxService class----------------------------------------------------------------------
```dart
import 'package:get/get.dart';
import 'package:shared_preferences/shared_preferences.dart';

class SettingServices extends GetxService
{
  late SharedPreferences sharePref;
  Future<SettingServices>init()async{
    sharePref = await SharedPreferences.getInstance();
    return this;
  }

}
```
-------------------------------------------------------GetxService in main function-----------------------------------------------------------------------
```dart
void main() async{
  WidgetsFlutterBinding.ensureInitialized();
  await initServices();
  runApp( MyApp());
}

Future initServices()async{
  await Get.putAsync(() => SettingServices().init());
}
```
--------------------------------------------------------------------How to access--------------------------------------------------------------------------
- توجد هناك طريقتين  
1- بأستخدام Get.find() بالشكل التالي
```dart
SettingServices controller = Get.find();
```
2- أو بأستخدام ال GetView بالشكل التالي
```dart
class اسم الكلاس extends GetView {
}
```
توفر الcontroller instance اي فينا نتخلى عن ال Get.find

