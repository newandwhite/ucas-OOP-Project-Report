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

