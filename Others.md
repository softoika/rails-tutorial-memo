リポジトリのサブディレクトリにherokuアプリを作成し、デプロイする方法  
1. サブディレクトリで```heroku create <アプリ名>```  
2. 親ディレクトリで```git subtree push --prefix <サブディレクトリ名> heroku master```
