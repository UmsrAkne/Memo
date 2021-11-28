# pygame

## import

	import pygame

## 初期化

	pygame.init()
	
## スクリーン初期化

	# 二重括弧なのは、引数に配列を一つ渡しているため
	screen: pygame.Surface = pygame.display.set_mode((700,500))
	
### ウィンドウタイトルの設定
	pygame.display.set_caption("My First Game")
	
### フレームレートの設定

	clock: Clock = pygame.time.Clock()
	clock.tick(60);
	
## メインループ

	# メインループ中に終了イベント（ Xボタンによる終了）を監視するコード
	looping: bool = True

	while looping:
		for event in pygame.event.get():
			if event.type == pygame.QUIT:
				looping = False