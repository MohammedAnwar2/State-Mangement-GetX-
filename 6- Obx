تعتمد برضو على مبدئ ال stream و لكن مافي داعي تستمع الى controller الذي يجي مع ال builder فممكن نستعمل اكثر من controller بداخلها بعمل 
var اسم المتغير= Get.put(اس الكلاس());


```dart
class ControllerLogic extends GetxController {
  RxIn counter = 0.obs;
  
  void increment() {
    counter++;
  }

  void decrement() {
    counter--;
  }
}

var c= Get.put(ControllerLogic());
return Scaffold(
      appBar: AppBar(title: Text("Home Page")),
      body: Center(
        child: Obx((c) => return Row(
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
                  
خالية من
ال (controller):builder وال controller الذي بداخلها وال init

ما نكتب شيء value بعد ال counter في حالة ال Obx

هنا عملنا متغير ، وخلاناه يستمع الى ControllerLogic ، 
واذا عندنا اكثر من Class وفيه عدة متغيرات و دوال نقدم برضو نخليه يستمع له مثل الطريقة السابقه اللي هي👇🏻

var اسم المتغير= Get.put(اس الكلاس());

لذا ال Obx تقدر تتعامل مع عدة Controller بداخلها بدون ايّة مشاكل ، 
المتغيرات تكون متبوعه .Obs الذي يتيح الاستماع للcontroller بدون عمل الدالة update 


ممكن تستمع اكثر من كنترولر بداخلها : الشرح👇🏻
- بداخل ال GetBuilder راح يتم الأستماع لكنترولر واحد فقط ، اي لا يمكن عمل اكثر من كنترولر بداخل ال GetBuilder
- بالنسبة لل Obx نقدر نعمل اكثر من كنترولر بداخلها.





If we want that every time the value of variable changes then all the widgets which uses the variable must update itself then the variable must be reactive or observable and to make it Reactive (Rx) .obs is used with variable value.

To update the widget which uses Rx variable must be placed inside Obx (() => Your widget which uses Rx)

The widget will only update if and only if the Rx variable value changes.

+ Other ways of making the variable Rx 
```dart
1- The first is using Rx(Type)
=============================
initial value is recommended, but not mandatory

final name = RxString(''); 

final isLogged = RxBool (false); 

final count = = RxInt (0);

final balance = RxDouble (0.0);

final items = RxList<String>([]);

final myMap = RxMap<String, int>({});



2- use Darts Generics, RxType>
=============================
final name = Rx<String>(''); final isLogged Rx<Bool>(false); =

final count = Rx<Int>(0); 

final balance = Rx<Double> (0.0);

final number = Rx<Num> (0)

final items = Rx<List<String>> ([]);

final myMap = Rx<Map<String, int>> ({}); 

// Custom classes it can be any class, literally

final user = Rx<User>();
```

