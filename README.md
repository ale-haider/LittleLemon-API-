# Little Lemon API

A RESTful backend API for the **Little Lemon restaurant**, built with **Django**, **Django REST Framework (DRF)**, and **Djoser** by Ali.

The API allows customers to browse menu items, manage their cart, place orders, and view their orders. Managers can manage menu items, users, and orders, while delivery crew members can view and update their assigned orders.

## Features

### Authentication & User Management

* User registration
* User login and token generation
* View the currently authenticated user
* Role-based access using Django user groups
* Manager group
* Delivery crew group
* Customers are users who are not assigned to either group

### Menu Management

Customers and delivery crew can:

* Browse all menu items
* View individual menu items
* Browse menu items by category
* Search and filter menu items
* Sort menu items
* Paginate menu items

Managers can:

* Add menu items
* Update menu items
* Delete menu items
* View all menu items

### Cart Management

Customers can:

* Add menu items to their cart
* View their current cart
* Remove all items from their cart

Each customer can have their own cart items.

### Order Management

Customers can:

* Place orders from their cart
* View their own orders
* View individual orders

Managers can:

* View all customer orders
* Assign orders to delivery crew
* Update order status
* Delete orders

Delivery crew can:

* View orders assigned to them
* Update the delivery status of their assigned orders

## User Roles

| Role          | Permissions                                                |
| ------------- | ---------------------------------------------------------- |
| Customer      | Browse menu, manage cart, place orders, view own orders    |
| Manager       | Manage menu items, users, orders, and delivery assignments |
| Delivery Crew | View assigned orders and update delivery status            |
| Admin         | Manage users, groups, categories, and administrative tasks |

## API Endpoints

### Authentication

| Endpoint               | Method | Description                      |
| ---------------------- | ------ | -------------------------------- |
| `/api/users`           | POST   | Register a new user              |
| `/api/users/users/me/` | GET    | Get current authenticated user   |
| `/token/login/`        | POST   | Generate an authentication token |

### Menu Items

| Endpoint                     | Method    | Access                           |
| ---------------------------- | --------- | -------------------------------- |
| `/api/menu-items`            | GET       | Customer, Delivery Crew, Manager |
| `/api/menu-items`            | POST      | Manager                          |
| `/api/menu-items/{menuItem}` | GET       | Customer, Delivery Crew, Manager |
| `/api/menu-items/{menuItem}` | PUT/PATCH | Manager                          |
| `/api/menu-items/{menuItem}` | DELETE    | Manager                          |

### Manager Group

| Endpoint                             | Method | Description                          |
| ------------------------------------ | ------ | ------------------------------------ |
| `/api/groups/manager/users`          | GET    | View managers                        |
| `/api/groups/manager/users`          | POST   | Add a user to the manager group      |
| `/api/groups/manager/users/{userId}` | DELETE | Remove a user from the manager group |

### Delivery Crew Group

| Endpoint                                   | Method | Description                      |
| ------------------------------------------ | ------ | -------------------------------- |
| `/api/groups/delivery-crew/users`          | GET    | View delivery crew               |
| `/api/groups/delivery-crew/users`          | POST   | Add a user to delivery crew      |
| `/api/groups/delivery-crew/users/{userId}` | DELETE | Remove a user from delivery crew |

### Cart

| Endpoint               | Method | Description                   |
| ---------------------- | ------ | ----------------------------- |
| `/api/cart/menu-items` | GET    | View current user's cart      |
| `/api/cart/menu-items` | POST   | Add a menu item to the cart   |
| `/api/cart/menu-items` | DELETE | Clear the current user's cart |

### Orders

| Endpoint                | Method    | Access        | Description            |
| ----------------------- | --------- | ------------- | ---------------------- |
| `/api/orders`           | GET       | Customer      | View own orders        |
| `/api/orders`           | POST      | Customer      | Place an order         |
| `/api/orders/{orderId}` | GET       | Customer      | View a specific order  |
| `/api/orders`           | GET       | Manager       | View all orders        |
| `/api/orders/{orderId}` | PUT/PATCH | Manager       | Update order           |
| `/api/orders/{orderId}` | DELETE    | Manager       | Delete an order        |
| `/api/orders`           | GET       | Delivery Crew | View assigned orders   |
| `/api/orders/{orderId}` | PATCH     | Delivery Crew | Update delivery status |

## HTTP Status Codes

The API uses appropriate HTTP status codes for different operations:

| Status Code        | Meaning                                       |
| ------------------ | --------------------------------------------- |
| `200 OK`           | Successful GET, PUT, PATCH, or DELETE request |
| `201 Created`      | Successful POST request                       |
| `400 Bad Request`  | Invalid or failed validation                  |
| `401 Unauthorized` | Authentication failed                         |
| `403 Forbidden`    | User does not have permission                 |
| `404 Not Found`    | Requested resource does not exist             |

## Database Models

The project uses SQLite and includes the following main models:

### Category

Stores menu categories.

* `slug`
* `title`

The category title is indexed to support searching.

### MenuItem

Represents a menu item.

* `title`
* `price`
* `featured`
* `category`

Each menu item belongs to a category.

### Cart

Stores items temporarily before an order is placed.

* `user`
* `menuitem`
* `quantity`
* `unit_price`
* `price`

A user can only have one cart entry for a particular menu item.

### Order

Represents a customer's order.

* `user`
* `status`
* `total`
* `date`
* `delivery_crew`

The order status indicates whether the order has been delivered.

### OrderItem

Stores the individual menu items belonging to an order.

* `order`
* `menuitem`
* `quantity`
* `unit_price`
* `price`

An order can contain multiple menu items.

## Filtering, Searching, Pagination & Sorting

The API provides additional functionality for menu items and orders, including:

* Filtering
* Searching
* Pagination
* Sorting

For example, menu items can be sorted by price and filtered by category.

## Throttling

API throttling is applied to:

* Authenticated users
* Anonymous/unauthenticated users

This helps prevent excessive API requests.

## Technologies Used

* Python
* Django
* Django REST Framework
* Djoser
* SQLite
* Pipenv
* VS Code
* Insomnia / Browser for API testing

## Project Structure

```text
LittleLemon/
│
├── LittleLemon/
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
│
├── LittleLemonAPI/
│   ├── migrations/
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── serializers.py
│   ├── urls.py
│   ├── views.py
│   └── tests.py
│
├── db.sqlite3
├── manage.py
├── Pipfile
├── Pipfile.lock
└── notes.txt
```

## Installation

### 1. Clone the project

```bash
git clone 
cd LittleLemon
```

### 2. Install Pipenv

```bash
pip install pipenv
```

### 3. Create and activate the virtual environment

```bash
pipenv install
pipenv shell
```

### 4. Run migrations

```bash
python manage.py makemigrations
python manage.py migrate
```

### 5. Create a superuser

```bash
python manage.py createsuperuser
```

Follow the prompts to create the administrator account.

### 6. Start the development server

```bash
python manage.py runserver
```

The API will be available at:

```text
http://127.0.0.1:8000/
```

## Django Admin

The Django admin panel can be accessed at:

```text
http://127.0.0.1:8000/admin/
```

From the admin panel, you can:

* Create users
* Create the Manager group
* Create the Delivery Crew group
* Assign users to groups
* Add categories
* Manage menu items
* Manage orders

## Testing

The APIs can be tested using:

* Browser
* Insomnia
* Django REST Framework browsable API

Authentication tokens should be included when accessing protected endpoints.

## Project Requirements

The completed API supports the following major requirements:

* Admin can assign users to the Manager group
* Managers can log in
* Managers can manage menu items
* Managers can assign delivery crew
* Managers can assign orders to delivery crew
* Delivery crew can access assigned orders
* Delivery crew can mark orders as delivered
* Customers can register and log in
* Customers can browse categories
* Customers can browse menu items
* Customers can filter menu items by category
* Customers can paginate menu items
* Customers can sort menu items by price
* Customers can add items to their cart
* Customers can view their cart
* Customers can place orders
* Customers can view their own orders

## Submission

The project submission should contain:

* Complete project source code
* `db.sqlite3`
* `notes.txt`
* Superuser credentials
* Credentials for other created users

The project should be compressed into a ZIP file before submission.

## Project Purpose

The purpose of this project is to provide a complete backend API for the Little Lemon restaurant. The API is designed so that web and mobile client applications can use the same backend to manage users, menu items, carts, orders, and deliveries.

---

**Little Lemon API — Django REST Framework Backend**
