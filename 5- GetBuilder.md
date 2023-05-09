
ال GetBuilder  هي اسرع من ال GetX وتستهلك ذاكره اقل كأدى ولكن ليست reactive اي ليست تفاعليه مع ال application يعني لو استخدمنا ال GetBuilder  راح نفتقد ميزة مهمه واللي هي ال stream

ليست receive وتعتمد على الدالة update لعمل rebuild 
```dart
class ControllerLogic extends GetxController {
  int counter = 0;
  
  void increment() {
    counter++;
    update();
  }

  void decrement() {
    counter--;
    update();
  }
}
```
نلاحظ من الكود السابق 
ان الController<اللوجك> يتكون من 
1 - متغير اسمه counter
2-دالتين زياده ونقصان
3- ال update الموجودة بداخلهن تعمل على الحديث القيمة مثلها مثل ال setState وال emit


```dart
    return Scaffold(
      appBar: AppBar(title: Text("Home Page")),
      body: Center(
        child: GetBuilder<ControllerLogic>(
            init: ControllerLogic(),
            builder: (c) {
              return Row(
                  children: [
                    ElevatedButton(
                        onPressed: () {
                          c.decrement();
                        },
                        child: Text(("-"))),
                    Text("${c.counter}"),
                    ElevatedButton(
                        onPressed: () {
                          c.increment();
                        },
                        child: Text("+")),
                  ]);
```
        
تعمل ال <GetBuilder<ControllerLogic على تحديث ال UI في المنطقة المحدده فقط ، يعني بواسطة ال GetBuilder حددنا المكان الذي يجب على البرنامج يعمل refresh لل UI فيه

تتكون ال GetBuilder من 

1- الinit: ControllerLogic الذي يتيح عمل initialisation 

2- وال builder: (c)  
فالc هنا يعتبر instance ونقدر بواسطته نصل الى المتغيرات او الدوال الموجود تحت ذلك النوع وهنا حاليا ال controller هو ControllerLogic



----------------
 ملاحظات مهمه جدا: 
_______________

0- نقدر نستخدم dependency injection مع ال GetBuilder 
 var اسم المتغير= Get.put(اس الكلاس());

1- ال update تعمل على rebuild لكل GetBuilder تستمع لنفس ال Controller

2-عند استعمال ال GetBuilder لعرض ال UI وعمل rebuild له ، نختار المكان الذي نريد عمل rebuild له فقط ولا ناخذ كل ال UI مثلا نأخذ Text او اي شيء يعمل على تغيير ال UI أثناء العرض

3-عند استعمال GetBuilder وكان ما بداخلها شيء لا يعمل على تغيير ال UI فهنا يظهر خطأ حتى تحسن من كودك
مثااال👇🏻
```dart
GetBuilder<CounterLogic>(builder: (logic) {
          return ElevatedButton(
              onPressed: () {
                logic.increment();
              },
              child: const Text("+"));
        })
```
هنا عملنا GetBuilder وكان ما بداخلها شيء لا يعمل على تحديث الصفحه هنا يظهر خطأ , او ان شاشة العرض ما تظهر ، 

ملاحظة بامكاننا ألغى هذا الخطأ ، بعمل dependency injection 
var اسم المتغير= Get.put(اس الكلاس());

يعني كيدا

final CounterLogic controller = Get.put(CounterLogic());
-------
```dart
GetBuilder<CounterLogic>(builder: (logic) {
          return ElevatedButton(
              onPressed: () {
                logic.increment();
              },
              child: const Text("+"));
        })
```
هنا راح يختفي الخطأ ، 

لكن الصيغة الافضل والمفروض نتبعها كالاتي...!!

# لا تستعمل ال GetBuilder  مع الاشياء الي لا تعمل تغيير او تحديث ال UI

في مثل هذه الحالة مفروض إذا كان عندنا نص وعندنا Button فالصحيح اننا نعمل GetBuilder لل Text فقط واما ال Button نستعمل معه  dependency injection إذا كان عمله الوصول الى دوال ال controller 

var اسم المتغير= Get.put(اس الكلاس());

 في خيار ثاني عندك تعمل Getbuilder لكل ال body ولكن هذا الخيار ليس جيدا ، لانه سيعمل على تحديث ال UI كامل ، ونحن بالاصل نريد تحديث جزء معين لا غير .

4- بامكاننا استعمال المتغير الذي يأتي مع الbuilder الذي يمكننا للوصول إلى المتغيرات والدوال حق ال controller لكن بواسطه استعمالة فوق كل الدوال مثلا وليكن في بداية ال body فراح يحدث ال body كامل وهذا الشيء موو تمااام ، لذا الافضل نستخدم dependency injection 
var اسم المتغير= Get.put(اس الكلاس());
ونعمل للشي الذي يحتاج تحديث(UI) GetBuilder  غير كذا راح نستخدم ال dependency injection ، وهذا بيعطي ادى  افضل للبرنامج

5- كل GetBuilder تستمع الى controller راح ينعمل لها refresh بسبب دالة ال update, لذا نستخدم ال GetBuilder بالاماكن التي تحتاج تحديث او تغيير في ال UI

6- لو عندنا اكثر من GetBuilder فاول ما يصير update فان كل GetBuilder التي تستمع لنفس ال controller اللي صار من دالة ال update الموجوده بداخل ذلك ال controller يُعمل لها refresh بسبب الupdate

7- عند وجود اكثر منGetBuilder فيكفي عمل init للcontroller مره واحده فقط....ويفضل عملها لأول GetBuilder ويتم الاستغناء عن كتابته في بقية ال GetBuilder بشرط ان كل ال GetBuilder من نفس ال controller ( اسم الكلاس حق ملف اللوجك وفي المثال الcontroller هو  CounterLogic ) ، يعني لو كان عندنا اكثر من controller فلازم نعمل init لكل نوع مره واحد في ال GetBuilder تحمل ذلك النوع

8- عند استعمال متغير ال GetBuilder, فلو كانت عندنا 
صفحه اسمها (home) و فيها 
صفحتين اسمهما (screen1 and screen2 )
لو دخلنا الى الصفحه screen1 وفيها عدااد ، و زدنا العداد بمقدار 5 وخرجنا من الصفحه ورجعنا لها مره أخرى ، فتلاحظ انه تم حذف قيمة العداد وصار مثل اول ما دخلنا

$# فالحل هنا نستخدم dependency injectior لحفظ القيم في فترة تنفيذ البرنامج مشروح بشكل كامل في ملف
ال  dependency injectior


```
