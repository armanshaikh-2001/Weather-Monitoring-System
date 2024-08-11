### **Java Code:**

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

public class WeatherAppGUI {
    private JFrame frame;
    private JTextField cityTextField;
    private JLabel cityLabel;
    private JLabel dateLabel;
    private JLabel temperatureLabel;
    private JLabel conditionLabel;

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                try {
                    WeatherAppGUI window = new WeatherAppGUI();
                    window.frame.setVisible(true);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        });
    }

    public WeatherAppGUI() {
        initialize();
    }

    private void initialize() {
        frame = new JFrame("Sadiq's Weather App");
        frame.setBounds(100, 100, 450, 350);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.getContentPane().setLayout(null);

        // City input field
        cityTextField = new JTextField();
        cityTextField.setFont(new Font("Tahoma", Font.PLAIN, 18));
        cityTextField.setBounds(120, 30, 200, 40);
        frame.getContentPane().add(cityTextField);
        cityTextField.setColumns(10);

        // City label
        cityLabel = new JLabel("Enter City");
        cityLabel.setFont(new Font("Tahoma", Font.PLAIN, 18));
        cityLabel.setBounds(10, 30, 100, 40);
        frame.getContentPane().add(cityLabel);

        // Fetch weather button
        JButton btnGetWeather = new JButton("Get Weather");
        btnGetWeather.setFont(new Font("Tahoma", Font.PLAIN, 18));
        btnGetWeather.setBounds(120, 80, 200, 40);
        frame.getContentPane().add(btnGetWeather);

        // Weather information labels
        cityLabel = new JLabel("City: ");
        cityLabel.setFont(new Font("Tahoma", Font.PLAIN, 20));
        cityLabel.setBounds(10, 150, 400, 25);
        frame.getContentPane().add(cityLabel);

        dateLabel = new JLabel("Date: ");
        dateLabel.setFont(new Font("Tahoma", Font.PLAIN, 20));
        dateLabel.setBounds(10, 180, 400, 25);
        frame.getContentPane().add(dateLabel);

        temperatureLabel = new JLabel("Temperature: ");
        temperatureLabel.setFont(new Font("Tahoma", Font.PLAIN, 50));
        temperatureLabel.setBounds(10, 220, 400, 50);
        frame.getContentPane().add(temperatureLabel);

        conditionLabel = new JLabel("Condition: ");
        conditionLabel.setFont(new Font("Tahoma", Font.PLAIN, 30));
        conditionLabel.setBounds(10, 280, 400, 40);
        frame.getContentPane().add(conditionLabel);

        // Action listener for the button
        btnGetWeather.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                String cityName = cityTextField.getText();
                fetchWeatherData(cityName);
            }
        });
    }

    private void fetchWeatherData(String cityName) {
        String apiKey = "your_api_key"; // Replace with your API key
        String apiUrl = "http://api.weatherapi.com/v1/current.json?key=" + apiKey + "&q=" + cityName;

        try {
            URL url = new URL(apiUrl);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            connection.setRequestMethod("GET");
            InputStreamReader reader = new InputStreamReader(connection.getInputStream());

            JsonParser jsonParser = new JsonParser();
            JsonObject jsonObject = (JsonObject) jsonParser.parse(reader);

            // Extract weather data
            String city = jsonObject.get("location").getAsJsonObject().get("name").getAsString();
            String country = jsonObject.get("location").getAsJsonObject().get("country").getAsString();
            String date = jsonObject.get("location").getAsJsonObject().get("localtime").getAsString();
            String condition = jsonObject.get("current").getAsJsonObject().get("condition").getAsJsonObject().get("text").getAsString();
            double temperature = jsonObject.get("current").getAsJsonObject().get("temp_c").getAsDouble();

            // Update GUI with fetched data
            cityLabel.setText(city + ", " + country);
            dateLabel.setText("Date: " + date);
            temperatureLabel.setText(temperature + "°C");
            conditionLabel.setText(condition);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
