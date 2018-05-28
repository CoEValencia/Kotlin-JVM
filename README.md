# Kotlin JVM   (WIP)

## Migration

### From Java to Kotlin


##### Java
```java
public class Main {

    public static void main(String[] args) {
        System.out.println("0");
        invokeLater();
        System.out.println("2");
    }

    private static void invokeLater() {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                System.out.println("1");
                final JFrame frame =  CreateFrame();
                final Point point = getPointForCentering(frame);
                frame.setLocation(point);
            }
        });
    }

    public static JFrame CreateFrame() {
        JFrame frame = new JFrame("Frame");
        JPanel panel = new JPanel();
        panel.setLayout(new FlowLayout());
        JLabel label = new JLabel("Label");
        JButton button = new JButton();
        button.setText("Close");
        button.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent arg0) {
                System.exit(0);
            }
        });

        panel.add(label);
        panel.add(button);
        frame.add(panel);
        frame.setSize(200, 300);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
        return frame;
    }

    //   frame.setLocationRelativeTo(null);
    public static Point getPointForCentering(JFrame window) {
        try {
            final Point mousePoint = MouseInfo.getPointerInfo().getLocation();
            final GraphicsDevice[] devices = GraphicsEnvironment
                    .getLocalGraphicsEnvironment().getScreenDevices();
            for (GraphicsDevice device : devices) {
                final Rectangle bounds = device.getDefaultConfiguration().getBounds();
                if (mousePoint.x >= bounds.x && mousePoint.y >= bounds.y
                        && mousePoint.x <= (bounds.x + bounds.width)
                        && mousePoint.y <= (bounds.y + bounds.height)) {
                    Integer  screenWidth = bounds.width;
                    Integer screenHeight = bounds.height;
                    Integer width = window.getWidth();
                    Integer height = window.getHeight();
                    return new Point(
                            ((screenWidth - width) / 2) + bounds.x,
                            ((screenHeight - height) / 2) + bounds.y
                    );
                }
            }
        }
        catch (Exception e) {
            System.out.println("Error!!!!! "+e.getMessage());
        }
        return new Point(0, 0);
    }
}


```
##### kotlin
```kotlin

```



#### @Author : [Víctor Bolinches](https://www.linkedin.com) | [@vicboma1](https://twitter.com/vicboma1)
