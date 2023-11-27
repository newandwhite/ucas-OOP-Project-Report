### 窗口与文本编辑器
1. createWindow方法：
建立窗口。
```
    window = new JFrame("Notepad");
    window.setSize(800,600);
    window.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
```
2. createTextArea方法：
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
3. createMenuBar方法与子菜单栏创建方法：
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

