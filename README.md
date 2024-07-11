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
Sure! Let's break down and describe each part of the provided example for a Customer Relationship Management (CRM) system.

1. Database Schema Design
The database schema includes four tables:

customers: Stores customer information.
contacts: Stores contact information related to customers.
opportunities: Stores sales opportunities related to customers.
interactions: Stores interaction records with customers.
SQL Schema

CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    phone VARCHAR(20),
    address TEXT
);

CREATE TABLE contacts (
    id SERIAL PRIMARY KEY,
    customer_id INT NOT NULL,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    phone VARCHAR(20),
    FOREIGN KEY (customer_id) REFERENCES customers(id) ON DELETE CASCADE
);

CREATE TABLE opportunities (
    id SERIAL PRIMARY KEY,
    customer_id INT NOT NULL,
    description TEXT NOT NULL,
    stage VARCHAR(50) NOT NULL,
    amount DECIMAL(10, 2),
    FOREIGN KEY (customer_id) REFERENCES customers(id) ON DELETE CASCADE
);

CREATE TABLE interactions (
    id SERIAL PRIMARY KEY,
    customer_id INT NOT NULL,
    type VARCHAR(50) NOT NULL,
    date TIMESTAMP NOT NULL,
    notes TEXT,
    FOREIGN KEY (customer_id) REFERENCES customers(id) ON DELETE CASCADE
);
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
java

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
Repositories
Provide CRUD operations for each entity.
JAVA

public interface CustomerRepository extends JpaRepository<Customer, Long> {}
public interface ContactRepository extends JpaRepository<Contact, Long> {}
public interface OpportunityRepository extends JpaRepository<Opportunity, Long> {}
public interface InteractionRepository extends JpaRepository<Interaction, Long> {}

Controllers
Define RESTful APIs for managing customer records.
JAVA

@RestController
@RequestMapping("/customers")
public class CustomerController {
    @Autowired
    private CustomerRepository customerRepository;

    @GetMapping
    public List<Customer> getAllCustomers() {
        return customerRepository.findAll();
    }

    @PostMapping
    public Customer createCustomer(@RequestBody Customer customer) {
        return customerRepository.save(customer);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Customer> updateCustomer(@PathVariable Long id, @RequestBody Customer customerDetails) {
        Customer customer = customerRepository.findById(id)
                .orElseThrow(() -> new ResourceNotFoundException("Customer not found for this id :: " + id));

        customer.setName(customerDetails.getName());
        customer.setEmail(customerDetails.getEmail());
        customer.setPhone(customerDetails.getPhone());
        customer.setAddress(customerDetails.getAddress());
        final Customer updatedCustomer = customerRepository.save(customer);
        return ResponseEntity.ok(updatedCustomer);
    }

    @DeleteMapping("/{id}")
    public Map<String, Boolean> deleteCustomer(@PathVariable Long id) {
        Customer customer = customerRepository.findById(id)
                .orElseThrow(() -> new ResourceNotFoundException("Customer not found for this id :: " + id));

        customerRepository.delete(customer);
        Map<String, Boolean> response = new HashMap<>();
        response.put("deleted", Boolean.TRUE);
        return response;
    }
}

3. Frontend Development with React
The React application includes the following components:

CustomerList.js: Displays a list of customers.
CustomerForm.js: Provides a form to add or edit a customer.
Dashboard.js: Displays key metrics like the number of opportunities, stages, and customer interaction history.
CustomerList.js
Fetches and displays a list of customers.

javascript

import React, { useState, useEffect } from 'react';
import axios from 'axios';

function CustomerList() {
    const [customers, setCustomers] = useState([]);

    useEffect(() => {
        axios.get('/customers')
            .then(response => {
                setCustomers(response.data);
            })
            .catch(error => {
                console.error("There was an error fetching the customers!", error);
            });
    }, []);

    return (
        <div>
            <h2>Customer List</h2>
            <ul>
                {customers.map(customer => (
                    <li key={customer.id}>{customer.name}</li>
                ))}
            </ul>
        </div>
    );
}

export default CustomerList;
CustomerForm.js
Provides a form to add or edit a customer.


//HTM
<!DOCTYPE html>
<html>
<head>
    <title>CRM System</title>
    <script>
        function loadCustomers() {
            fetch('/api/customers')
                .then(response => response.json())
                .then(data => {
                    let customerList = document.getElementById('customer-list');
                    customerList.innerHTML = '';
                    data.forEach(customer => {
                        let listItem = document.createElement('li');
                        listItem.textContent = customer.name;
                        customerList.appendChild(listItem);
                    });
                });
        }

        document.addEventListener('DOMContentLoaded', loadCustomers);
    </script>
</head>
<body>
    <h1>CRM System</h1>
    <h2>Customers</h2>
    <ul id="customer-list"></ul>
</body>
</html>

Explanation:

1. Database Schema Design
Tables:
Customers:

customer_id (Primary Key)
name
email
phone
address
created_at
updated_at
Contacts:

contact_id (Primary Key)
customer_id (Foreign Key to Customers)
name
email
phone
position
created_at
updated_at
Opportunities:

opportunity_id (Primary Key)
customer_id (Foreign Key to Customers)
name
amount
stage (e.g., prospecting, negotiation, closed-won, closed-lost)
close_date
created_at
updated_at
Interactions:

interaction_id (Primary Key)
customer_id (Foreign Key to Customers)
contact_id (Foreign Key to Contacts, optional)
type (call, meeting, email)
description
interaction_date
created_at
updated_at
Relationships:
Customers to Contacts: One-to-Many (One customer can have multiple contacts).
Customers to Opportunities: One-to-Many (One customer can have multiple opportunities).
Customers to Interactions: One-to-Many (One customer can have multiple interactions).
2. RESTful API Development
API Endpoints:
Customers:

GET /api/customers: Get all customers.
GET /api/customers/{customer_id}: Get a specific customer.
POST /api/customers: Create a new customer.
PUT /api/customers/{customer_id}: Update a customer.
DELETE /api/customers/{customer_id}: Delete a customer.
Contacts:

GET /api/contacts/{customer_id}: Get contacts for a specific customer.
POST /api/contacts/{customer_id}: Create a new contact for a customer.
PUT /api/contacts/{contact_id}: Update a contact.
DELETE /api/contacts/{contact_id}: Delete a contact.
Opportunities:

GET /api/opportunities/{customer_id}: Get opportunities for a specific customer.
POST /api/opportunities/{customer_id}: Create a new opportunity for a customer.
PUT /api/opportunities/{opportunity_id}: Update an opportunity.
DELETE /api/opportunities/{opportunity_id}: Delete an opportunity.
Interactions:

GET /api/interactions/{customer_id}: Get interactions for a specific customer.
POST /api/interactions/{customer_id}: Log a new interaction for a customer.
PUT /api/interactions/{interaction_id}: Update an interaction.
DELETE /api/interactions/{interaction_id}: Delete an interaction.
3. Frontend Interface
Functionality:
Customer Management: CRUD operations for customers and their contacts.
Interaction Logging: Form to log interactions (calls, meetings, emails).
Opportunity Tracking: CRUD operations for opportunities, displaying stages and close dates.
Dashboard: Display key metrics (number of opportunities, stages breakdown, interaction history).
Technologies:
Frameworks: Use frameworks like React.js for frontend development.
UI Libraries: Bootstrap or Material UI for responsive design.
API Integration: Axios or Fetch API for consuming RESTful APIs.




