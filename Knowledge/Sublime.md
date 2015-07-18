## Sublime的使用

### 1. 安装Package Control

打开SublimeText，使用Ctrl-`，出现python控制台，输入：

	import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')
	
重启SublimeText后可以在Preferences->Package Settings看到Package Control。

#### 2. 安装markdown preview

* 使用Ctrl+Shift+p调出命令面板，找到Package　Control: install Package这一项。搜索markdown preview，点击安装。

* 安装完成之后就可以对markdown文件进行预览了。按下键Ctrl+Shift+p调出命令面板，找到Markdown Preview: Preview in Browser
就可以预览了。另外还可以将markdown文件转换为html文件，输入Ctrl-B会在markdown文件所在目录生成同名的html文件。

* 设置markdown文件的语法高亮，将-[附件-zcf_markdown.tmTheme](appendix/zcf_markdown.tmTheme)复制到Sublime
的Packages目录下`(默认为：C:\Users\{username}\AppData\Roaming\Sublime Text 2\Packages)`，然后在Preferences-
Settings-User中加入：

		"color_scheme": "Packages/zcf_markdown.tmTheme"
	
保存后自动启用了markdown文件的语法高亮功能了。

#### 3. 启用vim快捷键

在Settings-User中将`ignored_packages":["Vintage"]`中的Vintage去除，保存后就可以使用vim的快捷键了。

#### 附件-配置备份

	{
		"auto_indent": true,
		"auto_match_enabled": true,
		"color_scheme": "Packages/zcf_markdown.tmTheme",
		"detect_indentation": true,
		"draw_indent_guides": false,
		"fade_fold_buttons": true,
		"fold_buttons": true,
		"font_face": "Courier New",
		"font_size": 14,
		"gutter": true,
		"ignored_packages":
		[
			"BracketHighlighter",
			"GBK Encoding Support",
			"Theme - Soda",
			"Markdown Preview",
			"MarkdownBuild",
			"Markdown to Clipboard",
			"Clipboard History"
		],
		"line_numbers": true,
		"match_brackets_braces": true,
		"rulers":
		[
			79
		],
		"smart_indent": false,
		"spell_check": false,
		"tab_size": 4,
		"translate_tabs_to_spaces": true,
		"word_wrap": false
	}
