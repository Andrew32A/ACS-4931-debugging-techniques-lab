# Debug Log

**Explain how you used the the techniques covered (Trace Forward, Trace Backward, Divide & Conquer) to uncover the bugs in each exercise. Be specific!**

In your explanations, you may want to answer:

- What is the expected vs. actual output?
- If there is a stack trace, what useful information does it contain?
- Which technique did you use, on which line numbers?
- What assumptions did you have about each line of code, and which ones were proven to be wrong?

_Example: I noticed that the program should show pizza orders once a new order is made, and that it wasn't showing any. So, I used the trace forward technique starting on line 13. I discovered the bug on line 27 was caused by a typo of 'pzza' instead of 'pizza'._

_Then I noticed another bug ..._

## Exercise 1

- Expected vs actual output: Expected pizza order form to work and display unfulfilled orders , but got an error `TypeError: 'topping' is an invalid keyword argument for PizzaTopping`
- Stack trace: `TypeError: 'topping' is an invalid keyword argument for PizzaTopping` which leads me to believe there's an error in the loop on line 78
- Technique: Trace backwards from line 78 and noticed that we needed a `.form.getlist` instead of `.form.get` and we needed to iterate through `toppings_list` instead
- Assumptions: `topping` is not a valid argument for `PizzaTopping` and `toppings_list` is not a list of toppings

### Detailed steps to solution:

- Noticed that the `topping` argument in `PizzaTopping(topping=toppings_list)` was not a valid argument so I used the trace backward technique to find where `topping` was defined
- Tried changing `for topping_str in ToppingType:` to `for topping_str in toppings_list:`
- Added a try catch for better error logs
- Noticed `return redirect(url_for('/'))` was incorrect and should go to `home` instead of `/`
- Added `toppings_list = request.form.getlist('toppings')` to get the list of toppings
- Replaced `pizza.toppings.append(PizzaTopping(topping=topping_str))` with `pizza.toppings.append(PizzaTopping(topping_type=topping_str))`
- Replaced `size` with `pizza_size`
- Noticed db wasn't storing pizza so I added `db.session.commit()` below the add in the order route
- Added conditional for home.html to check for pizza orders
- Renamed name in order name to `order_name` for better clarity
- Noticed it was displaying all toppings despite what the user selected, so I changed `for topping_str in ToppingType:` to `for toppings in toppings_list:`

## Exercise 2

[[Your answer goes here!]]

## Exercise 3

[[Your answer goes here!]]
