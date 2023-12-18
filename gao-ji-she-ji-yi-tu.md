# 高级设计意图

### 面向对象特征
1. 封装
类的创建和方法的实现隐藏了内部细节，对外部提供接口。例如，Function_File、Function_Edit 等类封装了与文件、编辑等功能相关的操作。

2. 多态
actionPerformed 方法中的 switch 语句中涉及多个不同的类（Function_File、Function_Edit 等），通过共同的接口来处理不同类的实例。

3. 继承
一些重写与复用的类继承某些基类，以实现一些通用的行为。例如修改表格格式的类CustomCellRenderer extends继承了类 DefaultTableCellRenderer。

********

### 设计模式
1. 简单工厂模式：定义一个工厂类，它可以根据参数的不同返回不同类的实例。
包含必要的判断逻辑，可以决定在声明时候创建哪一个产品实例，客户端可以免除直接创建产品对象的职责，简单工厂模式实现了对象的创建和使用的分离。客户端无需知道所创建的具体产品类的类名，只需要知道具体产品类所对应的参数即可。
代码使用 Function_Format 中的方法（createFont）创建各种字体大小的实例，这可以被视为简单形式的工厂方法。
```
case "size8":  format.createFont( 8); break;
case "size12": format.createFont(12); break;
case "size16": format.createFont(16); break;
case "size20": format.createFont(20); break;
case "size24": format.createFont(24); break;
case "size28": format.createFont(28); break;
```

2. 单例模式：确保一个类只有一个实例，而且自行实例化并向整个系统提供这个实例。
UndoManager 类的实例 undoManager 是一个单例对象，确保只有一个实例用于管理撤销和重做操作。
本处采用饿汉模式。下方代码在GUI类的JTextPane方法创建中。
```
UndoManager um = new UndoManager();
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

3. 观察者模式：当一个对象状态改变时，所有依赖的对象都会自动收到通知。
事件源经过事件的封装传给监听器，当事件源触发事件后，监听器接收到事件对象可以回调事件的方法
每个事件包含一个事件源，事件源是事件发生的地方，由于事件源的某项属性或状态改变时，就会生成相应的事件对象，然后将事件对象通知给所有监听该事件源的监听器。
    * 事件监听器
    
    事件监听器一般需要实现java.util.EventListener接口
    EventListener 是个空接口，监听器必须要有回调方法供事件源回调，这个回调方法可以在继承或者实现EventListener 接口的时候自定义。
    ```
    markdownTextArea.getDocument().addDocumentListener(new DocumentListener() {
        @Override
        public void insertUpdate(DocumentEvent e) {
            scheduleUpdatePreview();
        }

        @Override
        public void removeUpdate(DocumentEvent e) {
            scheduleUpdatePreview();
        }

        @Override
        public void changedUpdate(DocumentEvent e) {
            scheduleUpdatePreview();
        }
    });
    ```
    展开后可以看到以下代码：
    ```
    public interface DocumentListener extends EventListener {
        public void insertUpdate(DocumentEvent e);
        public void removeUpdate(DocumentEvent e);
        public void changedUpdate(DocumentEvent e);
    }
    ```

4. 策略模式：将行为定义为一个行为接口和具体行为的实现。行为之间可以相互替换。
wordWrap 方法通过设置 JTextPane 的 EditorKit 实现了对 Word Wrap 策略的切换。通过在 HTMLEditorKit 中自定义 ViewFactory 和相应的 View 子类，实现了对换行方式的定制。
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
else if (v instanceof ParagraphView) {
    return new ParagraphView(e) {
        protected SizeRequirements calculateMinorAxisRequirements(int axis, SizeRequirements r) {
            if (r == null) {
                r = new SizeRequirements();
            }
            float pref = layoutPool.getPreferredSpan(axis);
            float min = layoutPool.getMinimumSpan(axis);
            // Don't include insets, Box.getXXXSpan will include them.
            r.minimum = (int)min;
            r.preferred = Math.max(r.minimum, (int) pref);
            r.maximum = Integer.MAX_VALUE;
            r.alignment = 0.5f;
            return r;
        }

    };
}
return v;
```

********

### 一些目标
* UI界面优化（已完成）
* 插入图片（已完成）
* 支持公式输入（废弃）
* 支持插入表格（已完成）
* 支持markdown编辑（已完成）
* 导入导出
* 云存储接入（已完成）



