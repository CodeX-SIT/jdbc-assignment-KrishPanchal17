# JDBC Assignment 
## Connecting to MySQL Database and Running Basic Queries

---

### Objective:

This assignment aims to introduce students to JDBC (Java Database Connectivity) and guide them through connecting to a MySQL database and executing queries.

### Instructions:

#### Part 1: Setup

1. Download and install MySQL: Install MySQL on your machine if you haven't already. You can download it [here](https://dev.mysql.com/downloads/).

2. Create a Database: Using MySQL Workbench or any MySQL client, create a new database named `university`.

3. Create a Table: Within the `university` database, create a table named `students` with the following columns:
   - `id` (INT, PRIMARY KEY, AUTO_INCREMENT)
   - `name` (VARCHAR)
   - `age` (INT)

#### Part 2: Java Program

Write a Java program that connects to the MySQL database and performs the following operations:

1. **Connect to the Database:**
   - Use JDBC to establish a connection to the MySQL database.

2. **Insert Data:**
   - Insert at least two records into the `students` table.

3. **Retrieve Data:**
   - Retrieve and print all records from the `students` table.

4. **Update Data:**
   - Update the age of one student.

5. **Delete Data:**
   - Delete one student from the table.

#### Part 3: Submission

Submit the Java source code file along with a screenshot of the table you created using [MySQL Shell](https://www.mysqltutorial.org/mysql-administration/mysql-show-columns/) in the same way you submitted your last assignment.
Source Code:

import java.sql.*;
import java.util.Scanner;

public class Assignment2 {
    static final String JDBC_URL = "jdbc:mysql://localhost:3306/university";
    static final String USERNAME = "root";
    static final String PASSWORD = "**********";
    
    public static void main(String[] args) {
        try {
            Assignment2 assignment = new Assignment2();
            Connection connection = assignment.getConnection();
            System.out.println("Connected to the database");

            assignment.insertData(connection);
            assignment.retrieveData(connection);
            assignment.updateData(connection);
            assignment.retrieveData(connection); 
            assignment.deleteData(connection);

            connection.close();
            System.out.println("Connection closed");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public Connection getConnection() throws SQLException {
        return DriverManager.getConnection(JDBC_URL, USERNAME, PASSWORD);
    }

    public void insertData(Connection connection) throws SQLException {
        Statement statement = connection.createStatement();
        Scanner scanner = new Scanner(System.in);

        System.out.println("Enter student name:");
        String name1 = scanner.nextLine();
        System.out.println("Enter student age:");
        int age1 = scanner.nextInt();

        System.out.println("Enter another student name:");
        scanner.nextLine(); // Consume newline left by nextInt()
        String name2 = scanner.nextLine();
        System.out.println("Enter another student age:");
        int age2 = scanner.nextInt();

        statement.executeUpdate("INSERT INTO students (name, age) VALUES ('" + name1 + "', " + age1 + ")");
        statement.executeUpdate("INSERT INTO students (name, age) VALUES ('" + name2 + "', " + age2 + ")");
        System.out.println("Data inserted into students table");
    }

    public void retrieveData(Connection connection) throws SQLException {
        Statement statement = connection.createStatement();
        ResultSet resultSet = statement.executeQuery("SELECT * FROM students");
        System.out.println("Retrieving data from students table:");
        while (resultSet.next()) {
            int id = resultSet.getInt("id");
            String name = resultSet.getString("name");
            int age = resultSet.getInt("age");
            System.out.println("ID: " + id + ", Name: " + name + ", Age: " + age);
        }
    }

    public void updateData(Connection connection) throws SQLException {
        Statement statement = connection.createStatement();
        Scanner scanner = new Scanner(System.in);

        System.out.println("Enter the ID of the student whose data you want to update:");
        int id = scanner.nextInt();
        scanner.nextLine(); // Consume newline left by nextInt()

        System.out.println("What do you want to update? (age/name)");
        String choice = scanner.nextLine();

        if (choice.equalsIgnoreCase("age")) {
            System.out.println("Enter the new age:");
            int newAge = scanner.nextInt();
            statement.executeUpdate("UPDATE students SET age = " + newAge + " WHERE id = " + id);
            System.out.println("Age of student with ID " + id + " updated");
        } else if (choice.equalsIgnoreCase("name")) {
            System.out.println("Enter the new name:");
            String newName = scanner.nextLine();
            statement.executeUpdate("UPDATE students SET name = '" + newName + "' WHERE id = " + id);
            System.out.println("Name of student with ID " + id + " updated");
        } else {
            System.out.println("Invalid choice");
        }
    }

    public void deleteData(Connection connection) throws SQLException {
        Statement statement = connection.createStatement();
        Scanner scanner = new Scanner(System.in);

        System.out.println("Enter the ID of the student you want to delete:");
        int id = scanner.nextInt();

        int rowsDeleted = statement.executeUpdate("DELETE FROM students WHERE id = " + id);
        if (rowsDeleted > 0) {
            System.out.println("One student deleted from the table");
        } else {
            System.out.println("No student found with the given ID");
        }
    }
}

OUTPUT:
<img width="249" alt="Screenshot 2024-02-24 110717" src="https://github.com/CodeX-SIT/jdbc-assignment-KrishPanchal17/assets/139237988/ed120941-7f73-457e-9a7e-c6b6cf62e300">
<img width="424" alt="Screenshot 2024-02-24 110702" src="https://github.com/CodeX-SIT/jdbc-assignment-KrishPanchal17/assets/139237988/1156cb74-f2bb-402a-bd81-fe063a8ad666">
<img width="415" alt="Screenshot 2024-02-24 110707" src="https://github.com/CodeX-SIT/jdbc-assignment-KrishPanchal17/assets/139237988/4e89bbd7-7154-437d-b67c-ae3548642c8c">


### Additional Tips:

- Use the JDBC documentation ([Oracle JDBC API](https://docs.oracle.com/en/java/javase/14/docs/api/java.sql/java/sql/package-summary.html)) as a reference guide.
- Ensure you have the MySQL Connector/J library in your project. You can download it [here](https://dev.mysql.com/downloads/connector/j/).
- If you face any issues, you can contact your technical executives and peers in the community!
