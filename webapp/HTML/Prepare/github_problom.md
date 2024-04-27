# 在vscode和Github的一些问题及解决方法(持续补充完善改正)
> 声明：记录，为了让自己更好的理解问题，自己会常检查纠错。
## 一.本地仓库和远程仓库
* 本地仓库是指存储在本地计算机上的Git仓库，它包含了项目的完整历史记录和所有版本的文件。本地仓库可以进行版本控制、分支管理和代码提交等操作，是开发者在本地进行代码管理和开发的基础。
* 远程仓库是指存储在远程服务器上的Git仓库，它用于多人协作开发和备份代码。开发者可以将本地仓库的代码推送到远程仓库，也可以从远程仓库拉取最新的代码。远程仓库通常由代码托管平台（如GitHub、GitLab等）提供，也可以自行搭建。
## 二.4.14-- 的错误及解决方法
### 1.在Github上直接拉取文件放入库中？
答案是：NO。 <br>
起初第一次在vscode上编写了一个.html文件兴致冲冲想放入Github，直接将文件夹拉上去。(一直认为Github跟qq空间似的，存东西放东西发东西)
**直接把本地文件夹拖拽至GitHub网页版的仓库界面并不能实现版本控制和同步功能。**
 GitHub网页本身并不支持直接通过拖拽文件或文件夹的方式来上传或更新整个项目目录。如果想将本地文件夹的内容同步到GitHub仓库，需要遵循标准的Git工作流程，具体步骤以下几点：
1. **在本地初始化Git仓库**：
   在本地文件夹内运行 `git init` 命令，将其转换为Git可以管理的版本库。
2. **添加文件并提交**：
   使用 `git add .` 添加所有文件到暂存区（`.` 表示所有文件和子目录），或者添加特定文件。
   运行 `git status` 查看状态，确认哪些文件已被跟踪和暂存。
   运行 `git commit -m "描述性提交信息"` 来提交暂存区的更改。
3. **连接到GitHub仓库**：
   如果还没有关联远程仓库，需要先在GitHub上创建一个新的仓库，获取仓库的HTTPS或SSH克隆地址。copy啦~
   然后，在本地通过 `git remote add origin <远程仓库地址>` 命令添加远程仓库。
4. **推送本地更改到GitHub**：
   最后，使用 `git push -u origin master` （如果是主分支）或其他分支名来推送本地仓库的改动到GitHub上。

**这样，本地文件夹内容就会被正确地同步到GitHub仓库中。请注意，若初次推送，可能需要 `-f` 参数强制推送，（老师说过的fouse），因为本地没有与远程分支的共享历史记录。现在大多数新建的GitHub仓库默认分支名为 `main` 而不再是 `master`，所以实际命令是<br> `git push -u origin main`。同时，请确保已经正确设置了SSH密钥，以便无密码访问GitHub。**

5. **初次推送通常不需要 `-f` 参数**：
   `-f` 参数代表 `--force`，用于强制推送（force push）。在初次将本地仓库内容推送到一个全新的GitHub仓库时，由于本地分支和远程分支之间并没有任何历史记录，因此正常情况下不需要使用 `-f`。只需常规推送即可。

6. **推送命令**：
   在过去，GitHub上的默认分支名称通常是 `master`，当你在本地创建了一个新的Git仓库，并且想把它推送到GitHub上新创建的一个同样为空的仓库时，会使用这样的命令：
   ```sh
   git push -u origin master
   ```
   这条命令做了两件事：
   + `-u` 参数是 `--set-upstream` 的简写，它建立了本地分支与远程分支之间的**追踪**关系，这样后续可以直接使用 `git pull` 和 `git push` 而无需指定远程分支。
   + `origin` 是远程仓库的别名，默认指向GitHub上的仓库。这个我记得是可以改的，最后指向远程仓库。
   + `master` 以前是默认的主分支名称，但现在改为 `main`。

7. **现代实践：推送至 main 分支**：
   自2020年起，GitHub开始逐步将默认分支名称由 `master` 改为 `main`。所以在当前及之后创建的新仓库中，初次推送时应该使用：
   ```sh
   git push -u origin main
   ```
   这样操作将会把本地的当前分支内容推送到GitHub上的 `main` 分支，并建立追踪关系。

8. **SSH密钥设置**：
   若要无密码访问GitHub，需要配置SSH密钥对。 <br>
   这意味着在本地生成一对密钥（公钥和私钥），并将公钥添加到GitHub账户的SSH密钥列表中。这样每次与GitHub交互时，Git客户端会自动使用私钥进行身份验证，从而避免每次推送或拉取时都需要输入账号密码。这是提高效率和安全性的常见做法。**密钥建立过程很顺利，虽然那时候不知道有什么用。**

**总结起来，首次将本地Git项目推送到GitHub的新仓库时，正确的命令应为 `git push -u origin main`，前提是已正确设置了SSH密钥以实现无密码访问。**

----
### 2.git时遇见的部分问题(记录篇)
> 声明:此时我并不知道有好几种推送方式而且不明白其中的关联，就有了乱七八糟混用，文件乱七八糟乱移动的行为。
>>我说的web代指我的GitHub里的web_workspaces库。java表示以后打算写java的库。只有这两个库。
经过上述直接拉到库里的失败，先删了直接拉的文件，我开始找新路子，就是正确的拉取方式= =嘻嘻。
- 为了将文件推到GitHub的远程仓库里(就是那个直接拉文件夹的web仓库)，下载了git(包括其他需要的东西)。
- 填写用户名和邮箱，很成功。
- 下面就是弄SSH密钥(上面也说了，成功)。
- [学习网址](https://zhuanlan.zhihu.com/p/193140870)
---
##### 错误1：
- 4.18在知乎上看到的一篇文章。直接在桌面创建了一个文件夹，命名为test(其实本身就是为了测试)，经过知乎博主的教授，通过命令`git init`把这个文件夹变成Git可管理的仓库(当时并不知道这是给本地说，弄个仓库出来，只是跟着博主做)，然后里面多了一个.git文件(隐藏的)，知乎博主说：“这时候你就可以把你的项目粘贴到这个本地Git仓库里面（粘贴后你可以通过git status来查看你当前的状态）”。
- 我当时执着于，我的文件必须在我规定好的D盘，不！能！动！所以直接跳过了这个步骤，随便写了一个.txt文件后通过git add把项目添加到仓库。然后，用git commit把项目提交到仓库。
- 后面就是要和GitHub进行关联，博主说重新创建一个仓库，我心想我有了想放的仓库，省略了。然后根据他的步骤，就把我的web仓库和桌面这个test文件关联上了。(此时并不知道个GitHub仓库不能直接关联两个完全独立的本地文件夹作为两个不同的本地仓库)。那天测试完，就把test删了，觉得没啥用。发现GitHub上面那个test还在(上面也说了，以为跟qq空间似的)，然后在GitHub上把test删了。
[参考的链接](https://zhuanlan.zhihu.com/p/28377120#:~:text=%E6%80%BB%E7%BB%93%EF%BC%9A%E5%85%B6%E5%AE%9E%E5%8F%AA%E9%9C%80%E8%A6%81%E8%BF%9B%E8%A1%8C%E4%B8%8B%E9%9D%A2%E5%87%A0%E6%AD%A5%E5%B0%B1%E8%83%BD%E6%8A%8A%E6%9C%AC%E5%9C%B0%E9%A1%B9%E7%9B%AE%E4%B8%8A%E4%BC%A0%E5%88%B0Github%201%E3%80%81%E5%9C%A8%E6%9C%AC%E5%9C%B0%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA%E7%89%88%E6%9C%AC%E5%BA%93%EF%BC%88%E5%8D%B3%E6%96%87%E4%BB%B6%E5%A4%B9%EF%BC%89%EF%BC%8C%E9%80%9A%E8%BF%87git%20init%E6%8A%8A%E5%AE%83%E5%8F%98%E6%88%90Git%E4%BB%93%E5%BA%93%EF%BC%9B%202%E3%80%81%E6%8A%8A%E9%A1%B9%E7%9B%AE%E5%A4%8D%E5%88%B6%E5%88%B0%E8%BF%99%E4%B8%AA%E6%96%87%E4%BB%B6%E5%A4%B9%E9%87%8C%E9%9D%A2%EF%BC%8C%E5%86%8D%E9%80%9A%E8%BF%87git,add.%E6%8A%8A%E9%A1%B9%E7%9B%AE%E6%B7%BB%E5%8A%A0%E5%88%B0%E4%BB%93%E5%BA%93%EF%BC%9B%203%E3%80%81%E5%86%8D%E9%80%9A%E8%BF%87git%20commit%20-m%20%22%E6%B3%A8%E9%87%8A%E5%86%85%E5%AE%B9%22%E6%8A%8A%E9%A1%B9%E7%9B%AE%E6%8F%90%E4%BA%A4%E5%88%B0%E4%BB%93%E5%BA%93%EF%BC%9B)

**1总结：**
- 要想推什么文件夹去Github，就要在当前文件夹的上打开git push并进行git init命令，才能在它的子文件夹里出现隐藏目录.git。<br>
这个意味着：**初始化一个Git仓库：** git init命令会在这个指定的文件夹内创建一个全新的本地Git仓库。**创建.git目录：**Git会在该文件夹内部创建一个隐藏的.git目录，这个目录包含了所有版本控制所需的信息，如提交历史、暂存区域（stage）、分支信息等核心数据。**版本控制开始生效：** 从此刻起，**该文件夹及其子目录下的所有文件** 都处于Git的版本控制之下，不过此时尚未有任何文件被跟踪（tracked）。若要开始跟踪文件，还需要进一步执行git add命令将文件添加到暂存区，随后通过git commit命令来创建提交（commit）。
-  一个GitHub仓库对应的是一个单一的版本控制历史记录，所以它在本地只能有一个主要的关联仓库（尽管可以通过子模块包其他仓库）。
- 初始化一个Git仓库（Repository）：git init命令会在这个指定的文件夹内创建一个全新的本地Git仓库。

创建.git目录：Git会在该文件夹内部创建一个隐藏的.git目录，这个目录包含了所有版本控制所需的信息，如提交历史、暂存区域（stage）、分支信息等核心数据。

版本控制开始生效：从此刻起，该文件夹及其子目录下的所有文件都处于Git的版本控制之下，不过此时尚未有任何文件被跟踪（tracked）。若要开始跟踪文件，还需要进一步执行git add命令将文件添加到暂存区，随后通过git commit命令来创建提交（commit）。 如果希望在不同地方处理同一仓库的不同部分，可以分别在不同地方克隆（clone）同一个GitHub仓库，而不是关联两个不同的本地文件夹。在处理完各自的更改后，通过push操作将更改上传回相同的GitHub仓库。**？此处克隆理解理解为：库里的文件在本地仅仅处理，不会对远程库里的文件造成影响**

- - - -
##### 错误2：
- 经过1，我想把我的Tag.html push到Github的web上，就在单单的Tag.html上弄了隐藏文件夹，后面又在vscode里想推到github里，发现一直没成功。此处应该是我的远程仓库web已经关联了我错误事例1的test我文件夹，但是我这时候不知道要把关联远程仓库删掉。
- 又因为怕后期混乱，想给我的Tag.html弄个目录，让它成为一个非常小的分支似的，又给它上面套了几个文件夹，最顶上的是名为：web_examples文件夹**此处犯的一大忌：要想把整个文件夹放到远程仓库里，需要在那个文件夹上面弄隐藏文件，并且子文件不能有隐藏文件，不然推不上去**。当然，我一直不知道缘由所以一直没成功推上去。
- 后面实在没办法，就在vscode里摸索，开始因为里面好多东西不认识，没下汉化包，乱点了什么东西，让我的这个文件夹和Java库关联上了，我发现也一直弄不上去，又把web库删掉了，重新创建了一个和文件夹同名的web_examples库。

**总结：**
1. 要推某个文件夹，放到远程仓库里，需要在那个文件夹上面弄隐藏文件，并且子文件不能再有隐藏文件。
2. 要是想保存某个库，而不想要当前文件夹，及时取消关联的远程库。`git remote -v`查看当前关联的远程仓库信息。
`git remote remove origin`取消关联某个远程仓库。执行完上述命令后，本地仓库就不再是GitHub上那个仓库的副本，也不会再能通过 git pull 或 git push 直接与那个远程仓库交互。然而，本地仓库的文件和历史记录仍然会被保留。如果想完全清除本地仓库的.git目录（即去除Git版本控制），则需要手动删除 .git 文件夹。但通常情况下，仅需解除远程关联即可满足需求。
3. 从本地取消与GitHub远程仓库的关联时，这并不会影响远程仓库中的文件。远程仓库中的文件依然存在，并不受本地操作的影响。远程仓库的内容会一直保持你在取消关联前最后一次推送的状态。直到执行了删除远程仓库中相应文件的操作。
- - - -
##### 错误3—4.20：
- 由于我一直没给web_examples弄本地的git，肯定传不上去啊，在网上查完之后，加上问老师了缘由，就把这个文件夹下面所有的隐藏目录删掉，想在web_examples上面弄隐藏目录.git。猜猜怎么着？右键->Open git push here->找不到程序...此时已经陷入崩溃，不明白啊又在网上搜（老师批评我当时把Git的下载文件乱七八糟一顿移，给我改了环境变量）。
- 网上说调终端regedit，知道这个通过启动 regedit，用户可以浏览和修改注册表中的键值对（Keys 和 Values），这些键值对包含了诸如系统设置、应用程序配置、用户个性化选项、驱动程序详细信息以及其他系统级别的参数等内容。应该是我要打开它，但是位置不对所以打不开。改了之后还是没打开，下次问老师= =。
[regesit的链接](https://blog.csdn.net/qq2539879928/article/details/106082373)
- 不过通过打开终端也能调用git,就先成功给我的文件夹把隐藏目录成功搞上了。
- 后面又去vscode里，想给我推到web_examples库里，一直不成功。(此处显示fatal: remote origin already exists.)后面通过搜索知道，我已经将一个远程仓库添加并命名为origin了，现在我是在尝试将另一个远程仓库(web_examples库)添加并命名为origin，显然这是一个错误，一个origin怎么指向两个仓库呢？这个应该是给之前的Java库命名的origin，又发现我要推的一直是master分支，找不到我的web_examples里面的分支webapp。然后我用的方法是换个名字指向另一个远程仓库就可以了，远程库web_examples库别名origin2。
[解决链接](https://blog.csdn.net/qq_34769162/article/details/116379638)

- 后面推的时候不小心还是推到master上了，本来想推到webapp上，后面一向给webapp重命名为web，master改个名改成了webapp，成为了一个新分支，先在这个里面弄项目。

**总结：**
1. 别乱移动下载的文件，移动后也要知道改一些配置。
2. vscode中，view视图->终端,也可以进行相应的git操作。
3. 直接在git push中也可以推(我目前还是没解决右键点开无法打开的情况)。
4. 推的时候注意分支。
5. 多看报错。

- - - -
#### 错误4-4.22：
今天单单利用打开终端的方式，将我打算测试的内容推到远端的私有库test中。里面推送过一次文件(到后面通过试验应该知道了这个文件和远程仓库已经关联上了形成追踪关系)。
1. 
- 首先写了一个test.html的文件，单单用来测试本地仓库到远端仓库的关联推送。 <br>
- 然后利用储存这个文件的上一目录文件夹打开终端(因为我到现在都没弄明白为什么打不开我的git push)，进行本地工程的创建。
- `git init`  初始化一个本地仓库
- `git add .` 将当前文件添加到这个本地仓库
- `git commit -m'test2'` 提交到本地仓库，附加信息：test2
- `git remote add origintest2  https://github.com/yue24413/xxxxxx.git ` <br>
前面我把关联的远程仓库名别名改成了origintest2，后面xxxxx是我创建的用来测试的私有仓库名儿。
- `git remote -v` 我查看了一下是不是关联上了我的远程仓库
- `git push origintest2 main` ,报错，我想的是是将这个文件推到远程仓库的main分支里。我本地默认分支不叫main，是master，**实际上，本地分支master与main不同名，需要通过类似 git push origin local_branch:main 的方式来指定将本地的 local_branch 分支内容推送到远程的 main 分支。**
我确保当前本地分支是 master，并且想将 master 分支的提交推送到远程仓库的 main 分支，可以直接运行以下命令例如：(我的部分别名分支名修改了) <br>
`git push origin master:main`
这里的 master:main 表示将本地 master 分支的HEAD推送到远程仓库 origin 的 main 分支上。这样，即使本地和远程分支名称不同，也能实现需求。
但是这样的操作可能并不符合常规的分支管理策略，除非有特别的原因，否则通常保持本地和远程分支的对应关系清晰一致。如下
- `git branch --set-uostream-to=origintest2/master master` 我又想给他推到分支名为master里，此时并不明白加不加 -u的区别，后面以无法连接上报错。**解释：**
- git branch: 这是用于操作分支的核心 Git 命令
- --set-upstream-to: 或 -u，这是一个选项，用来设置或更改当前分支（在这个例子中是 master）的上游（upstream）**追踪**关系。
- origintest2/master: 这是指定的上游分支，表示**远程仓库 
- origintest2 中的 master 分支**。当设置上游后，本地分支会“追踪”这个远程分支，执行 git pull 时会默认从这个远程分支获取并合并更新，执行 git push 时也会默认推送到这个远程分支。
- master: 这是需要设置上游追踪关系的**本地分支**名称。
总结起来，git push -u origin master 这个命令的作用是**首次**(后面总结会说为什么首次)将本地 master 分支的内容推送到名为 origin 的远程仓库的 master 分支，并设置**本地分支与远程分支之间的追踪关系**。这样以后可以简化后续的推送操作。
- 关闭，重新来。
- - - 
#### 错误4.22第二次
- `git init` `git add .` `git commit -m'test2commit'`,改了附加信息
- `git pull origintest2 master` <br>
fatal:refusing to merge unrelated histories  <br>
这意味着Git发现试图合并的两个分支没有共同的历史记录（即它们是完全独立的两个项目或分支）。我这个是两个各自都git的，可以说是两个项目。
Git出于对历史完整性的保护，默认不会允许直接合并两个不相关的项目或分支。如果确实需要合并这两个不同的历史记录，可以在执行push或merge时添加 --allow-unrelated-histories 选项来**强制执行**此操作。
- 然后我就执行：`git pull origintest2 master--allow-unrelated-histories`，又有：fatal:couldn't find remote ref–allow-unrelated-histories,它不能识别我这句话。
- 搜索了一下，只要确保我的远程仓库里有master分支，可以用：`git pull --rebase origin master
git push origin master` <br>
> 我这块将origin改成origintest2,我自己命名的别名不同。
  [解决链接](https://blog.csdn.net/brownsville2/article/details/102246961)
 - 然后成功传上了。 <br>
 [Git 分支的 master、origin、origin/master 区别](https://blog.csdn.net/reykou/article/details/104866348)

**总结：**
1. 如果用关联远程仓库的方式，得用`git remote add origin +地址`的方式关联远程仓库，origin是远程仓库默认别名，可以改。
2. 在Git中，实际关联的是文件而非文件夹。也就是说，两个不同的本地文件夹不能直接共享一个Git仓库实例，上述我将两个文件夹关联到了库里，就造成合并两个不相关的项目或分支的情况，然后我强行让它们合并，可以看出我上次上传到master分支的名为4.22test.html文件，这个文件在本地同时也在被我强行合并的文件夹中。作为子文件了。
3. 养成好习惯，在日常的版本控制实践中，一个项目通常对应一个本地文件夹，并且这个本地文件夹与一个远程Git仓库进行关联。上述说到，首次和一个库关联，因为本地的这个项目文件夹只需要和远程库关联一次，并形成追踪关系，后面就非常容易修改推送。这种做法有助于清晰地管理和跟踪项目的所有变更，同时也能方便团队成员协作。不要利用多文件夹指向同一个库中，会让其中一个项目成为另一个项目的子项目。
- - - 
## 三.  .md文件的编写
[Markdown /.md学习链接](https://blog.csdn.net/carcarrot/article/details/119769300)

## 四.  GitHub学习
[GitHub学习链接](https://www.bilibili.com/read/cv9109010/?spm_id_from=333.999.0.0v)
### 1.分支的作用###
GitHub 上分支的主要作用在于支持并行开发、版本控制、协作和代码质量管理。具体来说：
1. **并行开发**：
   - 分支允许团队成员同时在项目上进行不同的任务，每个成员可以在自己的分支上开发新的功能、修复 bug 或者优化代码，而这些工作不会相互干扰。
   - 每个分支都有自己的代码副本，使得团队能够高效地处理多个特性或修复，同时保证主分支（通常是 `main` 或 `master`）的稳定性。

2. **版本控制**：
   - 分支可用于追踪不同版本的源代码，每个分支可以代表项目的一个**阶段性进展或特定功能。**
   - 开发者可以在分支上独立地添加、修改和删除代码，这些更改仅影响当前分支，从而便于维护不同版本的应用程序或组件。

3. **团队协作与代码审查**：
   - 团队成员可以通过创建个人分支共享和讨论各自的工作，促进代码审查过程。
   - 各自完成工作后，通过 Pull Request（拉取请求）的形式将分支**合并**回主分支，这期间可以邀请他人评审代码，接受反馈和建议，确保代码质量。

4. **问题修复与风险管理**：
   - 当需要快速修复主分支上的紧急问题时，可以创建一个临时分支进行调试和修复，避免影响正在进行的其他开发工作。
   - 修复完成后，分支合并回主分支，将修正后的代码集成到项目中。

5. **持续集成/持续部署（CI/CD）**：
   - 在自动化构建和部署流程中，可以使用特定的分支策略（如保护分支），只有满足一定条件（如所有测试通过）的代码变更才能合并进主分支。

6. **实验与探索性开发**：
   - 分支还可以用来试验新的想法或重构现有代码，即使实验失败也不会直接破坏主分支的稳定性。
总结起来，GitHub 的分支系统是 Git 分布式版本控制系统的核心部分之一，它极大地促进了软件开发的灵活性、协同性和可靠性。通过有效地管理分支，团队能够更高效地进行迭代开发，同时也更好地遵循软件工程的最佳实践。
> 来源于通义千问。

### 2.创建合并分支###
- `git branch<name>`:创建一个叫name的分支。
- `git switch<name>`：切换当前分支。switch也可以换成checkout。
- `git merge<name>`：合并某分支到当前分支。(一般合并到主分支)
- `git branch -d<name>`:删除分支。创建基础上加-d
### 3.解决分支合并冲突###
为什么会冲突呢？举个栗子：当我在我的分支进行修改并提交时，我的分支还没有合并到主分支，但是这个时候我的老师在主分支又修改提交**同一个文件**另一个版本，我的分支合并到主分支的时候会产生冲突。
- `git log --graph`:详细查看分支合并情况，也可以查看当前版本
- `git merge --no--ff -m'merage with no-ff'x1`：简易查看分支合并情况，x1是版本，可以将x1版本下的分支合并过来
- `git status`：查看合并冲突分文件。
- 手动解决冲突，打开文件，文件会显示两个冲突要修改的是什么，删除冲突，保留想要的。
- 再次提交，add和commit
### 4.分支管理及bug修复策略###
[学习链接](https://www.bilibili.com/video/BV17J411k7iX/?spm_id_from=333.788.recommend_more_video.5&vd_source=51e6dcb2185450a1245ee66e7b56b856)
- master 分支非常稳定，仅用来发布新版本，在其他的分支上干活。
- dev 分支，不稳定，可以作为干活分支，干完活合并到主分支，再在主分支中发布新版本。一个团队中，每个人在dev分支上干活，每个人都有自己的分支，是不是会合并到dev分支上。
- Bug 分支，当我在我的分支上工作时，还没提交。但是我发现master分支上有个大大的Bug，我得先此时自己的分支状态保存保护我的dev分支，然后去主分支master对bug建立新的临时分支用来修复，修复后合并分支，然后将临时分支删除，，再回到我的分支上保存好的状态继续工作。
- - -
- `git stash` 存储当前现场
- `git stash list` 查看git stash存储的修改列表
- `git stash apply` 恢复指定的stash内容，例如：`git stash drop stash@{0}`,上一条查看后就知道恢复哪个了
- `git stash drop` 删除指定stash内容，形式如上
- `git stash pop` 在我从主分支回来的时候执行这句，可以删除我之前保存的分支状态 <br>
 注意：在master分支上修复bug，并没有因此转移到我的分支上 
- `git cherry-pick<commit>` 在master分支上修复的bug，合并到dev分支，例如：`git cherry-pick cqy111`，cqy111就是提交所修改的bug版本
### 5.本地与远程交互/多人协作###
- `git remote -v` 查看远程库信息。本地新建的分支如果不推送到远程，对其他人就是不可见的。

- `git push origin master ` 从本地向远程推送主分支

- `git push origin branch-name` 从本地推送分支，例如推送Dev分支：git push origin dev。 <br>
从远程库clone时，默认情况下，你的小伙伴只能看到**本地的master分支**。
- `git checkout -b branch-name origin/branch-name`   在本地创建和远程分支对应的分支。（从不存在的分支建立，侧重**建立**） <br>
- 例子： 要在dev分支上开发，就必须创建远程origin的dev分支到本地：git checkout -b dev origin/dev <br>
推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，重新提交之后，再推送。
- `git  pull `  从远程抓取分支
- `git branch --set-upstream branch-name origin/branch-name`   建立本地分支和远程分支的关联 。（从已经存在的分支与远程进行关联，侧重**关联**）

**多人协作的工作模式通常是这样：**

1. 首先，可以试图用git push origin <branch-name>推送自己的修改；

2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

3. 如果合并有冲突，则解决冲突，并在本地提交；

4. 没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

5. 如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

- `git rebase`  可以把本地未push（没有向远程推送之前）的分叉提交历史整理成直线， 目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比




