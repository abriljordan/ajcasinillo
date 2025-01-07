---
title: Playwright test script for the Sauce Demo 
date: 2024-10-04
description: Software testing for the Sauce Demo website
tags: 
    - Playwright
categories:
    - Software Testing
    - Automation
    - Playwright
weight: 1
---


Using Playwright for automated testing is a powerful way to handle modern web applications. It provides better performance, multi-browser support, and rich APIs for handling complex scenarios. Below is an example of how to write detailed automation scripts with assertions, handle complex scenarios, and save logs for the Sauce Demo website.

## Setting Up Playwright

1.  Install Playwright:
```bash
npm init playwright@latest
```

2.  Set up a new project, install dependencies, and configure the necessary files.

### Test Scenario 1: Valid User Login and Product Purchase Flow

This script will:

* Log in to the website using valid credentials.

* Add multiple products to the cart.

* Complete the purchase.

* Save logs of the script execution.


## Playwright script with assertions and log saving functionality.

```js
const { test, expect } = require('@playwright/test');
const fs = require('fs');

// Test block
test('Valid user login and product purchase', async ({ page }) => {
  // Log file setup
  const logFile = fs.createWriteStream('test_logs.txt', { flags: 'a' });
  const log = (message) => {
    console.log(message);
    logFile.write(`${new Date().toISOString()} - ${message}\n`);
  };

  try {
    log('Starting test: Valid user login and product purchase');

    // 1. Navigate to the Sauce Demo site
    await page.goto('https://www.saucedemo.com/');
    log('Navigated to Sauce Demo site');

    // 2. Login with valid credentials
    await page.fill('#user-name', 'standard_user');
    await page.fill('#password', 'secret_sauce');
    await page.click('#login-button');
    log('Logged in with valid credentials');

    // 3. Verify successful login by checking URL contains 'inventory'
    await expect(page).toHaveURL(/.*inventory/);
    log('Login successful: User on inventory page');

    // 4. Add multiple products to the cart
    await page.click('#add-to-cart-sauce-labs-backpack');
    await page.click('#add-to-cart-sauce-labs-bike-light');
    log('Added two products to the cart');

    // 5. Navigate to the cart page
    await page.click('.shopping_cart_link');
    log('Navigated to the cart page');

    // 6. Verify that two items are in the cart
    const cartItems = await page.locator('.cart_item');
    expect(await cartItems.count()).toBe(2);
    log('Verified that there are two items in the cart');

    // 7. Proceed to checkout
    await page.click('#checkout');
    await page.fill('#first-name', 'John');
    await page.fill('#last-name', 'Doe');
    await page.fill('#postal-code', '12345');
    await page.click('#continue');
    log('Entered checkout information and proceeded');

    // 8. Complete the purchase
    await page.click('#finish');
    await expect(page.locator('.complete-header')).toHaveText('THANK YOU FOR YOUR ORDER');
    log('Purchase completed successfully');

  } catch (error) {
    log(`Error during test execution: ${error}`);
    throw error;
  } finally {
    // Close log file
    logFile.end();
  }
});
```

Key Points

1. Assertions:

* expect(page).toHaveURL(/.*inventory/): Verifies that the user is redirected to the inventory page.
  
* expect(cartItems.count()).toBe(2): Verifies that two items are in the cart.
  
* expect(page.locator('.complete-header')).toHaveText('THANK YOU FOR YOUR ORDER'): Verifies the order completion message.
  
2. Logging:
  
* All the actions and test results are logged in the test_logs.txt file.
  
* The log function handles console logging and writes the messages to the log file.


### Test Scenario 2: Invalid Login and Error Handling

This script will test login failure using invalid credentials and verify that an appropriate error message is displayed.

```js
test('Invalid login shows error message', async ({ page }) => {
  const logFile = fs.createWriteStream('test_logs.txt', { flags: 'a' });
  const log = (message) => {
    console.log(message);
    logFile.write(`${new Date().toISOString()} - ${message}\n`);
  };

  try {
    log('Starting test: Invalid login shows error message');

    // 1. Navigate to the Sauce Demo site
    await page.goto('https://www.saucedemo.com/');
    log('Navigated to Sauce Demo site');

    // 2. Attempt login with invalid credentials
    await page.fill('#user-name', 'invalid_user');
    await page.fill('#password', 'wrong_password');
    await page.click('#login-button');
    log('Attempted login with invalid credentials');

    // 3. Verify error message appears
    const errorMessage = await page.locator('[data-test="error"]');
    await expect(errorMessage).toBeVisible();
    await expect(errorMessage).toHaveText('Username and password do not match any user in this service');
    log('Error message verified: Invalid credentials');

  } catch (error) {
    log(`Error during test execution: ${error}`);
    throw error;
  } finally {
    logFile.end();
  }
});
```

### Test Scenario 3: Sorting Products by Price (Low to High)

This script tests that the product sorting feature works correctly.

```js
test('Sort products by price (low to high)', async ({ page }) => {
  const logFile = fs.createWriteStream('test_logs.txt', { flags: 'a' });
  const log = (message) => {
    console.log(message);
    logFile.write(`${new Date().toISOString()} - ${message}\n`);
  };

  try {
    log('Starting test: Sort products by price (low to high)');

    // 1. Navigate to the Sauce Demo site and log in
    await page.goto('https://www.saucedemo.com/');
    await page.fill('#user-name', 'standard_user');
    await page.fill('#password', 'secret_sauce');
    await page.click('#login-button');
    log('Logged in with valid credentials');

    // 2. Sort products by price (low to high)
    await page.selectOption('.product_sort_container', 'lohi');
    log('Sorted products by price (low to high)');

    // 3. Verify sorting (check first and last product prices)
    const prices = await page.$$eval('.inventory_item_price', elements => elements.map(el => parseFloat(el.textContent.replace('$', ''))));
    log(`Prices fetched: ${prices}`);

    expect(prices).toEqual(prices.slice().sort((a, b) => a - b));
    log('Verified that products are sorted by price (low to high)');

  } catch (error) {
    log(`Error during test execution: ${error}`);
    throw error;
  } finally {
    logFile.end();
  }
});
```

Conclusion

By using Playwright with assertions and complex scenarios like sorting, adding/removing items, and handling failed logins, you can achieve thorough automation testing for the Sauce Demo website. The inclusion of detailed logging, error handling, and data-driven testing makes the scripts robust and suitable for use in Continuous Integration (CI) pipelines.