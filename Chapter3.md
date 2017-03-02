# 静的ページの生成  
2章のscaffold生成と同じように```rails generate```コマンドを使用する  
チュートリアルでは以下のようにコントローラを生成している
```rails generate controller StaticPages home help```  
StaticPagesがコントローラで、homeとhelpがアクション  

```rails generate```で生成したファイルや変更は```rails destroy```コマンドで削除することができる  
例：```rails destroy controller StaticPages home help```  

