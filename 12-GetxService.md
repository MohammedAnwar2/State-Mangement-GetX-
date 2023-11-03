- تشبه بشكل كبييير لل GetxController بحيث انها تشترك معها في ال life cycle لكن ليست مهمتها ان تكون اللوجك للصفحات بل مهمتها ببساطة شديده انها reachable اي تسهّل الوصول للمتغيرات والدوال التي بداخلها و تستخدم بالغالب عند عمل initialised لل Application 

- بمعنى أول ما نشغل ال Application ممكن نعمل initialised لل services الموجوده عندنا مثل
- 1-hive.
- 2-shared preference.
- 3-get storage.
- 4-more connection.


- Easy Access to Services: Once registered, you can access the service using Get.find() or Get.put() from anywhere in your app. This allows you to easily access the methods and properties of your service, making it convenient to perform tasks such as making API calls, managing state, handling data persistence, and more.
- باختصار : اي شيء أنا بحاجة انه يشتغل أول ما يشتغل ال application نستعمل ال GetxService 


---------------------------------------------------------------------GetxService class using SharedPreferences----------------------------------------------------------------------
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
* الافضل كذا 
```dart
void main() async{
  WidgetsFlutterBinding.ensureInitialized();
  await Get.putAsync(() async => SettingServices().init());
  await initServices();
  runApp( MyApp());
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
class كلاس ما extends GetView<اسم الكلاس اللي بنحقن منه> {
}
```
توفر الcontroller instance اي فينا نتخلى عن الGet.find


# ملاحظه مهمة جدا
* اذا ظهر عندك الخطأ التالي 
```dart
PlatformException(channel-error, Unable to establish connection on channel., null, null) - saving to local storage
```
فالحل كما يلي  
```dart
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:tools="http://schemas.android.com/tools"
package="com.example.app">


<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```
- نضيفهن في <Inside android->app->src->main->AndroidManifest.xml  قبل ال <application

التوضيح بشكل كامل في الموقع التالي
```dart
https://stackoverflow.com/questions/72704184/platformexceptionchannel-error-unable-to-establish-connection-on-channel-nul
```
```dart
//don't use GerView in GetMaterialApp instead of StatelessWidget
  ```










---------------------------------------------------------------------GetxService class using GetStorage-----------------------------------------------
```dart
import 'package:get/get.dart';
import 'package:get_storage/get_storage.dart';

class StorageService extends GetxService {
  static StorageService instance = Get.find<StorageService>();
  late GetStorage _box;

  Future<StorageService> init() async {
    _box = GetStorage();
    return this;
  }

  T read<T>(String key) {
    return _box.read(key);
  }

  void write(String key, dynamic value) async {
    await _box.write(key, value);
  }
}
```
-------------------------------------------------------GetxService in main function------------------------------------------------------------------
```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await GetStorage.init();
  await Get.putAsync(() => StorageService().init());
  runApp(NoteAPP());
}
```
-------------------------------------------------------GetxService in Binding-----------------------------------------------------------------------
```dart
class AppBinding extends Bindings {
  @override
  void dependencies() {
    Get.put(StorageService());
  }
}
```




