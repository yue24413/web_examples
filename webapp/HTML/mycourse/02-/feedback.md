## feedback 2024.4.17第二次任务反馈
### 一.掌握基本 git/github 本地远程版本控制
#### 本地仓库和远程仓库
基于我自己的理解： <br>
* 本地仓库是存储在本地计算机上的Git仓库，它包含了项目的完整历史记录和所有版本的文件。本地仓库可以进行版本控制、分支管理和代码提交等操作。
* 远程仓库是指存储在远程服务器上的Git仓库，它用于多人协作开发和备份代码。开发者可以将本地仓库的代码推送到远程仓库，也可以从远程仓库拉取最新的代码。远程仓库通常由代码托管平台（如GitHub、GitLab等）提供。远程仓库可以理解为托管文件的地方，并且可以和本地仓库进行关联，实现本地和远程仓库之间进行高效率更新。

#### git本地远程基本操作
##### 基于vscode
- 在vscode里创建文件
- 写自己想写的内容
- 视图->终端
- commit
- push
##### 基于git push
- 在本地开一个文件夹专门存放本项目的文件，方便后面推送。
- 在此文件夹下写自己想写的内容。
- 打开git push
- `git init` 在当前文件夹下创建一个本地仓库,这个仓库的工作区即这个文件夹和子文件。
- `git add *` 将工作区的文件添加到暂存区。
- `git commit -m"修改信息"`
- `git status` 仓库当前状态。
- `git remote add origin url` 关联远程仓库。这里面的origin是后面url的别名,而url是远程库地址
- `git remote -v` 查看远程库信息。
- `git push -u origin master` 第一次推送master分支所有内容,并把本地的master分支和远程的master分支关联起来。
- `git push origin master`
- `git remote rm origin` 删除关联的(origin)远程仓库 
- `git remote -v` 查看远程库信息
- `git diff` 查看修改
- `git log` 显示从最近到最开始commit日志

### 二.掌握基于 gihub pull request 的团队项目开发
> origin; master; branch; branch types; commit; commit message types;
   push; pull; marge
> Github PR。操作基本可通过 vs code 实现
1. fork，源仓库至个人远程仓库
2. clone，个人仓库至本地
3. branch，创建 docs branch。有点麻烦，但还是掌握了吧
4. commit，docs branch 中修改提交
5. branch，切换回 master branch
6. upstream，关联源仓库
7. pull，更新拉取源仓库至 master branch
8. branch，切换回 docs branch
9. merge，将 master branch merge 至 docs branch。目的：如果有冲突，要在 branch 解决而不是 master branch
10. branch，切换回 master branch，再从 master merge docs branch。此时 master/docs 冲突已经在 docs branch 解决，因此可直接合并。master/docs branch 汇聚为最新提交节点
11. push，可直接向源仓库 push pull request。等待源仓库合并代码
12. push，可将修改推送至个人远程仓库，但源仓库还未 merge，仅为个人修改内容
13. pull，收到源仓库 merge 通知后，更新本地 master branch，并再次 push 至个人远程仓库
14. 此时，master branch; origin/master; upstream/master，同步
15. del branch，删除完成使命的 docs branch

首先，先解释每个序号的内容： <br>
1. fork 拉取他人的代码，在自己的库中会形成一个完全一样的副本库。
2. clone 此过程可以通过vscode实现，也可以通过本地的git命令，创建一个只处理这个项目的文件夹，从文件夹处打开git push,`git clone url`,其中url是自己副本库的地址。默认情况下会自动创建一个与远程仓库默认分支相对应的本地分支。
3. branch 创建一个自己的次分支，原因是不同的开发者有不同的任务，在自己的分支中工作，互不影响，即使出现问题也不会直接影响到稳定的主分支，降低了风险。自己分支的工作完毕再合并到主分支。`git branch name`,这个仅仅是创建了这个分支，`git branch -b name` ,创见并切换。
4. commit 在自己的分支上commit实际上是将暂存区中的文件快照永久记录到本地Git仓库历史中，而在自己分支上commit，修改仅限于该分支，不会影响到项目的主分支或其他开发者的正在进行的工作，如果在特定分支上的开发出现错误，只需回滚这个分支的更改，而不影响其他分支或已发布的产品版本。`git add *`  `git commit -m"修改信息"`
5. branch 切换回主分支master,`git checkout master`，后面的master代表主分支。
6. upstream 实际上，当我们克隆的后，本地默认关联的仓库是origin仓库，即clone的那个副本仓库，通过`git remote -v` 可以查看远程信息。我们需要手动将源仓库添加到关联仓库上，即：`git remote add upstream url`,这个url是源仓库地址。
7. pull 在多人协作过程中，难免会出现别人进度比我们快，优先于我合并到了主分支上，或者主分支更改了一些内容，我们必须在本地及时pull拉取最新的，不然后面会出现冲突。这个部分必须要注意，副本仓库与原作者的源仓库实际上是独立的，一定要关联上upstream源仓库，否则无法pull到最新的进展。`git remote -v`,查看远程信息。`git pull upstream master` 拉取并合并远程仓库中的master到当前分支(顾名思义，这里面master可以换，拉取其他分支也是可以的)，这个部分实际上可以分成三步：`git fetch upstream` `git checkout master` `git merge upstream/master`,其中注意：fetch仅仅只是拉取，还并未merge合并，后面更换完分支，再合并到更换完的分支上，如果在本地主分支上，就不用更换分支了。
8. branch 切换回自己独立工作的工作分支，这个时候要把本地主分支上拉取的最新内容合并到次分支来工作。在自己的次分支上进行add 和commit。
9. 这个我理解为：假如某次分支合并到主分支时出现冲突(次分支的版本没有及时pull更新)，要把主分支的内容重新merge到次分支，然后把次分支的内容根据自己的工作进行修改。**这块不是很明白，要是重新更新pull主分支内容，次分支弄好的部分岂不是全丢失？**
10. merge 次分支工作完之后就可以切换回主分支，记住一定要切换回主分支，要用哪个就去哪个分支里面merge，在主分支上将次分支的修改合并。 `git merge dev-branch`,dev-branch理解为要被主分支合并的次分支，工作分支。
11. **未弄清，没权限**
12. 最后就可以在本地的master分支里面push到远程仓库了，注意：此处push的是origin副本仓库。`git push origin master`,推送本地master分支到远程仓库的master分支。
13. **11未解决**，同步到个人副本仓库同12。
14. 此时本地master主分支，个人副本仓库主分支master，源仓库主分支master同步。
15. 删除当时在本地的工作分支，完成使命。`git branch -b dev-branch`,dev-branch为工作分支。
16. 拓展： <br>`git branch -set-upstream branch-name origin/branch-name` 建立本地分支origin/branch-name和远程分支branch-name的关联。
    
**回答：**
> origin; master; branch; branch types; commit; commit message types;
push; pull; marge
- 前三个根据上面的序号就可以了解其中的意思。
- branch types 
- Git commit message 中的类型（type）是一种最佳实践，用于标准化和结构化提交信息，使得项目的维护者和其他开发者能够更快地**理解每个提交的目的和影响**。以下是一些常用的commit message类型及其含义：
    1. feat (feature)：新增功能，表示本次提交引入了一个新的功能或者增强了现有功能。
    2. fix (bugfix)：修复问题，表示对程序中存在的错误或bug进行了修正。
    3. docs (documentation)：仅涉及文档的更改，不涉及代码本身的功能变化。
    4. style (styling)：代码格式、空格、缩进等不影响程序逻辑的修改。
    5. refactor：重构，对代码进行了改进，但并不添加新功能或修复bug。/ri'fæktə/
    6. perf (performance)：性能优化，提高了代码的执行效率。
    7. test：增加或修改测试用例，不改变生产代码。
    8. chore：维护性的任务，例如更新构建脚本、依赖包等。
    9. revert：撤销某个之前的提交。
    10. build：构建系统的相关更改，如修改构建脚本、配置等。
    11. ci：持续集成相关的更改，如CI流程、配置文件等。
    12. migration：数据库迁移或数据迁移相关的更改。
    13. security：安全相关的修复，如修补安全漏洞等。

- branch types 以下是一些广泛采用的分支类型： <br>

    1. 主分支（Main/Branch）:
        master/main: 传统上，这是用来跟踪生产就绪代码的分支，包含可部署到生产环境的最新稳定版本。
        main/release:在现代Git Flow中，有的团队会将main作为稳定的发布基线，而release分支用于预发布阶段的整合测试。
    2. 开发分支（Development/Branches）:
        develop: 表示最新的开发状态，通常在该分支上集成各个特性分支的成果，也是下一个发行版的基础。
        next/alpha/beta: 有时用于表示不同开发阶段的分支，比如下一个大版本的预览版或测试版。
    3. 特性分支（Feature/Branches）:
        feature/*: 用于开发新的功能，一般**从develop分支分出**，在该分支上进行功能开发，完成后合并回develop。
    4. 修复分支（Bugfix/Branches）:
        hotfix/*: 用于快速修复生产环境发现的问题，通常从master/main分支分出，修复后合并回master/main和develop。
        bugfix/*: 类似于hotfix，用于开发针对某一特定问题的修复，完成后合并回相应开发分支。
    5. 发布分支（Release/Branches）:
        release/*: 用于准备发布新的稳定版本，从develop分支分出，完成发布前的整理和测试，无误后合并回master/main和develop。
    6. 短期分支（Short-lived/Branches）:
        以上提到的feature、bugfix和release分支都是短期分支的例子，它们生命周期相对较短，完成目标后会被合并并删除。
    7. 个人分支（Personal/Branches）:
        开发者个人在进行独立工作时，可能会创建自己的临时分支，这类分支不一定遵循固定的命名模式，但在完成任务后也会合并回相应的开发分支。
### 三.HTML
> div; span; p; h1; br; hr; table; ul; img;
1. div 俗称大盒子，块状元素，在进行css/js时，可以对整个块修改属性，独占一行，宽高默认为父级的100%，添加宽高生效，可以包含其他块级元素或行内元素。
2. span 俗称小盒子，行内元素，尺寸由内容撑开，其内容的宽度和高度由内容本身决定。行内元素通常无法直接设置宽高，只能包裹行内内容（如文本、图片或其他行内元素），加宽高不生效，背景色生效。作为文本的一个容器，用于为部分文本提供额外的意义或者应用样式，但它本身不传达任何结构性信息
3. p 块级元素，独占一行，每个 <p> 段落之间默认会有空白行分隔。块级元素可以设置宽度、高度和其他一些盒模型属性，并且能够容纳其他内联或块级元素。p 标签具有明确的语义，代表一个段落，用于将逻辑相关的文本内容组合在一起，形成文档结构的一部分。p 标签通常用于分割文章内容，构成页面上的多个段落，可以通过CSS来调整其样式，但因为它是块级元素，所以更适合整体性的样式控制和布局。
4. div 与 span 的主要区别体现在它们的布局特性和内容承载能力上，div 更倾向于布局和块级内容的封装，而 span 更适用于行内文本的样式化和微调。p标签与span标签，从语义角度讲，p 标签内可以嵌套span标签，但是根据HTML规范，p 标签不能直接嵌套在span标签内，也不应该在另一个p标签内部直接嵌套（即一个段落内不能包含另一个段落）。
### 四.emmet语法
>   +;>; ^; (); *; $; lorem;
#### HTML
1. 类选择器：标签名.类名
2. id选择器 ：标签名#id名
3. 同级标签：标签名1+标签名2，例如div+p 
4. 父子标签：标签名1>标签名2
5. 多个相同标签：标签名*n
6. 有内容的标签：标签名{内容}，例：div{这是div标签}
7. 第一，$ 来引用数值索引。在循环生成元素时，item$*3 将生成三个元素，分别命名为 item1、item2 和 item3。第二，结合 $@ 表示从某个特定数字开始递增或递减。
8. lorem 是一个用于快速生成假文的简写指令。“lorem”并跟上一个数值，表示你想要生成的单词数量。若生成特定数量的段落，可以使用乘法操作符，如 lorem2*3 将生成两个单词的Lorem Ipsum文本共三段。


#### CSS
大多数简写方式为属性单词的首字母 <br>
1. w  width  w500  width:500px
2. h  hight
3. bgc  background-color
4. bgi  background-image
5. bgr  background-repeat        no-repeat/repeat/repeat-x/repeat-y
6. bgp  background-position      left/right/center/top/botton/数字px...
7. bgs  background-size          cover/contain/百分比/
8. bga  background-attachment    fixed
9. bg   background               色 图 平铺 位置/缩放 固定  -->空格隔开，无顺序先后

### 未解决问题
1.  **div>ul>lorem3^p>lorem32**
2.  维护md,需要记录，日积月累
3.  复制源仓库地址，直接在本地clone，在vscode打开，
4.  回滚  reset
5.  rebase 重构

解决：
1.^ 符号在Emmet中通常用于表示父元素（parent element）。例如，当你在一个元素的缩写后面加上 ^，Emmet会将该元素插入到其父级元素内，并且通常与嵌套元素的创建或定位有关。例如此式子：div是父级元素，子元素ul，希望有三个段落，每个段落标题自动生成，段落落里有32个随机左右随机字符。  