# Coffee Warehouse API

**About the API**
---

Build an API that allows authorized users to maintain coffee inventory, coffee and admin information, and allows customers to place orders. Create the server using **ExpressJS** and **Sequelize** as the ORM for the **Postgresql** Datastore. Use **React** to build the front-end User-Interface.

### The Specs
---

#### Tables

**Coffee**

|	 Columns  	   |	DataType  |	Description	 																	  	    |
|----------------|------------|-------------------------------------------------------|
|	coffee_name   |  String		| Name of coffee		 			|
|	 sale_cost     |  Integer		| Wholesale cost from manufacture   |
| sale_price | Integer | Retail cost
|  shipping_cost |  Integer		| Shipping cost for each item														|
|	 location      |  String		| Where coffee is grown/manufactured		  			|
|	 caffeine_level  |  Integer		| Caffiene level of coffee															|
|  inventory     |  Integer	  | Amount of coffee in stock		|

**Orders**

| Columns | DataType | Description |
|---------------|--------------|--------------------------------------------------------------|
| coffee_name	| String | Name of coffee(s), foreign key to table Coffee |
| first_name | String |	 First name of customer, foreign key to table Customer
| last_name | String | Last name of customer, foreign key to table Customer |
| company | String | Name of company if applicable, foreign key to table Customer |
| status | String | Pending, Shipped, Voided
| sale_price	| Integer | Cost per coffee item, foreign key to table Coffee	|
| quantity |	Integer |	 Amount of coffee purchased
| shipping_cost | Integer | Cost of shipping, foreign key to table Coffee
| subtotal |	Integer | Total of items in order excluding shipping and sales tax
| total | Integer | Total of order including shipping and sales tax	|

**Customers**

| Columns | DataType | Description |
|--------------------|---------------|------------------------------------|
| first_name  | String | First name |
| last_name | String | Last name |
| company | String | Company name if applicable |
| phone  | Integer | Contact number for Customer|
| email |	String | Email for Customer |
| billing_address |	String | Billing address of Customer | shipping_address | String	|	 Shipping address of Customer |
| orders | Orders associated with Customer, foreign key to table Orders	|

**Routes overview table:**

| METHOD ROUTE (uri) | Body | Responses | Action |
|---------------------|------|--------------------------|-------------------------|
| `GET /` | empty| render HTML `index.html` | serves the `index.html` |
| `GET /admin` | empty | renders admin page |
| `POST /admin` | { "email": string, "password":string } | redirect to admin profile | Authenticate login and redirect to admin profile |
| `POST /new`| { "email": string, "password":string, "security_question": string } | redirect to login | Creates a new admin user and redirects user to login page  |
| `GET /order` | empty | Render order form | Creates a coffee order form |
| `POST /order` | { "coffee_name": string, "quantity": integer, "first_name": string, "last_name": string, "company": string, "email": string, "phone": integer, "billing_address": string, "shipping_address": string, "subtotal": integer, "total": integer} | Redirect to confirmation page if order accepted | Creates a new order. Returns true if successful else false |
| `GET /order/:id/confirmation` | { "coffee_name": string, "quantity": integer, "first_name": string, "last_name": string, "company": string, "email": string, "phone": integer, "billing_address": string, "shipping_address": string, "shipping_cost": integer, "subtotal": integer, "total": integer} | Render confirmation page | Retrieves the newly created order based on its order confirmation (id) and renders the information |
| `GET /coffee` | {"coffee_name": string, "sale_price": integer, "shipping_cost": integer, "location": string, "caffeine_level": integer, "inventory": integer} | Renders page with list of all the coffee in database |
| `GET /coffee/:id` | { "coffee_name": string, "location": string, "caffeine_level": integer, "sale_price": integer, "shipping_cost": integer, "inventory": integer } | Returns JSON object coffee | Creates a list of coffee based on its id |
| `GET /coffee/:coffee_name`: | { "coffee_name": string, "location": string, "description": string, "caffeine_level": integer, "sale_price": integer, "shipping_cost": integer, "inventory": integer } | Returns JSON object coffee | Retrieves list of coffee based on `coffee_name`. |
| `POST /coffee` | { "coffee_name": string, "location": string, "description": string, "caffeine_level": integer, "sale_price": integer, "shipping_cost": integer, "inventory": integer } | Redirect to `/coffee` | Adds the new coffee item to the Object Coffee |
| `PUT /coffee/:id/edit` | { "coffee_name": string, "location": string, "description": string, "caffeine_level": integer, "sale_price": integer, "shipping_cost": integer, "inventory": integer } | {"success": true} | Updates coffee and the master list of coffee with the new coffee item if successful |
| `DELETE /coffee/:id` | { "id": integer } | {"success": true } | Removes the coffee item from the database |
|`GET /order`| {"id": integer, "coffee_name": string, "quantity": integer, "first_name": string, "last_name": string, "company": string, "email": string, "phone": integer, "billing_address": string, "shipping_address": string, "subtotal": integer, "total": integer, "status": string } | Returns JSON object order | Retrieves list of all coffee orders in the database |
| `GET /order/:id` | { "id": integer, "coffee_name": string, "quantity": integer, "first_name": string, "last_name": string, "company": string, "email": string, "phone": integer, "billing_address": string, "shipping_address": string, "subtotal": integer, "total": integer, "status": string } | Returns JSON object orders by id | Returns a coffee order based on its id |
| `GET /order/:coffee_name` | { "id": integer, "coffee_name": string, "quantity": integer, "first_name": string, "last_name": string, "company": string, "email": string, "phone": integer, "billing_address": string, "shipping_address": string, "subtotal": integer, "total": integer, "status": string } | Return object orders | Returns a list of orders that include a specific `coffee_name` |
| `PUT /order/:id/edit` | {"status": string} | "success": true} | Updates the status of the order |
| `GET /order/:status` | { "id": integer, "coffee_name": string, "quantity": integer, "first_name": string, "last_name": string, "companay": string, "email": string, "phone": integer, "billing_address": string, "shipping_address": string, "subtotal": integer, "total": integer, "status": string } | Returns a JSON object order based on `status` | Retrieves a list of orderes based on the `status` of the order |







**Routes detailed**

`GET /`: Serves the static file `index.html`. This is the homepage which should include a nav bar with a link to redirect admin users to setup an account or login to their existing account. It should also include a link to redirect customers to an order form.

`GET /admin`: Renders an admin page for users to setup an account or login to their account.

`POST /admin`: Authenticates the admin user and redirects them to the admin user's profile.

`POST /newuser`: Creates an admin account object. The body should include the following keys:

* `email` containing the user's email as a string
* `password` to verify the user
*  `security question` to verify user when they need to reset their password

Once an account is created, the user will be redirected to the admin page to login.

`GET /order/new`: Renders an order form for the customer to coplete.

`POST /order`: Creates an order object conatining the following:

* `coffee_name` : name of coffee(s) ordering
* `quantity`: Number of coffee items purchased
* `customer_name`: Name of the customer
* `email`: to send order confirmation
* `phone`: to contact guest
* `billing_address`: Billing address of the cusotmer
* `shipping_address`: Shipping address if differnt from billing
* `subtotal`: total of order (not including shipping or sales tax)
* `total`: Total of order (includes shipping and sales tax)

If the order is accepted then the customer will be redirected to a new page with the message that their order was a success and generates an order number for the customer to use to track the status of their order.

`GET /order/:id/confirmation`: Returns a JSJON response containing the information the customer submitted in the order form. Also displays the customer's order confirmation to track the status of their order."

`GET /coffee`: Returns a JSON response listing all the coffees in the database with the following keys:

* `coffee_name`: name of coffee
* `location`: where coffee beans were grown or where coffee was manufactured
* `description`: description of the cofee
* `caffeine_level`: caffeine level of the coffee
* `sale_price`: cost of item
* `shipping_cost`: shipping cost for each item
* `inventory`: how much coffee is in stock

`GET /coffee/:id`: Returns a JSON response of a coffee based on its id.

`GET /coffee/:coffee_name`: Returns a JSON response of coffee based on its name.

`GET /coffee/:location`: Returns a JSON response with a list of coffee based on its location.

`GET /coffee/:caffeine_level`: Returns a JSON response with a list of coffee based on its caffeine level.

`GET /coffee/new`: Authenticates the user then renders a form to add new coffees to the database.

* `coffee_name`: name of coffee
* `location`: where coffee beans were grown or where coffee was manufactured
* `description`: description of the cofee
* `caffeine_level`: caffeine level of the coffee
* `sale_price`: cost of item
* `shipping_cost`: shipping cost for each item
* `inventory`: how much coffee is in stock

`POST /coffee`: Authenticates the user then creates an new coffee object containing the following:

* `coffee_name`: name of coffee
* `location`: where coffee beans were grown or where coffee was manufactured
* `description`: description of the cofee
* `caffeine_level`: caffeine level of the coffee
* `sale_price`: cost of item
* `shipping_cost`: shipping cost for each item
* `inventory`: how much coffee is in stock

`PUT /coffee/:id/edit`: Authenticates the user and updates a coffee item based on its id.

`DELETE /coffee/:id`: Authenticates user and removes the coffee from the databse by its id. We'll want to archive the coffee item rather than destroy the data completely.

`GET /order`: Authenticates the user and renders a list of orders in the databse with the following:

* `id`: Coffee order number
*  `coffee_name` : name of coffee(s) ordering
* `quantity`: Number of coffee items purchased
* `first_name`: Customer's first name
* `last_name`: Customer's last name
* `email`: to send order confirmation
* `phone`: to contact guest
* `billing_address`: Billing address of the cusotmer
* `shipping_address`: Shipping address if differnt from billing
* `subtotal`: total of order (not including shipping or sales tax)
* `total`: Total of order (includes shipping and sales tax)
* `status`: Status of the coffee order (i.e. pending, shipped, voided)

`GET /order/:id`: This route should be made public for customers to view. Returns a JSON response with the following:

* `id`: Coffee order number
*  `coffee_name` : name of coffee(s) ordering
* `quantity`: Number of coffee items purchased
* `first_name`: Customer's first name
* `last_name`: Customer's last name
* `email`: to send order confirmation
* `phone`: to contact guest
* `billing_address`: Billing address of the cusotmer
* `shipping_address`: Shipping address if different from billing
* `subtotal`: total of order (not including shipping or sales tax)
* `total`: Total of order (includes shipping and sales tax)
* `status`: Status of the coffee order (i.e. pending, shipped, voided)

`GET /order/:coffee_name`: Authenticates the user then returns a JSON response listing all orders that ordered the same coffee.

`PUT /order/:id/edit`: Authenticates the user then updates an order based on its id. User can also tag an order's status as pending, shipped, or voided."

`GET /order/:status`: Authenticates the user then returns a JSON response with a list of all the orders based on its status