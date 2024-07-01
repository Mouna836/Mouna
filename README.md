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
   
    



