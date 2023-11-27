### 快捷键设置
>扩展KeyListener类，利用keyTyped、keyPressed和keyReleased函数完成设置（注意这三者一个都不能少，哪怕函数为空）。

被setAccelerator(KeyStroke.getKeyStroke())函数取代。例如：
```
    iSave.setAccelerator(KeyStroke.getKeyStroke(KeyEvent.VK_S, KeyEvent.CTRL_DOWN_MASK));
```