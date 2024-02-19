### vim

*****
#### vim基本操作

1. hjkl
2. i, o, A 
3. / ? n N 
4. d p y
5. :num 0 $ ^ e b w gg G { }
6. =
7. u  ctrl+r
8. ctrl+u ctrl+d

##### 多行注释
`0 control + v jjjj i // esc`

##### 全部删除
`gg d G`

##### int替换为long 
`:%s/int/long/g`

*****

#### config 

nvim 的相关配置 在~/.config/nvim 

##### 插件安装


##### 快捷键修改


##### lsp


*****

#### plugin

记录一些本人爱用的快捷键

##### [lazyvim]()

装上的第一个插件
需要配合nerd-font 和 一个终端软件使用

需要慢慢熟悉其中的一些快捷键
这里记录几个比较关键的

```bash
<space> + E 打开folder
<space> + ff 查找文件
<space> + /  查找word 
```


##### [markdown](https://github.com/iamcco/markdown-preview.nvim)

记笔记用 预览markdown


##### [chatgpt](https://github.com/jackMort/ChatGPT.nvim) 

无需多言

##### [hop](https://github.com/smoka7/hop.nvim)

关键命令
`:HopWordMW`
MW: multi window
多窗口跳转 使用的关键是看准地方
<C-hjkl>实现在tmux中跳转

