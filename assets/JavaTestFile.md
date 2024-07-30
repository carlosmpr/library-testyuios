---
  title: 'JavaTestFile.java'
  description: 'component description'
  pubDate: 'July 30, 2024'
  author: 'Carlos P'
  ---
  
  
  
  # JavaTestFile.java
  # BMI Chart Explanation

The provided code is a Java program that generates a BMI (Body Mass Index) trend chart using the JFreeChart library. Below is a breakdown of the code:

### Libraries Used:
- **JFreeChart**: A popular Java library for creating a variety of charts including pie charts, bar charts, line charts, etc.
- **Swing**: Java's GUI toolkit for creating graphical user interfaces.

### Code Explanation:
1. **BMIChart Class**:
   - Extends `ApplicationFrame` which is a Swing container for the chart.
   - Constructor `BMIChart(String title)`:
     - Initializes the chart by calling `createChart()` with the dataset created by `createDataset()`.
     - Creates a `ChartPanel` to display the chart and sets its size.
     - Sets the content pane of the frame to the `ChartPanel`.

2. **createDataset() Method**:
   - Creates a dataset with two series (Person 1 and Person 2) containing BMI values over time.
   - Each series is represented by an `XYSeries` object and added to an `XYSeriesCollection`.

3. **createChart() Method**:
   - Creates an XY line chart using `ChartFactory.createXYLineChart()`.
   - Sets chart title, x-axis label, y-axis label, dataset, orientation, and other properties.
   - Retrieves the plot of the chart and sets a `XYLineAndShapeRenderer` to customize the appearance of the lines.

4. **main() Method**:
   - Entry point of the program.
   - Creates an instance of `BMIChart`, sets its properties, and displays the chart using Swing utilities.

### Example Usage:
```java
public static void main(String[] args) {
    SwingUtilities.invokeLater(() -> {
        BMIChart example = new BMIChart("BMI Trend Chart");
        example.setSize(800, 600);
        example.setLocationRelativeTo(null);
        example.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
        example.setVisible(true);
    });
}
```

### Running the Code:
- Compile and run the Java program.
- A window will open displaying the BMI trend chart for Person 1 and Person 2 over time.

This code showcases how to create a simple BMI trend chart using JFreeChart in Java, making it easy to visualize and compare BMI values for different individuals over a period.
  
  ## Component Code
  ```jsx
  import org.jfree.chart.ChartFactory;
import org.jfree.chart.ChartPanel;
import org.jfree.chart.JFreeChart;
import org.jfree.chart.plot.PlotOrientation;
import org.jfree.chart.plot.XYPlot;
import org.jfree.chart.renderer.xy.XYLineAndShapeRenderer;
import org.jfree.data.xy.XYSeries;
import org.jfree.data.xy.XYSeriesCollection;
import org.jfree.ui.ApplicationFrame;

import javax.swing.*;
import java.awt.*;

public class BMIChart extends ApplicationFrame {

    public BMIChart(String title) {
        super(title);
        JFreeChart xylineChart = createChart(createDataset());
        ChartPanel chartPanel = new ChartPanel(xylineChart);
        chartPanel.setPreferredSize(new Dimension(800, 600));
        setContentPane(chartPanel);
    }

    private XYSeriesCollection createDataset() {
        XYSeries series1 = new XYSeries("Person 1");
        series1.add(1, 22.5);
        series1.add(2, 23.0);
        series1.add(3, 22.8);
        series1.add(4, 23.5);

        XYSeries series2 = new XYSeries("Person 2");
        series2.add(1, 25.0);
        series2.add(2, 25.4);
        series2.add(3, 25.1);
        series2.add(4, 26.0);

        XYSeriesCollection dataset = new XYSeriesCollection();
        dataset.addSeries(series1);
        dataset.addSeries(series2);

        return dataset;
    }

    private JFreeChart createChart(XYSeriesCollection dataset) {
        JFreeChart chart = ChartFactory.createXYLineChart(
                "BMI Trends",
                "Time (Months)",
                "BMI",
                dataset,
                PlotOrientation.VERTICAL,
                true, true, false);

        XYPlot plot = chart.getXYPlot();
        XYLineAndShapeRenderer renderer = new XYLineAndShapeRenderer();
        plot.setRenderer(renderer);
        return chart;
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            BMIChart example = new BMIChart("BMI Trend Chart");
            example.setSize(800, 600);
            example.setLocationRelativeTo(null);
            example.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
            example.setVisible(true);
        });
    }
}
  ```
  