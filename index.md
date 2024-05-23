---
title: Welcome to my blog!
---

---

---

## Intro to Javascript:

### _There and Back Again: The Prologue_

I started a software engineering bootcamp through Flatiron School a week ago. So far, learning how to be a software engineer has been like trying to drink from a fire hose. The more I learn, the more I discover just how much there is to learn. I feel like Bilbo Baggins leaving the Shire: my blissful existence as a user is over, and my journey to the Lonely Mountain has begun. But before I walk through the front gate, let’s do some light recon:

At a high-level, software engineering is divided into two areas of concern: frontend and backend. The frontend is typically comprised of a user-interface, and the backend stores data and data-processing logic. Because of my background in research, I have some experience building pipelines for processing research data like signals, which ran in a backend-type framework. However, I don’t have any experience in frontend development. (Un)lucky for me, that’s exactly where our journey starts!

Most people interact with front-end applications all the time; you’re reading this text on a front-end application right now! Most of the time, the frontend also needs information that lives in the backend in order to function properly. To access this information, the frontend communicates with the backend through an API (application programming interface). In the case of web-based applications, like Instagram, the frontend, which displays images and text for the user and supports interactivity through likes, comments, and posts, communicates with the backend, where the data and data-processing code actually live, via an API that uses HTTP (HyperText Transfer Protocol) requests/responses.
◊
Ok, that was a lot of technical jargon for this early in the prologue, but stay with me! Let’s stick with our Instagram example: When we open Instagram, our feed loads with user stories at the top, and images, videos, and text in the form of posts, captions, and comments below. As a user, we usually take this part for granted: of course the page loads when I open Instagram. But how did that actually happen?

There are a few different layers within the frontend that work together to give us our frontend experience of Instagram. The webpage structure is specified using HTML (HyperText Markup Language) on an HTML document. An example of an HTML header might look something like this:

    <h>This is the Instagram header!</h>;

Some of the architecture for Instagram is static: its characteristics don’t change. The background is always white (or black, if you have night-mode on), stories are always at the top, with the feed below that, and the ‘explore,’ ‘search,’ ‘post,’ ‘reels,’ and ‘my profile’ buttons are always at the bottom of the screen. In fact, if you’re looking at Instagram in the Google Chrome web browser, you can see the underlying HTML for yourself by right-clicking anywhere on the page and selecting “inspect” to open the developer window (neato!).

But some of the webpage contents are dynamic. Suppose we’re scrolling down in our feed, and we need the webpage to load new information, or maybe we want to make new content of our own by writing and posting a comment. How does that happen? For that, we need to be able to update the webpage dynamically, only without having to go in and hardcode the underlying HTML every time.

The HTML document is rendered on the webpage for us via the DOM (document object model). The DOM is just what the acronym states: a model of our underlying HTML document, and the DOM acts as a programming interface. It represents the page so programming languages can change the web page (document) dynamically. The DOM allows us to have an easier, faster, and interactive experience with our webpage.

Let’s say we want to ‘like’ a post. How does the webpage “know” to update itself and display a red heart and increase the ‘like count’? The DOM! Instagram has frontend code “listening” for events happening on the webpage, such as hitting the ‘like’ button, and that code then interacts with the DOM to update the HTML document. That code might look something like this:

    // select the like button so we can do things with it
    const likeButton = document.querySelector(“.like-button”);
    // add an event listener so we can do something when the like button is clicked
    likeButton.addEventListener(“click”, () => {
        // increase the like count
        // update the DOM to display the new like count
    });

Great, that means our Instagram page will update to reflect our ‘like’!

But how can we see the most recent like count in the first place? Along with a list of all the other users who previously liked the post? Behind the scenes, the frontend Instagram application is actually interacting with a backend server storing the data through an HTTP request/response communication pattern (ah, we’ve come full-circle!).

When we first open Instagram, the frontend sends a ‘GET’ request to the backend asking for the latest version of information to display in our stories and on our feeds. The backend sends a response, and if everything goes well, that response includes the information our frontend needs to update the webpage via the DOM. That code might look something like this:

    // GET request
    const fetchFeedInformation = async () => {
        const response = await fetch(“Instagram server address”);
        const feedInformation = await response.json();
        // do things with the feedInformation like load content
    };
    fetchFeedInformation();

When we hit the ‘like’ button, along with updating the DOM to reflect our new like count, the frontend also sends a ‘POST’ request to the backend with information necessary to update the like count and the user list in the server. That code might look something like this:

    // POST request
    const updateLikeCount = async () => {
        const response = await fetch("Instagram server address", {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
                Accept: "application/json"
            },
            body: JSON.stringify({updated like count info})
        });
    };
    updateLikeCount();

Turns out a lot is happening when we open up and interact with a webpage, even with an interaction as “simple” as liking a post!

Now that we’ve done a little recon we have a solid foundation for adding more complexity and creating more sophisticated web applications. We’re ready to leave the Shire and join the dwarves on their quest!

---

---

## Intro to React:

### _Journey to the Lonely Mountain_

We’ve left the Shire and have an important job: defeat the dragon, Smaug, so we can return the lost dwarven treasure (and maybe free the people of Lake Town while we're at it). In order to do this, the team will need a burglar of exceptional skill. However, exceptional skill won’t be enough – we’ll need some tools to up our game. Luckily, we know a bit of HTML and Javascript to get us started on our quest, but we’ll need the ring of invisibility to help us get through the Mirkwood to the Lonely Mountain and Smaug’s lair!

React, a Javascript library, is a bit like the ring of invisibility. We already know how to write Javascript ourselves (be a burglar) but our powers are enhanced with additional tools. React is a declarative, component-based library that returns JSX (an XML-like syntax) such that complex webpages can be broken down into smaller components. With these components, different aspects of a webpage’s functionality can be organized and siloed, allowing for increased complexity while minimizing the costly re-rendering that can accommodate dynamic data.

React components are organized into parent-child-sibling relationships in the same manner as Javascript elements. However, React accommodates dynamic data by monitoring the value(s) of components’ state(s) using a virtual DOM. React ‘State’ is a built-in object designed to contain data or information about a component. When a state changes, the new state value can be compared to the existing value before finding the least expensive way to update the actual DOM with the new state, after which the component containing the state can be re-rendered. Because of this, React can update state more or less automatically in the affected component without forcing the browser to recalculate the position of everything on the webpage.

Monitoring state in (effectively) real-time has powerful consequences. With Javascript alone, a task like validating user inputs to a form (such as only allowing characters or digits to be entered) might have to wait until after the user has submitted the form. With React state, the state of the user-inputs can be updated anytime there’s a change to the inputs, regardless of whether or not the form has been submitted. This means the input can be validated before the form is submitted, and before any information is persisted to the backend unnecessarily. The process for creating a form component that checks the validity of form inputs before form submission might look something like this:

    // import useState
    import React, { useState } from "react";

    // create React form component and set initial state
    function myForm() {
    const [formData, setFormData] = useState({
    name: “”
    phoneNumber: “”
    email: “”
    });

    // function to handle changes to the form input(s) and update the form data state
    function handleChange(event) {
        setFormData({...formData, [event.target.name]: event.target.value});
    }

    // functions to validate form inputs
    // Name
    function checkName(formData.name) {
        // check if form name is composed only of characters
    }

    // Phone Number
    function checkPhoneNumber(formData.phoneNumber) {
        // check if form phone number is composed only of digits
    }

    // return form JSX
    return (
        <section>
        <h1>Form</h1>
        <form>
            <label>
            Name:
            <input
                type="text"
                name="name"
                value={formData.name}
                onChange={handleChange}
                checkName={checkName}
            />
            </label>
            <label>
            Phone Number:
            <input
                type="text"
                name="phoneNumber"
                value={formData.phoneNumber}
                onChange={handleChange}
                checkPhoneNumber={checkPhoneNumber}
            />
            </label>
            <label>
            Email:
            <input
                type="text"
                name="email"
                value={formData.email}
                onChange={handleChange}
            />
            </label>
        </form>
        </section>
    );
    }

    // export React component so other components can import it, if necessary
    export default myForm;

Wow, that was much faster than it would have been for us to try to write Javascript to check the form inputs “live.” And, as a bonus, all of these checks are contained within our form component and are therefore quarantined from other components on the webpage. That way we can separate tasks and break features down into smaller, bite-sized chunks.

With the help of our new frontend tools, we’ve made our way through the Mirkwood to the Lonely Mountain. It’s time to learn how the backend works so we have all the skills we need to defeat Smaug and free the people of Lake-town!

---

---

## Intro to Python:

### _Smaug's Lair_

Having reached the Lonely Mountain, the team finds the secret door leading to Smaug's lair and sends Bilbo in to scout a way to defeat Smaug. Right now we have some important tools and knowledge gained on our journey. We have made it to the Lonely Mountain with the ring of invisibility and a capable team. But we are limited in our knowledge; we don't know _how_ to defeat Smaug. We must now go on a scouting mission through the secret door to Smaug's lair.

Luckily, we already have some tools at our disposal to help us find our way beyond the secret door. Our burglaring skills and the ring of invisibility (our knowledge of JavaScript and React) can help us learn new information to help us in our quest: Python. 

We already have a way to create and receive data on the front end and pass it back and forth to and from the backend, but no way to work within the backend itself. We know we must scout a way to access, manipulate, and store data displayed on/generated by the front end somewhere...else. In other words, we need to find a way to build server-side applications to help support our front-end applications. 

Python is an object-oriented programming lanuage, which makes use of the idea of an object as a packet of code containing all the data and logic required to complete a task. Object-oriented programming is a way of thinking about and contsructing code for maximum reusability. Objects are often modeled on relationships and concepts in the real world, and requires a mode of thinking that breaks down systems into smaller parts.

Just as we learned with JavaScript, accomplishing a task often requires a set of instructions, or functions. Objects are often created with their own special set of instructions that can be executed on data within the object itself, as well as on other objects. These special instructions are called **methods.** 

Python comes with a few types of objects already built-in: strings, and lists, for example. These objects have  methods defining instructions for common programming tasks that can be executed on the data contained within the object, and are called using dot notation. For example:

    ## instantiate an object of type string
    my_string = "Hello, I'm a string!"

    ## count the number of times the letter 'e' occurs in the string object
    my_string.count("e")

Open-source Python libraries exist for all kinds of tasks, like _math_ for performing mathematical operations, _NumPy_ for supporting large, multi-dimensional matrices, or _Tensor Flow_, for machine learning and artifical intelligence. But often, we will need to create our own, specialized objects to deal with the particular programming problem we are trying to solve. This is where the concept of **class** comes into play. 

A class is a sort of blueprint for building an object, and includes instructions for instantiating an object of that class, as well as associated functionality (methods). Let's say we're running a coffee shop and business is so good (!) that we need an automated way to keep track of all the orders coming in. Programmatically, we can model our system as a series of objects with properties and functionality that are related in some way. For example, we can model each type of coffee with a Coffee object that contains information (attritubtes) about the coffee:

    ## create the Coffee class with optional number of shots
    class Coffee:
        def __init__(self, type, size, shots = 2):
            self.type = type
            self.size = size
            self.shots = shots

Now when someone wants to order a type of coffee, we can create a coffee object to keep track of that information:

    ## create Coffee object
    mocha = Coffee("mocha", "small", 1)

But we might want to restrict the instantiation of coffee objects to reflect our menu: we can only create coffees with certain types and sizes. We can enforce these rules by managing the attributes (making them properties) using Python's **property()** class and decorators to attach getter and setter functions to our class object:

    ## define coffee menu
    coffee_types = ["cappuccino", "latte", "flat white", "cortado", "mocha", "americano", "pour-over"]
    coffee_sizes = ["small", "medium", "large"]
    
    ## manage attributes via property() decorators
    class Coffee:
        def __init__(self, type, size, shots = 2, price):
            self.type = type
            self.size = size
            self.shots = shots
            self.price = price

        # restrict coffee type
        @property
        def type(self):
            return self._type

        @type.setter
        def type(self, type):
            if type in coffee_types:
                self._type = type
            else:
                raise ValueException("type must exist in coffee_types")

        # restrict coffee size
        @property
        def size(self):
            return self._size

        @size.setter
        def size(self, size):
            if self._size in coffee_sizes:
                self._size = size
            else:
                raise ValueException("size must exist in coffee_sizes")

Ok, we have some coffees! What about our customers? We need a way to keep track of who is who. We can do that by defining a customer class:

    # create customer class, restricting name to non-empty type string
    class Customer:
        def __init__(self, name):
            self.name = name

        @property
        def name(self):
            return self._name

        @name.setter
        def name(self, name):
            if isinstance(name, str) and name:
                self._name = name
            else:
                raise ValueError("name must be non-empty string")

But what if we also want a method for keeping track of each customer's place in line? We can use information about the class _itself_ to determine the place_in_line

    #create customer class with name property and class method that returns the customer's place in line
    class Customer:
        # keep track of all customer objects
        all = []
        def __init__(self, name):
            self.name = name
            ## add each customer instance to the list of customers
            self.__class__.all.append(self)

        @property
        def name(self):
            return self._name

        @name.setter
        def name(self, name):
            if isinstance(name, str) and name:
                self._name = name
            else:
                raise ValueError("name must be non-empty string") 

        # calculate place in line
        @classmethod
        def place_in_line(cls):
            place_in_line = cls.all.index(self) + 1
            return place_in_line

Great! We have coffees and customers - but we need some way to establish a relationship between each customer and her/his coffee. We can do this by creating a third class that describes the relationship between coffee and customer objects, called Order. Each order might contain information about the customer, the coffee, the order total, and the method of payment, for example. Our Order class might look something like this:

    # create Order class to capture relationship between customers and coffees
    class Order:
        def __init__(self, customer, coffees = []):
            self.customer = customer
            self.coffees = coffees

        @property
        def customer(self):
            return self._customer

        @customer.setter
        def customer(self, customer):
            if isinstance(customer, Customer):
                self._customer = customer
            else:
                raise ValueError("customer must be of type Customer")

        # method for adding coffees to the order
        def add_coffee(self, coffee):
            if isinstance(coffee, Coffee):
                self.coffees.append(coffee)
            else:
                raise ValueError("only valid coffees can be added to the order")
                
        # method for calculating the order total
        def calculate_total(self):
            total = sum(coffee.price for coffee in coffees if isinstance(coffee, Coffee))
            return total

We now have our coffee shop system modeled as a series of three objects: Coffee, Customer, and Order, with the relationship(s) between the three. We can imagine how our system might get much more complex as we add items to the menu, create the menu itself, and even the coffee Shop. One of the primary benefits of object-oriented programming (OOP) is that it makes it easier to break complex systems down into smaller, reusable, constituent parts. We can create objects with specific characteristics and properties that follow rules and behave predictably.

Just as Bilbo spotted a vulnerability in Smaug's armour, we have successfully completed our scouting mission and we can prepare for the battle to defeat Smaug, reclaim the lost dwarven treasure, and free the people of Lake Town!

---

---

## Intro to Flask:

### _The Arkenstone_

Smaug has awoken, enraged that Bilbo has learned his secrets, and flies off to Lake Town to destroy those he suspects have helped his enemies. Just as Bilbo knows about the vulnerability in Smaug's armour, we know how to break down complex systems into their constituent parts. We can now use that knowledge to defeat Smaug with the help of our friends. We just need a way to get that information to the people of Lake Town!

While we know how to build models for systems, we still need a way to fascilitate communication between all of our partners (frontend and backend), and a way to store that information long-term. Just like we learned at the start of our journey, we can send information back-and-forth between the client-side and server-side with an Application Programming Interface (API) and store that information in a database. 

In general, when working with a server, our goal is basically always the same: 1) process a request from a client; 2) create a response; and 3) send the response to the client. Flask, a web framework written in Python, supports many different kinds of extensions that will allow us to bring a bunch tools together to achieve 1), 2), and 3). 

We can use Flask extensions like **Flask-SQLAlchemy** to get access to the SQLAlchemy library and ORM for python, **Flask-Migrate** to handle database migrations, and **jsonify** to structure the data being passed back and forth. Using these extensions, we can create our own web APIs with routes receive requests from the client side, access and manage data in our database, and send a response back to the client side in JSON format. 

Let's say we want to layer a frontend application on top of our coffee shop model from th previous blog post. Maybe we want to be able to display a menu of all the coffees, have a form where new orders cna be created, or show the waiting list for who's in line that dynamically updates when a new order is placed. To do this, we need to build the interface between the coffee, order, and customer information stored in our database and the front end displaying that information. 

We can use **Flask-RESTful** to build a restful API populated with resources that inherit from the **Resource** class, which includes conditions for throwing exceptions and base methods for each HTTP verb instance method (e.g. 'get', 'post', etc.). We might construct an API resource that accommodates a get request related to our coffee menu on the front end like this:

    # create a coffee menu resource
    class Coffees(Resource):
        def get(self):
            # create a list of all available coffees
            coffees = [coffee.to_dict() for coffee in Coffee.query.all()]

            # create a response to send the list of coffees in a JSON format to the front end, along with a status code
            return coffees, 200

Cool! We can now pass information from the backend to the front end and display our coffee menu. What about the other direction? What if we want to be able to update the coffee menu from the front end rather than having to go in and code new coffees on the backend? This is where the Flask **request** object comes in handy. The **request** object stores information related to the request, including any incoming JSON request data submitted via a frontend form. We can access the request data on the backend, and with a few lines of code, we can write a post method to persist the request information to our database:

    # ... continuing in our Coffees class from above:
        def post(self):
            try:
                # create a new coffee object using information in the post request coming from the frontend:
                new_coffee = Coffee(
                    type = request.form["type],
                    size = request.form["size],
                    shots = request.form["shots],
                    price = request.form["price]
                )
    
                # add the new coffee to our database coffees table
                db.session.add(new_coffee)
                db.session.commit()
    
                # create a response to send back to the frontend confirming everything went ok
                return new_coffee.to_dict(), 201

            except ValueError:
                # create a response to send back to the frontend indicating there was some kind of problem
                # with the coffee data
                return {"error": "Value Error detected in coffee data"}, 400

Maybe we also want to be able to update information from the frontend (maybe a customer wants to change or cancel their order):

        class OrderById(Resource):
            # change an order
            def patch(self, id):
                try:
                    # find corresponding order
                    order = Order.query.filter(Order.id == id).first()
                    
                    # update order with new information from the request
                    order = (
                        customer = request.form["customer"]
                        coffees = request.form["coffees"]
                    )

                    # udpate corresponding database table row
                    db.session.add(order)
                    db.session.commit()

                    # send response indicating the order has been updated successfully
                    return order.to_dict(), 201

                # send response indicating there was a problem with the order data
                except ValueError:
                    return {"error": "Value Error in order data"}, 400

            # cancel an order
            def delete(self, id):
                try:
                    # find corresponding order
                    order = Order.query.filter(Order.id == id).first()

                    # delete order
                    db.session.delete(order)
                    db.session.commit()

                    # send response indicating the order was deleted successfully
                    return {"message": "204: No Content Found"}, 204

                # send response indicating the order has been updated successfully
                except ValueError:
                    return {"error": "Value Error in order data"}, 400

Neat! We have established a method for communicating knowledge back-and-forth between our frontend and backend, just like the thrush who overhears Bilbo's discovery about the weakspot in Smaug's armour and flies to Lake Town to tell Bard. Smaug, enraged by Bilbo's intrusion, flies to Lake Town to destroy those he believes have helped his enemies. Learning of the weakness in Smaug's armour from the thrush, Bard of Lake Town is able to shoot Smaug down!

But wait, defeating Smaug was not as simple as it seems. The people of Lake Town are requesting compensation from Bilbo and the dwarves for awakening and enraging Smaug. But the dwarves are not so keen to relinquish their long-lost treasure, and tensions begin to flare. Just then, Gandlaf returns and warns that an army of Goblins and Wargs is on the way - they have their own designs for the treasure!

We now have a full arsenal at our disposal to create solutions to complex problems Can we bring everything we've learned together to avert all-out war between men, dwarves, elves, and goblins?                  
            
            
                    
            

            
        







