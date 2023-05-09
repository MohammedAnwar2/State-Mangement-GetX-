1- نقدر نرسل اي نوع من البيانات بواسطة arguments

+ ممكن تكون مع ال withNamed👇🏻👇🏻


How to send data to named Routes
بامكاننا إرسال اي شيء بعد كلمة arguments 
Just send what you want for arguments. 
Get accepts anything here, whether it is a String, a Map, a List, or even a class instance.
مثااال 
```dart Get.toNamed("/NextScreen", arguments: 'Get is the best');
```
on your class or controller:
واستقبال الداتا هذه ، يكون بالطريقة التالية
```dart print(Get.arguments);
or 
Text("${Get.arguments}")
```
//print out: Get is the best


+ وممكن أيضا تكون مع ال withoutNamed👇🏻👇🏻
مثال

```dartGet.to(()=>PageOne(), arguments: 'Mohammed');
on your class or controller:
واستقبال الداتا هذه ، يكون بالطريقة التالية
print(Get.arguments);
or 
Text("${Get.arguments}")
```
//print out: Mohammed







------------------------------------------------






2- إرسال البيانات بواسطة NamedParameters


+++++++++++++++ Part A +++++++++++++++++


A - Single Parameters(Single Data)

You can also receive NamedParameters with Get easily:
```dartvoid main() {
  runApp(
    GetMaterialApp(
      getPages: [
      GetPage(
        name: '/profile/:user',
        page: () => Third(),
      ),
     ],
)}```
لارسال داتا واحد بواسطة اسم الروت نعمل الاتي
1- نكتب اسم الملف عادي مثل كل مره
2- في الأخير نزيد له /ثم : ثم اسم الباراميتر اللي راح نستلم الداتا بواسطته
Send data on route name
نرسل الداتا الى الشاشه التي عملنا الروت بيها اللي هي profile وبعدها نعمل / ثم الداتا التي تريد ارسالها وتكون بالشكل التالي
Get.toNamed("/profile/34954");
ملاحظة إذا عملت كيدا 👇🏻
Get.toNamed("/profile/34954 77");
راح يتم إرسال الباراميتر 34954 فقط وال 77 لن يتم ارساله

وفي صفحة ال profile نستقبل الداتا التي تم ارسالها بالشكل التالي
On second screen take the data by parameter
بهذه الطريقة 👇🏻
print(Get.parameters['user']);
او حتى كيدا
Text("${Get.parameters['user']}")
// out: 34954


+ ملاحظة مهمه جدا 
-----------------------------

- ممكن نجعل هذا النوع Multiple ، يعني رااح نقدر نرسل بيانات كثييره كالتالي 
```dartgetPages: [
     GetPage(
     name: "page_one /:name/:jobe/:colloge", page: () => Page_one()),
     ],```


وفي صفحة ال Page_one نستقبل الداتا التي تم ارسالها بالشكل التالي
On second screen take the data by parameter
بهذه الطريقة 👇🏻

```dart Text("name = ${Get.parameters['name']}"),
Text("jobe = ${Get.parameters['jobe']}"),
Text("colloge = ${Get.parameters['colloge']}"),```



++++++++++++++ Part B +++++++++++++++++


B - Multiple Parameters(Multiple Data)

بامكاننا ارسل مجموعة من ال parameters 
مثاال 
بعد ال parameter الأول نستخدم ?وبعدها عادي نسند الباراميتر الى القيمة اللي راح تريد بعثها ومن أجل الاستمرار في البعث فيك تعمل & وتدخل اللي بعده
```dart Get.toNamed("/profile/34954?flag=true&country=italy");
or كيدا
var parameters = 
<String, String>{"flag":"true","country": "italy",};
Get.toNamed("/profile/34954", parameters: parameters);
```

وفي صفحة ال profile نستقبل الداتا التي تم ارسالها بالشكل التالي
On second screen take the data by parameters as usually

```dart print(Get.parameters['user']);
print(Get.parameters['flag']);
print(Get.parameters['country']);```
// out: 34954 true italy



------------------------------------------------


3- بواسطة ال Contractor 

```
