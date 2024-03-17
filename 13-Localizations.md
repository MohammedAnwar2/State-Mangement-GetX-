# Translations class
```dart
import 'package:get/get.dart';

class LocalizationModel extends Translations {
  @override
  // TODO: implement keys
  Map<String, Map<String, String>> get keys => {
        "ar": ar,
        "en": en,
      };
}

Map<String, String> ar = {"1": "اختر اللغة", "2": "عربي", "3": "انجليزي"};

Map<String, String> en = {
  "1": "Choose Language",
  "2": "Arabic",
  "3": "English"
};

```
# Contrller class
```dart
class MyLanguageController extends GetxController {
  Locale? language;
  MyServices myServices = Get.find<MyServices>();

  changeLangauge(String lang) {
    language = Locale(lang);
    myServices.sharePref.setString(AppKey.language, lang);
    Get.updateLocale(language!);
  }

  @override
  void onInit() {
    String? sharedPrefLang = myServices.sharePref.getString(AppKey.language);
    if (sharedPrefLang == "ar") {
      language = const Locale("ar");
    } else if (sharedPrefLang == "en") {
      language = const Locale("en");
    } else {
      language = Locale(Get.deviceLocale!.languageCode);
      // myServices.sharePref.setString(
      //     AppKey.language,
      //     Get.deviceLocale!
      //         .languageCode); //عرفه افضل في داله الخدمات <services>
    }
    super.onInit();
  }
}

```
# Service class
```dart
class MyServices extends GetxService {
  late SharedPreferences sharePref;

  Future<MyServices> init() async {
    sharePref = await SharedPreferences.getInstance();
    return this;
  }
}

initServices() async {
  await Get.putAsync(() => MyServices().init());
}

```
# Main class
```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();//-----------> here
  await initServices();//-----------> here
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  MyApp({Key? key}) : super(key: key);
  MyLanguageController controller = Get.put(MyLanguageController());//-----------> here
  @override
  Widget build(BuildContext context) {
    return ScreenUtilInit(
      designSize: const Size(360, 690),
      minTextAdapt: true,
      splitScreenMode: true,
      builder: (_, child) {
        return GetMaterialApp(
          translations: LocalizationModel(),//-----------> here
          locale: controller.language,//-----------> here
          debugShowCheckedModeBanner: false,
          title: 'First Method',
          getPages: getPages,
          theme: ThemeData(
              fontFamily: "PlayfairDisplay",
              textTheme: TextTheme(
                  bodyLarge: TextStyle(
                      color: AppColor.grey, fontSize: fontSize(14), height: 2),
                  displayLarge: TextStyle(
                      color: AppColor.black,
                      fontSize: fontSize(20),
                      fontWeight: FontWeight.bold))),
          home: Onbording(),
        );
      },
    );
  }
}

```
# SettingChangeLangauge page
```dart
class Language extends GetView<MyLanguageController> {
  const Language({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        body: SafeArea(
            child: Container(
      width: double.infinity,
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.center,
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Text(
            "1".tr,
            style: Theme.of(context).textTheme.displayLarge,
          ),
          verticalSizedBox(25),
          CustomLanguageButton(
            buttonName: "2".tr,
            onTap: () {
              controller.changeLangauge("ar");
            },
          ),
          verticalSizedBox(10),
          CustomLanguageButton(
            buttonName: "3".tr,
            onTap: () {
              controller.changeLangauge("en");
            },
          )
        ],
      ),
    )));
  }
}

```

