# 核心流程设计分析
### 窗口与文本编辑器
1. createWindow函数：
建立窗口。
```
    window = new JFrame("Notepad");
    window.setSize(800,600);
    window.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
```
2. createTextArea函数：
在窗口内建立富文本编辑器，设置默认值。
```
    textArea = new JTextPane();
    textArea.setFont(format.arial);
```
同时还需要添加事件监听，支持输入文本等操作。
```   
    textArea.addKeyListener(kHandler);
    textArea.getDocument().addUndoableEditListener(
        new UndoableEditListener() {
            @Override
            public void undoableEditHappened(UndoableEditEvent e) {
                um.addEdit(e.getEdit());
            }
        }
    );
```
3. createMenuBar函数与子菜单栏创建函数：
通过java自带的JMenuBar模块创建总菜单栏对象。
```
    menuBar = new JMenuBar();
    window.setJMenuBar(menuBar);
```
利用Jmenu模块创建可以生成子菜单栏的每一个菜单选项。
```
    menuFile = new JMenu("File");
    menuBar.add(menuFile);
```
在菜单栏下通过java自带的JMenuItem模块创建子菜单栏对象，添加监听以监视是否被点击。随后可以在actionPerformed函数里调用每一项子菜单项对应的具体功能。可以用java内置的setAccelerator函数对菜单栏的项目创建快捷方式。
```
    iNew = new JMenuItem("New");
    iNew.addActionListener(this);
    iNew.setActionCommand("New");
    menuFile.add(iNew);
    iNew.setAccelerator(KeyStroke.getKeyStroke(KeyEvent.VK_N,     KeyEvent.CTRL_DOWN_MASK));
```

### 文件操作
1. 新建文件：新建窗口与富文本框，并重新设置标题
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
4. 退出窗口：直接调用系统函数退出程序运行。
```
System.exit(0);
```

### 编辑操作
>撤销与重做：直接调用undo与redo函数
>```
    gui.um.undo();
    gui.um.redo();
>```

被快捷键功能替代

### 版式操作
1. 自动换行：
原先采用的JTextArea对象有自动换行的设置，但是JTextPane没有单独的相关函数。因此一种调整自动换行的思路是将当前文本转换为HTML再设置换行。
```
    if(v instanceof InlineView){
        return new InlineView(e){
            public int getBreakWeight(int axis, float pos, float len) {
                return GoodBreakWeight;
            }
            public View breakView(int axis, int p0, float pos, float len) {
                if(axis == View.X_AXIS) {
                    checkPainter();
                    int p1 = getGlyphPainter().getBoundedPosition(this, p0, pos, len);
                    if(p0 == getStartOffset() && p1 == getEndOffset()) {
                        return this;
                    }
                    return createFragment(p0, p1);
                }
                return this;
            }
        };
    }
```
一种优化策略是将当前换行设置为单词间换行，即需要判断允许换行的位置，然后创建换行。
```
    protected SizeRequirements calculateMinorAxisRequirements(int axis, SizeRequirements r) {
        if (r == null) {
            r = new SizeRequirements();
        }
        float pref = layoutPool.getPreferredSpan(axis);
        float min = layoutPool.getMinimumSpan(axis);
        r.minimum = (int)min;
        r.preferred = Math.max(r.minimum, (int) pref);
        r.maximum = Integer.MAX_VALUE;
        r.alignment = 0.5f;
        return r;
    }
```
不过目前在自动换行后不能再切换回不换行状态，因此自动换行设置功能暂时不能用了。
2. 字体设置：首先利用Font模块初始化待选字体，然后直接利用setFont函数传入该字体作为参数就可以完成设置。
```
    arial = new Font("Arial", Font.PLAIN, fontSize);
    gui.textArea.setFont(arial);
```
3. 字体大小：利用createFont函数的参数传入字体大小，比如：
```
format.createFont(8);
```

### 颜色设置
需要设置边框颜色（窗口颜色）、面板背景颜色（textPane颜色）与字体颜色。
1. 边框颜色：直接调用窗口对象的setBackgroud函数设置颜色。
```
    gui.window.getContentPane().setBackground(Color.white);
```
2. 面板背景颜色：不同于JTextArea，对于JTextPane对象而言，直接使用setBackground是无效的（一直是白面）。此处需要利用javax重写UIDefaults，从而完成对背景颜色的更改。
```
    Color bgColor = Color.BLACK;
    UIDefaults defaults = new UIDefaults();
    defaults.put("TextPane.background", new ColorUIResource(bgColor));
    defaults.put("TextPane[Enabled].backgroundPainter", bgColor);
    gui.textArea.putClientProperty("Nimbus.Overrides", defaults);
    gui.textArea.putClientProperty("Nimbus.Overrides.InheritDefaults", true);
```
3. 字体颜色：即前景颜色的设置。直接调用JTextPane对象的setForegroud函数设置颜色。
```
gui.textArea.setForeground(Color.black);
```

### 快捷键设置
>扩展KeyListener类，利用keyTyped、keyPressed和keyReleased函数完成设置（注意这三者一个都不能少，哪怕函数为空）。

被setAccelerator(KeyStroke.getKeyStroke())函数取代。

### UI美化
1. 滚动条美化： 重写覆盖原先的JScrollPane模块，并在主函数中调用。
原先的scroller创建过程：
```
    JScrollPane scrollPane = new JScrollPane();
    scrollPane = new JScrollPane(textArea, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED, JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
    scrollPane.setBorder(BorderFactory.createEmptyBorder()); //delete border
    window.add(scrollPane);
```
现在的scroller创建，采用的是写在scroll文件夹下的滚动条创建函数，替代原有函数。
```
    scrollPane = new ScrollPaneWin11();//create scroll pane
    scrollPane.setBorder(BorderFactory.createEmptyBorder()); //delete border
    scrollPane.setViewportView(textArea);

    javax.swing.GroupLayout layout = new javax.swing.GroupLayout(window.getContentPane());
    window.getContentPane().setLayout(layout);
```
然后分别设置水平与竖直的滚动条。
```
    //set horizontal scroller
    layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                        .addGroup(layout.createSequentialGroup()
                                .addContainerGap()
                                .addComponent(scrollPane, javax.swing.GroupLayout.DEFAULT_SIZE, 624, Short.MAX_VALUE)
                                .addContainerGap())
        );
    //set vertical scroller
    layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                        .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                                .addContainerGap()
                                .addComponent(scrollPane, javax.swing.GroupLayout.DEFAULT_SIZE, 444, Short.MAX_VALUE)
                                .addContainerGap())
        );
```
注：为了达到更好的效果，需要在main函数里对UIManager进行相应设置。
```
    javax.swing.UIManager.setLookAndFeel(info.getClassName());
```
