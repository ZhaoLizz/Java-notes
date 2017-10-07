# 一.File类

- 既可以指文件,也指文件夹,可以通过方法盘对

  - `file.isFile()`
  - `file.isDirectory()`

## 目录列表器list,listFiles

- 它既能代表**一个特定文件的名称**,又能代表**一个目录下的一组文件的名称**

  - 如果它指一个文件集,就可以对此集合调用`list()`,这个方法返回一个**String[]**
  - `File.list(FilenameFilter)`可以对文件名过滤,返回String[]
  - `File.listFiles(FileFilter)`返回`File[]`

```java
public class Main {
    public static void main(String[] args) {
        File path = new File(".");

        String[] list;

        if (args.length == 0) {
            list = path.list();
        } else {
            list = path.list(filter("txt"));
        }
        if (list.length != 0) {
            Arrays.sort(list);
        }

        for (String dirItem : list) {
            System.out.println(dirItem);
        }
    }

    //也可以直接在list(new ...)匿名内部类参数取代此方法
    public static FilenameFilter filter(final String type) {
        return new FilenameFilter() {
            @Override
            public boolean accept(File dir, String name) {
                return (name != null && name.toLowerCase().endsWith(type));
            }
        };
    }
}
```

## 目录(文件夹) 的检查和创建

- 检查: `file.exists()`
- 删除: `file.delete()`
- 创建: `file.mkdir() / mkdirs()`

```java

//在根目录创建 newMade文件夹

        File file = new File("./newMade");
        System.out.println(file.mkdir());

        File file1 = new File("./1/2/3");
        System.out.println(file1.mkdir());   //false
        System.out.println(file1.mkdirs());  //true
```

# 二.输入和输出

- 任何自`InputStream 或 Reader`派生而来的类都含有名为`read()`的方法用于读取**单个字节或字节数组**
- 任何自`OutputStream 或 Writer`派生而来的类都含有`write()`方法用于**写单个字节或者字节数组**
- 自我独立的类`RandomAccessFile`适用于大小已知的文件
