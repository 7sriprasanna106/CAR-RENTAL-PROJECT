# CAR-RENTAL-PROJECT
Always show details

Copy
# Add more detailed functionality to the car rental project with additional classes and SQL script.

# Define new files
more_files = {
    "CarRentalProject/src/Car.java": """
public class Car {
    private int id;
    private String model;
    private String brand;
    private boolean available;

    public Car(int id, String model, String brand, boolean available) {
        this.id = id;
        this.model = model;
        this.brand = brand;
        this.available = available;
    }

    public int getId() { return id; }
    public String getModel() { return model; }
    public String getBrand() { return brand; }
    public boolean isAvailable() { return available; }
}
""",
    "CarRentalProject/src/CarDAO.java": """
import java.sql.*;
import java.util.*;

public class CarDAO {
    public static List<Car> getAllCars() {
        List<Car> cars = new ArrayList<>();
        try (Connection conn = DBConnection.getConnection()) {
            String sql = "SELECT * FROM cars";
            Statement stmt = conn.createStatement();
            ResultSet rs = stmt.executeQuery(sql);
            while (rs.next()) {
                cars.add(new Car(
                    rs.getInt("id"),
                    rs.getString("model"),
                    rs.getString("brand"),
                    rs.getBoolean("available")
                ));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return cars;
    }
}
""",
    "CarRentalProject/src/Dashboard.java": """
import javax.swing.*;
import java.awt.*;
import java.util.List;

public class Dashboard extends JFrame {
    public Dashboard() {
        setTitle("Car Dashboard");
        setSize(400, 300);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        List<Car> cars = CarDAO.getAllCars();
        String[] columnNames = {"ID", "Model", "Brand", "Available"};
        String[][] data = new String[cars.size()][4];

        for (int i = 0; i < cars.size(); i++) {
            Car c = cars.get(i);
            data[i][0] = String.valueOf(c.getId());
            data[i][1] = c.getModel();
            data[i][2] = c.getBrand();
            data[i][3] = c.isAvailable() ? "Yes" : "No";
        }

        JTable table = new JTable(data, columnNames);
        add(new JScrollPane(table));
        setVisible(true);
    }
}
""",
    "CarRentalProject/src/LoginFrame.java": """
import javax.swing.*;
import java.awt.event.*;

public class LoginFrame extends JFrame {
    public LoginFrame() {
        setTitle("Car Rental Login");
        setSize(300, 150);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);
        JPanel panel = new JPanel();
        add(panel);
        placeComponents(panel);
        setVisible(true);
    }

    private void placeComponents(JPanel panel) {
        panel.setLayout(null);

        JLabel userLabel = new JLabel("User:");
        userLabel.setBounds(10, 10, 80, 25);
        panel.add(userLabel);

        JTextField userText = new JTextField(20);
        userText.setBounds(100, 10, 160, 25);
        panel.add(userText);

        JLabel passwordLabel = new JLabel("Password:");
        passwordLabel.setBounds(10, 40, 80, 25);
        panel.add(passwordLabel);

        JPasswordField passwordText = new JPasswordField(20);
        passwordText.setBounds(100, 40, 160, 25);
        panel.add(passwordText);

        JButton loginButton = new JButton("Login");
        loginButton.setBounds(100, 80, 80, 25);
        panel.add(loginButton);

        loginButton.addActionListener(e -> {
            dispose();
            new Dashboard();
        });
    }
}
""",
    "CarRentalProject/carrental.sql": """
CREATE DATABASE IF NOT EXISTS carrental;
USE carrental;

CREATE TABLE IF NOT EXISTS cars (
    id INT AUTO_INCREMENT PRIMARY KEY,
    model VARCHAR(100),
    brand VARCHAR(100),
    available BOOLEAN DEFAULT TRUE
);

INSERT INTO cars (model, brand, available) VALUES
('Civic', 'Honda', TRUE),
('Corolla', 'Toyota', TRUE),
('Model 3', 'Tesla', FALSE);
"""
}

# Write new files
for path, content in more_files.items():
    full_path = os.path.join("/mnt/data", path)
    os.makedirs(os.path.dirname(full_path), exist_ok=True)
    with open(full_path, "w") as f:
        f.write(content.strip())

# Update zip with new files
with ZipFile("/mnt/data/CarRentalProject.zip", 'a') as zipf:
    for path in more_files:
        zipf.write(os.path.join("/mnt/data", path), arcname=path.replace("CarRentalProject/", ""))

"/mnt/data/CarRentalProject.zip"
STDOUT/STDERR
/usr/local/lib/python3.11/zipfile.py:1558: UserWarning: Duplicate name: 'src/LoginFrame.java'
  return self._open_to_write(zinfo, force_zip64=force_zip64)
Result
'/mnt/data/CarRentalProject.zip'
