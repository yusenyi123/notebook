# utools插件编写

## 参考

https://www.jianshu.com/p/ddb5d7a921b3

## plugin.json文件

```json
{
	"pluginName": "复制纯文本",
	"description": "复制纯文本，不带任何格式。",
	"logo": "img/logo.png",
    
    //这是一个关键文件，你可以在此文件内调用 uTools、 nodejs、 electron 提供的 api。 main 与 preload 至少存在其一
	"preload": "preload.js",
    
	"version": "1.0.0",
	"author": "林泉",
	"homepage": "https://www.jianshu.com/u/d62037f3ab47",
    
    //features 描述了当 uTools 主输入框内容产生变化时，此插件是否显示在搜索结果列表中，一个插件可以有多个功能，一个功能可以提供多个命令供用户搜索
    //鼠标选中文字时会将文字装入到utools的主输入框
	"features": [{
		"code": "copy_text", //插件提供的某个功能的唯一标示，此为必选项，且插件内不可重复
		"explain": "复制纯文本", //对此功能的说明，将在搜索列表对应位置中显示
        
        //该功能下可响应的命令集，支持 6 种类型，由 cmds 的类型或 cmds.type 决定，包括如下类型：，utools输入框中不同内容弹出不同的功能
		"cmds": [{
			"type": "regex",
            // 文字说明，在搜索列表中出现（必须）
            //可以在utools输入label的值 搜索到该命令运行
			"label": "复制纯文本",
			"match": "/(.*)/"
		}]
	}],
	"pluginSetting": {
		"single": true
	}
}

```



```json
{
	"pluginName": "复制纯文本",
	"description": "复制纯文本，不带任何格式。",
	"logo": "img/logo.png",
	"preload": "preload.js",
	"version": "1.0.0",
	"author": "林泉",
	"homepage": "https://www.jianshu.com/u/d62037f3ab47",
	"features": [{
		"code": "copy_text",
		"explain": "复制纯文本", 
		"cmds": [{
			"type": "regex",
			"label": "复制纯文本",
			"match": "/(.*)/"
		}]
	}],
	"pluginSetting": {
		"single": true
	}
}

```



## preload.js文件

```
window.exports = {
   "features.code": { // 注意：键对应的是 plugin.json 中的 features.code
      mode: "none",  // 用于无需 UI 显示，执行一些简单的代码
      args: {
         // 点击插件就会调用
         enter: (action) => {
            // action = { code, type, payload }
            
             utools.copyText(123)
          
            utools.simulateKeyboardTap('v', 'ctrl')
            window.utools.hideMainWindow()
            // do some thing
            window.utools.outPlugin()
         }  
      } 
   }
}
```

