# sql-query-task4
-- Query 1: Select specific columns from PelicanStore
SELECT
    Customer,
    Sales,
    'Method of Payment'
FROM
    PelicanStore;
    
-- Query 2: Transactions with Sales > $75
SELECT
    Customer,
    Sales,
    "Method of Payment",
    Gender
FROM
    PelicanStore
WHERE
    Sales > 75.00;

-- Query 3: Order by Sales (descending)
SELECT
    Customer,
    Sales,
    Items,
    Discount
FROM
    PelicanStore
ORDER BY
    Sales DESC;

-- Query 4: Count transactions per Method of Payment
SELECT
    "Method of Payment",
    COUNT(Customer) AS NumberOfTransactions
FROM
    PelicanStore
GROUP BY
    "Method of Payment"
ORDER BY
    NumberOfTransactions DESC;

-- Query 5: Average Age and Total Discount by Gender
SELECT
    Gender,
    AVG(Age) AS AverageAge,
    SUM(Discount) AS TotalDiscount
FROM
    PelicanStore
GROUP BY
    Gender;

-- Query 6: Sales greater than overall average sales
SELECT
    Customer,
    Sales,
    "Method of Payment"
FROM
    PelicanStore
WHERE
    Sales > (SELECT AVG(Sales) FROM PelicanStore)
ORDER BY
    Sales DESC;

-- Query 7: Method of Payment with highest average Discount
SELECT
    "Method of Payment"
FROM
    PelicanStore
GROUP BY
    "Method of Payment"
HAVING
    AVG(Discount) = (
        SELECT
            MAX(AvgDiscount)
        FROM (
            SELECT
                AVG(Discount) AS AvgDiscount
            FROM
                PelicanStore
            GROUP BY
                "Method of Payment"
        ) AS AvgDiscountsPerMethod
    );

-- Query 8: Total Sales
SELECT
    SUM(Sales) AS TotalSales
FROM
    PelicanStore;

-- Query 9: Max and Min Age
SELECT
    MAX(Age) AS MaximumAge,
    MIN(Age) AS MinimumAge
FROM
    PelicanStore;

-- View 1: Sales Summary by Marital Status
CREATE VIEW SalesSummaryByMaritalStatus AS
SELECT
    "Marital Status",
    SUM(Sales) AS TotalSales,
    SUM(Items) AS TotalItemsSold,
    COUNT(Customer) AS NumberOfCustomers
FROM
    PelicanStore
GROUP BY
    "Marital Status"
ORDER BY
    TotalSales DESC;

-- To query the view:
-- SELECT * FROM SalesSummaryByMaritalStatus;

-- View 2: Customer Transaction Details
CREATE VIEW CustomerTransactionDetails AS
SELECT
    Customer,
    Gender,
    Age,
    "Marital Status",
    "Method of Payment",
    Sales,
    Discount,
    Items
FROM
    PelicanStore
ORDER BY
    Customer, Sales DESC;

-- To query the view:
-- SELECT * FROM CustomerTransactionDetails WHERE Sales > 50;

-- Index 1: On Sales, for queries filtering or ordering by sales
CREATE INDEX idx_pelicanstore_sales ON PelicanStore (Sales);

-- Index 2: On Method of Payment, for grouping or filtering by payment method
CREATE INDEX idx_pelicanstore_method_of_payment ON PelicanStore ("Method of Payment");

-- Index 3: On Gender, for grouping or filtering by gender
CREATE INDEX idx_pelicanstore_gender ON PelicanStore (Gender);

-- Index 4: On Customer, if you frequently look up individual customer transactions
CREATE INDEX idx_pelicanstore_customer ON PelicanStore (Customer);

-- Index 5: On Age, for filtering or grouping by age
CREATE INDEX idx_pelicanstore_age ON PelicanStore (Age);
