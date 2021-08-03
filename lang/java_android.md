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