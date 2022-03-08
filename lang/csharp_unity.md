# Unity に関するメモ

## 外部のファイルを読み込む

Unity プロジェクトの `Resources` フォルダに入っているファイルが読めるのは当然だが、それ以外のPC 内のファイルは読めるか？

実際に読めた。でもやらないで済むならやるべきではなさそう。自分はやります。

	using System.IO;
	using UnityEngine;
	using UnityEngine.UI;

	string path = Directory.GetCurrentDirectory() + "/texts/testtext.txt";
	GameObject textWindow = GameObject.Find("TextField");

	string data = File.ReadAllText(path);
	textWindow.GetComponent<Text>().text = data;

ゲームの実行ファイルのパスは `Directory.GetCurrentDirectory()` で取得することができる。  
他にも、設定ファイル等が入っていると思われるフォルダ(AppDataとか)を取得するメソッドもあるらしい。  
今回は関係ないのでスルー。

実際の読み込みは `System.IO.File.ReadAllText(string path)` で可能。驚くほど普通に読める。

### 注意

コードを見れば明らかなことだけど、確実に存在しているわけでもないフォルダの中のファイルを参照している。  
行儀は良くない。というか、コードを実装した人間でないとこのソフトは使えない（当然）。  
一般公開するソフトには使えないことに注意。

## 外部のファイルを読み込む（画像編）

参考ってかコピペ 
ソース : http://kainoshizuku.blog.fc2.com/blog-entry-51.html?sp 

コピペ元に感謝。

	using UnityEngine;
	using System.IO;

	public class ImageLoader : MonoBehaviour
	{
		private SpriteRenderer sr;

		void Start()
		{
			gameObject.AddComponent<SpriteRenderer>();
			sr = gameObject.GetComponent<SpriteRenderer>();
			Texture2D texture = ReadTexture(Directory.GetCurrentDirectory() + @"\graphics\image001.png", 1280, 720);
			Sprite createdSprite = Sprite.Create(texture, new Rect(0, 0, 1280, 720), new Vector2(0, 0), 72);
			sr.sprite = createdSprite;
			sr.transform.position = new Vector3(-9, -5);
		}

		private Texture2D ReadTexture(string path, int width, int height)
		{
			byte[] bytes = File.ReadAllBytes(path);
			Texture2D texture = new Texture2D(width, height);
			texture.LoadImage(bytes);
			texture.filterMode = FilterMode.Point;
			return texture;
		}

		void Update() {}
	}

多少弄って動作するようにしている。  
詳細はわからないが、いつもお世話になっている `System.Drawing.Image` は使えなかった。  
外部にある画像を読み込むには、画像ファイルのバイト配列が必要になるらしい。  
`System.IO.File.ReadAllBytes` にパスを入力することでロード。こっちは問題なく使える。

また、このスクリプトをアタッチしたオブジェクト単体で使えるように `SptreRenderer` を追加している。

## 差分を含めた画像の表示

ある画像の上に差分となる画像を重ねて表示する。  
このとき、画像の透明度を下げると、差分が重なっている部分の透明度に違いが出て違和感が出る。  
そこで、差分の部分をマスクとして見做し、下部の画像を隠すことによって透明度に違いが出ないようにする。

	using System.Collections.Generic;
	using System.Linq;
	using UnityEngine;
	using UnityEngine.Rendering;
	using UnityEngine.UI;

	public class ImageContainer : MonoBehaviour
	{
		private int counter;
		private float alpha = 1.0f;

		private List<GameObject> gos = new List<GameObject>();

		public float Alpha
		{
			get => alpha;
			set
			{
				Renderers.ForEach(r => r.color = new Color(1.0f, 1.0f, 1.0f, value));
				alpha = value;
			}
		}

		private List<SpriteRenderer> Renderers { get; set; } = new List<SpriteRenderer>();

		// Start is called before the first frame update
		public void Start()
		{
			Draw(new List<string>()
			{
				$"sample01/images/sampleImage001",
				$"sample01/images/sampleImage005",
				$"sample01/images/sampleImage006",
				$"sample01/images/sampleImage007"
			});
		}

		public void Draw(List<string> paths)
		{
			var container = this.gameObject;
			gameObject.AddComponent<SortingGroup>();

			var gameObjects = new List<GameObject>()
			{
				new GameObject(),
				new GameObject(),
				new GameObject(),
				new GameObject()
			};

			Enumerable.Range(0, paths.Count).ToList().ForEach(n =>
			{
				var g = gameObjects[n];
				gos.Add(gameObjects[n]);
				g.transform.SetParent(container.transform, false);
				var renderer = g.AddComponent<SpriteRenderer>();
				renderer.sprite = Resources.Load<Sprite>(paths[n]);
				if (n != 0)
				{
					g.AddComponent<SpriteMask>().sprite = renderer.sprite;
				}

				Renderers.Add(renderer);
			});

			Renderers[0].sortingOrder = -1;
			Renderers[0].maskInteraction = SpriteMaskInteraction.VisibleOutsideMask;
		}

		// Update is called once per frame
		public void Update()
		{
			counter++;
			if (counter % 20 == 0)
			{
				Alpha -= 0.02f;
			}

			if (counter == 40)
			{
				Renderers[1].sprite = Resources.Load<Sprite>("sample01/images/sampleImage002");
				gos[1].GetComponent<SpriteMask>().sprite = Renderers[1].sprite;
				Debug.Log("test");
			}
		}

これをシーンに登録されているゲームオブジェクトにアタッチして使用する。  
画像の読み込みは Resources から行っている。  
画像の下地は `SampleImage001`特に重要なのは `Draw()` 内の処理。
