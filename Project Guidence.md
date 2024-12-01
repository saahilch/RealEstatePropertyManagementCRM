**RealEstatePropertyManagementCRM**

1. Ensure you have JDK 11 or later installed on your machine.
2. Install Maven for dependency management.
3. Clone the project repository or download the project files.
4. Import the project into your IDE (e.g., IntelliJ IDEA, Eclipse, or STS).
5. Configure your database connection in the `application.properties` file, for example:

    spring.datasource.url=jdbc:mysql://localhost:3306/ 
    spring.datasource.username=root
    spring.datasource.password=root123
    spring.jpa.hibernate.ddl-auto=update
    spring.jpa.show-sql=true

6. Use Maven to install dependencies by running `mvn clean install` in the terminal.
7. Start the application by running the main class (e.g., `PropertyManagementApplication.java`).


For Database Use Below Constraints. (If ORM not Worked)


CREATE TABLE Users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    role ENUM('ADMIN', 'MANAGER', 'TENANT') NOT NULL
);

CREATE TABLE Properties (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    location VARCHAR(200) NOT NULL,
    availability BOOLEAN NOT NULL,
    status ENUM('AVAILABLE', 'OCCUPIED', 'MAINTENANCE') NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    manager_id INT NOT NULL,
    FOREIGN KEY (manager_id) REFERENCES Users(id)
);

CREATE TABLE Payments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    paymentDate DATE NOT NULL,
    amount DECIMAL(10, 2) NOT NULL,
    paymentMethod VARCHAR(50) NOT NULL,
    tenant_id INT NOT NULL,
    property_id INT NOT NULL,
    FOREIGN KEY (tenant_id) REFERENCES Users(id),
    FOREIGN KEY (property_id) REFERENCES Properties(id)
);

CREATE TABLE MaintenanceRequests (
    id INT AUTO_INCREMENT PRIMARY KEY,
    issueDescription VARCHAR(255) NOT NULL,
    status ENUM('PENDING', 'IN_PROGRESS', 'RESOLVED') NOT NULL,
    tenant_id INT NOT NULL,
    property_id INT NOT NULL,
    reportedDate DATE NOT NULL,
    resolvedDate DATE,
    FOREIGN KEY (tenant_id) REFERENCES Users(id),
    FOREIGN KEY (property_id) REFERENCES Properties(id)
);
Running Instructions

1. Start the application in your IDE or using `mvn spring-boot:run` in the terminal.
2. Ensure your MySQL database is running.
3. Use Postman or any API client to test the endpoints.
4. Example base URL: `http://localhost:8080`

API Usage
AuthController
Base URL:** / auth**`

- POST /register
  - Description: Register a new user.
  - Request Body:
    {
        "email": "test@example.com",
        "password": "password123"
    }
  - Response: "User registered successfully"

- POST /login
  - Description: Authenticate user and return a JWT token.
  - Request Body:
    {
        "email": "test@example.com",
        "password": "password123"
    }
  - Response: JWT token as a string.

MaintenanceRequestController
Base URL: `/api/maintenance`

- **POST** /
  - Description: Create a new maintenance request.
  - Request Body:
    {
        "issueDescription": "Pipe leakage in flat 101"
    }
  - Response: Maintenance request object.

- **GET** /
  - Description: Retrieve all maintenance requests.
  - Response: List of maintenance requests.

**- GET /{id}
**  - Description: Retrieve a maintenance request by ID.
  - URL Example: `/api/maintenance/1`
  - Response: Maintenance request object.

- **PUT** /{**id**}
  - Description: Update a maintenance request.
  - URL Example: `/api/maintenance/1`
  - Request Body:
    {
        "issueDescription": "Pipe leakage resolved",
        "status": "RESOLVED",
        "resolvedDate": "2024-12-01"
    }
  - Response: Updated maintenance request.

- **DELETE /{id}**
  - Description: Delete a maintenance request.
  - URL Example: `/api/maintenance/1`
  - Response: 204 No Content.

PropertiesController
Base URL: `/properties`

- POST /
  - Description: Add a new property.
  - Request Body:
    {
        "propertyName": "Shree Residency",
        "location": "Mumbai",
        "managerId": 1
    }
  - Response: Property object.

- PUT /{id}
  - Description: Update a property.
  - URL Example: `/properties/1`
  - Request Body:
    {
        "propertyName": "Shree Residency Updated",
        "location": "Thane"
    }
  - Response: Updated property object.

- DELETE /{id}
  - Description: Delete a property.
  - URL Example: `/properties/1`
  - Response: 204 No Content.

- GET /{id}
  - Description: Get property by ID.
  - URL Example: `/properties/1`
  - Response: Property object.

- GET /
  - Description: Get all properties.
  - Response: List of properties.

- GET /manager/{managerId}
  - Description: Get properties managed by a specific manager.
  - URL Example: `/properties/manager/1`
  - Response: List of properties.

Sample Test Data

- Register User
  {
      "email": "manager@example.com",
      "password": "manager123"
  }

- Create Maintenance Request :
  {
      "issueDescription": "Water leakage in kitchen"
  }

- Add Property :
  {
      "propertyName": "Lodha Paradise",
      "location": "Pune",
      "managerId": 1
  }


