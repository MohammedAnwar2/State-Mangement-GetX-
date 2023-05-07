```dart
هي بكج وتقدم لنا ايكو سستم كامل الى البروجكت اللي عندنا
الايكو سستم هو عبارة عن state management بالاضافة الى بعض الادوات التي توفر لنا جهد ووقت
تعتمد على معمارية ال MVC ( Model - view - Controls )



صورة


View: يحتوي على ال UI
Controller: اللي يحتوي على البزنز لوجك
Model: يحتوي على الداتا التي يتم جلبها من الAPIاوالداتابيس






# الGetX توفر لنا total decopling فصل كامل لل UI و ال Business Logic 

# ال GetX تعتمد على مبدأ ال MVC
تنقسم الى ثلاثه اقسام

1- ال Model : هو ال data source اللي بيتعامل مع قواعد البيانات او مع ال API بشكل عام

2- ال View : هو ال UI الذي يتم عرض البيانات فيه بالاضافة انه بياخذ ال input من ال user

3 - ال Controller : هو الذي يحتوي على ال variable ، method والذي يتم فيه Handling للبيانات او الداتا بشكل عام 


# شرح العملية التي تتم في ال MVC 
---------------------------

+ لعرض المعلومات من قواعد البيانات او Api

أ - ال user يرسل request الى ال Controller

ب - وال Controller يرسل request الى ال Model

ج - وال Model يجيب المعلومات من ال Data Base or API وبيرجعها الى ال Controller

د - وال Controller بيعمل Handle للبيانات بالشكل اللازم وبعدها راح يرسلها الى ال View

ه - وال View راح يعرض البيانات





```

