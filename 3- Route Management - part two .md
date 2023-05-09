B - With Name
--------------------------------

كنا من أجل كتاب ال route بداخل ال MaterialApp نعمل هكذا
```dart
routes : {
"/home" : (context)=>PageOne(),
} 
```
لكن مع ال GetX صار الأمر مختلف وسهل جدا
نكتب بداخل ال GetMaterialApp 
```dart
getPages:[
GetPage(name:"/PageOne" , page:()=>PageOne()), 
GetPage(name:"/PageTwo" , page:()=>PageTwo())
]
```
نطبق على كل الاكواد السابقه

```dart
1- Navigator.pushNamed(context ,"PageOne");
    Get.toNamed("PageOne");

2-Navigator.pushReplacementNamed(context ,"PageOne");
   Get.offNamed("PageOne")

3-Navigator.pushNamedAndRemoveUntil(context ,"PageOne");
   Get.offAllNamed("PageOne")

```
ملاحظة مهمه جدا ، عند كتاب ال route  لازم يكون في بداية اسم ال route ، الرمز ذا "/" والا راح يطلع خطأ
نقدر نعمل صفحه ابتدائية واول ما يشتغل البرنامج يفتح هذه الصفحة بواسطة ال route في ال GetMaterialApp
```dart
initialRoute: '/ اسم الروت '
```
