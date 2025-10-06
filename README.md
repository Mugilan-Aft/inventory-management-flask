# **Flask-based Inventory Management System**

A feature-rich web application for managing inventory, offering seamless user authentication, comprehensive product and stock management, and detailed reporting.

## **Key Features**

### **User Management**
- **Authentication**  
  - Secure login & registration system  
  - Password encryption with **Werkzeug**  
  - Session handling via **Flask-Login**  
  - Admin functionalities for privileged users

### **Product & Inventory Management**
- **Product Operations**  
  - Full CRUD (Create, Read, Update, Delete) capabilities  
  - Categorize products for better organization  
  - Unique identification with SKU  
  - Link products to suppliers  
  - Low stock notifications  
  - Search and filtering features  
  - Pagination support for large datasets

- **Stock Management**  
  - Track stock movements: **IN** and **OUT**  
  - Real-time stock quantity updates  
  - Maintain a transaction log  
  - Automatic alerts for low stock  
  - Set up stock quantity thresholds for notifications

- **Categories & Suppliers**  
  - Categorize products for easy navigation  
  - Store and manage supplier details  
  - Supplier contact and address management  
  - Track supplier relationships efficiently

### **Reporting & Insights**
- **Dashboard Analytics**  
  - Real-time stock data and performance metrics  
  - Display total inventory value  
  - View low-stock items  
  - See top-performing products by value  
  - Category-specific stock insights  
  - Export reports as CSV files

### **Technical Stack**
- **Frontend**: Responsive UI with **Bootstrap 5**  
- **Backend**: **Flask** with RESTful API architecture  
- **ORM**: **SQLAlchemy** for smooth database management  
- **Forms**: Handled with **Flask-WTF**  
- **CSRF Protection**: Included by default  
- **Database Migrations**: Managed using **Flask-Migrate**

## **Getting Started**

### **Prerequisites**
- Python 3.8+  
- pip for package installation  
- Virtual Environment (recommended for isolation)

### **Installation Instructions**

1. **Clone the Project**  
   Run the following in your terminal:
   ```bash
   git clone https://github.com/your-repo/inventory-management-flask.git
   cd inventory-management-flask

2. **Set Up Virtual Environment**
   For **Windows**:

   ```bash
   python -m venv venv
   venv\Scripts\activate
   ```

   For **Linux/Mac**:

   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install Dependencies**
   Use `pip` to install required packages:

   ```bash
   pip install -r requirements.txt
   ```

4. **Configure Environment Variables**
   Create a `.env` file based on `.env.example`:

   ```bash
   cp .env.example .env
   ```

   Then edit the `.env` file with your settings:

   ```env
   SECRET_KEY=your-secret-key
   DATABASE_URL=sqlite:///inventory.db
   FLASK_APP=run.py
   FLASK_ENV=development
   ```

5. **Initialize the Database**
   In Python shell:

   ```python
   >>> from app import create_app, db
   >>> app = create_app()
   >>> with app.app_context():
   >>>     db.create_all()
   >>> exit()
   ```

   Or just run the app, which will create the database automatically:

   ```bash
   python run.py
   ```

### **Running the Application**

To start the development server:

```bash
python run.py
```

The app will be accessible at [http://localhost:5000](http://localhost:5000).

### **Default Admin Login**

* Username: `admin`
* Password: `admin123`

**Note:** Don’t forget to change the admin credentials after your first login, especially for production!

---

## **Project Directory Structure**

```plaintext
inventory-management-flask/
│
├── app/
│   ├── __init__.py               # Application setup
│   ├── models.py                 # Database models
│   ├── forms.py                  # Form handling
│   ├── routes/                   # Blueprint routes
│   ├── templates/                # Jinja2 HTML templates
│   └── static/                   # Static assets (CSS, JS)
├── config.py                     # App configuration
├── requirements.txt              # List of dependencies
├── run.py                        # App entry point
├── .env.example                  # Environment variables template
└── README.md                     # Project documentation
```

---

## **Database Schema Overview**

### **User**

* `id`, `username`, `email`, `password_hash`, `is_admin`, `created_at`

### **Category**

* `id`, `name`, `description`, `created_at`

### **Supplier**

* `id`, `name`, `contact_person`, `email`, `phone`, `address`, `created_at`

### **Product**

* `id`, `name`, `sku`, `description`, `quantity`, `min_quantity`, `unit_price`, `category_id`, `supplier_id`, `created_at`, `updated_at`

### **StockTransaction**

* `id`, `product_id`, `user_id`, `transaction_type`, `quantity`, `unit_price`, `notes`, `transaction_date`

---

## **Advanced Configuration Options**

### **Adjusting Low Stock Threshold**

To modify the low stock threshold (the point at which notifications are triggered), edit `config.py`:

```python
LOW_STOCK_THRESHOLD = 10
```

### **Pagination Control**

Set the number of items per page for lists:

```python
ITEMS_PER_PAGE = 10
```

### **Theme Customization**

Change the look of your app by modifying the primary theme colors in `static/css/style.css`:

```css
:root {
    --primary-color: #3498db;
    --secondary-color: #f39c12;
}
```

---

## **Usage Instructions**

### **Adding a New Product**

1. Go to **Products > Add Product**
2. Fill in the details:

   * Name, SKU (unique identifier), Description
   * Initial stock quantity, Minimum stock level
   * Price per unit
3. Select a category and supplier
4. Click **Save**

### **Managing Stock Levels**

* **To Add Stock**:

  * Navigate to **Stock > Add Stock**
  * Choose the product and input the quantity
  * Optionally include notes and price
  * Submit the form

* **To Remove Stock**:

  * Navigate to **Stock > Remove Stock**
  * Select product and specify quantity to remove
  * The system will validate sufficient stock
  * Submit the form

### **Generating Reports**

1. Go to **Reports**
2. View key insights (inventory value, top products, etc.)
3. Export to CSV:

   * **Products Report**
   * **Transactions Report**

### **Monitoring Low Stock**

* The dashboard provides low stock alerts
* View all low-stock products under **Products > Low Stock Alert**

---

## **Security Best Practices**

* All passwords are hashed using **Werkzeug**
* Form submissions are **CSRF-protected**
* **SQLAlchemy** prevents SQL injection
* Session-based authentication for secure login

---

## **Deployment Guide**

1. **Change Secret Key**:
   Update the `SECRET_KEY` in `.env` with a secure value.

2. **Disable Debug Mode**:
   In `config.py`, set:

   ```python
   DEBUG = False
   ```

3. **Set Up Production Database**:
   Configure PostgreSQL or MySQL as the production database and update `DATABASE_URL`.

4. **Use Gunicorn** for Production:
   Install Gunicorn and run the app:

   ```bash
   pip install gunicorn
   gunicorn -w 4 -b 0.0.0.0:5000 run:app
   ```

5. **Configure Reverse Proxy (Nginx/Apache)** and enable **SSL** for secure connections.

---

## **API Endpoints**

* **Authentication**

  * `POST /auth/login` - Login
  * `POST /auth/register` - Register
  * `GET /auth/logout` - Logout

* **Product Management**

  * `GET /products/` - List all products
  * `POST /products/create` - Create new product

* **Stock Management**

  * `GET /stock/` - View transactions
  * `POST /stock/add` - Add stock
  * `POST /stock/remove` - Remove stock

* **Reports**

  * `GET /reports/` - Dashboard
  * `GET /reports/export/products` - Export products to CSV

---

## **Contributing**

We welcome contributions! Follow these steps:

1. Fork the repo
2. Create a new branch
3. Make your changes
4. Commit and push
5. Submit a Pull Request

---

## **License**

This project is licensed under the MIT License.

---

## **Future Improvements**

* Barcode Scanning
* Automated email notifications for low stock
* Advanced Reporting with visualizations
* Multi-warehouse management
* Integrating mobile app
* Audit logs for transactions

---

**Crafted with ❤️ using Flask**

```

You can copy this markdown into a `.md` file, and it should display properly formatted when rendered in a
```


markdown viewer.
