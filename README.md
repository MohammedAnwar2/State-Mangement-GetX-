```dart

هو إدارة توجيه الصفحات ، يعني التنقل بين الصفحات.
أولا نغير ال MaterialApp الى GetMaterialApp.


------------------------------
A - Without Name's Route
------------------------------

1- push == Get.to
Navigator.push(context , MaterialPageRoute(builder: (context)=>PageOne());
- الان صار الأمر سهل مع ال gets ونستعمل هذا الكود
Get.to(()=>PageOne ());
- هنا يوجد back -> يعني في سهم يوجد بعد استخدام هذا الكود ونقدر نرجع به مثل استخدام ال push
- يعني ال Get.to وال push هم عمليات إضافة الى ال screen 
To navigate to a new screen:
Get.to(()=>NextScreen());



2- pushReplacement == Get.off

Navigator.pushReplacement(context , MaterialPageRoute(builder: (context)=>PageOne());
مهمتها هي استبدال ال route , يعني تنتقل الى صفحه وتستبدلها بالصفحه اللي كانت قبلها ، يعني لو كانت صفحه ال home وعند الانتقال مثلا الى صفحة اسمها screen فان يتم استبدال صفحة ال home بصفحة ال screen يعني ما رح نقدر على العودة إلى الصفحه السابقه ابدا لانه تم محيها من ال stack
Get.off(()=>PageOne());
- هنا لا يوجد back -> يعني لا يوجد سهم  بعد استخدام هذا الكود ولانقدر نرجع به الى الصفحه السابقه فهذا الكود يكافئ كود ال pushReplacement 

Get.off(()=>NextScreen());
To go to the next screen and cancel all previous routes (useful in shopping carts, polls, and tests)


3 -maybePop == Get.back;
Navigator.maybePop(context);
يعني بالبداية يتم التحقق هل في امكانية للعودة أو لا ، إذا في امكانية بدون إغلاق البرنامج أو إذا لم يكن شيء يلزمها بعدم العوده فإنه يتم العودة للصفحه السابقه ابدا
Get.back();
To go to the next screen and no option to go back to the previous screen (for use in SplashScreens, login screens and etc.)



4- pushAndRemoveUntil == Get.offAll
مهمتها تحذف كل ال pages من ال screens 
مثاااال👇🏻
لو مثلا عندنا عدة pages داخل ال stack
One , two , three , home
وانا الان بصفحه ال one وحبت انتقل من ال one الى two في هذه اللحظة كل pages تنحذف ويتم النقل الى صفحة ال two ولا يوجد سهم من أجل عمل back للصفحه السابقه لانه لا توجد صفحات بالاساس بداخل ال stack من شان يرجع لها

ملاحظة:-
لو عملنا button  وفيه كود الارجاع Get.back فإنه لا يرجع ولا يخرج من البرنامج
لكن لو استخدمنا  Navigator.maybePop(context);
راح يخرج من البرنامج
Get.offAll(()=>NextScreen());
To navigate to the next route, and receive or update data as soon as you return from it:
```
