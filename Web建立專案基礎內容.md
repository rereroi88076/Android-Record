# Web建立專案基礎內容


## 建立資料庫
1. 進入MySQLWorkBench
2. 輸入指令
```
create database smart_bp default character set utf8mb4 collate utf8mb4_unicode_ci;
create user 'smart_bp'@'%' IDENTIFIED BY 'dsMdj3s11';
GRANT ALL PRIVILEGES ON smart_bp.* to 'smart_bp'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```


## Clone專案
```
git clone url
```
## Terminal進入專案資料夾
1. 輸入指令
```
composer update
php artisan storage:link
chmod -R 777 storage
```


## 新增.env檔
```
複製其他專案.env檔，新增自新專案內
```

## 設定.env檔
1. 修改專案名稱
```
APP_NAME="Smart BP" //改為專案名稱
```
2. 重新生成key
```
terminal進入專案資料夾 
輸入指令
php artisan key:generate
```
3. DB設定
設定成步驟1相同之參數
```
DB_DATABASE=smart_bp
DB_USERNAME=smart_bp
DB_PASSWORD=fk539gDRx
```

## 設定路徑
1. Terminal進入Apache
```
cd /opt/homebrew/var/www
```
2. 產生路徑
```
sudo ln -s 專案路徑/public 專案名稱
```

## 創建資料表
1. Terminal進入專案
```
cd 專案路徑
```
2. 執行migrations內的所有sql
```
php artisan migrate
```
### 注意
* 回到上一步
```
php artisan migrate:rollback
```
* 全部刪除再重新執行
```
php artisan migrate:refresh
```
## 創建基礎資料
1. Terminal進入專案
```
cd 專案路徑
```
2. 執行指令(將會執行database>seeders>BaseSeeder.php檔案)
```
php artisan db:seed --class=BaseSeeder
```