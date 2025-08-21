sql code 
- This script will create the necessary tables for the marketing campaign application.
-- It is designed for PostgreSQL.

-- Drop tables if they exist to start with a clean slate
DROP TABLE IF EXISTS Interactions;
DROP TABLE IF EXISTS Customers;
DROP TABLE IF EXISTS Campaigns;

-- Create the Campaigns table
CREATE TABLE Campaigns (
    campaign_id SERIAL PRIMARY KEY,
    campaign_name VARCHAR(255) NOT NULL,
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    budget DECIMAL(12, 2)
);

-- Create the Customers table
CREATE TABLE Customers (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    age INT,
    city VARCHAR(100)
);

-- Create the Interactions table with foreign key constraints
CREATE TABLE Interactions (
    interaction_id SERIAL PRIMARY KEY,
    customer_id INT NOT NULL,
    campaign_id INT NOT NULL,
    interaction_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    conversion_status BOOLEAN NOT NULL DEFAULT FALSE,
    revenue_generated DECIMAL(10, 2),
    CONSTRAINT fk_customer
        FOREIGN KEY(customer_id)
        REFERENCES Customers(customer_id)
        ON DELETE CASCADE, -- If a customer is deleted, their interactions are also deleted
    CONSTRAINT fk_campaign
        FOREIGN KEY(campaign_id)
        REFERENCES Campaigns(campaign_id)
        ON DELETE CASCADE -- If a campaign is deleted, its interactions are also deleted
);

-- Insert some sample data for demonstration
INSERT INTO Campaigns (campaign_name, start_date, end_date, budget) VALUES
('Q3 Loyalty Program', '2024-07-01', '2024-09-30', 50000.00),
('End of Summer Sale', '2024-08-15', '2024-08-31', 75000.50);

INSERT INTO Customers (name, email, age, city) VALUES
('Alice Johnson', 'alice.j@email.com', 28, 'New York'),
('Bob Smith', 'bob.smith@email.com', 35, 'Los Angeles');

INSERT INTO Interactions (customer_id, campaign_id, conversion_status, revenue_generated) VALUES
(1, 1, TRUE, 150.75),
(2, 2, TRUE, 300.00),
(1, 2, FALSE, 0.00);# marketing-
