# Kotlin JVM - Migration Code

### From Java to Kotlin

##### Java
```java
public class Main {

    public static void main(String[] args) {
        invokeLater();
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
object Main {

    @JvmStatic
    fun main(args: Array<String>) = launch {
            CreateFrame().apply{
                location = Point().let { it.centerDesktop(this,-1) }
            }
        }

    fun CreateFrame() = JFrame("Frame").apply {
        this.add(
                JPanel().apply{
                    this.layout = FlowLayout()
                    this.add(JLabel("Label"))
                    this.add(
                            JButton().apply {
                                this.text = "Close"
                                this.addActionListener { System.exit(0) }
                            })
                })
        this.setSize(200, 300)
        this.defaultCloseOperation = JFrame.EXIT_ON_CLOSE
        this.isVisible = true
    }
    
    fun Point.centerDesktop(component: Component, numDesktop: Int) : Point {
        val pZero = Point(0,0)
        try {
            val devices = GraphicsEnvironment.getLocalGraphicsEnvironment().screenDevices
            for ((index, device) in devices.withIndex()) {
                if( numDesktop!= index && numDesktop !=-1)
                    return pZero

                val mousePoint = MouseInfo.getPointerInfo().location
                val bounds = device.defaultConfiguration.bounds
                if (    mousePoint.x >= bounds.x &&
                        mousePoint.y >= bounds.y &&
                        mousePoint.x <= bounds.x + bounds.width &&
                        mousePoint.y <= bounds.y + bounds.height) {

                    return Point(
                            (bounds.width - component.width) / 2 + bounds.x,
                            (bounds.height - component.height) / 2 + bounds.y
                    )
                }
            }
        } catch (e: Exception) {
            println("Error!!!!! " + e.message)
        }
        return pZero
    }
}
```



#### @Author : [Víctor Bolinches](https://www.linkedin.com) | [@vicboma1](https://twitter.com/vicboma1)
