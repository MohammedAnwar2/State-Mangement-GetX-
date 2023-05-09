ال GetBuilder  هي اسرع من ال GetX وتستهلك ذاكره اقل كأدى ولكن ليست reactive اي ليست تفاعليه مع ال application يعني لو استخدمنا ال GetBuilder  راح نفتقد ميزة مهمه واللي هي ال stream

فال GetX تعتمد على مبدئ ال stream يعني التغيرات التي تتم بال controller ( مكان اللوجك مكان المتغيرات والدوال ) مباشرة راح يتم الاستماع لها بال View

```dart
 class ControllerLogic extends GetxController {
  RxInt counter = 0.obs;
  
  void increment() {
    counter++;
  }

  void decrement() {
    counter--;
  }
}
```
- نوع المتغيرات يكون من النوع RxInt لذا الcounter راح يكون نوعه RxInt 
- ال obs يعني راح يلاحظ التغيرات، يعني التغيير الذي يحصل لل counter و راح يستمع له مباشرة ال Getx ويعرض التغير بدون عمل update في ال UI ، لذا هو receive

```dart
return Scaffold(
      appBar: AppBar(title: Text("Home Page")),
      body: Center(
        child: GetX<ControllerLogic>(
            init: ControllerLogic(),
            builder: (c) {
              return Row(
                  children: [
                    ElevatedButton(
                        onPressed: () {
                          c.decrement();
                        },
                        child: Text(("-"))),
                    Text("${c.counter.value}"),
                    ElevatedButton(
                        onPressed: () {
                          c.increment();
                        },
                        child: Text("+")),
                  ]);
```

بما ان النوع هو RxIn راح نضيف كلمة value لل counter اي للمتغيرات



نلاحظ انه مع تشغيل البرنامج القيمة تزيد مع انه لا توجد الدالة update بداخل الدوال وذلك بفضل ال obs اللي هي ميزة من مزايا ال steam اللي تخلي GetX تستمع لها بدون استعمال الدالة update اللي بتكافى setState في ال statefullwidget وال emit في ال bloc

___________________

 ملاحظات مهمه جدا
___________________


1- لو عندنا اكثر من Getx بدون استخدام ال dependency injection فاول ما نعمل تغيير على المتغير فإنه راح يتم عمل rebuild في المكان الذي يتواجد فيه هذا المتغير فقط ، 
مثااال على ذلك وائل ابو حمزة الحلقة 5 الدقيقة 12:53

2- عند وجود اكثر من Getx فيكفي عمل init للcontroller مره واحده فقط....ويفضل عملها لأول GetX ويتم الاستغناء عن كتابته في بقية ال GetX بشرط ان كل ال GetX من نفس ال controller ( اسم الكلاس حق ملف اللوجك وفي المثال الcontroller هو  CounterLogic ) ،
 يعني لو كان عندنا اكثر من controller فلازم نعمل init لكل نوع مره واحد في ال GetX تحمل ذلك النوع
 
3- عند استعمال GetX وكان ما بداخلها شيء لا يعمل على تغيير ال UI فهنا يظهر خطأ حتى تحسن من كودك
مثااال👇🏻

```dart
Getx<CounterLogic>(builder: (logic) {
          return ElevatedButton(
              onPressed: () {
                logic.increment();
              },
              child: const Text("+"));
        })
```
هنا عملنا GetX وكان ما بداخلها شيء لا يعمل على تحديث الصفحه هنا يظهر خطأ , او ان شاشة العرض ما تظهر ، 
في مثل هذه الحالة مفروض إذا كان عندنا نص وعندنا Button فالصحيح اننا نعمل GetX لل Text فقط واما ال Button نستعمل معه  dependency injection إذا كان عمله الوصول الى دوال ال controller 
```dart
var اسم المتغير= Get.put(اس الكلاس());
```

والا في خيار ثاني عندك تعمل GetX لكل ال body ولكن هذا الخيار ليس جيدا

