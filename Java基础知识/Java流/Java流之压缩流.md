# 1. 使用
- 文件压缩
```
public class ZipStream {
    public static void main(String[] args) throws IOException {
        // 定义要压缩的文件
        File file = new File("d:" + File.separator + "ada.txt");
        // 定义压缩文件名称
        File zipFile = new File("d:" + File.separator + "ada.zip");
        // 定义文件的输入流
        InputStream input = new FileInputStream(file);
        // 声明压缩流对象
        ZipOutputStream zipOut = null;
        zipOut = new ZipOutputStream(new FileOutputStream(zipFile));
        // 设置ZipEntry对象
        zipOut.putNextEntry(new ZipEntry(file.getName()));
        // 设置注释
        zipOut.setComment("www.ada.cn");
        int temp = 0;
        // 读取内容
        while ((temp = input.read()) != -1) {
            // 压缩输出
            zipOut.write(temp);
        }
        // 关闭输入流
        input.close();
        // 关闭输出流
        zipOut.close();
    }
}
```
- 压缩文件夹
```
public class ZipFolderStream {
    public static void main(String[] args) throws IOException {
        // 定义要压缩的文件夹
        File file = new File("d:" + File.separator + "ada");
        // 定义压缩文件名称
        File zipFile = new File("d:" + File.separator + "ada.zip");
        // 定义文件输入流
        InputStream input = null;
        // 声明压缩流对象
        ZipOutputStream zipOut = null;
        zipOut = new ZipOutputStream(new FileOutputStream(zipFile));
        // 设置注释
        zipOut.setComment("www.ada.cn");
        int temp = 0;
        // 判断是否是文件夹
        if (file.isDirectory()) {
            // 列出全部文件
            File lists[] = file.listFiles();
            for (int i = 0; i < lists.length; i++) {
                // 定义文件的输入流
                input = new FileInputStream(lists[i]);
                // 设置ZipEntry对象
                zipOut.putNextEntry(new ZipEntry(file.getName()
                        + File.separator + lists[i].getName()));
                // 读取内容
                while ((temp = input.read()) != -1) {
                    // 压缩输出
                    zipOut.write(temp);
                }
                // 关闭输入流
                input.close();
            }
        }
        // 关闭输出流
        zipOut.close();
    }
}
```

