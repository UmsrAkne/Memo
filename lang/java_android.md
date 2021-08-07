# Java (Android) Memo

Java のメモのうち、Android アプリ作成に関する部分のメモ

## フラグメントの作成

アクティビティの上に載せたり、差し替えたりすることができるビューをフラグメントと言う。  
以下のサンプルでは、フラグメントを１枚だけメインアクティビティに乗せている。

フラグメント本体のレイアウトファイル  
何も配置しないと、表示されたか判断できないため、テキストビューを配置してある。android:id だけはしっかり設定すること。  
app.res.layout.fragment_main.xml


	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
		android:orientation="vertical" android:layout_width="match_parent"
		android:layout_height="match_parent">

		<androidx.appcompat.widget.AppCompatTextView
			...
			android:id="@+id/textView"
			/>

	</LinearLayout>
	
アクティビティ本体。  
このサンプルではデフォルト状態から、"androidx.constraintlayout.widget.ConstraintLayout" に android:id を加えたのみ。  
MainActivity から ConstraintLayout を参照するために android:id が必要になる。
app.res.layout.activity_main.xml
	
	<?xml version="1.0" encoding="utf-8"?>
	<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
		...
		android:id="@+id/container"
		tools:context=".MainActivity">

		<TextView
			...
			/>

	</androidx.constraintlayout.widget.ConstraintLayout>
	
Fragment のクラス。  
androidx.fragment.app.Fragment を継承して作成する。
	
	package com.example.androidtodoapp.fragments;

	import android.os.Bundle;
	import android.view.LayoutInflater;
	import android.view.View;
	import android.view.ViewGroup;
	import android.widget.TextView;

	import androidx.annotation.NonNull;
	import androidx.annotation.Nullable;
	import androidx.fragment.app.Fragment;
	import com.example.androidtodoapp.R;

	public class MainFragment extends Fragment {

		private TextView textView;

		@Nullable
		@Override
		public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
			super.onCreateView(inflater, container, savedInstanceState);
			
			// 第一引数に先程作成した fragment_main を追加する。ちゃんと自動入力候補にも出る。
			return inflater.inflate(R.layout.fragment_main, container, false);
		}

		@Override
		public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
			super.onViewCreated(view, savedInstanceState);
			textView = view.findViewById(R.id.textView);
			textView.setText("setted text");
		}
	}
	
最後に MainActivity クラス。  
MainActivity.onCreate にフラグメントを乗っけるコードを追加する。  
これでフラグメントを表示できる。

	package com.example.androidtodoapp;

	import androidx.appcompat.app.AppCompatActivity;
	import androidx.fragment.app.FragmentTransaction;
	import android.os.Bundle;
	import com.example.androidtodoapp.fragments.MainFragment;

	public class MainActivity extends AppCompatActivity {

		@Override
		protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_main);

			// ここからフラグメントを追加するコード

			MainFragment fragment = new MainFragment();
			FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();
			transaction.add(R.id.container,fragment);
			transaction.commit();
		}
	}

## ListView

指定した要素をリスト表示する android の view の使用方法

レイアウトファイル。 Layout 要素に挟み込んで使用。

	<ListView
		...
		android:id="@+id/testListView"/>

クラスファイル

	import android.widget.ArrayAdapter;
	import android.widget.ListView;

    private ListView listView;
    private ArrayAdapter<String> upListViewAdapter;

	public xxx m(){
		listView = view.findViewById(R.id.mainUpListView);
		
		// android.R.layout.simple_list_item_1 はデフォルトで準備されている。
		upListViewAdapter = new ArrayAdapter<String>(getActivity(),android.R.layout.simple_list_item_1);
		listView.setAdapter(upListViewAdapter);

		// 要素の追加
		upListViewAdapter.add("test1");
	}

ListView への要素の追加は、ArrayAdapter を介して行う。  
生成した ArrayAdapter を setAdapter でセットすることで要素を追加することが可能になる。  
上記サンプルはフラグメントのコードのため、ArrayAdapter のコンストラクタの第一引数に getActivity() を使用。  
アクティビティに上記コードを記述する際には this でOK。

## フラグメントからアクティビティを取得

フラグメントから Context が必要な処理を行う場合は `Fragment#getActivity()` を使用。  
フラグメントを呼び出したアクティビティを取得することができる。  

## Room の導入

データベースを扱うライブラリ。Android 公式推奨。

app/build.gradle の依存関係に以下を追記
build.gradle はプロジェクト中に２つあるらしいので注意。(Module.app の方)

	dependencies{

		def room_version = "2.2.5"

		implementation "androidx.room:room-runtime:$room_version"
		annotationProcessor "androidx.room:room-compiler:$room_version"
	}

"kapt" を含む情報が出てくるがそっちではできなかった Kotlin 用？  
上記を書いたら、Android Studio の右上あたりの Sync project ボタンを押す。ゾウアイコンのボタン。  
尚、情報収集中にボタンが出ないとの投稿を見かけた。ない場合もある？

次にデータベースの Entity (テーブルのデータ？) を作成する。  
作成するというよりは、既存のデータクラスに追記する。  
Todo.java を Entity として扱う。

	import androidx.room.Entity;
	import androidx.room.Ignore;
	import androidx.room.PrimaryKey;

	import java.util.Date;

	@Entity(tableName = "todos")
	public final class Todo {

		@PrimaryKey
		public int id;

		private String title = "";

		@Ignore
		private Boolean completed;

		@Ignore
		private Date creationDateTime;

		@Ignore
		private Date completeDateTime;
	}
	
今回、書く中でいくつか注意。

 * Data 型は扱えない。SQLite だし当然だけど。int に変換してデータを用意する等の工夫が必要そう。  
 * 外部からセットできない値はNG 。setter が存在しない旨のエラーが出力される。
 	* 上記に関して。setXXX メソッドがある場合は認識してくれているらしい。

対策として @Ignore を付与すると、そのフィールドが無視される。

続いて必要なクラスを作成していく。最初にインターフェース。  
Database Access Object 略して DAO  
インターフェースだけど SQL はここに記述。扱う型は先程の Entity クラスとなる。

	package com.example.androidtodoapp;

	import androidx.room.Dao;
	import androidx.room.Delete;
	import androidx.room.Insert;
	import androidx.room.Query;

	import java.util.List;

	@Dao
	public interface TodoDAO {
		@Query("SELECT * FROM todos")
		List<Todo> getAll();

		@Insert
		void insertAll(Todo... todos);

		@Insert
		void insert(Todo todo);

		@Delete
		void delete(Todo todo);
	}

以下で更に作成。以下２つは理解しきれてないので写経。  
あえてシングルトンを採用しているのは、Room 制作側からの推奨とのこと。  
複数のインスタンスを作る意味はほぼないらしい。

抽象クラス AppDatabase

	package com.example.androidtodoapp;

	import androidx.room.Database;
	import androidx.room.RoomDatabase;

	@Database(entities = {Todo.class}, version = 1, exportSchema = false)
	public abstract class AppDatabase extends RoomDatabase {
		public abstract TodoDAO tododAO();
	}

AppDatabaseSingleton  
database名等はここで設定しているっぽい。

	package com.example.androidtodoapp;

	import android.content.Context;
	import androidx.room.Room;

	public class AppDatabaseSingleton {
		private static AppDatabase instance = null;

		public static AppDatabase getInstance(Context context) {
			if (instance != null) {
				return instance;
			}

			instance = Room.databaseBuilder(context,
					AppDatabase.class, "database-name").build();
			return instance;
		}
	}
	
ラスト。今回はフラグメントに記述する。これもほぼネットのコードを写経。  
以下は非同期処理を行うクラスをインナークラスとしてフラグメントに記述。  
new DataStoreAsyncTask() で実行可能。
Room のデータベースは同期処理では書けない。

    private static class DataStoreAsyncTask extends AsyncTask<Void, Void, Integer> {
        private WeakReference<Activity> weakActivity;
        private AppDatabase db;
        private StringBuilder sb;

        public DataStoreAsyncTask(AppDatabase db, Activity activity) {
            this.db = db;
            weakActivity = new WeakReference<>(activity);
        }

        @Override
        protected Integer doInBackground(Void... params) {
            TodoDAO dao = db.tododAO();
            dao.insert(new Todo());

            return 0;
        }

        @Override
        protected void onPostExecute(Integer code) {
            Activity activity = weakActivity.get();
            if(activity == null) {
                return;
            }
        }
    }
