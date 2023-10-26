# Translations class
```dart
class LocalizationModel extends Translations
{
  @override
  // TODO: implement keys
  Map<String, Map<String, String>> get keys => {
    "ar":ar,
    "en":en,
  };
}
Map<String,String> ar ={"1": "الصفحة الرئيسة", "2": "عربي", "3": "انجليزي"};

Map<String,String> en ={"1": "Home Page", "2": "Arabic", "3": "English"};
```
# Contrller class
```dart
class MyLanguageController extends GetxController {
  Locale local= SettingServices.initServices.pref.getString("lang")==null?Get.deviceLocale!:Locale(SettingServices.initServices.pref.getString("lang")!);

  changeLangauge(String lang) {
    local = Locale(lang);
    SettingServices.initServices.pref.setString("lang",lang);
    Get.updateLocale(local);
  }
}
```
# Service class
```dart
class SettingServices extends GetxService
{
  static SettingServices initServices = Get.find();
  late SharedPreferences pref ;
  Future<SettingServices> init()async{
    pref = await SharedPreferences.getInstance();
    return this;
  }
}
```
# Main class
```dart
void main()async {
  WidgetsFlutterBinding.ensureInitialized();// <<---------- this one
  await Get.putAsync(() async => SettingServices().init());// <<---------- this one
  HomeBinding().dependencies();// <<---------- this one
  return runApp( MyApp());
}

class MyApp extends StatelessWidget {
  MyApp({Key? key}) : super(key: key);
  
  MyLanguageController loacal = Get.find(); // <<---------- this one
  
  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      debugShowCheckedModeBanner: false,
      initialBinding: HomeBinding(),// <<---------- this one
      initialRoute: "/page1",
      locale:loacal.local , // <<---------- this one
      translations: LocalizationModel(),// <<---------- this one
      getPages: [
        GetPage(name: "/page1",page: () => SettingChangeLangauge(),),
      ],
    );
  }
}
```
# SettingChangeLangauge page
```dart
class SettingChangeLangauge extends GetView<MyLanguageController> {
  const SettingChangeLangauge({super.key});
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('1'.tr),
      ),
      body:  SizedBox(width: Get.width,child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          ElevatedButton(onPressed: (){
            controller.changeLangauge("ar");
          }, child:  Text("2".tr)),
          ElevatedButton(onPressed: (){
            controller.changeLangauge("en");
          }, child:  Text("3".tr)),
        ],
      )),
    );
  }
}
```
# Binding class
```dart
class HomeBinding extends Bindings {
  @override
  void dependencies() {
    Get.lazyPut(()=> SettingServices());
    Get.lazyPut(()=> MyLanguageController());
  }
}
```
