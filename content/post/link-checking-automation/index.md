---
title: Automate Link Checking 
date: 2024-10-04
description: Automate Using Python and Ruby Script
tags: 
    - Python
    - Ruby
categories:
    - Python
    - BeautifulSoup
    - requests
weight: 1
---

To automate checking whether the links on a website are working, you can use several tools and methods, depending on your preference for programming or using external services. 

## Python
Python is a great option if you want more control and automation. You can write a script that crawls your website and checks the status of each link using libraries like requests and BeautifulSoup.

Here’s an example:

```python
import requests
from bs4 import BeautifulSoup

# Function to check if the URL is working
def check_url(url):
    try:
        response = requests.get(url)
        if response.status_code == 200:
            return f"{url} is working!"
        else:
            return f"{url} returned {response.status_code}."
    except requests.exceptions.RequestException as e:
        return f"{url} failed to load. Error: {str(e)}"

# Function to scrape all links from a given page
def get_links_from_page(page_url):
    try:
        response = requests.get(page_url)
        soup = BeautifulSoup(response.content, "html.parser")
        links = []
        for link in soup.find_all('a', href=True):
            links.append(link['href'])
        return links
    except Exception as e:
        return f"Error scraping links from {page_url}: {str(e)}"

# URL of the website to check
website_url = "https://yourwebsite.com"
links = get_links_from_page(website_url)

for link in links:
    print(check_url(link))
```

## Ruby
You can automate checking links on a website using Ruby by leveraging libraries like nokogiri to parse HTML and net/http to check link statuses.

Here’s a simple Ruby script to check the links of a website:

Step-by-step guide:

	1.	Install necessary libraries:
Run the following command to install the required libraries if you don’t have them installed:

```bash
gem install nokogiri
```

	2.	Create the Ruby script:

```ruby
require 'net/http'
require 'nokogiri'
require 'uri'

# Function to check if the link is working
def check_link(url)
  begin
    uri = URI.parse(url)
    response = Net::HTTP.get_response(uri)
    if response.code == "200"
      puts "#{url} is working!"
    else
      puts "#{url} returned status code: #{response.code}"
    end
  rescue => e
    puts "Error checking #{url}: #{e.message}"
  end
end

# Function to extract all links from a given page
def get_links(page_url)
  begin
    uri = URI.parse(page_url)
    response = Net::HTTP.get(uri)
    page = Nokogiri::HTML(response)
    links = page.css('a').map { |link| link['href'] }.compact
    return links
  rescue => e
    puts "Error fetching links from #{page_url}: #{e.message}"
    return []
  end
end

# URL of the website you want to check
website_url = "https://yourwebsite.com"

# Get all links from the website's homepage
links = get_links(website_url)

# Check each link
links.each do |link|
  # Handle relative URLs by converting them to absolute
  if link.start_with?('/')
    full_link = URI.join(website_url, link).to_s
  else
    full_link = link
  end

  check_link(full_link)
end
```

How the script works:

	1.	get_links function:
	    •	This function takes a page URL and uses nokogiri to extract all the <a> tag links from the webpage.
	    •	It returns an array of links.
	2.	check_link function:
	    •	This function takes a link, makes an HTTP request using net/http, and checks the status code.
	    •	It will print whether the link is working or if it returned an error code (like 404, 500, etc.).
	3.	Handling Relative URLs:
	    •	The script ensures that relative URLs (like /about-us) are converted into absolute URLs.

Running the script:

	1.	Save the script in a file, e.g., check_links.rb.
	2.	Run the script with:

```bash
    ruby check_links.rb
```

This script can be scheduled to run periodically using cron jobs for continuous monitoring. You can also modify it to save the results to a log file or send a notification when broken links are detected.