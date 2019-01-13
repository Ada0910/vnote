File类的一下常用方法和说明
# 1. 1.访问文件名相关方法：

- String getName(); 返回此File对象所表示的文件名和路径名（如果是路径，则返回最后一级子路径名）。
- String getPath(); 返回此File对象所对应的路径名。
- File getAbsolutePath(); 返回此File对象所对应的绝对路径名。
- String getParent(); 返回此File对象所对应目录（最后一级子目录）的父路径名。
- boolean renameTo(File newName); 重命名此File对象所对应的文件或目录，如果重命名成功，则返回true:否则返回false.（A）
# 2. 2.文件检测相关方法

- boolean exists(); 判断File对象所对应的文件或目录是否存在。
- boolean canWrite(); 判断File对象所对应的目录或文件是否可写- 。
- boolean canRead(); 判断File对象所对应的目录或文件是否可读。
- boolean isFile(); 判断File对象所对应的是否是文件，而不是目录。
- boolean isDirectory(); 判断File对象所对应的是否是目录，而不是文件。
- boolean isAbsolute(); 判断File对象所对应的文件或目录是否是绝对路径。该方法消除了不同平台的差异，可以直接判断File对象是否为绝对路径。在UNIX/- -- Linux/BSD等系统上，如果路径名开头是一条斜线（/）,则表明该File对象对应一个绝对路径；在Windows等系统上，如果路径开头是盘符，则说明它是绝对路径。
# 3. 3.获取常规文件信息

- long lastModified(); 返回文件最后修改时间。
- long length(); 返回文件内容的长度。
# 4. 4.文件操作相关的方法

- boolean createNewFile(); 当此File对象所对应的文件不存在时，该方法将新建的一个该File对象所指定的新文件，如果创建成功则返回true；否则返回false.(C)
- boolean delete(); 删除File对象所对应的文件或路径。
- static File CreateTempFile(String prefix,String suffix);在默认的临时文件目录创建一个临时空文件，使用给定前缀、系统生成的随机数和给定后缀作为文件名- 。这是一个静态方法，可以直接通过File来调用。preFix参数必须至少是3个字节长。建议前缀使用一个短的、有意义的字符串。建议前缀使用一个短的、有意义的字符串，比如”hjb“ 或”main”. suffix参数可以为null,在这种情况下，将使用默认的后缀”.tmp”.
- static File CreateTempFile(String prefix,String suffix,File directory);在directory所指定的目录中创建一个临时空文件，使用给定前缀、系统生成的随机数和给定后缀作为文件名。这是一个静态方法，可以直接通过File来调用。
- void deleteOnExit(); 注册一个删除钩子，指定当Java虚拟机退出时，删除File对象随对应的文件和目录。
# 5. 5.目录操作相关方法

- boolean mkdir(); 试图创建一个File对象所对应的目录，如果创建成功，则返回true;否则返回false. 调用该方法时File对象必须对应一个路径，而不是一个文件。
- String[] list(); 列出File对象的所有子文件名和路径名，返回String数组。- 
- File[] listFiles(); 列出File对象的所有子文件和路径，返回File数组。
- static File[] listRoots(); 列出系统所有的根路径。这是一个静态方法，可以直接通过File类来调用