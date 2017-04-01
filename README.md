# Coffee Warehouse API

**About the API**
---

Build an API that allows authorized users to maintain coffee inventory, coffee and admin information, and customer drink orders. Create the server using **ExpressJS** and **Sequelize** as the ORM for the **Postgresql** Datastore. Use **React** to build the front-end User-Interface.

### The Specs
---

#### Tables

**Coffee**

|	 Columns  	   |	DataType  |	Description	 																	  	    |
|----------------|------------|-------------------------------------------------------|
|	coffee_name   |  String		| Name of coffee		 			|
|	 sale_cost     |  Integer		| Wholesale cost for coffee													    |
| sale_price | Integer | Retail cost
|  shipping_cost |  Integer		| Shipping cost for each item														|
|	 location      |  String		| Location Where Coffee is grown/manufactured		  			|
|	 caffeine  |  Integer		| Caffiene level of coffee															|
|  inventory     |  Integer	  | How much of this Coffee is in stock		|

**Orders**

|  Columns			|		DataType	 |	 Description																								|
|---------------|--------------|--------------------------------------------------------------|
|	 coffee_name	|		String		 |	 Name of coffee(s), foreign key to table Coffee										|
|  customer_name|		String		 |	 First and Last name of customer, foreign key to table Customer	|
|	 order_status |	  String		 |	 Pending, Shipped, Voided																		|
|	 sale_price		|	  Integer		 |	 Cost per coffee item, foreign key to Coffee								|
|	 quantity	    |		Integer		 |	 Amount of coffee purchased	|
|	 shipping_cost|	  Integer	   |	 Cost of shipping, foreign key to table Coffee |
|	 sub_total	  |	  Integer	   |	 Total of items in order excluding shipping and sales tax				|
|	 ordert_total	|	  Integer	   |	 Total of order including shipping and sales tax											|
|  ship_date	  |		Date		   |	 Date order was shipped from warehouse											|
|	 status |	  String	   |	 Tracking number of shipped order														|

**Customers**

|	 Columns			  	 |		DataType	 |	 Description											|
|--------------------|---------------|------------------------------------|
|	 customer_name  	 |   String		   |	 Company or Individual Name	|
|  phone   	 |	  Integer		 |	 Contact number for Customer			|
|	 email				  	 |	  String		 |	 Email for Customer								|
|	 billing_address	 |		String		 |	 Billing address of Customer			|
|	 shipping_address	 |		String		 |	 Shipping address of Customer			|
|	 orders					   |   						 |	 Orders associated with Customer, foreign key to table Orders	|

**Routes overview table:**

|	METHOD ROUTE (uri)  | Body | Responses 						 		| Action 								  |
|---------------------|------|--------------------------|-------------------------|
| `GET /` 						| empty| render HTML `index.html` | serves the `index.html` |
| `GET /admin` | empty | renders admin page | |
| `POST /admin` | { "email": string, "password":string } | redirect to admin profile | Authenticate login and redirect to admin profile |
| `POST /newuser`| { "email": string, "password":string, "securityQuestion": string } | redirect to login | Creates a new admin user and redirects user to login page |
| `GET /order` | empty | Render order form | Creates a coffee order form |
| `POST /order` | { "coffeeName": string, "quantity": integer, "firstName": string, "lastName": string, "email": string, "phone": integer, "billingAddress": string, "shippingAddress": string, "shipMethod": string, "shipCost": integer, "subtotal": integer, "total": integer} | Redirect to confirmation page if order accepted | Creates a new order. Returns true if successful else false |
| `GET /order/confirmation/:id` | { "coffeeName": string, "quantity": integer, "firstName": string, "lastName": string, "email": string, "phone": integer, "billingAddress": string, "shippingAddress": string, "shipMethod": string, "shipCost": integer, "subtotal": integer, "total": integer} | Render confirmation page | Retrieves the newly created order based on its order confirmation (id) and renders the information |



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

`GET /order/confirmation/:id`: Returns a JSJON response containing the information the customer submitted in the order form. Also displays the customer's order confirmation to track the status of their order."

`GET /coffee`: Returns a JSON response listing all the coffees in the database with the following keys:

* `coffee_name`: name of coffee
* `location`: where coffee beans were grown or where coffee was manufactured
* `description`: description of the cofee
* `caffeine_level`: caffeine level of the coffee
* `sale_price`: cost of item
* `shipping_cost`: shipping cost for each item
* `inventory`: how much coffee is in stock

`GET /coffee:id`: Returns a JSON response of a coffee based on its id.

`GET /coffee:coffee_name`: Returns a JSON response of coffee based on its name.

`GET /coffee:location`: Returns a JSON response with a list of coffee based on its location.

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
* `customer_name`: Name of the customer
* `email`: to send order confirmation
* `phone`: to contact guest
* `billing_address`: Billing address of the cusotmer
* `shipping_address`: Shipping address if differnt from billing
* `subtotal`: total of order (not including shipping or sales tax)
* `total`: Total of order (includes shipping and sales tax)
* `status`: Status of the coffee order (i.e. pending, shipped, voided)

`GET /order:id`: This route should be made public for customers to view. Returns a JSON response with the following:

* `id`: Coffee order number
*  `coffee_name` : name of coffee(s) ordering
* `quantity`: Number of coffee items purchased
* `customer_name`: Name of the customer
* `email`: to send order confirmation
* `phone`: to contact guest
* `billing_address`: Billing address of the cusotmer
* `shipping_address`: Shipping address if differnt from billing
* `shipping_method`: shipping service and speed guest would like their item shipped
* `subtotal`: total of order (not including shipping or sales tax)
* `total`: Total of order (includes shipping and sales tax)
* `status`: Status of the coffee order (i.e. pending, shipped, voided)

`GET /order:coffeename`: Authenticates the user then returns a JSON response listing all orders that ordered the same coffee.

`PUT /order:id/edit`: Authenticates the user then updates an order based on its id. User can also tag an order's status as pending, shipped, or voided."

`GET /order:id:status`: Authenticates the user then returns a JSON response with a list of all the orders based on its status










