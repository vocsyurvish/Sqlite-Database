# Sqlite-Database


## Features...
  - Get Start With Database...
  - How to create table...
  - Insert ti Database...
  - Check Data is exiest in database...
  - Delete Data From Database...
  - Read Data from Database...

# .
# Code Start From Here...

- extends SQLiteOpenHelper

```java
    public DatabaseHandler(@Nullable Context context) {
        super(context, DATABASE_NAME, null, 1);
        this.context = context;
    }
​
    @Override
    public void onCreate(SQLiteDatabase sqLiteDatabase) {
        sqLiteDatabase.execSQL(CREATE_TABLE_QUERY);
    }
​
    @Override
    public void onUpgrade(SQLiteDatabase sqLiteDatabase, int i, int i1) {
        sqLiteDatabase.execSQL("DROP TABLE IF EXISTS " + TABLE_NAME);
    }
```

- Create table Query...

```java

 private static final String QUERY = "CREATE TABLE " + TABLE_NAME + "(" +
            KEY_ID + " INTEGER PRIMARY KEY AUTOINCREMENT, " +
            KEY_VALUE_1 + " TEXT, " +
            KEY_VALUE_2 + " TEXT, " +
            KEY_VALUE_3 + " TEXT, " +
            // continue editing...
            KEY_VALUE_END + " TEXT" +
            ")";

```

- insert data...

```java

  public void insertValue(String velue1, String velue2, String velue3) {
        SQLiteDatabase database = this.getWritableDatabase();
        ContentValues values = new ContentValues();
        values.put(KEY_VALUE_1, velue1);
        values.put(KEY_VALUE_2, velue2);
        values.put(KEY_VALUE_3, velue3);
        // continue editing...
        database.insert(TABLE_NAME, null, values);
        database.close();
    }

```

- check data is exiest or not...

```java

  public boolean checkIfRecordExist(String tableName, String columnName, String fieldValue) {
        SQLiteDatabase database = this.getReadableDatabase();
        String Query = "Select * from " + tableName + " where " + columnName + " =? ";
        Cursor cursor = database.rawQuery(Query, new String[]{fieldValue});
        if (cursor.getCount() <= 0) {
            cursor.close();
            return false;
        }
        cursor.close();
        return true;
    }

```

- remove entry from database...

```java

  public void removeEntry(String value) {
        SQLiteDatabase db = this.getWritableDatabase();
        if (checkIfRecordExist(TABLE_NAME, COLUMN_NAME, value)) {
            db.delete(TABLE_NAME, COLUMN_NAME + " =?", new String[]{value});
        }
        db.close();
    }

```

- get data from database....

```java

  public ArrayList<YOUR_MODEL> getData(String value) {
        ArrayList<YOUR_MODEL> arrayList = new ArrayList<>();
        SQLiteDatabase database = this.getReadableDatabase();
​
        String selectQuery;
        Cursor cursor;
​
        if (value != null) {
            selectQuery = "Select * from " + TABLE_NAME + " where " + COLUMN_NAME + " = ?";
            cursor = database.rawQuery(selectQuery, new String[]{value});
        } else {
            selectQuery = "Select * from " + TABLE_NAME;
            cursor = database.rawQuery(selectQuery, null);
        }
​
        if (cursor.moveToFirst()) {
            do {
                try {
                    String value1 = cursor.getString(cursor.getColumnIndex(COLUMN_NAME));
                    String value2 = cursor.getString(cursor.getColumnIndex(COLUMN_NAME));
                    // continue editing...
​
                    arrayList.add(new YOUR_MODEL(value1, value2, ...));
                } catch (Exception e) {
                    Log.e("TAG", " => " + e.getMessage());
                }
            } while (cursor.moveToNext());
        }
        return arrayList;
    }

```
