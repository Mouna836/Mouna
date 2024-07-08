EXERCISE 1

CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL
);

CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    department_id INT,
    hire_date DATE NOT NULL,
    FOREIGN KEY (department_id) REFERENCES departments(department_id)
);

CREATE TABLE salaries (
    employee_id INT,
    salary DECIMAL(10, 2) NOT NULL,
    from_date DATE NOT NULL,
    to_date DATE NOT NULL,
    FOREIGN KEY (employee_id) REFERENCES employees(employee_id),
    PRIMARY KEY (employee_id, from_date)
);

EXERCISE 2

SELECT e.employee_id, e.first_name, e.last_name, d.department_name, s.salary
FROM employees e
JOIN salaries s ON e.employee_id = s.employee_id
JOIN departments d ON e.department_id = d.department_id
ORDER BY s.salary DESC
LIMIT 100;

programming assesment

exercise 1
@Entity
public class Customer {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;
    private String phone;
    private String address;
    // Getters and setters
}

@Entity
public class Contact {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private Long customerId;
    private String name;
    private String email;
    private String phone;
    // Getters and setters
}

@Entity
public class Opportunity {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private Long customerId;
    private String description;
    private String stage;
    private BigDecimal amount;
    // Getters and setters
}

@Entity
public class Interaction {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private Long customerId;
    private String type;
    private Timestamp date;
    private String notes;
    // Getters and setters
}
public interface CustomerRepository extends JpaRepository<Customer, Long> {}
public interface ContactRepository extends JpaRepository<Contact, Long> {}
public interface OpportunityRepository extends JpaRepository<Opportunity, Long> {}
public interface InteractionRepository extends JpaRepository<Interaction, Long> {}
@Service
public class CustomerService {
    @Autowired private CustomerRepository customerRepository;
    // Methods for add, edit, delete, view customer records
}

@Service
public class InteractionService {
    @Autowired private InteractionRepository interactionRepository;
    // Methods for logging interactions
}

@Service
public class OpportunityService {
    @Autowired private OpportunityRepository opportunityRepository;
    // Methods for tracking sales opportunities
}
@RestController
@RequestMapping("/api/customers")
public class CustomerController {
    @Autowired private CustomerService customerService;
    // CRUD APIs
}

@RestController
@RequestMapping("/api/interactions")
public class InteractionController {
    @Autowired private InteractionService interactionService;
    // CRUD APIs
}

@RestController
@RequestMapping("/api/opportunities")
public class OpportunityController {
    @Autowired private OpportunityService opportunityService;
    // CRUD APIs
}


   Explanation:

To execute this setup effectively, follow these steps:

1.Database Configuration:

Ensure your database (e.g., MySQL, PostgreSQL) is set up and running.
Update application.properties or application.yml with your database connection details.

2.Entity Classes:

Verify that your Customer, Contact, Opportunity, and Interaction classes are correctly annotated with @Entity.
Each entity should have a primary key (@Id) annotated with @GeneratedValue(strategy = GenerationType.IDENTITY) for auto-generation.

3.Repository Interfaces:

Confirm that CustomerRepository, ContactRepository, OpportunityRepository, and InteractionRepository extend JpaRepository.
These interfaces provide CRUD operations and other query methods out-of-the-box.

4.Service Classes:

CustomerService, InteractionService, and OpportunityService are annotated with @Service and autowire their respective repositories (@Autowired).
Implement business logic methods (add, edit, delete, view, etc.) in these services using repository methods.

5.Controller Classes:

CustomerController, InteractionController, and OpportunityController are annotated with @RestController and @RequestMapping for their respective API endpoints (/api/customers, /api/interactions, /api/opportunities).
Autowire the respective service classes (@Autowired) and implement CRUD APIs using HTTP methods (GET, POST, PUT, DELETE).

6.Testing and Deployment:

Test your APIs using tools like Postman or curl to verify functionality (create, read, update, delete operations).
Ensure error handling and validation are implemented where necessary.
Deploy your application to your preferred environment (e.g., local server, cloud platform).

7.Security Considerations:

Implement security measures such as authentication and authorization (e.g., using Spring Security) to protect your APIs from unauthorized access.

8.Monitoring and Maintenance:

Set up logging and monitoring to track API usage and errors.
Schedule regular maintenance to ensure database performance and application stability.
By following these steps, you can effectively set up and execute your Spring Boot application with CRUD operations for Customer, Interaction, and Opportunity entities, organized into separate layers (controller, service, repository) for clear separation of concerns and maintainability.
