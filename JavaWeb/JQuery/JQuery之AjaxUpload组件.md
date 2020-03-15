# 1. AjaxUpload 
- jQuery ajaxupload插件实现无刷新上传文件功能
- 是一个JQuery的插件
# 2. 使用
## 2.1. 先引入js文件
- 首先需要在页面里引入jquery.js和ajaxupload.js
- 本地（需要下载到本地修改对应的js文件路径）
```
<script src="jquery-1.9.min.js" type="text/javascript"></script>
<script src="ajaxupload3.9.js" type="text/javascript"></script>
```
- 在线
```
  <script src="//lampon.oss-cn-hangzhou.aliyuncs.com/JQuery/lampon.ajaxupload.3.6.js "></script>
```
## 2.2. js代码添加
- 例如对id="upgradeFile" 的input作为触发上传的组件
```
<td><input type ="button"  id="upgradeFile" value="上传升级文件"></input></td>
```
- 其响应的事件是这样的
```
		var uploadFile =new AjaxUpload($("#upgradeFile"),{
			action:  'onuUpgrade/uploadUpgradeFile.do',//请求路径
			type:"POST",
			data:{},//data为参数列表
			autoSubmit:true,//是否自动提交：true为自动提交，false不自动提交，当为false时，需要通过uploadFile.submit进行提交
			dataType:"json",//返回类型是什么
			name:'file',
			onChange: function(file, ext){
				//重选文件触发
			},
			onSubmit: function (file, extension) {
				//提交文件触发
				
			 },
			onComplete: function(file, response){
				//文件上传完成触发
				
			},
			});
```
## 2.3. controller层
```
/*升级文件上传到服务器*/
    @RequestMapping({"/uploadUpgradeFile"})
    public @ResponseBody String upload(HttpServletRequest request, @RequestParam("file") MultipartFile file) throws IOException, URISyntaxException {
        //文件名
        String fileName = file.getOriginalFilename();
        String realPath = request.getSession().getServletContext().getRealPath("\\WEB-INF")+"\\upload\\";
        StartTftpServerThread.path = realPath;	      
        File filePath = new File(realPath);
        String serverIp = request.getParameter("serverIp");
        //创建文件
        File destFile = new File(realPath + fileName);
        try {
            if (!filePath.exists()) {
            	/**如果目录不存在，则创建对应的目录*/
            	  filePath.mkdirs();
                if (!filePath.mkdir()) {
                    throw new IOException("文件夹创建失败,路径为：" + filePath);
                }
            }
            //可以使用该文件将上传文件保存到一个目标文件中
            file.transferTo(destFile);
        } catch (IOException e) {
            e.printStackTrace();
            return "fail";
        }

        return fileName;
    }
```
# 3. 附录

- AjaxUpload.js
