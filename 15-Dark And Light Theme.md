# Controller class
```dart
class ThemeController extends GetxController {
  final _key = 'isDarkMode';
  ThemeMode get theme => _loadTheme() ? ThemeMode.dark : ThemeMode.light;
  bool _loadTheme() => SettingServices.initServices.read(_key) ?? false;
  void saveTheme(bool isDarkMode) => SettingServices.initServices.write(_key, isDarkMode);
  void changeTheme(ThemeData theme) => Get.changeTheme(theme);
  void changeThemeMode(ThemeMode themeMode) => Get.changeThemeMode(themeMode);
}
```
# Constant Colors
```dart
class AppColors {
  AppColors._();

  // Dark Theme colors
  static const Color darkGrey = Color(0xff303041);
  static const Color lightGrey = Color(0xFF3D3A50);
  static const Color white = Color(0xFF0EA2F6);
  static const Color burgundy = Color(0xFF880d1e);
  static const Color spaceCadet = Color(0xFFF4FCFE);

  // Light Theme Colors
  static const Color babyPink = Color(0xFFFECEE9);
  static const Color lavender = Color(0xFFEB9FEF);
  static const Color gunMetal = Color(0xFF545677);
  static const Color spaceBlue = Color(0xFF03254E);
  static const Color darkBlue = Color(0xFF011C27);
}
```
# Dark and Light Theme
```dart
class Themes {
  static final lightTheme = ThemeData(
    scaffoldBackgroundColor: Colors.greenAccent,
    elevatedButtonTheme: ElevatedButtonThemeData(style: ButtonStyle(backgroundColor: MaterialStateProperty.all(Colors.orangeAccent))),

    appBarTheme: AppBarTheme(color: Colors.brown),
    colorScheme: const ColorScheme.light(
      primary: AppColors.burgundy,
      secondary: AppColors.spaceBlue,
      onSecondary: AppColors.spaceCadet,
      background: AppColors.babyPink,
    ),
  );

  static final darkTheme = ThemeData(
      appBarTheme: AppBarTheme(color: Colors.yellowAccent),
    elevatedButtonTheme: ElevatedButtonThemeData(style: ButtonStyle(backgroundColor: MaterialStateProperty.all(Colors.purpleAccent))),
    scaffoldBackgroundColor: Colors.green,
      colorScheme: const ColorScheme.dark(
        primary: AppColors.spaceBlue,
        secondary: AppColors.burgundy,
        onSecondary: AppColors.spaceCadet,
        background: AppColors.spaceCadet,
      ));
}
```
# Services class
```dart
class SettingServices extends GetxService
{
  static SettingServices initServices = Get.find();
  late SharedPreferences pref ;
  Future<SettingServices> init()async{
    pref = await SharedPreferences.getInstance();
    return this;
  }
  bool? read<T> (String key) {
     return pref.getBool(key);
  }
  void write(String key, dynamic value)
  {
    pref.setBool(key, value);
  }
}
```
# Binding class
```dart
class HomeBinding extends Bindings {
  @override
  void dependencies() {
    Get.lazyPut(()=> SettingServices(),fenix: true));
    Get.lazyPut(()=> ThemeController(),fenix: true));
  }
}
```
# Mina class
```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();//-------------> this one
  await Get.putAsync(() async => SettingServices().init());//-------------> this one
  HomeBinding().dependencies();//-------------> this one
  return runApp(MyApp());
}

class MyApp extends StatelessWidget {
  MyApp({Key? key}) : super(key: key);

  final themeController = Get.find<ThemeController>();//-------------> this one

  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'First Method',
      theme: Themes.lightTheme,//-------------> this one
      darkTheme: Themes.darkTheme ,//-------------> this one
      themeMode: themeController.theme,//-------------> this one
      initialBinding: HomeBinding(),//-------------> this one
      initialRoute: "/page1",
      getPages: [
        GetPage(name: "/page1", page: () => DarkLightMode(),),
      ],
    );
  }
}
```
# Dark Light Mode Page
```dart
class DarkLightMode extends StatelessWidget {
  DarkLightMode({super.key});

  final themeController = Get.find<ThemeController>();//-------------> this one

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Page'),
      ),
      body: SizedBox(width: Get.width, child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          ElevatedButton(onPressed: () {//-------------> this one
              if(!Get.isDarkMode){//-------------> this one
                    themeController.changeTheme(Themes.darkTheme);//-------------> this one
                    themeController.changeThemeMode(ThemeMode.dark);//-------------> this one
                    themeController.saveTheme(true);//-------------> this one
              }
              else
                {
                  themeController.changeTheme(Themes.lightTheme);//-------------> this one
                  themeController.changeThemeMode(ThemeMode.light);//-------------> this one
                  themeController.saveTheme(false);//-------------> this one
                }
          }, child: const Text("Dark Theme")),
          SizedBox(height: 50,),
          Container(alignment: Alignment.center,height: 50,width: 50,color: Theme.of(context).colorScheme.primary,//-------------> this one
          child: Text("1234"),)
        ],
      )),
    );
  }
}
```
