- ال Middleware هي نظام الصلاحيات
- راح تساعدنا في تنظيم الصلاحيات ، مثلا ال admin, مشرف و....و... ال Middleware راح تساعدنا كثيير بالاضافة الى ال peoirty بإمكانك عمل اكثر من Middleware لصفحة واحده

- ملاحظة مهمه : الحالة الافتراضيه في حالة أنا عملت / فان الحالة الافتراضيه هي / يعني لو مسحته عادي ، راح يعتبر أول صفحة ينتقل لها هي ال /


لاستدعاء ال sharedPreferences من اي مكان بالبرنامج لازم نجعلها public و ذلك بالشكل التالي

```dart
sharedPreferences? sharepref;
void main()async{
          WidgetsFlutterBinding.ensureInitialized();
          sharepref = await sharedPreferences.getInstance();
}
```

- لما يكون عندنا async و await في دالة ال main لازم ودائما نستخدم ال
```dart
WidgetsFlutterBinding.ensureInitialized();
```
من أجل ان نتأكد ان كل الأمور تم استدعاءها بشكل جيد ، وهي علشان نتأكد من عملية ال initialization

- بهذه الحالة ، الان اقدر اصل لل sharedPreferences من اي مكان بالبرنامج بدون ما نعمل عملية استدعاء لها 



لتخزين القيم في sharedPreferences نستعمل
```dart
sharepref!.setString("id","1");
```
لاسترجاع القيم من sharedPreferences نستعمل
```dart
sharepref!.getString("id");
```

لإلغاء ال sharedPreferences نستعمل
```dart
sharepref!.clear();
```
فنكشن جاهزة تتحكم بالروت ، اسمها redirect وترجع دالة اسمها RouteSettings وتقبل name:"اسم الروت"

للتحقق من الصلاحيات ( Middleware) قبل ما يدخل البرنامج نعمل الآتي

```dart
 getPages: [
        GetPage(name: "/", page: () => Login(), middlewares: [AutoLogin()]),
        GetPage(name: "/Signup", page: () => Signup()),
]
```

```dart
class AutoLogin extends GetMiddleware {
  @override
  RouteSettings? redirect(String? route) {
    if (sharepref!.getString("id") == "1") {
      return const RouteSettings(name: "/Signup");
    }
  }
}

or

class AutoLogin extends GetMiddleware {
  @override
  RouteSettings? redirect(String? route) {
    if (sharepref!.getString("id") != null) {
      return const RouteSettings(name: "/Signup");
    }
  }
}
```
------------------------------------------------------------------------priority--------------------------------------------------------------------------
عمل اثنين Middleware وعمل بينهم تفضيل باستخدام 
```dart
@override
int? get priority=> 1 ;
ملاحظة مهمه : كلما زاد الرقم كلما قلّة الأهمية او الافضلية ، و كلما قل الرقم كلما زادت الأهمية
getPages: [
        GetPage(
            name: "/",
            page: () => Login(),
            middlewares: [SuperMarket(), AutoLogin() ]),
            
           GetPage(name: "/Signup", page: () => Signup()),
           GetPage(name: "/Market", page: () => Market()),
      ]
```

Middleware 👇🏻👇🏻👇🏻👇🏻👇🏻


```dart
class AutoLogin extends GetMiddleware {
  @override
  int? get priority => 2;
  @override
  RouteSettings? redirect(String? route) {
    if (sharepref!.getString("id") == "1") {
      return const RouteSettings(name: "/Signup");
    }
  }
}
```

```dart
class SuperMarket extends GetMiddleware {
  @override
  // TODO: implement priority
  int? get priority => 1;
  String x = "2";
  @override
  RouteSettings? redirect(String? route) {
    if (x == "2") {
      return const RouteSettings(name: "/Market");
    }
  }
}
```

# - ملاحظات مهمه جدا

1- نلاحظ ان الكلاس SuperMarket أكثر أهمية من AutoLogin بسبب ان ال priority الخاصة بSuperMarket اقل من ال priority الخاصة بAutoLogin

2-في حالة عملنا ال priority متساويين لكل الكلاسات فان الاهمية تؤخذ للكلاس الذي يوضع أولا في ال   
```dart
middlewares: [SuperMarket(), AutoLogin() ]
اللي هو هنا ()SuperMarket 
```

3- لو مثلا SuperMarket أكثر أهمية من AutoLogin و لكن شرط ال SuperMarket مش محقق فمباشرة تنتقل الأهمية لل Middleware التالية واللي هي هنا AutoLogin وبرضو لو كانت غير محققه راح يتم الأنتقال الى الصفحة التي تم وضع ال Middlewares  فيها اللي هي هنا()Login  
4- اذا حاب تدخل صفحة ال Home من ضمن ال Priority لا تعمل الروت حقها كذا "/" ابدا ولا ما رح تعمل ال Priority بالشكل الصحيح ابدا , اعمله اي اسم بشرط لا يكون "/" مثال =>    "home/"
