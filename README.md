# Lab 9 - Product Management System (ASP.NET Core MVC)

A simple **Product Management System** built for Lab 9. The application performs full **CRUD** (Create, Read, Update, Delete) operations on a `products` table stored in **SQL Server**, using **ASP.NET Core MVC** with Razor views and **Bootstrap 5**.

Data access is implemented with **ADO.NET** (`Microsoft.Data.SqlClient`) inside a dedicated repository class. All queries are parameterized, and POST actions are protected with anti-forgery tokens.

## Features

- List all products with image preview, price, quantity, and status
- Add a new product with server-side validation
- Edit an existing product
- Delete a product with a confirmation page
- Success notifications after each operation (`TempData`)
- Responsive Bootstrap 5 UI with validation scripts
- Secure, parameterized SQL queries to prevent SQL injection

## Tech Stack

| Layer          | Technology                                        |
| -------------- | ------------------------------------------------- |
| Framework      | ASP.NET Core MVC (.NET 8)                         |
| Language       | C#                                                |
| Database       | SQL Server / LocalDB                              |
| Data Access    | ADO.NET (`Microsoft.Data.SqlClient` 7.1.1)        |
| Front End      | Razor views, Bootstrap 5, jQuery Validation       |
| IDE            | Visual Studio 2022 (project runs with `dotnet`)   |

## Project Structure

```
Lab-09/
├── Lab9/
│   ├── Controllers/
│   │   ├── HomeController.cs          # Home and Privacy pages
│   │   └── ProductsController.cs      # CRUD actions for products
│   ├── Data/
│   │   └── ProductRepository.cs       # ADO.NET data access (CRUD)
│   ├── Models/
│   │   └── Product.cs                 # Product entity + validation rules
│   ├── Views/
│   │   ├── Products/                  # Index, Create, Edit, Delete
│   │   └── Shared/                    # Layout and partial views
│   ├── wwwroot/                       # CSS, JS, Bootstrap, jQuery
│   ├── Program.cs                     # App startup and DI registration
│   ├── appsettings.json               # Connection string
│   └── pms_mvc_example.csproj         # Project file (.NET 8)
├── sql_script.txt                     # Lab notes and database table script
├── .gitignore
└── README.md
```

## Product Model

Each product has the following fields:

| Field       | Type    | Rules                          |
| ----------- | ------- | ------------------------------ |
| Id          | int     | Primary key, auto-increment    |
| Title       | string  | Required, max 50 characters    |
| Description | string  | Required, max 50 characters    |
| Image       | string  | Required, max 255 characters   |
| Price       | int     | 0 or greater                   |
| Quantity    | int     | 0 or greater                   |
| Status      | string  | Required, max 150 characters   |

## Database Setup

1. Create the `pmsdb` database in SQL Server LocalDB (skip if it already exists):

   ```sql
   CREATE DATABASE pmsdb;
   ```

2. Create the `products` table:

   ```sql
   CREATE TABLE [dbo].[products] (
       [Id]          INT           IDENTITY (1, 1) NOT NULL,
       [Title]       VARCHAR (50)  NOT NULL,
       [description] VARCHAR (50)  NOT NULL,
       [image]       VARCHAR (255) NOT NULL,
       [price]       INT           NOT NULL,
       [quantity]    INT           NOT NULL,
       [status]      VARCHAR (150) NOT NULL,
       PRIMARY KEY CLUSTERED ([Id] ASC)
   );
   ```

   You can also execute this using `sqlcmd`:

   ```powershell
   sqlcmd -S "(localdb)\MSSQLLocalDB" -Q "CREATE DATABASE pmsdb;"
   sqlcmd -S "(localdb)\MSSQLLocalDB" -d pmsdb -Q "CREATE TABLE [dbo].[products] ([Id] INT IDENTITY (1, 1) NOT NULL, [Title] VARCHAR (50) NOT NULL, [description] VARCHAR (50) NOT NULL, [image] VARCHAR (255) NOT NULL, [price] INT NOT NULL, [quantity] INT NOT NULL, [status] VARCHAR (150) NOT NULL, PRIMARY KEY CLUSTERED ([Id] ASC));"
   ```

Note: `sql_script.txt` is the lab reference sheet and contains notes alongside the SQL, so run only the SQL statements shown above.

## Configuration

The connection string is defined in `Lab9/appsettings.json`:

```json
"ConnectionStrings": {
  "DefaultConnection": "Data Source=(LocalDB)\\MSSQLLocalDB;Initial Catalog=pmsdb;Integrated Security=True;Pooling=False;Encrypt=True;Trust Server Certificate=True"
}
```

Update it if your SQL Server instance has a different name.

## Getting Started

### Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- SQL Server LocalDB (installed with Visual Studio 2022 or SQL Server Express)
- Visual Studio 2022 (optional)

### Run the application

```powershell
cd Lab9
dotnet restore
dotnet run
```

Then open one of the URLs shown in the console:

- http://localhost:5080
- https://localhost:7080

To open the project in Visual Studio, open `Lab9/pms_mvc_example.csproj`.

## Routes

| Method | Route                    | Description               |
| ------ | ------------------------ | ------------------------- |
| GET    | `/Products`              | List all products         |
| GET    | `/Products/Create`       | Show create form          |
| POST   | `/Products/Create`       | Save new product          |
| GET    | `/Products/Edit/{id}`    | Show edit form            |
| POST   | `/Products/Edit/{id}`    | Save product changes      |
| GET    | `/Products/Delete/{id}`  | Show delete confirmation  |
| POST   | `/Products/Delete/{id}`  | Delete the product        |

## How It Works

1. **Controller** - `ProductsController` handles incoming requests, validates the model state, and returns the appropriate views.
2. **Repository** - `ProductRepository` opens a SQL connection using the configured connection string and runs parameterized queries for each operation.
3. **Model** - `Product` uses data annotations (`[Required]`, `[StringLength]`, `[Range]`) to validate user input on both the client and server.
4. **Views** - Razor views in `Views/Products` render the product table and forms, with Bootstrap styling and jQuery validation.

The repository is registered with dependency injection in `Program.cs` as a scoped service.

## Author

Student Project — Lab 9, ASP.NET Core MVC.
