الموقع اللي فيه الشرح هو👇🏻
https://medium.com/flutter-community/the-flutter-getx-ecosystem-dependency-injection-8e763d0ec6b9


- we can (and should) separate  our dependencies from the view using Bindings and it can help us to organize our code
 

-  ال Binding هي مجموعة من الكلاسات بامكااننا ان نعلن عن ال dependency الخاص بينا وبعدها نربطها بالroute

- Bindings are classes where we can declare our dependencies and then ‘bind’(ربطها) them to the routes.

- ونستخدم ال route الخاص بال GetX وليس الroute المبني build in في فلاتر

---------------------------------------------- codes ----------------------------------------------

``dart
class HomeBinding implements Bindings {
//ال Bindings من مكتبة ال Getx
  @override
  void dependencies() {
    Get.put(Controller1());
    Get.put(Controller2());
  }
}
```
_______________________

```dart
GetMaterialApp( 
  initialRoute: "/",
  getPages: [
    GetPage(name: "/", page: () => HomePage(), binding: HomeBinding()), // here!
  ],
);
```
__________________________

```dart
Get.to(HomePage(), binding: HomeBinding());
 
Get.toNamed("/", binding: HomeBinding()); 
```
_____________________________

```dart
GetMaterialApp(
  initialRoute: "/",
  initialBinding: HomeBinding(), // here!
);
```
____________________

```dart
GetMaterialApp(//في حالة عدم عمل الكلاس
  initialRoute: "/",
  initialBinding: BindingsBuilder(() { //here
    Get.put(Controller());
  }),
);


_____________________

```dart
class HomePage extends StatelessWidget {

  Controller controller = Get.find(); // it'll work!

}
```

___________________________

+ راح نعتبر ان ال Binding عبارة عن صندوق وهذا الصندوق يحتوي على مجموعة من ال instance اللي عم يتم حقنها ولكن هذه ال instance ما رح يتم حقنها الا بعد استدعاء هذا الصندوق ، كيف يتم استدعاء هذا الصندوق??
+ يتم استدعاءه بواسطة ثلاث طرق..!!👇🏻

1- عند التوجه الى صفحة بواسطة ال getPages 👇🏻

```dart
getPages: [
    GetPage(name: "/", page: () => HomePage(), binding: HomeBinding()), // here!
  ],
```


  2 - عند التوجه الى صفحة ولكن ليس بال getPages 👇🏻
  
```dart
without Named ( without route )
Get.to(()=>HomePage(),binding:HomeBinding());

with Named (with route )
Get.toNamed("/", binding: HomeBinding()); 
```


  3- عند تشغيل البرنامج مباشرة بواسطة ال initialBinding
  
  ```dart
  GetMaterialApp(
  initialRoute: "/",
  initialBinding: HomeBinding(), // here!
);
```
فعند تشغيل البرنامج راح يتم استدعاء هذا الصندوق وحقن جميع ال instance الموجودة بداخله في ال dependencies فصار بامكاننا استعمالة باي مكان في ال application 


4- إذا أنا مش معرف الكلاس حق ال Binding وانا أريد اعمله مباشرة على وجه السرعة في صفحة ال main فالطريقة نستخدم BindingsBuilder التي تتيح لنا استخدام ال Binding بدون إنشاء الكلاس.

- GetX also provides BindingsBuilder that lets us use bindings without creating a separate class.

```dart
GetMaterialApp(
  initialRoute: "/",
  initialBinding: BindingsBuilder(() { //here
    Get.put(Controller());
  }),
);
```

----------------------------------------------- Access ------------------------------------------------------------

Now to use access the dependencies, we can simply use Get.find.
نستخدمه للوصول للبيانات
```dart
class HomePage extends StatelessWidget {

  Controller controller = Get.find(); // it'll work!

}
```
//Controller is name of controller 
لازم نحدد اسم ال controller والا بيطلع خطأ أو ما رح يستجيب

------------------------------------------------

بامكاننا عمل Binding عند الانتقال من صفحة الى أخرى ، 

- لحفظ البيانات نستخدم ال Get.put سواءا ال 
permanent true or false
لما تكون فوق كل شيء

- لعدم حفظ البيانات نستخدم ال Get.lazyPut مع fenix


```dart
class HomeBinding implements Bindings {
//ال Bindings من مكتبة ال Getx
  @override
  void dependencies() {
  
    Get.put(Controller1());
    
    //Controller is the Name of controller 
    
    Get.lazyPut(Controller2());
  }
}
```


