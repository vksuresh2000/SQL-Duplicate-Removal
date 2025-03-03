SQL Duplicate Removal

This repository provides SQL scripts to find and remove duplicate records in a database efficiently.

📚 Table of Contents

Finding Duplicates

Deleting Duplicates While Keeping One

Removing Duplicates and Keeping Only Unique Rows

How to Use

Contributing

🔍 Finding Duplicates

To check for duplicate records based on specific columns:

```SELECT column1, column2, COUNT(*)  
FROM table_name  
GROUP BY column1, column2  
HAVING COUNT(*) > 1;```

✅ Explanation:

Groups records by column1 and column2.

HAVING COUNT(*) > 1 filters duplicate rows.

🛢️ Deleting Duplicates While Keeping One

Method 1: Using MIN(id)

```DELETE FROM table_name  
WHERE id NOT IN (  
    SELECT MIN(id)  
    FROM table_name  
    GROUP BY column1, column2  
);```

Method 2: Using ROW_NUMBER() (Recommended)

```WITH CTE AS (  
    SELECT *, ROW_NUMBER() OVER (PARTITION BY column1, column2 ORDER BY id) AS rn  
    FROM table_name  
)  
DELETE FROM table_name WHERE id IN (  
    SELECT id FROM CTE WHERE rn > 1  
);```

✅ Explanation:

ROW_NUMBER() assigns a number to each duplicate.

Only records where rn > 1 are deleted.

🔄 Removing Duplicates and Keeping Only Unique Rows

If you want to remove duplicates completely, use:

```CREATE TABLE temp_table AS  
SELECT DISTINCT * FROM table_name;```

```DROP TABLE table_name;```

ALTER TABLE temp_table RENAME TO table_name;

✅ Explanation:

Creates a new table with unique records.

Deletes the old table.

Renames the new table back to the original name.

🚀 How to Use

Clone this repository:

git clone https://github.com/your-username/sql-duplicate-removal.git

Navigate into the project:

cd sql-duplicate-removal

Open remove_duplicates.sql and execute the script in your SQL database.

🤝 Contributing

Feel free to submit a pull request if you have any improvements or suggestions!

