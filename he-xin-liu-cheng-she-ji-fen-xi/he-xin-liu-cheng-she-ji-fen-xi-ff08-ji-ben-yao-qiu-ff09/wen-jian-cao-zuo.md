### 文件操作
1. 新建文件：新建窗口与富文本框，并重新设置标题
```
    gui.textArea.setText("");
    gui.window.setTitle("New");
    fileName = null;// reset
    fileAddress = null;// reset
```

2. 读取文件（打开文件）：通过FileDialog对象的LOAD功能调出文件窗口并利用getFile函数选择文件。
```
    FileDialog fd = new FileDialog(gui.window, "Open", FileDialog.LOAD);
    fd.setVisible(true);
    if(fd.getFile() != null){
        fileName = fd.getFile();
        fileAddress = fd.getDirectory();
        gui.window.setTitle(fileName);
    }
```
读取文件的关键在于按行读入被打开文件的内容并写入当前已经被清空的富文本框。可以直接利用java的文件读取模块与相应功能实现这一步。注意写入时每一行需要添加换行符。最后关闭文件。
```
    BufferedReader br = new BufferedReader(new FileReader(fileAddress+fileName)); 
    gui.textArea.setText("");
    String line;
    while ((line = br.readLine()) != null){
        gui.textArea.setText(gui.textArea.getText() + line + "\n");
    }
    br.close();
```

3. 保存文件与另存文件：两者代码重叠性很高。可以先写好另存，再拓展为保存。它们都是基于FileDialog.SAVE实现的。
```
    FileDialog fd = new FileDialog(gui.window, "Save", FileDialog.SAVE);
    fd.setVisible(true);
```
另存时，在确定保存文件的名字时设置默认存为txt格式，除非用户有提前设置。保存地址通过内置的getDirectory函数获得。保存后将文件名设置为当前窗口名称。
```
    if (fd.getFile() != null){
        fileName = fd.getFile();
        if (!fileName.contains(".")){
            if (!fileName.contains(".txt")){
                fileName = fileName + ".txt";
            }
        }
        fileAddress = fd.getDirectory();
        gui.window.setTitle(fileName);
    }
```
而保存与另存最大的区别在于保存多一个不用输入文件名与文件位置而直接保存的功能。因而应先判断当前文件是否已经被命名，若没有则直接调用另存函数，否则直接保存在当前位置。直接保存体现在代码上就是对原文件的写入操作。
```
    FileWriter fw =  new FileWriter(fileAddress + fileName);
    fw.write(gui.textArea.getText());
    fw.close();
```

4. 打印文件（导出为pdf）：直接使用JTextPane自带的print方法。
```
    gui.textArea.print();
```

5. 退出窗口：直接调用系统函数退出程序运行。
```
System.exit(0);
```

