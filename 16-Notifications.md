# Controller class
```dart
class NotificationController extends GetxController {
  static NotificationController instance = Get.find();

  bool isSelected=true;

  set setIsSelected(bool selected){
    StorageService.instance.write("notifySelected",selected);
    isSelected=selected;
    checkTheStatus();
    update();
  }

  get getIsSelected{
    if(StorageService.instance.read("notifySelected")==null) {
      isSelected=true;
      update();
      return isSelected;
    }
    else{
      setIsSelected=StorageService.instance.read("notifySelected");
      update();
      return isSelected;
    }
  }

  checkTheStatus()async{
    if(isSelected==true)
    {
      await FirebaseMessaging.instance.subscribeToTopic('b');
    }else{
      await FirebaseMessaging.instance.unsubscribeFromTopic('b');
    }
  }

//put it in the first page that will open in project ,,, do not put in the "onInit" of GetXController or GetXServer something will be wrong
  initNotifications()async{
    if(StorageService.instance.read("notifyWork1")==null) {
      await FirebaseMessaging.instance.subscribeToTopic('b');
      StorageService.instance.write("notifyWork1", true);
    }
  }

}
```
# GetxServices class
```dart
class StorageService extends GetxService {
  static StorageService instance = Get.find<StorageService>();
  late GetStorage _box;

  Future<StorageService> init() async {
    _box = GetStorage();
    return this;
  }

  T read<T>(String key) {
    return _box.read(key);
  }

  void write(String key, dynamic value) async {
    await _box.write(key, value);
  }
}
```
# binding class
```dart
class AppBinding extends Bindings {
  @override
  void dependencies() {
    Get.put(StorageService());
    Get.put(NotificationController());
  }
}
```
# main class
```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await GetStorage.init();
  await Get.putAsync(() => StorageService().init());
  AppBinding().dependencies();
  runApp(NoteAPP());
}
```
# Api class
```dart
// using token
RequestNotifications({required String title,required String body,required String token})async{
  String serverToken = 'AAAAxfKVswM:APA91bFoUNmeVb2PXuh1rmSR6KZK0uN9K3dRqNGT2GlCjBK-SRzVHNusfHgOO0lF0z97fme2zjzWXlamdhblPeRPExQscSNxwdokr9eETTXmxt4_Q-XRJ_WYszoOrmyak3ZxRBP0qtfg';
  var headers = {
    'Content-Type': 'application/json',
    'Authorization':'key=$serverToken'
  };
  var request = http.Request('POST', Uri.parse('https://fcm.googleapis.com/fcm/send'));
  request.body = json.encode({
    "to": token,
    "notification": {
      "title": title,
      "body": body,
      "mutable_content": true,
      "sound": "Tri-tone"
    },

    "data": {
      "name": "MOHAMMED ANWAE",
      "age": "23",
      "phone": "Ridmi Note 10"
    }

  });
  request.headers.addAll(headers);
  http.StreamedResponse response = await request.send();
  if (response.statusCode == 200) {
    print(await response.stream.bytesToString());
  }
  else {
  print(response.reasonPhrase);
  }
}


// using topic
RequestNotificationsTopic({required String title,required String body,required String topic})async{
  String serverToken = 'AAAAxfKVswM:APA91bFoUNmeVb2PXuh1rmSR6KZK0uN9K3dRqNGT2GlCjBK-SRzVHNusfHgOO0lF0z97fme2zjzWXlamdhblPeRPExQscSNxwdokr9eETTXmxt4_Q-XRJ_WYszoOrmyak3ZxRBP0qtfg';
  var headers = {
    'Content-Type': 'application/json',
    'Authorization':'key=$serverToken'
  };
  var request = http.Request('POST', Uri.parse('https://fcm.googleapis.com/fcm/send'));
  request.body = json.encode({
    "to": "/topics/$topic",
    "notification": {
      "title": title,
      "body": body,
      "mutable_content": true,
      "sound": "Tri-tone"
    },

    "data": {
      "name": "MOHAMMED ANWAE",
      "age": "23",
      "phone": "Ridmi Note 10"
    }

  });
  request.headers.addAll(headers);
  http.StreamedResponse response = await request.send();
  if (response.statusCode == 200) {
    print(await response.stream.bytesToString());
  }
  else {
  print(response.reasonPhrase);
  }
}
```
# Notification Page
```dart
class TestPage extends StatefulWidget {
  const TestPage({super.key});

  @override
  State<TestPage> createState() => _TestPageState();
}

class _TestPageState extends State<TestPage> {
  @override
  void initState() {
    NotificationController.instance.initNotifications();//always put in the first page that will open to get the initilization of Notifications
  }

  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Notification Page'),
      ),
      body: ListView(
        children: [
          GetBuilder<NotificationController>(builder: (logic) {
            return Checkbox(
              value: NotificationController.instance.getIsSelected,
              onChanged: (value) {
                NotificationController.instance.setIsSelected = value!;
              },);
          }),

          ElevatedButton(onPressed: () async {
            print("------------->>>>>>> ${StorageService.instance.read("t")}");
            await RequestNotificationsTopic(
                title: "See the Error", body: "Mohammed Anwar", topic: 'b');
          }, child: const Text("Send Notification")),
        ],
      ),
    );
  }
}
```
