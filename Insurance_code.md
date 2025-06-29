# 🧱 Tech Stack
| Layer       | Technology           |
| ----------- | -------------------- |
| Frontend    | HTML, CSS, JSP       |
| Backend     | Servlet API          |
| Database    | MySQL                |
| JDBC Driver | mysql-connector-java |
| Server      | Apache Tomcat        |
| Build Tool  | Manual or Maven      |

# 📁 Project Structure
InsuranceManagementSystem/
├── WebContent/
│   ├── addCustomer.jsp
│   ├── addPolicy.jsp
│   ├── viewPolicies.jsp
│   └── index.jsp
├── src/
│   ├── model/
│   │   ├── Customer.java
│   │   └── Policy.java
│   ├── dao/
│   │   ├── CustomerDAO.java
│   │   └── PolicyDAO.java
│   └── servlet/
│       ├── AddCustomerServlet.java
│       ├── AddPolicyServlet.java
│       └── ViewPoliciesServlet.java
├── lib/
│   └── mysql-connector-java-8.x.x.jar
└── web.xml


#🛢️ Database Schema (MySQL)
```sql
CREATE TABLE customer (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(100) NOT NULL,
  contact VARCHAR(20),
  email VARCHAR(100)
);

CREATE TABLE policy (
  id INT PRIMARY KEY AUTO_INCREMENT,
  policy_type VARCHAR(50) NOT NULL,
  premium DECIMAL(10,2) NOT NULL,
  customer_id INT,
  FOREIGN KEY (customer_id) REFERENCES customer(id)
    ON DELETE CASCADE
    ON UPDATE CASCADE
);
```

## 1️⃣ model/Customer.java
```java
public class Customer {
    private int id;
    private String name;
    private String contact;
    private String email;

    // Getters & setters
}

```

## 2️⃣ model/Policy.java
```java
public class Policy {
    private int id;
    private String policyType;
    private double premium;
    private int customerId;

    // Getters & setters
}
```
## 3️⃣ dao/CustomerDAO.java
```java
import java.sql.*;

public class CustomerDAO {
    private Connection conn;

    public CustomerDAO(Connection conn) {
        this.conn = conn;
    }

    public void addCustomer(Customer c) throws SQLException {
        String sql = "INSERT INTO customer (name, contact, email) VALUES (?, ?, ?)";
        try (PreparedStatement stmt = conn.prepareStatement(sql)) {
            stmt.setString(1, c.getName());
            stmt.setString(2, c.getContact());
            stmt.setString(3, c.getEmail());
            stmt.executeUpdate();
        }
    }
}
```
## 4️⃣ dao/PolicyDAO.java
```java
public class PolicyDAO {
    private Connection conn;

    public PolicyDAO(Connection conn) {
        this.conn = conn;
    }

    public void addPolicy(Policy p) throws SQLException {
        String sql = "INSERT INTO policy (policy_type, premium, customer_id) VALUES (?, ?, ?)";
        try (PreparedStatement stmt = conn.prepareStatement(sql)) {
            stmt.setString(1, p.getPolicyType());
            stmt.setDouble(2, p.getPremium());
            stmt.setInt(3, p.getCustomerId());
            stmt.executeUpdate();
        }
    }

    public ResultSet getAllPolicies() throws SQLException {
        String sql = "SELECT * FROM policy";
        Statement stmt = conn.createStatement();
        return stmt.executeQuery(sql);
    }
}
```
## 5️⃣ servlet/AddCustomerServlet.java
```java
@WebServlet("/add-customer")
public class AddCustomerServlet extends HttpServlet {
    protected void doPost(HttpServletRequest req, HttpServletResponse res)
            throws ServletException, IOException {
        String name = req.getParameter("name");
        String contact = req.getParameter("contact");
        String email = req.getParameter("email");

        try {
            Connection conn = DBUtil.getConnection();
            CustomerDAO dao = new CustomerDAO(conn);
            Customer c = new Customer();
            c.setName(name);
            c.setContact(contact);
            c.setEmail(email);
            dao.addCustomer(c);
            res.sendRedirect("index.jsp");
        } catch (Exception e) {
            throw new ServletException(e);
        }
    }
}
```
## 6️⃣ servlet/AddPolicyServlet.java
```java
@WebServlet("/add-policy")
public class AddPolicyServlet extends HttpServlet {
    protected void doPost(HttpServletRequest req, HttpServletResponse res)
            throws ServletException, IOException {
        String type = req.getParameter("policyType");
        double premium = Double.parseDouble(req.getParameter("premium"));
        int customerId = Integer.parseInt(req.getParameter("customerId"));

        try {
            Connection conn = DBUtil.getConnection();
            PolicyDAO dao = new PolicyDAO(conn);
            Policy p = new Policy();
            p.setPolicyType(type);
            p.setPremium(premium);
            p.setCustomerId(customerId);
            dao.addPolicy(p);
            res.sendRedirect("viewPolicies.jsp");
        } catch (Exception e) {
            throw new ServletException(e);
        }
    }
}
```

## 7️⃣ viewPolicies.jsp
```jsp
<%@ page import="java.sql.*, dao.PolicyDAO, util.DBUtil" %>
<html>
<head><title>Policies</title></head>
<body>
    <h2>All Policies</h2>
    <table border="1">
        <tr><th>ID</th><th>Type</th><th>Premium</th><th>Customer ID</th></tr>
        <%
            Connection conn = DBUtil.getConnection();
            PolicyDAO dao = new PolicyDAO(conn);
            ResultSet rs = dao.getAllPolicies();
            while(rs.next()) {
        %>
        <tr>
            <td><%= rs.getInt("id") %></td>
            <td><%= rs.getString("policy_type") %></td>
            <td><%= rs.getDouble("premium") %></td>
            <td><%= rs.getInt("customer_id") %></td>
        </tr>
        <% } %>
    </table>
</body>
</html>
```
## 🧩 DBUtil.java
```java
public class DBUtil {
    public static Connection getConnection() throws Exception {
        Class.forName("com.mysql.cj.jdbc.Driver");
        return DriverManager.getConnection(
            "jdbc:mysql://localhost:3306/insurance_db", "root", "yourpassword");
    }
}
```
##📑 web.xml
```
<web-app>
    <servlet>
        <servlet-name>AddCustomer</servlet-name>
        <servlet-class>servlet.AddCustomerServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>AddCustomer</servlet-name>
        <url-pattern>/add-customer</url-pattern>
    </servlet-mapping>
</web-app>
```


