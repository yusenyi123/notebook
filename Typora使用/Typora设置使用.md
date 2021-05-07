# Typora设置使用



## 1.Typora安装后设置

### 1.通用设置

![image-20210410223723953](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210410223724.png)



### 2.外观设置

![image-20210410224125432](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210410224125.png)

### 3.编辑器设置

![image-20210410224144503](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210410224144.png)

### 4.图像设置

![image-20210410224217705](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210410224217.png)

### 5.MarkDown设置

![image-20210410223614446](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210410223614.png)





## 2.Typora快捷键修改



### 操作流程

![在这里插入图片描述](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210421093624.png)







### 最新版需要将文字换成相应的英文下的选项，可以将Typora的语言切换成英文查看英文，设置完成再切换成中文



```
	"一级标题": "alt+1"
	"二级标题": "alt+2"
	"三级标题": "alt+3"
	"四级标题": "alt+4"
	"有序列表": "alt+w"
	"无序列表": "alt+e"
	"代码块": "alt+1"
	"引用":"alt+2"
//最新版使用下面的需要 Update 2021.2.2
	"Heading 1": "alt+1",
    "Heading 2": "alt+2",
    "Heading 3": "alt+3",
    "Heading 4": "alt+4",
    "Code Fences": "alt+q",
    "Ordered List": "alt+5",
    "UnOrdered List": "alt+6"
```

![image-20210421102033822](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210421102033.png)







```
/** For advanced users. */
{
  "defaultFontFamily": {
    "standard": null, //String - Defaults to "Times New Roman".
    "serif": null, // String - Defaults to "Times New Roman".
    "sansSerif": null, // String - Defaults to "Arial".
    "monospace": null // String - Defaults to "Courier New".
  },
  "autoHideMenuBar": false, //Boolean - Auto hide the menu bar unless the `Alt` key is pressed. Default is false.

  // Array - Search Service user can access from context menu after a range of text is selected. Each item is formatted as [caption, url]
  "searchService": [
    ["Search with Google", "https://google.com/search?q=%s"]
  ],

  // Custom key binding, which will override the default ones.
  "keyBinding": {
    // for example: 
    // "Always on Top": "Ctrl+Shift+P"
    "代码块": "alt+1"
	"引用":"alt+2"
  },

  "monocolorEmoji": false, //default false. Only work for Windows
  "autoSaveTimer" : 3, // Deprecidated, Typora will do auto save automatically. default 3 minutes
  "maxFetchCountOnFileList": 500,
  "flags": [] // default [], append Chrome launch flags, e.g: [["disable-gpu"], ["host-rules", "MAP * 127.0.0.1"]]
}

```

