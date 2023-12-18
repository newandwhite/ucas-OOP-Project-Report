### UI美化
1. 滚动条美化： 重写覆盖原先的JScrollPane模块，并在主函数中调用。
```
public class ScrollBarWin11UI extends BasicScrollBarUI {

    private Animator animator;
    private float animate;
    private boolean show;
    private boolean hover;
    private boolean press;
    private final int scrollSize = 14;
    private final MouseAdapter mouseEvent;

    public static ComponentUI createUI(JComponent c) {
        return new ScrollBarWin11UI();
    }

    public ScrollBarWin11UI() {
        mouseEvent = new MouseAdapter() {
            @Override
            public void mousePressed(MouseEvent e) {
                press = true;
            }

            @Override
            public void mouseReleased(MouseEvent e) {
                press = false;
                if (!hover) {
                    start(false);
                }
            }

            @Override
            public void mouseEntered(MouseEvent e) {
                hover = true;
                if (!show) {
                    start(true);
                }
            }

            @Override
            public void mouseExited(MouseEvent e) {
                hover = false;
                if (!press) {
                    start(false);
                }
            }
        };
    }

    @Override
    public void installUI(JComponent c) {
        super.installUI(c);
        c.setPreferredSize(new Dimension(scrollSize, scrollSize));
        c.addMouseListener(mouseEvent);
        c.setForeground(new Color(150, 150, 150));
        initAnimator();
    }

    private void start(boolean show) {
        if (animator.isRunning()) {
            float f = animator.getTimingFraction();
            animator.stop();
            animator.setStartFraction(1f - f);
        } else {
            animator.setStartFraction(0f);
        }
        this.show = show;
        animator.start();
    }

    private void initAnimator() {
        animator = new Animator(350, new TimingTargetAdapter() {
            @Override
            public void timingEvent(float fraction) {
                if (show) {
                    animate = fraction;
                } else {
                    animate = 1f - fraction;
                }
                if (scrollbar != null) {
                    scrollbar.repaint();
                }
            }
        });
        animator.setResolution(5);
        animator.setAcceleration(.5f);
        animator.setDeceleration(.5f);
        animator.setStartDelay(100);
    }

    @Override
    protected JButton createIncreaseButton(int orientation) {
        return new ScrollButton(scrollbar.getOrientation(), true);
    }

    @Override
    protected JButton createDecreaseButton(int orientation) {
        return new ScrollButton(scrollbar.getOrientation(), false);
    }

    @Override
    protected void paintTrack(Graphics g, JComponent c, Rectangle trackBounds) {
        Graphics2D g2 = (Graphics2D) g.create();
        g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
        g2.setComposite(AlphaComposite.SrcOver.derive(animate * 0.5f));
        g2.setColor(scrollbar.getForeground().brighter());
        g2.fill(new Rectangle2D.Double(trackBounds.x, trackBounds.y, trackBounds.width, trackBounds.height));
        g2.dispose();
    }

    @Override
    protected void paintThumb(Graphics g, JComponent c, Rectangle thumbBounds) {
        Graphics2D g2 = (Graphics2D) g.create();
        g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
        g2.setColor(scrollbar.getForeground());
        double border = scrollSize * 0.3f - (animate * 1);
        double sp = 10 * animate;
        g2.setComposite(AlphaComposite.SrcOver.derive(1f - (1f - animate) * 0.3f));
        if (scrollbar.getOrientation() == JScrollBar.VERTICAL) {
            double width = thumbBounds.getWidth() - border * 2;
            double height = thumbBounds.getHeight() - sp * 2;
            g2.fill(new RoundRectangle2D.Double(thumbBounds.x + border, thumbBounds.y + sp, width, height, width, width));
        } else {
            double width = thumbBounds.getWidth() - sp * 2;
            double height = thumbBounds.getHeight() - border * 2;
            g2.fill(new RoundRectangle2D.Double(thumbBounds.x + sp, thumbBounds.y + border, width, height, height, height));
        }
        g2.dispose();
        g.dispose();
    }

    private class ScrollButton extends JButton {

        private final int orientation;
        private final boolean isIncrease;
        private final Shape arrow;
        private boolean mouseHover;
        private boolean mousePress;

        public ScrollButton(int orientation, boolean isIncrease) {
            this.orientation = orientation;
            this.isIncrease = isIncrease;
            setContentAreaFilled(false);
            setPreferredSize(new Dimension(scrollSize, scrollSize));
            List<Point2D> points = new ArrayList<>();
            double width = scrollSize * 0.8f;
            double height = scrollSize * 0.7f;
            if (orientation == JScrollBar.VERTICAL) {
                if (isIncrease) {
                    points.add(new Point2D.Double(width / 2, height));
                    points.add(new Point2D.Double(width, 0));
                    points.add(new Point2D.Double(0, 0));
                } else {
                    points.add(new Point2D.Double(width / 2, 0));
                    points.add(new Point2D.Double(width, height));
                    points.add(new Point2D.Double(0, height));
                }
            } else {
                if (isIncrease) {
                    points.add(new Point2D.Double(0, 0));
                    points.add(new Point2D.Double(width, height / 2));
                    points.add(new Point2D.Double(0, height));
                } else {
                    points.add(new Point2D.Double(width, 0));
                    points.add(new Point2D.Double(0, height / 2));
                    points.add(new Point2D.Double(width, height));
                }
            }
            arrow = new PolygonCorner().getRoundedGeneralPathFromPoints(points, scrollSize * 0.5f);
            addMouseListener(mouseEvent);
            addMouseListener(new MouseAdapter() {
                @Override
                public void mouseEntered(MouseEvent e) {
                    mouseHover = true;
                }

                @Override
                public void mouseExited(MouseEvent e) {
                    mouseHover = false;
                }

                @Override
                public void mousePressed(MouseEvent e) {
                    mousePress = true;
                }

                @Override
                public void mouseReleased(MouseEvent e) {
                    mousePress = false;
                }
            });
        }

        @Override
        protected void paintComponent(Graphics g) {
            Graphics2D g2 = (Graphics2D) g.create();
            g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
            g2.setComposite(AlphaComposite.SrcOver.derive(animate * 0.5f));
            g2.setColor(scrollbar.getForeground().brighter());
            int x = 0;
            int y = isIncrease ? 8 : 0;
            int width = getWidth();
            int height = getHeight() - 8;
            if (orientation == JScrollBar.VERTICAL) {
                g2.fill(new Rectangle2D.Double(x, y, width, height));
            } else {
                g2.fill(new Rectangle2D.Double(y, x, height, width));
            }
            g2.setComposite(AlphaComposite.SrcOver.derive(animate));
            if (mousePress) {
                g2.setColor(new Color(110, 110, 110));
            } else if (mouseHover) {
                g2.setColor(new Color(130, 130, 130));
            } else {
                g2.setColor(scrollbar.getForeground());
            }
            double ax = scrollSize * 0.1f;
            double ay = scrollSize * 0.15f;
            g2.translate(ax, ay);
            g2.fill(arrow);
            g2.dispose();
            super.paintComponent(g);
        }
    }
}
```
类ScrollBarWin11UI继承自BasicScrollBarUI，表示新建的滚动面板是基于原式ScrollBar。然后为它加入鼠标事件的监听，并添加向上向下滚动的按键。由于采用的win11版滚动条，因此需要为它创造鼠标悬停在滚轮上之后滚轮变化的每一帧。这个变化过程由2D绘图创建。
2. 创建滚动面板：原先的scroller创建过程：
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

#### 最终效果如下：
* 菜单栏
![](/pic/ui1.png)

* 被鼠标放置时的滚动条
![](/pic/ui2.png)