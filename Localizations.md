# Translations class
```dart
class MyLanguage extends Translations {
  @override
  Map<String, Map<String, String>> get keys => {"ar": ar, "en": en};
}

Map<String, String> ar = {
  "1":"محمد",
  "2":"سالم",
};
Map<String, String> en = {
  "1":"Mohammed",
  "2":"Salem",
};
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
  WidgetsFlutterBinding.ensureInitialized();
  await Get.putAsync(() async => SettingServices().init());// <<---------- this one
  HomeBinding().dependencies();
  return runApp( MyApp());
}

class MyApp extends StatelessWidget {
   MyApp({Key? key}) : super(key: key);
   MyLanguageController loacal = Get.find(); // <<---------- this one
  @override
  Widget build(BuildContext context) {
    //Set the fit size (Find your UI design, look at the dimensions of the device screen and fill it in,unit in dp)
    return ScreenUtilInit(
      designSize: const Size(360, 690),
      minTextAdapt: true,
      splitScreenMode: true,
      // Use builder only if you need to use library outside ScreenUtilInit context
      builder: (_ , child) {
        return GetMaterialApp(
          debugShowCheckedModeBanner: false,
          title: 'First Method',
          // You can use the library anywhere in the app even in theme
          theme: ThemeData(
            primarySwatch: Colors.blue,
            textTheme: Typography.englishLike2018.apply(fontSizeFactor: 1.sp),
          ),
          initialBinding: HomeBinding(),
          home: child,
          initialRoute: "/page1",
          locale:loacal.local , // <<---------- this one
          translations: LocalizationModel(),// <<---------- this one
          getPages: [
            GetPage(name: "/page1",page: () => SettingChangeLangauge(),),
          ],
        );
      },
      child: HomePage(),
    );
  }
}
```


