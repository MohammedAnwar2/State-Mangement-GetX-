كيف ممكن ان يجتمع ال GetService  و ال Bindings
```dart
void main(){
 //  await initlization();
  runApp(MyApp());
}
Future initlization()async{
  await Get.putAsync(() => SettingService().init());
}
class MyApp extends StatelessWidget {
   MyApp({Key? key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      locale:Get.deviceLocale,
      translations: MyLanguage(),
      initialBinding: MyBindings(),
      getPages: [
        GetPage(name: "/", page: () => Home())
      ],
    );
  }
}
```
