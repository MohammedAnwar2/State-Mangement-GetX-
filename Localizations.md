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

```dart
import 'package:firebase_project/main.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';

//SettingServices services = Get.find();
class LoclizationController extends GetxController
{
  // Locale? locale=sharePrefr?.getString("language")==null?Get.deviceLocale:Locale(sharePrefr!.getString("language")!);
   //Locale? locale=sharePrefr?.getString("language")==null?Get.deviceLocale:Locale(sharePrefr!.getString("language")!);
  //late Locale? locale=services.sharePref.getString("language")==null?Get.deviceLocale:Locale(services.sharePref.getString("language")!);
   Locale locale=sharePrefr?.getString("language")=="ar"?Locale("ar"):Locale("en");

  void changeLanguage(String langCode)
  {
    Locale locale = Locale(langCode);
    //services.sharePref?.setString("language", langCode);
   // services.sharePref.setString("language", langCode);
    sharePrefr?.setString("language", langCode);
    Get.updateLocale(locale);
  }
  @override
  void onInit() {
    // services.init();
    super.onInit();
  }
}
```
