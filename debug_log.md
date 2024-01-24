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

- Expected vs actual output: Expected pizza order form to work and display unfulfilled orders, but got an error `TypeError: 'topping' is an invalid keyword argument for PizzaTopping`
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

- Expected vs actual output: Expected the weather to retrieve properly, but ran into an internal server error with a KeyError of `'name'` in `'city': result_json['name']`
- Stack trace: `KeyError: 'name'`, `line 52, in results 'city': result_json['name'],`
- Technique: Trace backwards from line 52
- Assumptions: Assumed that `'city': result_json['name']`, turns out that the API requires `'q': city` in the params

### Detailed steps to solution:

- Identified the KeyError: 'name' on line 52, indicating a missing/incorrect key
- Traced back to the API call and recognized the incorrect parameter `'place'`: city
- Updated the API call parameters by changing `'place': city` to `'q': city` for correct API usage
- Corrected the key for temperature access by changing temperature to temp in line with API's JSON structure
- Replaced `'users_city'` and `'requested_units'` in request.args.get with `'city'` and `'units'`
- Implemented error handling for the API response to manage potential data retrieval issues
- Noticed an error in `<h2>What's the weather like today?</h1>` and updated to h2

## Exercise 3

- Expected vs actual output: Expected `merge_sort` to sort the list, but got an error `IndexError: list index out of range`. Once fixed, noticed that the sorting order was reversed, and the binary search did not find the correct index
- Stack trace: `IndexError: list index out of range`, `line 37, in merge_sort arr[k] = right_side[i]`. Merge sort and binary search also had strange behavior
- Technique: Trace backwards from line 37. Then had to use divide and conquer to fix functionality
- Assumptions: Assumed `merge_sort` had to be reversed, k wasn't being accounted for, and binary_search's mid was incorrect

### Detailed steps to solution:

- Noticed that the comparison in `merge_sort` was `if left_side[i] > right_side[j]:`, this sorted the array in descending order
- Updated the comparison in `merge_sort` to `if left_side[i] < right_side[j]:` for ascending order
- Identified an incorrect index reference in `merge_sort` while merging the right side of the array. It was incorrectly using the index `i` instead of `j`
- Changed the line `arr[k] = right_side[i] to arr[k] = right_side[j]` in the merge_sort function
- Noticed the binary search used a float for calculating `mid: (high + low) / 2`
- Updated the mid calculation in `binary_search` to use integer division: `(high + low) // 2`
- Noticed that the loop condition in `binary_search` was `while low < high:` which missed the case where low equals high
- Changed the loop condition in `binary_search` to `while low <= high:` to include this case
- Added `return -1` at the end of `binary_search` to handle cases where the element is not found
