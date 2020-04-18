# 1. 示例
- html代码
```
<div class="layui-input-inline">
             <input type="file" class="layui-btn layui-btn-primary" id="importTerminal" multiple="multiple"></input>
            </div>
```

- js 代码
```
$(function(){
	// 监听上传文件的事件
    $('#importTerminal').change(function(e) {
    	var files = e.target.files;
    	 importTerminalExcel(files);
    	 
    	// 文件拖拽
 	    $('body')[0].ondragover = function(e) {
 	      e.preventDefault();
 	    }
 	    $('body')[0].ondrop = function(e) {
 	      e.preventDefault();
 	      var files = e.dataTransfer.files;
 	     importTerminalExcel(files);
 	    }

    });
});
```