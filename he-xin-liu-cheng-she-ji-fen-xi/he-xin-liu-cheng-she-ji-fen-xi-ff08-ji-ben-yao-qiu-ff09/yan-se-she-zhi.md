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

#### 最终效果如下：


