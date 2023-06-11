---
title: "Writing Clean and Readable Code with If-Else Statements in Python"
seoTitle: "Mastering If and Else Statements in Python - A Comprehensive Guide"
seoDescription: "Learn everything you need to know about If and Else statements in Python with our comprehensive guide. From basic syntax to advanced techn"
datePublished: Thu Mar 16 2023 10:21:49 GMT+0000 (Coordinated Universal Time)
cuid: clfayour700090al0h6dm17u0
slug: writing-clean-and-readable-code-with-if-else-statements-in-python
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/vXInUOv1n84/upload/90480f35f49e2a84365245a0f3e03ce6.jpeg
tags: python, python-beginner, if-else

---

## Everything you'd ever need to know about if and else statements.

If you ever had to use a robot, say an ATM, you probably know that they need to be told what to do in a certain way, otherwise they won't understand. An ATM would keep staring at you if you input your card without asking it to dispense money to you. In programming, we use something called "if-else statements" to tell the computer what to do in a certain situation, just like you would give instructions to a robot.

For example, let's say you have a robot that can move forward, but only if there's nothing in its way. You would use an if-else statement in Python to give it that instruction. Here's what it might look like:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678959786677/03c539d3-637a-489c-9a37-ce57eec76b53.jpeg align="center")

In this example, the 'if' statement checks if there's nothing in front of the robot. If that's true, it runs the code inside the statement (in this case, moving the robot forward). If it's false (meaning there is something in front of the robot), it runs the code in the else statement (stopping the robot).

Here's another example. Let's say you have a robot that can change its color based on what it sees. If it sees something red, you want the robot to stop moving. If it sees something green, you want the robot to keep moving. Here's how you might do that in Python:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678959915320/29836ed7-e66d-49ea-a1fb-3ff01c6f9339.jpeg align="center")

In this example, the 'if' statement checks if the object color is equal to "red". If it's true, it stops the robot. If it's false (meaning the object is not red), it runs the code in the else statement (keeping the robot moving).

So, if-else statements are a way to tell the computer what to do in different situations, just like you would tell a robot what to do in different situations.

# Nested 'if' statements

In Python, we can use an "if" statement to check a condition and do something if that condition is true. For example, we might use an "if" statement to check if the weather is rainy, and if it is, we might decide to stay inside and play games.

Sometimes, we might want to check more than one condition before we decide what to do. That's where nested if statements come in!

A nested if statement is when we put an "if" statement inside another "if" statement. It's like putting a toy inside a toy box, and then putting that toy box inside another toy box. We keep opening boxes until we find the toy we're looking for!

Here's a simple example of a nested if statement in Python:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678960406289/73e3043a-5bfc-47fe-addc-5e7bd08aad1f.jpeg align="center")

In this example, we check if the age is greater than 5 with the first "if" statement. If it is, we check if the age is less than 10 with the second "if" statement. If it is, we print a message saying that the age is between 6 and 9.

Now, let's look at a more complex example of a nested if statement:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678961619015/eb6cbd59-91fb-43eb-b191-5ff811423b5a.jpeg align="center")

In this example, we check if the color is "red" with the first "if" statement. If it is, we check if the number is greater than 5 with the second "if" statement. If it is, we print a message saying that the person likes red and picked a big number. If the number is less than or equal to 5, we print a message saying that the person likes red but picked a small number.

If the color is not "red", we move on to the next "if" statement and check if the color is "green". If it is, we check if the number is greater than 5 with the second "if" statement. If it is, we print a message saying that the person likes green and picked a big number. If the number is less than or equal to 5, we print a message saying that the person likes green but picked a small number.

If the color is not "red" or "green", we move on to the "else" statement and print a message saying that we're not sure what color the person likes.

So, that's what nested if statements are in Python! They're a way to check more than one condition before deciding what to do, and they can be used in simple or complex situations.