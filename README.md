package weatherapiclient;


	

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import org.json.JSONObject;


public class weatherapppp {

    private static final String API_KEY = "74ae8a41d985d4000f819419ac81e5c3";
    private static final String API_URL = "http://api.openweathermap.org/data/2.5/weather";

    public static void main(String[] args) {
        try {
            System.out.println("Enter city name:");
            BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
            String city = reader.readLine();

            String urlString = API_URL + "?q=" + city + "&appid=" + API_KEY + "&units=metric";

            URL url = new URL(urlString);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            connection.setRequestMethod("GET");

            int responseCode = connection.getResponseCode();
            if (responseCode == 200) {
                BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream()));
                String inputLine;
                StringBuilder content = new StringBuilder();

                while ((inputLine = in.readLine()) != null) {
                    content.append(inputLine);
                }
                in.close();

                // ✅ Parse JSON from API response

             // Parse the JSON string from API response
                JSONObject json = new JSONObject(content.toString());

                // ✅ Extract weather data correctly
                String cityName = json.getString("name");
                int timeZone = json.getInt("timezone");
                double temperature = json.getJSONObject("main").getDouble("temp");
                int humidity = json.getJSONObject("main").getInt("humidity");
                double windSpeed = json.getJSONObject("wind").getDouble("speed");
                String description = json.getJSONArray("weather").getJSONObject(0).getString("description");


                // ✅ Display structured output
                System.out.println("\n=== Weather in " + cityName + " ===");
                System.out.printf("Temperature: %.1f°C%n", temperature);
                System.out.println("Humidity: " + humidity + "%");
                System.out.println("Wind Speed: " + windSpeed + " m/s");
                System.out.println("Condition: " + description);
                System.out.println("Timezone Offset: UTC+" + (timeZone / 3600));

            } else {
                System.out.println("Error fetching data. Response code: " + responseCode);
            }

            connection.disconnect();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
