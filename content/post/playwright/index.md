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


Playwright test script for the Sauce Demo website (https://www.saucedemo.com/) that tests logging in, adding an item to the cart, and checking out.

## Requirements:
    You need to have Node.js and Playwright installed. 
    If you haven’t installed Playwright, you can do so with the following command:
```bash
npm init playwright@latest
```

## Sample Playwright Test Script:

```js
const { test, expect } = require('@playwright/test');

test.describe('Sauce Demo Test Suite', () => {

  test.beforeEach(async ({ page }) => {
    // Go to the Saucedemo website
    await page.goto('https://www.saucedemo.com/');
  });

  test('Login with standard user and verify inventory page', async ({ page }) => {
    // Login credentials
    const username = 'standard_user';
    const password = 'secret_sauce';

    // Fill in username and password
    await page.fill('#user-name', username);
    await page.fill('#password', password);

    // Click on the login button
    await page.click('#login-button');

    // Verify we are on the inventory page
    await expect(page).toHaveURL('https://www.saucedemo.com/inventory.html');
    await expect(page.locator('.inventory_list')).toBeVisible();
  });

  test('Add an item to the cart and proceed to checkout', async ({ page }) => {
    // Login
    await page.fill('#user-name', 'standard_user');
    await page.fill('#password', 'secret_sauce');
    await page.click('#login-button');

    // Verify we are on the inventory page
    await expect(page).toHaveURL('https://www.saucedemo.com/inventory.html');

    // Add the first item to the cart
    await page.click('button[data-test="add-to-cart-sauce-labs-backpack"]');

    // Verify the item is added by checking the cart count
    const cartBadge = page.locator('.shopping_cart_badge');
    await expect(cartBadge).toHaveText('1');

    // Click on the cart icon to view the cart
    await page.click('.shopping_cart_link');

    // Verify we are on the cart page
    await expect(page).toHaveURL('https://www.saucedemo.com/cart.html');
    await expect(page.locator('.cart_item')).toBeVisible();

    // Proceed to checkout
    await page.click('button[data-test="checkout"]');

    // Verify we are on the checkout step one page
    await expect(page).toHaveURL('https://www.saucedemo.com/checkout-step-one.html');

    // Fill in checkout information
    await page.fill('#first-name', 'John');
    await page.fill('#last-name', 'Doe');
    await page.fill('#postal-code', '12345');

    // Click continue
    await page.click('button[data-test="continue"]');

    // Verify we are on the checkout step two page
    await expect(page).toHaveURL('https://www.saucedemo.com/checkout-step-two.html');

    // Finish checkout
    await page.click('button[data-test="finish"]');

    // Verify we are on the confirmation page
    await expect(page).toHaveURL('https://www.saucedemo.com/checkout-complete.html');
    await expect(page.locator('.complete-header')).toHaveText('THANK YOU FOR YOUR ORDER');
  });

});
```

## What does this test do?

	1.	Login Test:
	    •	Logs in using the standard user credentials.
	    •	Verifies that the inventory page loads correctly.
	2.	Add Item to Cart and Checkout:
	    •	Adds the Sauce Labs Backpack to the cart.
	    •	Verifies the item is added by checking the cart badge.
	    •	Proceeds to checkout, fills in the necessary details, and finishes the checkout process.
	    •	Verifies that the order confirmation page loads.

## Running the Test

	1.	Save this script in your tests folder (e.g., as saucedemo.test.js).
	2.	Run the test using the following command:
```bash
npx playwright test
```

This script covers the core functionality of logging in, adding an item to the cart, and completing a purchase on the Sauce Demo website. You can expand the test suite to include more test cases as needed!