تشبه بشكل كبييير لل GetxController بحيث انها تشترك معها في ال life cycle لكن ليست مهمتها ان تكون اللوجك للصفحات بل مهمتها ببساطة شديده انها reachable اي تسهّل الوصول للمتغيرات والدوال التي بداخلها و تستخدم بالغالب عند عمل initialised لل Application بمعنى
أول ما نشغل ال Application ممكن نعمل initialised لل services الموجوده عندنا مثل
1- hive
2-shared preference
3-get storage
4-more connection
باختصار : اي شيء أنا بحاجة انه يشتغل أول ما يشتغل ال application نستعمل ال GetxService 
