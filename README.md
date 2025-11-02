[README.md](https://github.com/user-attachments/files/23289073/README.md)
# ExecutionCoach

**ExecutionCoach** is a console-based C++ application (no UI) designed to analyze trading data from a CSV file using predefined rules (`rules.conf`) and store the results in a PostgreSQL database.

---

## ⚙️ System Requirements
- **C++ Version:** C++17 or higher  
- **Database:** PostgreSQL (recommended version 14 or above)  
- **Library:** libpqxx (C++ interface for PostgreSQL)  
- **Operating System:** Windows  
- **Compiler Options:**  
  - Windows → `g++` (MinGW) or Microsoft Visual Studio (MSVC)

---

## 🚀 Installation & Setup

### Step 1 — Install PostgreSQL
1. Download PostgreSQL for Windows:  
   https://www.postgresql.org/download/windows/
2. During installation:  
   - Remember your **username** (default: `postgres`)  
   - Remember your **password**  
   - Select **pgAdmin** when asked to install it.  
3. After installation, verify PostgreSQL is running by:  
   - Opening **pgAdmin**, or  
   - Running this command in Command Prompt:  
     ```bash
     psql -U postgres -l
     ```

---

### Step 2 — Verify Folder Structure
Make sure your folder looks like this:

```
C:\ExecutionCoach\
├── ExecutionCoach.exe
├── pqxx.dll
├── libpq.dll
├── libssl-3-x64.dll
├── libcrypto-3-x64.dll
├── rules.conf
├── schema.sql
└── trades.csv
```

**Important:** All required DLLs must be in the same folder as `ExecutionCoach.exe`.

---

### Step 3 — (Optional) Build the Project Yourself
> Skip this step if you already received a compiled folder with `ExecutionCoach.exe` and all required DLLs.  
> This step is only needed if you have the source code (`.cpp` files) and want to compile it manually.

If you have `g++` installed, compile using:
```bash
g++ -std=c++17 -I"path\to\libpqxx\include" -L"path\to\libpqxx\lib" *.cpp -lpqxx -lpq -o ExecutionCoach.exe
```

Or build it with **Visual Studio**, making sure to configure include and linker paths for `libpqxx`.

---

### Step 4 — Prepare the Database (Required First Time Only)
Before running the program, create the required table by executing `schema.sql` in PostgreSQL:
```bash
psql -U postgres -f schema.sql
```

Run this **only once** — it creates the database table.  
If the table already exists, you can skip this step.

---

### Step 5 — Run the Application
Open **Command Prompt**, navigate to the folder where `ExecutionCoach.exe` is located, and run:

```bash
ExecutionCoach.exe "C:\path\to\trades.csv" "C:\path\to\rules.conf" "host=localhost port=5432 dbname=postgres user=postgres password=YOUR_PASSWORD"
```

**Example:**
```bash
ExecutionCoach.exe "trades.csv" "rules.conf" "host=localhost port=5432 dbname=postgres user=postgres password=MyPassword123"
```

---

## ✅ Output Example
When executed correctly, the console will display:

```
Connected to: postgres

=== Execution Summary ===
Processed trades: 120
Parser errors:    0
DB write errors:  0

Decisions:
OK          : 80
WARN        : 25
ADJUST_QTY  : 10
BLOCK       : 5
==========================
```

---

## 🧰 Troubleshooting

| Problem | Cause | Solution |
|----------|--------|-----------|
| **pqxx.dll not found** | Missing DLL files | Ensure all DLLs are in the same directory as `ExecutionCoach.exe`. |
| **Connection failed** | Wrong database credentials | Check username, password, and port in the connection string. |
| **relation "trades" does not exist** | `schema.sql` not executed | Run `psql -U postgres -f schema.sql`. |
| **PostgreSQL not running** | Service stopped or blocked by firewall | Restart PostgreSQL or allow it through Windows Firewall. |

