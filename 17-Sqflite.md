# Database Configration
```dart
class SQLite {
  static Database? _db;

  Future<Database?> get db async {
    if (_db == null) {
      _db = await initSQLite();
      return _db;
    } else {
      return _db;
    }
  }

  initSQLite() async {
    var databasesPath = await getDatabasesPath();
    String path = join(databasesPath, 'LearnSQLite.db');//Name of the database
    Database database = await openDatabase(path,
        onCreate: _oncreate, version: 0, onUpgrade: _onUpgrade);
    return database;
  }

  readData({String? table, String? orderedBy}) async {
    Database? database = await db;
    List<Map> response = await database!.query(table!); //,orderBy: "`id` DESC" // orderBy: "`id` ASC " TaskTable.col_classification
    return response;
  }

  insertData({String? table, Map<String, Object?>? values}) async {
    Database? database = await db;
    int response = await database!.insert(table!, values!);
    return response;
  }

  deleteData({String? table, String? my_where}) async {
    Database? database = await db;
    int response = await database!.delete(table!, where: my_where!);
    return response;
  }

  updateData(
      {String? table, Map<String, Object?>? values, String? my_where}) async {
    Database? database = await db;
    int response = await database!.update(table!, values!, where: my_where);
    return response;
  }

  deleteMyDatabase() async {
    String databasesPath = await getDatabasesPath();
    String path = join(databasesPath, 'LearnSQLite.db');
    await deleteDatabase(path);
    print("delete database ----------------------");
  }

  Future<List<Map<String, dynamic>>> displayNameOfColumns(
      {required String tableName}) async {
    Database? database = await db;
    List<Map<String, dynamic>> columns =
        await database!.rawQuery('PRAGMA table_info($tableName)');
    return columns;
  }

  _onUpgrade(Database db, int oldVersion, int newVersion) async {
    print("_onUpgrade ==============================");
    // await db.execute("ALTER TABLE notes ADD COLUMN BABY Text ");
    //await db.execute("ALTER TABLE notes DROP COLUMN BABY");
  }

  _oncreate(Database db, int version) async {
    await db.execute('''
      CREATE TABLE `${TaskTable.taskTableName}` (
        "${TaskTable.col_id}" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
        "${TaskTable.col_title}" TEXT ,
        "${TaskTable.col_time}" TEXT ,
     ''');
    print("_oncreate ==============================");
  }
}
```
# Tables of Database
```dart
class TaskTable {
  static String taskTableName = "task";
  static String col_id = "id";
  static String col_title = "title";
  static String col_time = "time";
}

```

# Database Controller
```dart
abstract class TaskDatabaseController extends GetxController {
  Future<void> fetchTasks();
  Future<void> addTask({required Task task});
  Future<void> updateTask({required Task task});
  Future<void> deleteTask({required int id});
}
class ImpTaskDatabaseController extends TaskDatabaseController {
  final RxList<Task> tasks = <Task>[].obs;
  SQLite sq = SQLite();

  Future<void> fetchTasks() async {
    final List<Map<String, dynamic>> taskMaps = await sq.readData(
        table: TaskTable.taskTableName,
    tasks.value = taskMaps.map((data) => Task.fromMap(data)).toList();
    // print("---------------fetchTasks-------------");
  }

  Future<void> addTask({required Task task}) async {
    var response = await sq.insertData(
        table: TaskTable.taskTableName,
        values: task.toMap()); 
    print("addTask done------------->$response");
    fetchTasks(); 
  }

  Future<void> updateTask({required Task task}) async {
    int response = await sq.updateData(
        table: TaskTable.taskTableName,
        values: task.toMap(),
        my_where:"`${TaskTable.col_id}`=${task.id} "); 
    print("updateTask done------------->$response");
    fetchTasks(); //reset the tasks
  }

  Future<void> deleteTask({required int id}) async {
    int response = await sq.deleteData(
        table: TaskTable.taskTableName,
        my_where: "`${TaskTable.col_id}`=$id"); 
    print("deleteTask done------------->$response");
    fetchTasks(); //reset the tasks
  }
}

```
