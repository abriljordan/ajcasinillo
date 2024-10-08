---
title: Sauce Demo Test Plan
date: 2024-10-08
description: Sauce Demo test plan and test cases
tags: 
    - test plan
    - test cases
    - sauce demo website
categories:
    - test plan
    - test cases
    - sauce demo website
weight: 1
---

## Functionality Testing of the Sauce Demo website 

To test the functionality of the Sauce Demo website (https://www.saucedemo.com/), you can break the process down into the following steps:


### Identify Key Functional Areas to Test

* Login Functionality: Test different user credentials (standard user, locked-out user, invalid users).
* Product Page: Check if products are displayed, filtered, and sorted properly.
* Shopping Cart: Verify adding/removing products from the cart and viewing the cart.
* Checkout Process: Ensure that users can complete the checkout with valid/invalid data.
* Navigation: Test if all links and buttons work correctly.

## Manual Testing Steps

A. Login Functionality

Test valid credentials:

1.  Open the Sauce Demo site.
2.  Use standard_user as the username and secret_sauce as the password.
3.  Verify that you are redirected to the products page after login.

Test invalid credentials:

1.  Enter incorrect username or password and click “Login.”
2.  Verify that an error message appears: “Username and password do not match any user in this service.”

Test locked-out user login:

1.  Use locked_out_user as the username.
2.  Verify that an error message “Sorry, this user has been locked out” appears.

B. Product Page

Test product display:
1.  After logging in, ensure that the product inventory page displays all available products.
2.  Check if images, names, and prices are displayed for each product.

Test filtering and sorting:
1.  Use the sorting dropdown to sort products by price (low to high) or by name (A to Z).
2.  Verify that products are correctly sorted.

C. Shopping Cart

Test adding products to the cart:
1.  On the product page, click “Add to Cart” for a few items.
2.  Verify that the cart icon in the top-right corner updates the product count.

Test removing products from the cart:
1.  Open the cart by clicking the cart icon.
2.  Remove one or more products and verify that the product count updates accordingly.

D. Checkout Process

Test valid checkout:
1.  Add products to the cart and click “Checkout.”
2.  Enter valid information (first name, last name, postal code) and proceed.
3.  Ensure the order is placed, and a confirmation message appears.

Test invalid checkout:
1.  Attempt to checkout with empty or invalid fields.
2.  Verify that error messages appear when fields are invalid or missing.

E. Navigation

* Test the functionality of the navigation links and buttons (e.g., clicking the cart icon, going back to the products page, etc.).

## Test Plan for Sauce Demo Site

### Objective

The objective of this test plan is to ensure that the basic functionalities of the Sauce Demo site work correctly, with a focus on the login, product browsing, shopping cart, and checkout features.

### Scope

In Scope:
* Functional testing for user login, adding products to the cart, checkout, and product browsing.
* Out of Scope:
* Performance testing, non-functional testing, and security testing.

### Test Environment

* Devices: MacBook Air (using macOS)
* Browsers: Safari, Chrome, Firefox (based on what you can test)
* Operating System: macOS
* Dependencies: Sauce Demo website (https://www.saucedemo.com/)

### Testing Approach

* Manual Testing: Functional testing using different browsers on your MacBook.
* Automation (Optional): You can create automation scripts using tools like Selenium WebDriver if required.

### Features to be Tested

1.  Login Page
* Valid Login
* Invalid Login
* Locked-out user scenario
2.  Product Page
* View products
* Filter/sort products
3.  Shopping Cart
* Add product(s) to cart
* Remove product(s) from cart
* View cart items
4.  Checkout
* Checkout process
* Enter valid/invalid details
* Complete purchase

## Test Cases for Sauce Demo Site

| Case ID      | Case Description               | Steps                                                                                                         | Expected Result                                                                 | Status |
|--------------|------------------------------- |---------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|--------|
| TC_01        | Verify valid user login        | 1. Navigate to https://www.saucedemo.com/ <br> 2. Enter valid username and password <br> 3. Click on "Login"   | User is successfully logged in and redirected to the inventory page              |        |
| TC_02        | Verify login with invalid creds| 1. Enter invalid username and password <br> 2. Click on "Login"                                                | Error message is displayed, and the user remains on the login page               |        |
| TC_03        | Verify locked-out user login   | 1. Enter username "locked_out_user" <br> 2. Enter password <br> 3. Click on "Login"                            | Error message "Sorry, this user has been locked out" is displayed                |        |
| TC_04        | Verify product display         | 1. Navigate to the product page after login                                                                    | All available products are displayed                                             |        |
| TC_05        | Verify product filtering       | 1. Use the product filter/sorting dropdown <br> 2. Sort by price or name                                       | Products are correctly filtered based on the selection                           |        |
| TC_06        | Verify adding products to cart | 1. From the product page, click "Add to Cart"                                                                  | Product is added to the cart, and cart count increases                           |        |
| TC_07        | Verify removing products       | 1. Open the cart <br> 2. Click "Remove"                                                                        | Product is removed from the cart, and cart count decreases                       |        |
| TC_08        | Verify the checkout process    | 1. Add product(s) to the cart <br> 2. Click "Checkout" <br> 3. Enter valid details in the checkout form <br> 4. Complete purchase | Purchase is successfully completed, and confirmation message is displayed |        |
| TC_09        | Verify checkout with invalid   | 1. Add product(s) to the cart <br> 2. Click "Checkout" <br> 3. Enter invalid/empty details in the form         | Error message is displayed for invalid details                                   |        |


