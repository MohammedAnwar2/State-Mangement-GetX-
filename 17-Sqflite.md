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
```flutter
class TaskTable {
  static String taskTableName = "task";
  static String col_id = "id";
  static String col_title = "title";
  static String col_time = "time";
}

```
