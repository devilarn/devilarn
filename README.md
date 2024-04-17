import java.sql.*;

public class EVBookingSystem 
{
    // JDBC URL, username, and password of MySQL server
    private static final String JDBC_URL = "jdbc:mysql://localhost:3306/ev_booking_system";
    private static final String USERNAME = "your_username";
    private static final String PASSWORD = "your_password";

    // JDBC variables for opening, closing, and managing connection
    private static Connection connection;
    private static Statement statement;
    private static ResultSet resultSet;

    public static void main(String[] args) 
    {
        try 
        {
            // Connecting to the MySQL database
            connection = DriverManager.getConnection(JDBC_URL, USERNAME, PASSWORD);

            // Creating a statement object
            statement = connection.createStatement();

            // Retrieving all bookings
            System.out.println("All Bookings:");
            getAllBookings();

            // Inserting a new booking
            insertBooking(1, "John Doe", "2024-04-12", "10:00", "2 hours");

            // Updating an existing booking
            updateBooking(1, "2024-04-12", "11:00");

            // Deleting a booking
            deleteBooking(1);

            // Closing the connection
            connection.close();
        } 
        catch (SQLException e) 
        {
            e.printStackTrace();
        }
    }

    // Method to retrieve all bookings
    private static void getAllBookings() throws SQLException 
    {
        String query = "SELECT * FROM bookings";
        resultSet = statement.executeQuery(query);

        while (resultSet.next()) 
        {
            int id = resultSet.getInt("id");
            String name = resultSet.getString("customer_name");
            String date = resultSet.getString("booking_date");
            String time = resultSet.getString("booking_time");
            String duration = resultSet.getString("duration");

            System.out.println("ID: " + id + ", Name: " + name + ", Date: " + date + ", Time: " + time + ", Duration: " + duration);
        }
    }

    // Method to insert a new booking
    private static void insertBooking(int id, String name, String date, String time, String duration) throws SQLException 
    {
        String query = "INSERT INTO bookings (id, customer_name, booking_date, booking_time, duration) VALUES (?, ?, ?, ?, ?)";
        PreparedStatement preparedStatement = connection.prepareStatement(query);
        preparedStatement.setInt(1, id);
        preparedStatement.setString(2, name);
        preparedStatement.setString(3, date);
        preparedStatement.setString(4, time);
        preparedStatement.setString(5, duration);

        preparedStatement.executeUpdate();
        System.out.println("New booking inserted successfully.");
    }

    // Method to update an existing booking
    private static void updateBooking(int id, String date, String time) throws SQLException 
    {
        String query = "UPDATE bookings SET booking_date = ?, booking_time = ? WHERE id = ?";
        PreparedStatement preparedStatement = connection.prepareStatement(query);
        preparedStatement.setString(1, date);
        preparedStatement.setString(2, time);
        preparedStatement.setInt(3, id);

        preparedStatement.executeUpdate();
        System.out.println("Booking updated successfully.");
    }

    // Method to delete a booking
    private static void deleteBooking(int id) throws SQLException {
        String query = "DELETE FROM bookings WHERE id = ?";
        PreparedStatement preparedStatement = connection.prepareStatement(query);
        preparedStatement.setInt(1, id);

        preparedStatement.executeUpdate();
        System.out.println("Booking deleted successfully.");
    }
}
