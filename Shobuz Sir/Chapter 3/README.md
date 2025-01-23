## Steps to Set Up the Database Using XAMPP

### 1. **Start XAMPP**
   - Open the XAMPP Control Panel.
   - Start the `Apache` and `MySQL` services by clicking the "Start" button next to each.

### 2. **Access phpMyAdmin**
   - Open your web browser and go to `http://localhost/phpmyadmin`.

### 3. **Create a Database**
   - In phpMyAdmin, click on the "Databases" tab.
   - Enter the database name as `VERSITY1` and click "Create."

### 4. **Import `drop.sql`**
   - Select the `VERSITY1` database from the left sidebar.
   - Click on the "Import" tab.
   - Click "Choose File" and select the `drop.sql` file from your computer.
   - Scroll down and click "Go" to import the file.

### 5. **Import `largeRelationsInsertFile.sql`**
   - Ensure you are still in the `VERSITY1` database.
   - Click on the "Import" tab again.
   - Click "Choose File" and select the `largeRelationsInsertFile.sql` file.
   - Scroll down and click "Go" to import the file.

### 6. **Verify the Import**
   - After importing, check the tables in the `VERSITY1` database to ensure the data has been imported correctly.

---

## Notes
- Ensure that the SQL files (`drop.sql` and `largeRelationsInsertFile.sql`) are located in a directory you can easily access.
- If you encounter any errors during the import process, check the SQL files for syntax issues or compatibility problems.

---

Save this as `README.txt` and place it in your project directory for future reference. Let me know if you need further assistance!