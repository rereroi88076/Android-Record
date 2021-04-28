# Git紀錄
## 上傳

```
1.到目錄
2.git status //確認更新項目
3.git add . //將檔案交給git 
4.git commit -m '' //說明
5.git push
```
    
## 下載

```
1.到目錄
2.git clone 網址 
```

## 第一次上傳

```
1.到目錄
2.git init 初始化
3.git remote add origin https://xxxx
(git remote -v 查看遠端位置)
4.git add . //將檔案交給git 
5.git commit -m '' //說明
6.git push -u origin master //第一次上傳到雲端
```

## 回退版本

```
1.到目錄
2.查找版本編號 //commit_id
git reflog (印出全部記錄)
git log --oneline -n (n代表印出幾次紀錄)
3.回退的方法 //原檔案皆刪除
git reset --hard HEAD~ (1個波浪代表回退1次)
git reset --hard HEAD~~ (2個波浪代表回退2次)
git reset --hard HEAD~3 (波浪後面接數字，代表回退幾次)
git reset --hard commit_id (用log的commit_id也可以直接指定回退)
```

## 分支那些事
```
1.到目錄
2.git branch //查看分支(*代表目前分之)
3.git branch a//新增分支a(a是分支名)
4.git checkout -b a //快速版 (新增+切換分支)
5.git branch -m a b //幫分支改名(將a改成b)
6.git branch -d a //刪除分支(將a分支刪除)
7.git branch -D a //強制刪除分支(將a分支強制刪除)
8.git checkout a //切換分支(切換至a分支)
```
## 合併
```
1.到目錄
2.切換到要合併人家的分支
3.git merge 分支名 (快速合併)
4.git merge 分支名 --no-ff (有耳朵的合併)
```