***Here’s how to implement the project step by step:***

---

### **Step 1: Set Up the Environment**
1. **Install Python and Required Libraries**:  
   - Install Python3.
   - Use `pip` to install required libraries: Like rich, flask, etc,.
     ```bash
     pip install flask mysql-connectoR
     ```

2. **Install MySQL Database**:  
   - Install MySQL Server and configure it with a root user.

3. **Create a Database and Tables**:  
   - Log in to MySQL and create a database named `Eshop`:
     ```sql
     CREATE DATABASE Eshop
     DEFAULT CHARACTER SET = 'utf8mb4';
     ```
   - Create the `Customer` and `Empt` tables with sample data:
     ```sql
     USE Eshop;
     CREATE TABLE Customer(id INT, name VARCHAR(250), mail VARCHAR(250));
     INSERT INTO Customer (id, name, mail) VALUES (1,'kavi','kavi@gmail'), (2,'kavina','kavina@gmail');
     SELECT * FROM Customer;
     CREATE Table Empt(statement VARCHAR(250));
     INSERT INTO Empt(statement) VALUES('NO DATA FOUND');
     SELECT * FROM Empt;
     ```

---

### **Step 2: Create the Backend Code**
1. **Create `dbservice.py`**:
   - Write code to handle database connection, data retrieval, and formatting:
     ```python
     import mysql.connector
      from rich import print
      
      def createConnection():
          conn = mysql.connector.connect(
              host = "localhost",     port = 3306,          user="Kavina",        
              password="Kavina@25",   database = "Eshop",   auth_plugin='mysql_native_password')
          conn.autocommit = True
          return conn
      
      def formatter(cursor, data):  
          result = []
          for row in data:
              row_dict = {}
              for idx, column in enumerate(cursor.description):
                  row_dict[column[0]] = row[idx]
              result.append(row_dict)
          return result
     
      def fetchDataDB(id):
          print(id)
          conn = createConnection()
          cursor = conn.cursor()
          if int(id)<=2 and int(id)<=1:
              cursor.execute(f"select * from Customer where id = {id}")
          else:
              cursor.execute(f"select * from Empt")
          data = cursor.fetchall()
          return formatter(cursor, data)

     ```

2. **Create `app.py`**:
   - Set up the Flask app and define an endpoint:
     ```python
     from flask import Flask, request
      from dbservice import fetchDataDB
      
      app = Flask(__name__)
      
      # Query Params
      @app.route("/fetchData", methods=["GET"])
      def hello() :
          id = request.args.get('id')
          print(id)
          return fetchDataDB(id)
      
      # Path Params
      # @app.route("/fetchData/<id>", methods=["GET"])
      # def hello(id) :
      #     print(id)
      #     return fetchData(id)

      if __name__=="__main__":
          app.run(port=5000, host="0.0.0.0", debug=True)
    
     ```

---

### **Step 3: Run the Application**
1. Start the Flask app:
   ```bash
   python app.py
   ```

2. Access the API at:  
   ```
   http://127.0.0.1:5000/fetchData
   ```

---

### **Step 4: Test Using Postman**
1. Open Postman and create a new GET request.
2. Enter the URL, e.g., `http://127.0.0.1:5000/fetchData?id=1`.
3. Send the request and view the JSON response.  

---

### **Step 5: Improve Security**
- Update SQL queries to use parameterized queries to prevent SQL injection:
  ```python
  cursor.execute("SELECT * FROM Customer WHERE id = %s", (id,))
  ```

---
