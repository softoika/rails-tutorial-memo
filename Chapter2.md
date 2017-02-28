scaffoldでコードを簡易生成できる  
2.1.1章のユーザモデルの生成はrails generateにscaffoldコマンドを渡す  
```rails generate scaffold User name:string email:string```  
    
データベースをマイグレート(migrate)することでデータベースを更新し、usersデータモデルを作成できる  
  
REST(REpresentational State Transfer): Webアプリケーションなどのシステムを構築するためのアーキテクチャスタイルの一つ  
```RailsアプリケーションにおけるRESTとは、アプリケーションを構成するコンポーネント (ユーザーやマイクロポストなど) を「リソース」としてモデル化することを指します。   
これらのリソースは、リレーショナルデータベースの作成/取得/更新/削除 (Create/Read/Update/Delete: CRUD) 操作と、4つの基本的なHTTP requestメソッド (POST/GET/PATCH/DELETE) の両方に対応しています。```  


