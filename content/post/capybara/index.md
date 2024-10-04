---
title: Capybara test script for the Sauce Demo 
date: 2024-10-04
description: Software testing for the Sauce Demo website
tags: 
    - Capybara
categories:
    - Software Testing
    - Automation
    - Capybara
weight: 1
---

Here’s a basic example of a Capybara test for logging into Sauce Demo. This assumes you’re using RSpec for your testing framework.

First, make sure you have Capybara and Selenium installed. You can add them to your Gemfile if you’re using Bundler:

```bash
gem 'capybara'
gem 'selenium-webdriver'
```

Then run:

```bash
bundle install
```

Now, here’s a simple test to automate the login process using Capybara and RSpec:

```bash
# spec/saucedemo_login_spec.rb

require 'capybara/rspec'
require 'selenium-webdriver'

# Set up Capybara to use Selenium with Chrome
Capybara.default_driver = :selenium_chrome

RSpec.describe 'Sauce Demo Login', type: :feature do
  before do
    # Set up the base URL for the tests
    Capybara.app_host = 'https://www.saucedemo.com/'
  end

  it 'logs into Sauce Demo successfully' do
    # Visit the Sauce Demo login page
    visit('/')
    
    # Fill in the login form
    fill_in 'user-name', with: 'standard_user'
    fill_in 'password', with: 'secret_sauce'
    
    # Click the login button
    click_button 'Login'
    
    # Expectation: Verify the user is redirected to the inventory page
    expect(page).to have_content('Products')
  end
end
```

Explanation:

	1.	Capybara Setup: We’re using selenium_chrome as the driver to run the tests in the Chrome browser.
	2.	RSpec Test: The test navigates to the login page, fills in the username and password fields (standard_user and secret_sauce), clicks the login button, and then verifies that the user has been redirected to the “Products” page, indicating a successful login.
	3.	Capybara Matchers: expect(page).to have_content('Products') ensures that after login, the page contains the word “Products,” confirming we’re on the inventory page.

Running the Test:

To run this test, you can execute:

```bash
rspec spec/saucedemo_login_spec.rb
```

This test will open a browser window, interact with the page, and check the login functionality of the Sauce Demo website.