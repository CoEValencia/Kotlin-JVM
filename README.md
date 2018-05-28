# Kotlin JVM - Migration Code

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
object Main {

    @JvmStatic
    fun main(args: Array<String>) = invokeLater()

    private fun invokeLater() {
        println("0")
        SwingUtilities.invokeLater {
            println("1")
            CreateFrame().centerDesktopDefault()
        }
        println("2")
    }

    fun CreateFrame() = JFrame("Frame").apply {
        this.add(JPanCreate())
        this.setSize(200, 300)
        this.defaultCloseOperation = JFrame.EXIT_ON_CLOSE
        this.isVisible = true
    }

    fun JPanCreate() = JPanel().apply{
        this.layout = FlowLayout()
        this.add(JLabel("Label"))
        this.add(JBtnCreate())
    }

    fun JBtnCreate() = JButton().apply {
            this.text = "Close"
            this.addActionListener { System.exit(0) }
        }

    fun JFrame.centerDesktopDefault() = this.centerDesktop(-1)

    fun JFrame.centerDesktop(num : Int){
        this.location = Point().centerDektop(this,num)
    }

    fun Point.centerDektop(component: Component, numDesktop: Int) : Point {
        try {
            val mousePoint = MouseInfo.getPointerInfo().location
            val devices = GraphicsEnvironment.getLocalGraphicsEnvironment().screenDevices
            for ((index, device) in devices.withIndex()) {
                if( numDesktop!= index && numDesktop !=-1)
                    return Point(0,0)

                val bounds = device.defaultConfiguration.bounds
                if (   mousePoint.x >= bounds.x &&
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
        return Point(0,0)
    }
}
```



#### @Author : [Víctor Bolinches](https://www.linkedin.com) | [@vicboma1](https://twitter.com/vicboma1)
