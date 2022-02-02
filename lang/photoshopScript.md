# Photoshop で使えるスクリプト

## saveAsPng

レイヤーを一枚ずつ非表示にしながら画像を出力するスクリプト

		//  このスクリプトは、現在開いているpsdドキュメントをpsdファイルで出力するものです。
		//  使い方...

		//  1.レイヤーグループか、レイヤーグループに含まれるレイヤーをアクティブレイヤーにしてください。
		//  2.このスクリプトを使用します。

		//  このスクリプトでは、１で指定したレイヤーグループ内の画像を一旦全て不可視状態にしてその後、１枚ずつ可視状態にして、
		//  pngとして出力します。１枚をpngに出力したら、その画像を不可視状態にして、次のレイヤーにも同様の処理を行います。
		//  これをグループ内のレイヤー全てに対して行います。

		//  photoshop標準のレイヤー毎に出力では、該当レイヤーに含まれる画像しか出力されません。
		//  このスクリプトでは、標準機能よりも柔軟に大量の画像の生成が可能です。

		if(!currentLayerIsLayerSet () && !isLayerSetChild()){
			throw "カレントレイヤーがレイヤーセットではなく、またレイヤーセットに含まれてもいません"
		}

		if(!currentLayerIsLayerSet()){
			activateParentLayerSet ();
		}

		var layers = activeDocument.activeLayer.layers;

		for(var i = 0; i < layers.length; i++){
			layers[i].visible = false;
		}

		for(i = 0; i < layers.length; i++){
			layers[i].visible = true;
			saveAsPng(getDocumentPath(),getDateString() + String(i));
			layers[i].visible = false;
		}

		function currentLayerIsLayerSet(){
			return (activeDocument.activeLayer.typename == "LayerSet");
		}

		//-------------------------------------------------------
		//	現在開いているドキュメントをpng形式で保存する。
		//	保存先は、開いているドキュメントと同じパスが指定される。
		//	ファイル名は現在の時刻を使用する。

		function getDateString(){
			var date = new Date();

			var y = String(date.getFullYear());
			var M = String(date.getMonth());
			var d = String(date.getDate());
			var h = String(date.getHours());
			var m = String(date.getMinutes());

			var arr = [y,M,d,h,m];
			var unit = "";

			for(var i = 0; i < arr.length; i++){
				if(arr[i].length == 1){
					arr[i] = "0" + arr[i];
				}
				unit += arr[i];
			}
			
			return unit;
		}

		function saveAsPng(destPath, fileName){
			fileObj = new File(destPath + "/"+ fileName + ".png");
			pngOpt = new PNGSaveOptions();
			pngOpt.interlaced = false;
			activeDocument.saveAs(fileObj, pngOpt, true, Extension.LOWERCASE);
		}

		function getDocumentPath(){
			return activeDocument.path;
		}

		function isLayerSetChild(){
			return(activeDocument.activeLayer.parent.typename == "LayerSet");
		}

		//	カレントレイヤーが含まれているレイヤーセットの名前を取得します。
		function getCurrentLayerSetName(){
			return activeDocument.activeLayer.parent.name;
		}

		//	カレントレイヤーを含むレイヤーセットをアクティブの状態にします。
		function activateParentLayerSet(){
			activeDocument.activeLayer = activeDocument.layers[ getCurrentLayerSetName() ];
		}