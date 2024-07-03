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
1. Database Schema Design
The database schema includes four tables:

customers: Stores customer information.
contacts: Stores contact information related to customers.
opportunities: Stores sales opportunities related to customers.
interactions: Stores interaction records with customers   

2. API Development with Spring Boot
The Spring Boot application includes the following components:

Entities: Define the structure of the data stored in the database.
Repositories: Provide CRUD operations for each entity.
Controllers: Define RESTful APIs for managing customer records, contacts, opportunities, and interactions.
Entity Classes
Customer: Represents a customer in the CRM.
Contact: Represents a contact related to a customer.
Opportunity: Represents a sales opportunity related to a customer.
Interaction: Represents an interaction with a customer.

3. Frontend Development with React
The React application includes the following components:

CustomerList.js: Displays a list of customers.
CustomerForm.js: Provides a form to add or edit a customer.
Dashboard.js: Displays key metrics like the number of opportunities, stages, and customer interaction history.
CustomerList.js
Fetches and displays a list of customers.

4. Dashboard Component
Aggregates and displays key metrics.

Dashboard.js
Fetches and displays key metrics like the number of opportunities, stages, and customer interaction history.
This example sets up a basic CRM system
