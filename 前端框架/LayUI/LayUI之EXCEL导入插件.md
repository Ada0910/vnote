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


/**导入EXCEL函数*/
function importTerminalExcel(files){
	  try {
		  LAY_EXCEL.importExcel(files, {
		       // 读取数据的同时梳理数据
		       fields: {
		         'terminalId': 'A'
		         ,'terminalOUI': 'B'
		         ,'termianlUniqueIdentify': 'C'
		         ,'terminalPath': 'D'
		         ,'terminalSerialNumber': 'E'     
		         ,'terminalUsername': 'F'
		         ,'terminalPassword': 'G'
		         ,'terminalNetworkUsername': 'H'
		         ,'terminalNetworkPassword': 'I'
		         ,'terminalVerificationMethod': 'J'         	         
		         ,'terminalDesc': 'K' 
		        ,'terminalWanConfig': 'L'
		         ,'terminalAccessTime': 'M'	  
		         ,'terminalTreeId': 'N'
		        ,'terminalIsuse': 'O'
		       }
		     }, 
		     function(data) {
		    	 console.log(data);

		      // 如果不需要展示直接上传，可以再次 $.ajax() 将JSON数据通过 JSON.stringify() 处理后传递到后端即可
		      $.ajax({
	              url: '/importTerminal'
	              , data:{data: JSON.stringify(data)}
	              , dataType: 'json'
	              , success: function (res) {
	                  if(res.code == 200){
	                      layer.msg('导入成功',{icon:1,time:1000});
	                  }
	              }
	          })

		     });
		   } catch (e) {
		     layer.alert(e.message);
		   }
		   
		   
 }//导入函数
```