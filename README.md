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
|	 Coffee Name   |  String		| Type of Coffee (i.e. black, latte, cappucino)		 			|
|	 Item Cost     |  Integer		| Wholesale cost for coffee													    |
|  Shipping Cost |  Integer		| Shipping cost for each item														|
|	 Location      |  String		| Location Where Coffee is Grown/Manufactured		  			|
|	 Caffeine Lvl  |  Integer		| Caffiene level of coffee															|
|  Inventory     |  Integer	  | How much of this Coffee is in Stock at Location				|

**Orders**

|  Columns			|		DataType	 |	 Description																								|
|---------------|--------------|--------------------------------------------------------------|
|	 Coffee Name	|		String		 |	 Type of Coffee(s), foreign key to Coffee										|
|  Customer Name|		String		 |	 First and Last name of customer, foreign key to Customers	|
|	 Order Status |	  String		 |	 pending, shipped, voided																		|
|	 Item Cost		|	  Integer		 |	 Cost per coffee item, foreign key to Coffee								|
|	 Quantity	    |		Integer		 |	 How much of each coffee item is purchased									|
|	 Ship Method	|	  String		 |	 Method of shipping (i.e. FedEx, UPS, Freight, USPS)				|
|	 Shipping Cost|	  Integer	   |	 Cost of shipping based on ship method and order weight			|
|	 Sub Total	  |	  Integer	   |	 Total of Items in Order not including shipping cost				|
|	 Order Total	|	  Integer	   |	 Total of Order include shipping cost												|
|  Ship Date	  |		Date		   |	 Date order was shipped from warehouse											|
|	 Tracking No. |	  String	   |	 Tracking number of shipped order														|

**Customers**

|	 Columns			  	 |		DataType	 |	 Description											|
|--------------------|---------------|------------------------------------|
|	 Customer Name  	 |   String		   |	 Company or Individual Name	|
|  Phone Number   	 |	  Integer		 |	 Contact number for Customer			|
|	 Email				  	 |	  String		 |	 Email for Customer								|
|	 Billing Address	 |		String		 |	 Billing address of Customer			|
|	 Shipping Address	 |		String		 |	 Shipping address of Customer			|
|	 Orders					   |   						 |	 Orders associated with Customer	|

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

* `Coffee Name` : name of coffee(s) ordering
* `Quantity`: Number of coffee items purchased
* `Customer Name`: Name of the customer
* `E-mail`: to send order confirmation
* `Phone`: to contact guest
* `Billing Address`: Billing address of the cusotmer
* `Shipping Address`: Shipping address if differnt from billing
* `Shipping Method`: shipping service and speed guest would like their item shipped
* `Subtotal`: total of order (not including shipping or sales tax)
* `Total`: Total of order (includes shipping and sales tax)

If the order is accepted then the customer will be redirected to a new page with the message that their order was a success and generates an order number for the customer to use to track the status of their order.

`GET /order/confirmation/:id`: Returns a JSJON response containing the information the customer submitted in the order form. Also displays the customer's order confirmation to track the status of their order."

`GET /coffee`: Returns a JSON response listing all the coffees in the database with the following keys:

* `Coffee Name`: name of coffee
* `Location`: where coffee beans were grown or where coffee was manufactured
* `Description`: description of the cofee
* `Caffeine Level`: caffeine level of the coffee
* `Item Cost`: cost of item
* `Shipping Cost`: shipping cost for each item

`GET /coffee:id`: Returns a JSON response of a coffee based on its id and returns the following keys:

* `Coffee Name`: name of coffee
* `Location`: where coffee beans were grown or where coffee was manufactured
* `Description`: description of the cofee
* `Caffeine Level`: caffeine level of the coffee
* `Item Cost`: cost of item
* `Shipping Cost`: shipping cost for each item

`GET /coffee:coffeename`: Returns a JSON response of coffee based on its name and returns the following keys:


* `Coffee Name`: name of coffee
* `Location`: where coffee beans were grown or where coffee was manufactured
* `Description`: description of the cofee
* `Caffeine Level`: caffeine level of the coffee
* `Item Cost`: cost of item
* `Shipping Cost`: shipping cost for each item

`GET /coffee:location`: Returns a JSON response with a list of coffee based on its location and returns the following keys:

* `Coffee Name`: name of coffee
* `Location`: where coffee beans were grown or where coffee was manufactured
* `Description`: description of the cofee
* `Caffeine Level`: caffeine level of the coffee
* `Item Cost`: cost of item
* `Shipping Cost`: shipping cost for each item

`GET /coffee/:caffeinelevel`: Returns a JSON response with a list of coffee based on its caffeine level

* `Coffee Name`: name of coffee
* `Location`: where coffee beans were grown or where coffee was manufactured
* `Description`: description of the cofee
* `Caffeine Level`: caffeine level of the coffee
* `Item Cost`: cost of item
* `Shipping Cost`: shipping cost for each item

`GET /coffee/new`: Authenticates the user then renders a form for admin users to add new coffees to the database. 

`POST /coffee`: Authenticates the user then creates an new coffee object containing the following:

* `Coffee Name`: name of coffee
* `Location`: where coffee beans were grown or where coffee was manufactured
* `Description`: description of the cofee
* `Caffeine Level`: caffeine level of the coffee
* `Item Cost`: cost of item
* `Shipping Cost`: shipping cost for each item

`PUT /coffee/:id/edit`: Authenticates the user and updates a coffee item based on its id.

`DELETE /coffee/:id`: Authenticates user and removes the coffee from the databse by its id. We'll want to archive the coffee item rather than destroy the data completely.

`GET /order`: Authenticates the user and renders a list of orders in the databse with the following:

* `id`: Coffee order number
*  `Coffee Name` : name of coffee(s) ordering
* `Quantity`: Number of coffee items purchased
* `Customer Name`: Name of the customer
* `E-mail`: to send order confirmation
* `Phone`: to contact guest
* `Billing Address`: Billing address of the cusotmer
* `Shipping Address`: Shipping address if differnt from billing
* `Shipping Method`: shipping service and speed guest would like their item shipped
* `Subtotal`: total of order (not including shipping or sales tax)
* `Total`: Total of order (includes shipping and sales tax)
* `Status`: Status of the coffee order (i.e. pending, shipped/delivered
* `Tracking No.`: Tracking number of order once it is shipped

`GET /order:id`: This route should be made public for customers to view. Returns a JSON response with the following:

* `id`: Coffee order number
*  `Coffee Name` : name of coffee(s) ordering
* `Quantity`: Number of coffee items purchased
* `Customer Name`: Name of the customer
* `E-mail`: to send order confirmation
* `Phone`: to contact guest
* `Billing Address`: Billing address of the cusotmer
* `Shipping Address`: Shipping address if differnt from billing
* `Shipping Method`: shipping service and speed guest would like their item shipped
* `Subtotal`: total of order (not including shipping or sales tax)
* `Total`: Total of order (includes shipping and sales tax)
* `Status`: Status of the coffee order (i.e. pending, shipped/delivered
* `Tracking No.`: Tracking number of order once it is shipped

`GET /order:coffeename`: Authenticates the user then returns a JSON response listing all orders that ordered the same coffee.

`PUT /order:id/edit`: Authenticates the user then updates an order based on its id. User can also tag an order's status as pending, shipped, or voided."

`GET /order:id:status`: Authenticates the user then returns a JSON response with a list of all the orders based on its status










