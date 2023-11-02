```dart
https://blog.logrocket.com/ultimate-guide-getx-state-management-flutter/

https://stackoverflow.com/questions/74621686/how-to-use-colorscheme-in-flutter-themedata

https://deku.posstree.com/en/flutter/getx/theme/


https://instagram.fblr1-6.fna.fbcdn.net/v/t51.2885-15/309611807_461429132715552_8485053316498374899_n.webp?stp=dst-jpg_e35&efg=eyJ2ZW5jb2RlX3RhZyI6ImltYWdlX3VybGdlbi4xMDgweDEwODAuc2RyIn0&_nc_ht=instagram.fblr1-6.fna.fbcdn.net&_nc_cat=100&_nc_ohc=SQYYtfjXf6wAX9HvD4Q&edm=AEcnVisBAAAA&ccb=7-5&ig_cache_key=MjkzODUzMTc3ODMwNjg3NDI4MA%3D%3D.2-ccb7-5&oh=00_AfCoJCvelYQME6fqUc1wRlFSmtMgihbpe_08ga-lCQzTcA&oe=654864D5&_nc_sid=55bbed
```
https://medium.com/flutter-community/the-flutter-getx-ecosystem-dependency-injection-8e763d0ec6b9

هي بكج وتقدم لنا ايكو سستم كامل الى البروجكت اللي عندنا
الايكو سستم هو عبارة عن state management بالاضافة الى بعض الادوات التي توفر لنا جهد ووقت
تعتمد على معمارية ال MVC ( Model - view - Controls )



صورة


View: يحتوي على ال UI
Controller: اللي يحتوي على البزنز لوجك
Model: يحتوي على الداتا التي يتم جلبها من الAPIاوالداتابيس






- الGetX توفر لنا total decopling فصل كامل لل UI و ال Business Logic 

- ال GetX تعتمد على مبدأ ال MVC
تنقسم الى ثلاثه اقسام

1- ال Model : هو ال data source اللي بيتعامل مع قواعد البيانات او مع ال API بشكل عام

2- ال View : هو ال UI الذي يتم عرض البيانات فيه بالاضافة انه بياخذ ال input من ال user

3 - ال Controller : هو الذي يحتوي على ال variable ، method والذي يتم فيه Handling للبيانات او الداتا بشكل عام 


- شرح العملية التي تتم في ال MVC 
------------------------------------------------------

+ لعرض المعلومات من قواعد البيانات او Api

أ - ال user يرسل request الى ال Controller

ب - وال Controller يرسل request الى ال Model

ج - وال Model يجيب المعلومات من ال Data Base or API وبيرجعها الى ال Controller

د - وال Controller بيعمل Handle للبيانات بالشكل اللازم وبعدها راح يرسلها الى ال View

ه - وال View راح يعرض البيانات

