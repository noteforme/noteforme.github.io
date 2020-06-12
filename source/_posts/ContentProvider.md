---
title: ContentProvider
comments: true
date: 2017-07-31 09:35:40
tags:
categories: ANDROID

---
https://developer.android.com/guide/topics/providers/content-provider-basics.html?hl=zh-cn
   还需要完善单条记录 查询 修改等
一、新建数据库
    https://github.com/BlogForMe/AndroidProject/blob/master/ContentProvider/mylibrary/src/main/java/com/hyhy/mylibrary/ArticleDbHelper.java
二、创建ContentProvider
      
        public class ArticlesProvider extends ContentProvider {
            public static final String AUTHORITY = "com.hyhy.contenttest.db.ArticlesProvider";  //标识特定ContentProvider,使用包名使他唯一
            private static final Uri NOTIFY_URI = Uri.parse("content://" + AUTHORITY + "/" + TABLE_NAME);
            private static final UriMatcher uriMatcher;


            /**
             * Match Code
             */
            public static final int ARTICLE_ALL = 0;
            public static final int ARTICLE_SINGLE = 1;


            /**
             * MIME
             */
            public static final String CONTENT_TYPE = "vnd.android.cursor.dir/vnd.com.hyhy.article";
            private static final String CONTENT_ITEM_TYPE = "vnd.android.cursor.item/vnd.com.hyhy.article";
    
            static {
                uriMatcher = new UriMatcher(UriMatcher.NO_MATCH);
                uriMatcher.addURI(AUTHORITY, TABLE_NAME, ARTICLE_ALL);          //匹配记录集合
                uriMatcher.addURI(AUTHORITY, TABLE_NAME + "/#", ARTICLE_SINGLE);    //匹配单条记录
            }
    
            private ArticleDbHelper helper;
            private SQLiteDatabase db;
            private ContentResolver resolver;
    
            @Override
            public boolean onCreate() {
                resolver = getContext().getContentResolver();
                helper = new ArticleDbHelper(getContext());
                return false;
            }
    
            @Nullable
            @Override
            public String getType(@NonNull Uri uri) {
                int match = uriMatcher.match(uri);
                switch (match) {
                    case ARTICLE_ALL:
                        return CONTENT_TYPE;
                    case ARTICLE_SINGLE:
                        return CONTENT_ITEM_TYPE;
                    default:
                        throw new IllegalArgumentException("Unknown URI: " + uri);
                }
            }
    
            @Override
            public Cursor query(@NonNull Uri uri, @Nullable String[] projection, @Nullable String selection, @Nullable String[] selectionArgs, @Nullable String sortOrder) {
                db = helper.getReadableDatabase();
                int match = uriMatcher.match(uri);
                switch (match) {
                    case ARTICLE_ALL:
                        //doesn't need any code in my provider.
                        break;
                    case ARTICLE_SINGLE:
        //                long _id  = ContentUris.parseId()
                        break;
                    default:
                        throw new IllegalArgumentException("Unknown URI: " + uri);
                }
                return db.query(TABLE_NAME, projection, selection, selectionArgs, null, null, sortOrder);
            }


            @Nullable
            @Override
            public Uri insert(@NonNull Uri uri, @Nullable ContentValues values) {
                int match = uriMatcher.match(uri);
                if (match != ARTICLE_ALL) {
                    throw new IllegalArgumentException("Wrong URI:" + uri);
                }
                db = helper.getWritableDatabase();
                long rowId = db.insert(TABLE_NAME, null, values);
                if (rowId > 0) {
                    notifyDataChanged();
                    return ContentUris.withAppendedId(uri, rowId);
                }
                return null;
            }


            @Override
            public int delete(@NonNull Uri uri, @Nullable String selection, @Nullable String[] selectionArgs) {
                return 0;
            }
    
            @Override
            public int update(@NonNull Uri uri, @Nullable ContentValues values, @Nullable String selection, @Nullable String[] selectionArgs) {
                return 0;
            }
    
             private void notifyDataChanged() {
                getContext().getContentResolver().notifyChange(NOTIFY_URI, null);
             }
         }
配置Androidmanifest.xml
     
      <provider
             android:name=".db.ArticlesProvider"
            android:authorities="com.hyhy.contenttest.db.ArticlesProvider"
            android:exported="true" />

三、操作ContentProvider
     
     public class MainActivity extends AppCompatActivity {
        private static final String AUTHORITY = "com.hyhy.contenttest.db.ArticlesProvider";
        private static final Uri ARTICLE_ALL_URI = Uri.parse("content://" + AUTHORITY + "/" + TABLE_NAME);
        private ContentResolver resolver;
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            resolver = getContentResolver();
        }


        public void addDb(View v) {
            ContentValues values;
            for (int i = 1; i < 5; i++) {
                values = new ContentValues();
                values.put(ArticleReaderContract.Articles.COLUMN_NAME_ENTRY_ID, i);
                values.put(ArticleReaderContract.Articles.COLUMN_NAME_TITLE, "heh " + i);
                values.put(ArticleReaderContract.Articles.COLUMN_NAME_SUBTITLE, "describe " + i);
                resolver.insert(ARTICLE_ALL_URI, values);
            }
            Toast.makeText(MainActivity.this, "插入成功", Toast.LENGTH_SHORT).show();
        }
    
        public void btQueryAll(View v) {
            Cursor c = resolver.query(ARTICLE_ALL_URI, null, null, null, null);
            while (c.moveToNext()) {
                System.out.println(c.getString(c.getColumnIndex(COLUMN_NAME_TITLE)));
            }
        }
    }

   



   参考 ： http://blog.csdn.net/liuhe688/article/details/7050868
   这一块还不熟悉