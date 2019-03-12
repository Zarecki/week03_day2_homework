# Homework: Pizza Shop

## Today

Today we created a program that tracks pizza orders. We created a Ruby class (`PizzaOrder`) and a database table (pizza\_orders) which is able to persist objects of this type. Currently, our database table is structured as follows...

| first_name | last_name | topping         | quantity |
|------------|-----------|-----------------|----------|
| Luke       | Skywalker | Pepperoni       | 2        |
| Leia       | Organa    | Ham & Pineapple | 1        |

Our table stores a combination of pizza and customer details. This isn't ideal for a number of reasons. If a single customer makes many orders then their details appear in our database many times. Firstly, this is not efficient. It would be much nicer if we could create a customer once and then attribute many orders too them. This would also help to protect us against typos as we would only have to submit a new customer once.

## Tomorrow

Tomorrow we are going to refactor our program. We will add another class, `Customer`, which will also have the full set of CRUD actions. This class will be responsible for the customer details, while `PizzaOrders` will be responsible for the pizza order that is being processed.

We will also extract some of the database connection code into a new class, `SqlRunner`.

Once completed, our tables should be structured as follows...

### customers

| id | first_name | last_name |
|----|------------|-----------|
| 1  | Luke       | Skywalker |
| 2  | Leia       | Organa    |

### pizza_orders

| id | topping         | quantity | customer_id |
|----|-----------------|----------|-------------|
| 1  | Pepperoni       | 2        | 1           |
| 2  | Ham & Pineapple | 1        | 2           |
| 3  | Meat Feast      | 1        | 1           |

## Task

Your task this evening is to read over the completed end\_code for tomorrow's lesson, understand it as much as possible, and answer the following questions.

1) What is the relationship between customers and pizza\_orders?

Customers passes it's customer id into pizza orders. 

2) At what point is the id of a `PizzaOrder` created?

During the save method on line 26 of the pizza order file. It is assigned an id when It is inserted into the pizza_orders table.

3) At what point do we assign a value to the `@id` instance variables of our objects?

customer = line 31 where @id = id_string.to_i
pizza_order = line 31 where @id = SqlRunner.run(sql, values).first['id'].to_i

4) Name 2 things that the `Customer`'s `@id` property is used for.

customer.rb line 13 - the customer id is used here to return the customer's order history as PizzaOrder objects.

pizza_order.rb line 15 - the customer id is used to return a new customer object with the atrributes of the customer to whom the id belonged.

5) Why might it be important to check if `options['id']` is `nil` in our `initialize` method before assigning `@id` the value of `options[‘id’].to_i?`

we do not assign an id to an object when we create a new one, this is assigned when the object is added to a table.
if our class required an id at creation, rather than an id being able to be referenced once assigned, we would recieve errors each time we initialize a new instance of the class.

6) What are the responsibilities of `SqlRunner`?

SqlRunner is a class which has a single method and no constructors. It performs the same function as 
"db.prepare("example", sql)
db.exec_prepared("example", value)
db.close()"
did in our labs today. 
first it allows the string of SQL code in our ruby method to be read and performed as SQL.
Then it supplies values to be applied to each of the variable in the SQL code, allowing it to run.
It then closes the connection to the database.


7) How does `SqlRunner` improve the quality of our code?

SQL runner makes our code DRYer and increases readbility. Whereas before we would have to write three lines of code repeatedly, we can call the SqlRunner to run itself and peform the same function in a single method in another file.
