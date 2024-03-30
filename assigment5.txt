import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

class Department {
    private int id;
    private String name;

    public Department(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}

public class DepartmentDAO {

    private static final String DB_URL = "jdbc:mysql://localhost:3306/departments";
    private static final String DB_USER = "your_username";
    private static final String DB_PASSWORD = "your_password";

    private static final String INSERT_QUERY = "INSERT INTO department (id, name) VALUES (?, ?)";

    public void saveDepartment(Department department) {
        try (Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD);
             PreparedStatement preparedStatement = conn.prepareStatement(INSERT_QUERY)) {

            preparedStatement.setInt(1, department.getId());
            preparedStatement.setString(2, department.getName());

            int rowsAffected = preparedStatement.executeUpdate();
            if (rowsAffected > 0) {
                System.out.println("Department inserted successfully.");
            } else {
                System.out.println("Failed to insert department.");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        Department department = new Department(1, "IT");
        DepartmentDAO departmentDAO = new DepartmentDAO();
        departmentDAO.saveDepartment(department);
    }
}