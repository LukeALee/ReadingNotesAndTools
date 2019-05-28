# Vim实用技巧

## vim -u NONE -N

纯净模式启动（并防止vi兼容模式  -N)

## 普通模式

### 移动

- h:左
- j:下

	- J： 连接本行及下一行

- k:上

	- K ： 查看光标下单词的手册页 

- l:右
- w:下一个词开头
- e:词尾
- b:词头
- 数字0：移动到行首
- $:移到行尾
- zz: 重回屏幕，使当前行处于屏幕正中

### 理解重复

- 点号"."命令

	- 重复上次修改

	  从进入到离开插入模式形成一次修改；
	  方向键也会重置修改的起始位置；

	- . 是一个宏

- x:删除光标下字符
- u:撤销最新的修改

	- 注意将修改分粒度

- ctr+r:重做
- dd:删除当前行
- s:删除光标下字符并进入插入模式
- >G:增加当前行到文档末尾的缩进

  同理>gg增加当前行到文档开头行的缩进

- @: :重复上次的Ex命令

	- Ex命令：所有':'开头的命令

- ：s/target/replacment：执行替换

	- &:重复
	- u:撤销

- qx{changes}q:执行changes

	- @x:重复
	- u:撤销

### 减少无关移动

- y:复制
- p:粘贴
- f{char}:在行内查找下一个char的位置

	- ；：重复f的查找，即重复
	- , : 反向重复f的查找，即撤销

- F{char}:在行内查找上一个char的位置
- t{char}:同f但将光标置于{char}前一个字符

	- ；：重复f的查找，即重复
	- , : 反向重复f的查找，即撤销

- T{char}:同f但将光标置于{char}后一个字符
- /pattern<CR>: 在文档中查找下一个匹配的pattern
- ？pattern<CR>: 在文档中查找上一个匹配的pattern

### 算术运算

- <C-a>:对数字用加法
- <C-X>:对数字用减法

### 操作符+动作命令=操作

- 操作符

	- c:修改
	- d:删除
	- y:复制到寄存器
	- g~ : 反转大小写
	- gu:转换为小写
	- gU: 转换为大写
	- >:增加缩进
	- <:减少缩进
	- ！:使用外部程序
     过滤{motion}
     所跨越的行

## 插入模式

### 进入插入模式

- a:在光标之后添加内容
- A:在行尾添加
- i:在光标之前添加
- I:在行首添加

### 替换模式

- R：进入替换模式
- r{char}:单次替换
- gR: 虚拟替换模式
- gr{char}: 单词虚拟替换模式
- <Insert>: 切换插入模式和替换模式

### 一般插入模式

- 更正错误

  一个建议：输错一个单词，删除整个单词重新输入（通过这种方式记住哪些单词总是出错）

	- 退格键
	- <C-h>:删除前一个字符，同BS
	- < C-w>: 删除前一个单词
	- <C-u>:删除至行首

- 粘贴

	- <C-r>{register}: 插入寄存器中的内容

	  此插入方式相当于键盘输入，所以文本量较大时较慢，同时如果激活了
	  textwidth或autoident时有可能造成不必要的缩进或换行

		- 0：文本寄存器
		- =：计算寄存器

	- <C-r><C-p>{register}:以纯文本 插入寄存器中的内容
	- 更推荐返回普通模式去粘贴
	- <C-v>{code}: 插入编码为code的字符

	  默认code为三位十进制数字,可以在code前加u使其转变为16进制，如果输入了非数字的code，则会插入code本身代表的字符

		- ga:查看光标下字符的编码

	- <C-k>{char1}{char2}: 插入char1和char2代表的二合字母

	  二合字母： ”:digraph" 查看二合字母表
	  “:h digraph-table"查看更多二合字母

		- ”:digraph" ： 查看二合字母表
		- “:h digraph-table": 查看更多二合字母

- 返回普通模式

	- <C-o> : 切换至插入-普通模式
	- <ESC>
	- <C-[ >: 效果同<ESC>

### 插入—普通模式

- <C-o>: 执行一次普通模式命令并自动返回插入模式

## 可视/选择模式 

### <C-g>: 切换可视/选择模式

### v：激活面向字符的可视模式

### V: 面向行的可视模式

### <C-v>: 面向列块的可视模式

对列的修改只在第一行可见，切换回普通模式是才对其下的列生效

### gv: 重选上次的可视选取

### o: 切换选区活动端

- 一端固定一端移动

## 命令行模式

### 冒号: : 进入命令行模式

### :p : print

### @: :重复上一条EX命令

### :{range}delete [x] : 删除range内的行到寄存器x中 

### :{range}yank[x] : 复制range内的行到寄存器x中 

### :{line}put[x] : 在指定行后粘贴寄存器x中的内容 

### :{range}copy {address}
 : 拷贝range范围内的内容到address指定的行之下

### :{range}move{address}
 : 移动range范围内的内容到address指定的行之下

### :{range}join : 连接指定范围内的行

### :{range}normal{commands}
  : 对range范围内的每一行执行普通模式命令commands

### :{range}substitute /{pattern}/{string}/[flags]
 : 将range范围内的pattern替换为string

### :{range}global/{pattern}/[cmd]
 : 将range范围内出现pattern的行执行EX命令cmd

### :{range}copy/t {address} 复制选中范围到指定地址

- 不使用寄存器

### :{range}move/m {address}：移动range到address

### 自动补全EX命令

- <C-d>: 显示可用的补全列表

	- <Tab>依次显示可补全项
	- 例子：":colorscheme <C-d>" 列出主题配色方案

- :h :command-complete
- 自定义补全行为：:h 'wildmode'

### 批处理EX命令

- 存为xx.vim 文件
- 使用:source xx.vim执行该.vim文件
- 在.vim文件中每行代表一个Ex命令，故不用每行加:
- :args ：查看该vim进程的参数列表
- :first :指定下一条Ex命令作用在第几个参数上
- :next :指定下一条EX命令作用在当前参数的下一个参数上

### <C-r><C-w>: 插入光标下单词

### <C-r><C-a>: 插入光标下字串

### 回溯命令行命令

- ：<UP>(↑)：上一条命令
- ：<DOWN>(↓)：下一条命令

### 结识命令行窗口

- q: ：打开Ex命令历史的命令行窗口
- :q ：关闭
- <C-f>:从命令行切换到窗口
- :w 保存
- :e! 放弃更改并重新读取源文件

### 运行外部命令

- 在命令前加"!"即可
- :shell :启动交互式shell窗口

	- exit : 退出该shell并返回vim
	- Ctrl-z：挂起vim所属进程
	- fg: 唤醒被挂起进程

- read !{cmd} : 将cmd命令的输出重定向至缓冲区
- write !{cmd} : 将缓冲区 的内容作为cmd命令的标准输入  
- ！的位置会影响命令，所以要注意此处应该将！与cmd连在一起
- {range}!{filter} : 使用外部fiter过滤制定range范围内的内容

	- 使用 !{motion} 可以快速指定range

## 文件

### 第六章
管理多个文件

- S37.用缓冲区列表管理打开的文件

	- 缓冲区

		- :ls 查看缓冲区列表
		- %指明了哪个缓冲区在当前窗口可见
		- #代表轮换文件
		- <C-^>切换当前文件和轮换文件
		- 使用

			- :bnext 切换到列表中的下一个缓冲区（文件）
			- :bprev 切换至上一缓冲区
			- :bfirst 跳至开头
			- :blast 跳至结尾
			- :buffer N 直接跳转至第N个缓冲区
			- :buffer{bufname} 直接跳转至名为bufname的缓冲区
			- :bufdo 允许在:ls列出的所有缓冲区执行Ex命令

		- 删除缓冲区

			- :bdelete N 删除第N
			- N,M bdelete 删除第N至M个缓冲区

- S38.用参数列表将缓冲区分组

	- :args 显示缓冲区参数列表（文件列表）

		- [ ] 指向了活动文件 
		- 填充缓冲区

			- : args

				-  file_name_list

					- 按指定名称和顺序添加文件

				- *

					- 通配符，但不会递归子目录
					- 例：:args *.*

						- 匹配文件

							- index.html
							- app.js

				- ** 

					- 通配符，递归添加子目录内容
					- 例：:args **/*.*

						- 匹配文件

							- index.html
							- lib/framework.js
							- app/controllers/Mailer.js

				- ``(反引号)

					- vim 会在shell中执行反引号括起来的命令
					- 用sh命令列举文件然后添加进缓冲区

	- :argdo command

		- 在参数列表的每个文件上执行一条Ex命令

- S.39 管理隐藏缓冲区

	- 退出时的操作

		- :w[rite]

			- 把缓冲区内容写入磁盘

		- :e[dit]!

			- 回滚所做修改

		- :qa[ll]!

			- 忽略警告关闭所有窗口

		- :wa[ll]!

			- 把所有改变的缓冲区写入磁盘

	- 运行*.do命令前启用hidden设置

		- 普通模式下，对缓冲区进行了修改而未保存不允许跳转至其他缓冲区
则:argdo执行修改操作会失败
		- 启用hidden模式会自动将未保存的缓冲区设为隐藏

			- 最后用:wn逐个检查并保存
			- 或用:wa直接全部保存

- S.40 切分工作区为窗口

	- 并排显示缓冲区
	- <C-w>s水平切分窗口
	- <C-w>v垂直切分窗口
	- <C-w>s 
:edit{file_name}

		- 等价于

			- :split{file_name}

				- 切分窗口并打开file_name文件
				- :sp{file_name}

					- 水平切分

				- :vsp{file_name}

					- 垂直切分

	- 窗口切换

		- <C-w>w循环切换

			- <C-w><C-w> = <C-w>w.

		- <C-w>h向左切换
		- <C-w>j向下切换
		- <C-w>k向上切换
		- <C-w>l向右切换

	- Closing Windows

		- :clo[se]

			- <C-w>c

				- Close the active window

		- :on[ly]

			- <C-w>o

				- Keep only the active window, closing all others

	- Resizing  Windows

		- <C-w>=

			- Equalize width and height of all windows

		- <C-w>_

			- Maximize height of the active window

		- <C-w>|

			- Maximize width of the active window

		- [N]<C-w>

			- Set active window height to [N] rows

		- [N]<C-w>|

			- Set active window width to [N] columns

	-  Rearranging Windows

		- http://vimhelp.appspot.com/windows.txt.html#window-moving
		- :h window-moving

- S.41 用标签页将窗口分组

	- Vim keeps track of the set of files that are open using the buffer list 
	- :lcd {path} set the working directory locally for the current window.
	- :lcd applies locally to the current window, not to the current tab page

		- :windo lcd{path}set the local working directory for all windows

	- open a new tab

		- :tabe[dit] {filename}

	- <C-w>T

		- Move the current window into its own tab

	- :tabc[lose]

		- Close the current tab page and all of its windows

	- :tabo[nly]

		- Keep the active tab page, closing all others

	- Switching Between Tabs

		- {N}gt

			- :tabn[ext] {N}

				- Switch to tab page number {N}

		- gt

			- :tabn[ext]

				- Switch to the next tab page

		- gT

			- :tabp[revious]

				- Switch to the previous tab page

	- Rearranging Tabs

		- :tabmove [N]

			- Move the current tab page to the Nth.

### 第七章.打开及保存文件

- S.42 用:edit打开指定路径文件

	- :edit %<Tab>

		- The % symbol is a shorthand for the filepath of the active buffer

	- :edit %:h<Tab>

		- The :h modifier removes the filename while preserving the rest of the path

	- %:h is so useful, lets creating a mapping for it.

		- Try sourcing this line in your vimrc file
		- cnoremap <expr> %% getcmdtype() == ':' ? expand('%:h').'/' : '%%'

- S.43 使用find打开文件

	- configure the ‘path’ setting first

		- then :find command will find file in those 'path'
		- such as 

			- :set path+=app/**

	- :find Main.js<Tab>

		- <Tab>Cycle select the same name file

- S.44 使用netrw管理文件系统

	- make sure the netrw plugin works

		- In essential.vim

			- add

				- set nocompatible
filetype plugin on

	- Opening the File Explorer

		- :edit {path}

			- {path} argument should be a directory name

		- :E

			- :Explore
			- :edit %:h

		- :e.

			- :edit .

				- Open file explorer for current working directory

		- In Explore, press <CR> open a file, in this file, press <C-^> back to the explore

	- Doing More with netrw

		- http://vimcasts.org/e/15
		- netrw can also new，delete，rename file or dir

- S.45 把文件保存到不存在目录中

	- :edit an none exists directory

		- before :write

			- :!mkdir -p %:h
			- or Vim raises an error

		- <C-g> can view the file status

- S.46 以超级用户权限保存文件

	- open a root file

		- <C-g> 

			- [readonly]

		- Write something in this file, 
there comes an Warning

			- but you can ignore it and use
:w !sudo tee % > /dev/null
to force save.

		- :write !{cmd}

			- sends the contents of the buffer as standard input to the specified {cmd},

## 插件

### unimpaired.vim

- github.com/tpope/vim-unimpaired

### rails.vim

- https://github.com/tpope/vim-rails

*XMind: ZEN - Trial Version*