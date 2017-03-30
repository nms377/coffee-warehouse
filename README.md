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
|	 Customer Name  	 |   String		   |	 Company Name or Individual Name	|
|  Phone Number   	 |	  Integer		 |	 Contact number for Customer			|
|	 Email				  	 |	  String		 |	 Email for Customer								|
|	 Billing Address	 |		String		 |	 Billing address of Customer			|
|	 Shipping Address	 |		String		 |	 Shipping address of Customer			|
|	 Orders					   |   						 |	 Orders associated with Customer	|

**Routes overview table:**

|	METHOD | 			|												 |				 								 |
| ROUTE  |			|												 |				 							   |
| (uri)  | Body | Responses 						 | Action 								 |
|--------|------|------------------------|-------------------------|
| GET/   | empty| render HTML index.html | serves the `index.html` |




