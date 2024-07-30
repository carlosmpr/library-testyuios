---
  title: 'JavaTestFile.java'
  description: 'component description'
  pubDate: 'July 30, 2024'
  author: 'Carlos Polanco'
  ---
  
  
  
  # JavaTestFile.java
  # BMI Chart Explanation

The provided code is a Java program that generates a BMI (Body Mass Index) trend chart using the JFreeChart library. Below is a breakdown of the code:

### Libraries Used
- **JFreeChart**: A popular Java library for creating a variety of charts including line charts, bar charts, pie charts, etc.
- **Swing**: Java's GUI toolkit for creating graphical user interfaces.

### Class: BMIChart
- **Constructor `BMIChart(String title)`**: Initializes the BMIChart class by setting up the chart with the provided title.
- **Method `createDataset()`**: Creates a dataset containing BMI values for two individuals over time.
- **Method `createChart(XYSeriesCollection dataset)`**: Creates the actual chart using the dataset provided. It sets up the chart title, axis labels, and plot orientation.

### Main Method
- **`main(String[] args)`**: The entry point of the program. It creates an instance of the BMIChart class, sets up the chart's size and appearance, and displays it on the screen.

### Example Usage
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

### Purpose
The code generates a line chart showing the BMI trends of two individuals over time. It visualizes how their BMI values change across different months.

### Parameters
- **`String title`**: The title of the chart.
- **`XYSeriesCollection dataset`**: A collection of XYSeries data points representing the BMI values of individuals over time.

### Operations
1. The `createDataset()` method populates the dataset with BMI values for two individuals.
2. The `createChart()` method creates a line chart with the provided dataset, setting up the chart's title, axis labels, and orientation.
3. The main method initializes the BMIChart, sets its size and appearance, and displays it on the screen.

### Usage
To use the code, you can create an instance of the `BMIChart` class with a title, set its size and appearance, and display the BMI trend chart on the screen.

This code provides a simple and effective way to visualize BMI trends using Java and the JFreeChart library.
  
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
  