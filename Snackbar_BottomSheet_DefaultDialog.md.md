```dart
class Snackbar_BottomSheet_DefaultDialog extends StatelessWidget {
  const Snackbar_BottomSheet_DefaultDialog({Key? key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Dark Mode"),
      ),
      body: Container(
        width: MediaQuery.of(context).size.width,
        child: Column(
          mainAxisAlignment: MainAxisAlignment.spaceAround,
          children: [
            MaterialButton(
              onPressed: () {
                Get.defaultDialog(
                    title: "error",
                    content: const Text("Hi Mohammed"),
                    backgroundColor: Colors.amberAccent,
                    textCancel: "cancle",
                    textConfirm: "Confirm",
                    onCancel: () {},
                    onConfirm: () {},
                    confirmTextColor: Colors.white,
                    titleStyle: const TextStyle(color: Colors.deepOrange));
              },
              child: Container(
                alignment: Alignment.center,
                width: 100,
                height: 100,
                color: Colors.blueAccent,
                child: Text(
                  "Dialog",
                  style: GoogleFonts.asap(fontSize: 25, color: Colors.white),
                ),
              ),
            ),
            MaterialButton(
              onPressed: () {
                Get.snackbar(
                  "Hi", "Mohammed",
                  snackPosition: SnackPosition.BOTTOM,
                  // duration: Duration(seconds: 3),
                  animationDuration: const Duration(seconds: 1),
                  maxWidth: MediaQuery.of(context).size.width,
                  margin: const EdgeInsets.only(bottom: 100),
                  //borderRadius: 40,
                  colorText: Colors.deepPurple,
                  backgroundColor: Colors.lightGreenAccent,
                  snackStyle: SnackStyle.FLOATING,
                  borderColor: Colors.deepOrange,
                  borderWidth: 2,
                  isDismissible: true,
                  dismissDirection: DismissDirection.horizontal,
                  forwardAnimationCurve: Curves.linear,
                );
              },
              child: Container(
                alignment: Alignment.center,
                width: 100,
                height: 100,
                color: Colors.amberAccent,
                child: Text(
                  "Snackbar",
                  style: GoogleFonts.asap(fontSize: 20.sp, color: Colors.white),
                ),
              ),
            ),
            MaterialButton(
              onPressed: () {
                Get.bottomSheet(
                  Container(
                      width: double.infinity,
                      height: 120.h,
                      color: Colors.deepPurple,
                      child: Column(
                        children: [
                          ListTile(
                            leading: MaterialButton(
                              onPressed: () {
                                },
                              child:  Icon(Icons.light_mode_outlined,
                                  color: Colors.white, size: 30),
                            ),
                            title: Text("Light Mode",style: GoogleFonts.asap(color: Colors.white,fontSize: 15),),

                          ),
                          ListTile(
                            leading: MaterialButton(
                              onPressed: () {
                              },
                              child: const Icon(Icons.dark_mode_outlined,
                                  color: Colors.white, size: 30),
                            ),
                            title: Text("Dark Mode",style: GoogleFonts.asap(color: Colors.white,fontSize: 15),),

                          )
                        ],
                      )),
                  isDismissible: true,
                  enterBottomSheetDuration: const Duration(seconds: 1),
                  exitBottomSheetDuration: const Duration(microseconds: 1),
                );
              },
              child: Container(
                alignment: Alignment.center,
                width: 100,
                height: 100,
                color: Colors.deepOrange,
                child: Text(
                  "Bottom\nSheet",
                  style: GoogleFonts.asap(fontSize: 20.sp, color: Colors.white),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

```
