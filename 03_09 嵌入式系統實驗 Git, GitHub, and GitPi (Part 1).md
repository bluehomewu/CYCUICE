# 03/09 嵌入式系統實驗 Git, GitHub, and GitPi (Part 1)

<small>2022/03/09 嵌入式系統實驗</small>
<small>本篇會著重在解釋 Git 指令的用法</small>
<small>GitHub 以及 GitPi 應該會在街下來幾周才會上到，這篇就先不贅述</small>
<small>Git 的背景可以參考 [為你自己學 Git](https://gitbook.tw/chapters/introduction/what-is-git) 這篇文章</small>

本篇環境皆會在 Raspberry Pi 下進行操作， Windows 系統或是 macOS 可以到 [Git](https://git-scm.com/) 下在對應自己系統的 Git 安裝程式進行安裝


前置作業：
在 Raspberry Pi 上安裝 Git
```
$ sudo apt update            ## 更新apt 套件倉庫
$ sudo apt install git -y    ## 下載並安裝 git
```

Git
---
什麼是 Git?<i class="fa fa-github-alt"></i>
如果你問大部份正在使用 Git 這個工具的人「什麼是 Git」，他們大多可能會回答你「Git 是一種版本控制系統」，專業一點的可能會回答你說「Git 是一種分散式版本的版本控制系統」。「版本控制系統」的英文是「Version Control System」，但這個回答，對沒接觸過的新手來說，有講跟沒講差不多。到底什麼是「版本」？要「控制」什麼東西？什麼又是「分散式」？

資料來源：[什麼是 Git？為什麼要學習它？ - 為你自己學 Git | 高見龍](https://gitbook.tw/chapters/introduction/what-is-git)

使用 Git 的過程中，會學到 repo（repository 儲存庫）、 branch（分支）、tag（標籤）、commit（提交）...... 等很多指令以及物件的用法

1. [設定 Git 環境](https://gitbook.tw/chapters/config/user-config)
```
$ git config --global user.email "您的 E-mail"
$ git config --global user.name "您的名字" 
$ git config –-global core.editor "偏好的編輯器" ## Linux 內支援 vi / vim / nano
$ git config –-global  core.autocrlf true/input   ## 由於 Windows 與 macOS 的系統特性差異，導致會有不同形式的換行符 

## –-global 參數表示對系統全局進行進行設定
## --local  參數表示只對該 repo 進行設定
```
![](https://i.imgur.com/6vHzmAE.png)

[Git 其他方便環境設定](https://gitbook.tw/chapters/config/convenient-settings)

2. [開始使用 Git](https://gitbook.tw/chapters/using-git/init-repository)
```
$ cd ~/              ## 切換至使用者家目錄 ( ~/ 可以等同於 /home/使用者名稱 )
$ mkdir git-learn    ## 建立一個名為 git-learn 的資料夾在家目錄底下
$ cd git-learn       ## 切換至 git-learn 資料夾
$ git init           ## 初始化這個目錄，讓 Git 對這個目錄開始進行版控
```
![](https://i.imgur.com/MOPh3Vs.png)

其中，第 4 步這個指令會在這個目錄裡建立一個 .git 目錄，整個 Git 的精華就都是在這個目錄裡了。如果有興趣可以先看一下這個目錄裡面的內容

> 小數點開頭的目錄或檔案名稱（例如 .git），在一些作業系統中預設是隱藏的，可能會需要開啟檢視隱藏檔之類的設定才看得到。

3. [透過 Git 管理檔案](https://gitbook.tw/chapters/using-git/add-to-git)

#### 檢查資料夾狀態
```
$ git status     ## 檢查狀態
```
![](https://i.imgur.com/vjfPviJ.png)

在這個資料夾內，目前除了初始化 git 控制時所建立的 `.git/` 隱藏資料夾以外，什麼都沒有，所以這段訊息就是要跟你說「現在沒東西可以提交（nothing to commit）」

現在，我們可以在這個資料夾內開始進行檔案的管理。
#### 編輯檔案
首先，使用 nano 編輯器建立並打開一個名為 `Hello-Git.txt` 的記事本文件
![](https://i.imgur.com/jJk0jwA.png)

在 `Hello-Git.txt` 的記事本文件內輸入 Hello Git
接著依序按下
```
Ctrl+O ## 儲存檔案 > Enter > Ctrl+X ## 離開 nano 編輯器
```
![](https://i.imgur.com/wNp6QRU.png)

在編輯完檔案之後，再次使用 git status 指令檢查資料夾狀態
```
$ git status     ## 檢查狀態
```
![](https://i.imgur.com/Bmc79te.png)

現在可以看到 Hello-Git.txt 這個檔案已經被標示為紅字並且歸類於 Untracked files，表示目前才剛加入資料夾，還沒被 Git 追蹤管理


#### 讓 Git 接手管理檔案
讓 Git 接手管理檔案，所需要用到的指令為 `git add` 後方加上檔案名稱

```
$ git add Hello-Git.txt         ## 把 Hello-Git.txt 加入暫存區
```
![](https://i.imgur.com/p3Xi0Qm.png)

> 情境一：
> 如果同時有多個 .txt 副檔名格式的檔案要一並加入暫存區，則可以使用 `git add *.txt` 
> 透過 * 這個萬用字元把所有副檔名為 txt 的檔案加入

> 情境二：
> 有兩個檔案要加入暫存區，可以在原有的 git add 之後分別加上兩個檔案的檔名
> 例如： `git add file1.txt file2.txt` 

>情境三：
>將子資料夾下的所有變更都新增至暫存區
>則可以使用 `git add .` 或是 `git add --all`

再度檢視資料夾狀態
```
$ git status     ## 檢查狀態
```
![](https://i.imgur.com/OC3Oq1H.png)

在執行 git add 之前，檔案是處於未被追蹤管理的狀態，執行之後，可以看到 Hello-Git.txt 由紅字轉為綠字  
並且狀態由 Untracked file 變成 new file

#### 把暫存區的內容提交到倉庫裡存檔
`git add` 只會把檔案的異動加入暫存區，並沒有在本機儲存庫進行存檔
因此，我們必須使用 `git commit` 將檔案進行提交並且為此次變更進行簡短的提交內容描述。
```
$ git commit -m "Initial commit"
##  -m 參數表示 message 後方需要需要使用雙引號 "" 並且將提交內容鍵入到雙引號內
##  也可以使用 git -am 參數去取代前面加入檔案進暫存區的 git add 指令
##  Initial commit 意指「初始提交」
```

![](https://i.imgur.com/huLsUu3.png)


>`git commit` 這個指令只會把暫存區內的檔案進行提交，所以還處於 Untracked files 的檔案室無法提交的
>需要完成 git commit 才是真正意義上的完成處理版本控制

> Commit 內容要怎麼寫？
> Commit 內容需要簡潔有力，能夠讓其他共同開發的人可以快速的理解這一條的 commit 修改了什麼東西


4. [如何在 Git 裡刪除檔案或變更檔名？](https://gitbook.tw/chapters/using-git/rename-and-delete-file)

利用 Git 來刪除檔案或是修改檔案名稱，跟手動刪除或是變更檔案名稱，最大的區別在於，如果使用 Git 來完成這些事情，則可以一次性加入變更至暫存區，就可以省去一個步驟。  
那麼接下來我們看看如何使用 Git 來進行刪除或變更檔案名稱

```
$ rm -rf Hello-Git.txt    ## 刪除 Hello-Git.txt
$ git status              ## 檢查狀態
```

![](https://i.imgur.com/pF23Z5s.png)

目前這個步驟只有在 local repo 內刪除檔案，且尚未提交「修改」到暫存區  
注意！對於 Git 來說， Users 不是把刪除檔案這件事新增到暫存區，而是新增 「刪除檔案這個***修改***」到暫存區

##### 使用 `git rm` 或是 `git mv` 修改檔案
原本需要使用 `rm` 指令刪除檔案 或是 `mv` 指令移動檔案 / 變更檔案名稱，之後還需要手動使用 `git add` 將修改新增到暫存區
在改用 `git rm` 或是 `git mv` 之後，便可以省去 `git add` 的步驟

```
$ git rm Hello-Git.txt                   ## 透過 Git 刪除 Hello-Git.txt
$ git mv Hello-Git.txt Hello-GitHub.txt  ## 透過 Git 把 Hello-Git.txt 改名成 Hello-GitHub.txt
$ git status                             ## 檢查狀態
```

![](https://i.imgur.com/FFXW48A.png)


##### 加上 –cached 參數
不管是系統的 `rm` 或是 `git rm` 指令，都會真的把這個檔案從工作目錄裡刪掉，但如果只是「不是真的想把這個檔案刪掉，只是不想讓這個檔案再被 Git 控管了」的話，可以加上 `--cached` 參數

```
$ git rm Hello-Git.txt --cached   ## 透過 Git 刪除 Hello-Git.txt 在 git 內的追蹤狀態
$ git status                      ## 檢查狀態
```

![](https://i.imgur.com/rMCnbAt.png)



5. 檢視檔案詳細資訊

詳細資訊這部分可以分為三種檢視
- 第一種：檢視 commit history
- 第二種：檢視某一條 commit 內對檔案「們」的修改
- 第三種：檢視某個檔案在某條 commit 上的內容

對於第一種，可以使用 `git log` 指令來檢視
```
$ git log    ## 檢視 commit history
可以使用 --oneline 參數來檢視 commit history name
```
![](https://i.imgur.com/XWWDUcT.png)

第二種，可以使用 `git show <SHA-1>` 

```
$ git show 4161a46   ## 檢視 4161a46 這條 commit 內對檔案們的修改
```
![](https://i.imgur.com/HNwFniG.png)

第三種，可以使用 `git show <SHA-1>:<檔案名稱>` 

```
$ git show HEAD:Hello-Git.txt   ## 檢視 Hello-Git.txt 這個檔案在 HEAD(4161a46) 這條 commit 時的內容

## 如果沒有填上 <SHA-1> 則會表示暫存區的檔案
```

> 注意！ HEAD 是一個指標，指向某一個分支，通常你可以把 HEAD 當做「目前所在分支」看待。
> 資料來源：[【冷知識】HEAD 是什麼東西？](https://gitbook.tw/chapters/using-git/what-is-head)

![](https://i.imgur.com/SEnsEU7.png)



6. 比對檔案不同 commit 之間的差異
比對檔案內容的差異，可以使用 `git diff` 
```
$ git diff --staged      ## 與暫存區的檔案進行比對
$ git diff --check       ## 合併上游分支時比對衝突檔案
```
7. 復原檔案變更
復原檔案變更可以使用 `git restore`
```
$ git restore --staged  <檔案名稱>         ## 復原暫存區內特定檔案的變更
$ git restore --source=<SHA-1> <檔案名稱>  ## 使用某一個版本下的內容來復原檔案變更
```



學習 Git 的參考資料
---
1. [為你自己學 Git](https://gitbook.tw/)
2. [為你自己學 Git 筆記 — GitHub 應用篇](https://link.medium.com/oQfJxibSRbb)
3. [修改 Commit 紀錄](https://gitbook.tw/chapters/using-git/amend-commit1.html)
4. [剛才的 Commit 後悔了，想要拆掉重做…](https://gitbook.tw/chapters/using-git/reset-commit.html)
5. [追加檔案到最近一次的 Commit](https://gitbook.tw/chapters/using-git/amend-commit2.html)
6. [同步遠端分支](https://zlargon.gitbooks.io/git-tutorial/content/remote/sync.html)
7. [Git 基礎 - 與遠端協同工作](https://git-scm.com/book/zh-tw/v2/Git-%E5%9F%BA%E7%A4%8E-%E8%88%87%E9%81%A0%E7%AB%AF%E5%8D%94%E5%90%8C%E5%B7%A5%E4%BD%9C)
8. [rename git branch locally and remotely](https://git.io/JIUKI)
9. [Git cherry-pick from another repository](https://coderwall.com/p/sgpksw/git-cherry-pick-from-another-repository)
10. [使用 Git 時如何做出跨 repo 的 cherry-pick](https://blog.m157q.tw/posts/2017/12/30/git-cross-repo-cherry-pick/)
11. [git 場景 ：從一個分支cherry-pick多個commit](https://www.itread01.com/content/1529341336.html)
12. [合併發生衝突了，怎麼辦？](https://gitbook.tw/chapters/branch/fix-conflict.html)
13. [下載Github上特定Repository內的資料夾](https://notes.andywu.tw/2018/%e4%b8%8b%e8%bc%89github%e4%b8%8a%e7%89%b9%e5%ae%9arepository%e5%85%a7%e7%9a%84%e8%b3%87%e6%96%99%e5%a4%be/)


[GitHub](https://github.com/)
---
GitHub 是一個透過Git進行版本控制的軟體原始碼代管服務平台
同時還有許多類似的平台提供服務，如 [GitLab](https://gitlab.com/) 或是 [Gitee]([https:/](https://gitee.com/)/)

資料來源：[GitHub 維基百科](https://zh.wikipedia.org/zh-tw/GitHub)

GitPi
---
在自己的 Raspberry Pi 上搭建一個使用 Git 的軟體原始碼代管服務平台


同場加映
---
- [03/02 嵌入式系統實驗 SSH & VNC](https://hackmd.io/@EdwardWu/SSH_VNCTutorial)
- [03/09 嵌入式系統實驗 Git, GitHub, and GitPi (Part 1)](https://hackmd.io/@EdwardWu/GitTutorial)

###### tags: `嵌入式系統實驗` `Linux` `Git` `GitHub` `GitPi` `中原資工`