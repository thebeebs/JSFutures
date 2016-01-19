# 1. Compile a basic Javascript  file

	function greeter(person) {
		return "Hello, " + person;
	}

	var user = "Martin Beeby";

	document.body.innerHTML = greeter(user);

# 2. TypeScript

To compile this file you will need to use the TypeScript
compiler. The compiler is simply a node package that can be installed by typing

	npm install typescript -g

Open up a CMD or Terminal windows and navigate to the folder.

	tsc greeter.ts

# 2. Adding a Type
Now we can start taking advantage of some of the new tools
TypeScript offers. Add a ": string" type annotation to
the 'person' function argument as shown below.

	function greeter(person: string) {
		return "Hello, " + person;
	}

# 3. Pass an array
The next will error: 
	function greeter(person: string) {
		return "Hello, " + person;
	}

	var user = [0, 1, 2];

	document.body.innerHTML = greeter(user);

If you recompile this you will now get an error:

	tsc greeter.ts

The error will look like this:

	greeter.ts(7,27): error TS2082: Supplied parameters do not match any signature of call target:
        Could not apply type 'string' to argument 1 which is of type 'number[]'.



# You can remove
	function greeter() {
		return "Hello, ";
	}

	var user = [0, 1, 2];

	document.body.innerHTML = greeter(user);

# 3. Interface
Let's develop our sample further. Here we use an interface that describes objects that have a firstname and lastname field. In TypeScript, two types are compatible if their internal structure is compatible. This allows us to implement an interface just by having the shape the interface requires, without an explicit 'implements' clause.

	interface Person {
		firstname: string;
		lastname: string;
	}

	function greeter(person : Person) {
		return "Hello, " + person.firstname + " " + person.lastname;
	}

	var user = {firstname: "Martin", lastname: "Beeby"};

	document.body.innerHTML = greeter(user);

# 4. Classes
Also of note, the use of 'public' on arguments to the constructor is a shorthand that allows us to automatically create properties.

	class Student {
		fullname : string;
		constructor(public firstname, public middleinitial, public lastname) {
			this.fullname = firstname + " " + middleinitial + " " + lastname;
		}
	}

	interface Person {
		firstname: string;
		lastname: string;
	}

	function greeter(person : Person) {
		return "Hello, " + person.firstname + " " + person.lastname;
	}

	var user = new Student("Martin", "L", "Beeby");

	document.body.innerHTML = greeter(user);

Re-run `tsc greeter.ts` and you'll see the generated JavaScript is the same as the earlier code.
Classes in TypeScript are just a shorthand

# 5. Template Strings

	function greeter(person : Person) {
		return "Hello, " + person.firstname + " " + person.lastname;
	}

To make it better ES2015 now has template strings which allow this syntax:

	function greeter(person : Person) {
		return `Hello, ${person.firstname} ${person.lastname}`;
	}

Template strings are enclosed by the back-tick (\`) (grave accent) character instead of double or single quotes. On many keyboard the grave key
can be found under or near the `ESC` key (or next to the `ctrl` key on Mac keyboard).

You can also use this to span multiple lines. For example this is valid and the new lie will actually be interpreted.

	function greeter(person : Person) {
		return `Hello, ${person.firstname} ${person.lastname}
				this is valid`;
	}

Run the function through the TypeScript compiler and see how the resultant JS file include a new line.

# 6. Use in a site

Open the index.html file and add the following code. It's basically just adding the JavaScript file to our page.

	<!DOCTYPE html>
	<html>
		<head><title>TypeScript Greeter</title></head>
		<body>
			<script src="greeter.js"></script>
		</body>
	</html>

In the command line or terminal run:

	npm install

Then

	npm start

When the browser loads go to the url http://localhost:8080/lab-javascript/

# 7. For As loops

If we have an array of students we might want to loop through them. Here is a new Syntax called "for of". Once you compile the code you
will see that it uses a standard for loop in the .js file.

	var user = new Student("Martin", "L", "Beeby");
	var user1 = new Student("Martin", "Maverick", "Kearn");

	var userArray = [user,user1];

	for (var v of userArray) {
		document.body.innerHTML += greeter(v);
	}

# 8. Rest Parameters

Sometimes, you want to work with multiple parameters as a
group, or you may not know how many parameters a function will ultimately take. In JavaScript, you can work with
the arguments direction using the  arguments variable that is visible inside every function body.

	var user = new Student("Martin", "L", "Beeby");
	var user1 = new Student("Martin", "Maverick", "Kearn");
	var user2 = new Student("Mike", "The Hit Man", "Taulty");

	printStudents(user,user1, user2);

	function printStudents(...listOfStudents: Student[]) {
		for (var v of listOfStudents) {
			document.body.innerHTML += greeter(v);
		}
	}


# 9. Destructuring

A destructuring declaration introduces one or more named variables and initializes
them with values extracted from properties of an object or elements of an array.

	// Create 3 new variables and populate them from an array.
	// Now we have 3 new variables called beeby, kearn , taulty
	var [beeby, kearn, taulty] = printStudents(user,user1, user2);

	// We can now send the beeby object to be printed again.
	printStudents(beeby);

	function printStudents(...listOfStudents: Student[]) {
		for (var v of listOfStudents) {
			document.body.innerHTML += greeter(v);
		}
		return listOfStudents;
	}

# 10. Arrow Functions and using 'this'
How 'this' works in JavaScript functions is a common theme in programmers coming to JavaScript. Indeed, learning how to use it is something of a rite of passage as developers become more accustomed to working in JavaScript. Since TypeScript is a superset of JavaScript, TypeScript developers also need to learn how to use 'this' and how to spot when it's not being used correctly. A whole article could be written on how to use 'this' in JavaScript, and many have. Here, we'll focus on some of the basics.

 In JavaScript, 'this' is a variable that's set when a function is called. This makes it a very powerful and flexible feature, but it comes at the cost of always having to know about the context that a function is executing in. This can be notoriously confusing, when, for example, when a function is used as a callback.

 Let's look at an example:

	var deck = {
		suits: ["hearts", "spades", "clubs", "diamonds"],
		cards: Array(52),
		createCardPicker: function() {
			return function() {
				var pickedCard = Math.floor(Math.random() * 52);
				var pickedSuit = Math.floor(pickedCard / 13);

				return {suit: this.suits[pickedSuit], card: pickedCard % 13};
			}
		}
	};

	var cardPicker = deck.createCardPicker();
	var pickedCard = cardPicker();

	alert("card: " + pickedCard.card + " of " + pickedCard.suit);

If we tried to run the example, we would get an error instead of the expected alert box. This is because the 'this' being used in the function created by 'createCardPicker' will be set to 'window' instead of our 'deck' object. This happens as a result of calling 'cardPicker()'. Here, there is no dynamic binding for 'this' other than Window. (note: under strict mode, this will be undefined rather than window).

 We can fix this by making sure the function is bound to the correct 'this' before we return the function to be used later. This way, regardless of how its later used, it will still be able to see the original 'deck' object.

 To fix this, we switching the function expression to use the lambda syntax ( ()=>{} ) rather than the JavaScript function expression. This will automatically capture the 'this' available when the function is created rather than when it is invoked:

```
var deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    createCardPicker: function() {
        // Notice: the line below is now a lambda, allowing us to capture 'this' earlier
        return () => {
            var pickedCard = Math.floor(Math.random() * 52);
            var pickedSuit = Math.floor(pickedCard / 13);

            return {suit: this.suits[pickedSuit], card: pickedCard % 13};
        };
    }
};

var cardPicker = deck.createCardPicker();
var pickedCard = cardPicker();

alert(`card: ${pickedCard.card} of ${pickedCard.suit}`);
```
