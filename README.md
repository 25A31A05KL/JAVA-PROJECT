🌾 RythuMitra - Smart Farmer Assistant
RythuMitra is a Java Swing based desktop application designed to help farmers manage their agricultural activities efficiently. The application provides crop management, expense tracking, market price monitoring, task reminders, and financial reporting in a simple and user-friendly interface.

🚀 Features
🌱 Crop Management
Add and manage crop details
Store:
Crop Name
Land Area
Sowing Date
Expected Harvest Date
Expected Yield
💰 Expense Tracking
Record farming expenses
Expense categories:
Seeds
Fertilizer
Labour
Irrigation
Transport
Machinery
Pesticides
Other
Track total farm expenditure
📈 Market Price Monitoring
Store crop market prices
Record:
Crop Name
Market Name
Price per Quintal
Date
⏰ Reminders
Add farming-related tasks
Mark tasks as completed
Automatically identify:
Pending tasks
Completed tasks
Overdue tasks
📊 Dashboard
Displays:

Total Crops
Total Expenses
Pending Reminders
📄 Financial Reports
Generate farm reports containing:

Crop information
Expense summary
Market price records
Reminder status
💾 Data Persistence
The application automatically saves records using Java Serialization.

Data is stored in:

UserHome/RythuMitra/data.ser
and is restored automatically when the application starts.

🛠 Technologies Used
Java
Swing GUI
AWT
Java Collections Framework
Serialization
LocalDate API
📂 Project Structure
RythuMitra
│
├── Crop
├── Expense
├── MarketPrice
├── Reminder
├── AppData
│
└── RythuMitra.java
▶️ How to Run
Step 1: Compile
javac RythuMitra.java
Step 2: Run
java RythuMitra
🖥 System Requirements
Java JDK 8 or higher
Windows/Linux/macOS
Minimum 4 GB RAM
📸 Main Modules
Dashboard
Provides an overview of farm activities and statistics.

Crop Management
Manage crop information and expected production.

Expense Management
Track and analyze farming expenses.

Market Prices
Maintain crop market price history.

Reminders
Schedule important farming activities.

Reports
Generate a financial overview of farm operations.

✅ Validation Included
The application validates:

Empty fields
Invalid dates
Negative values
Invalid numeric inputs
to ensure accurate data entry.

Future Enhancements
Weather Forecast Integration
SMS Notifications
PDF Report Export
Database Support (MySQL)
Multi-language Support (English & Telugu)
Farmer Analytics Dashboard
Crop Recommendation System
Author
Developed as a Smart Agriculture Management System project using Java Swing.

🌾 "Empowering Farmers with Smart Digital Solutions"
