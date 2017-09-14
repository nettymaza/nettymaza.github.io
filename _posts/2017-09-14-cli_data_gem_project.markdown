---
layout: post
title:  "CLI Data Gem Project"
date:   2017-09-14 16:21:15 -0400
---

![Powered by friendship and Utah hiking ](https://i.imgur.com/kVjvlhS.jpg?1)

## A small guide to get better at scraping Ajax loaded sites 

I must say this is what I was looking forward to the most, the part when you learn how to put things together to help fully understand how things work. Although I wasn’t prepared for the struggles that were to come, I am fully satisfied with the learning process.  

Starting my project went fairly smooth, I had some ideas of what I wanted to do and after finishing TTT with CLI I definitely knew I wanted something simple and to the point. I started looking around for websites to scrape, after some searching around I had my eyes set on one of my favorites called https://canopy.co/ so I made the decision that was the one and I was going to stick to it. How hard could it be? Pretty similar to the student’s page we scraped on Student Scraper Lab. But let me tell you about this site, it is beautifully crafted to display curated functional items you will instantly want to spend all your money on. 

My worry was figuring out how to even start? Lucky for me, the first thing I ran across when getting to the project’s page was Avi’s CLI Data Gem walkthrough, whew! lifesaver! he thinks of everything!. Okay, a little scary starting my own gem, and if we’re being honest here, I didn’t even know exactly what a gem was, but rest assured that now I do. I carefully followed Avi’s video so I could get all my files to communicate properly, I got my read/write permissions setup, I learned how to go into the gemspec file to set up pry and nokogiri and I revisited my knowledge of github to remind myself how to setup a remote repo and connect it to my local one.  Felt very cool, super accomplished, after all I am a developer in the making, sweet! It is now time to make a plan for how I want my CLI to work. 

##### *But first… *

Let’s talk about scraping and how the way websites load can affect it. This is an important topic for me, specially because it took me a while to understand, and it is something that in my two months of wanting to be a web developer had never come across. 

#### Nokogiri

To fully understand this let’s revisit Nokogiri and what it does; described as a parser, it is most often used to extract data from structured documents, mainly HTML data, nokogiri then allows us to treat this HTML data as if it were nested nodes.

A good definition for what a parser is, in this case Nokogiri is this quote I found in a website. 
> “something that turns some kind of data (usually a string) into another kind of data (usually a data structure)”. 

You can read more about Nokogiri here: https://www.sitepoint.com/nokogiri-fundamentals-extract-html-web/

I structured my CLI so that it would greet the user, provide a short description of what this gem is about and finally display a list of categories each with an index number, which the user could select to then display a list of products within that category. When scraping the categories, I could not believe how easy it was, they all had classes and I was able to easily target the name and url for each one of them. They displayed according to plan, it was a success, what an easy project!.

Then time came to scrape the products within each category, my plan was to use the url I had collected from the first scrape to target the needed items. To my surprise nothing I was seeing on the actual website was pulling up in pry. Nightmare! Ahh! and Why?! 

After much searching around, I ran into this site: https://github.com/JonasCz/How-To-Prevent-Scraping
Which is probably not the thing you want to learn about when you’re actually scraping, but something was preventing my scraping journey so I kept reading.

I found this method as a way to prevent scraping: 

##### Use JavaScript + Ajax to load your content

You could use JavaScript + AJAX to load your content after the page itself loads. This will make the content inaccessible to HTML parsers which do not run JavaScript. This is often an effective deterrent to newbie and inexperienced programmers writing scrapers. 

They're totally referring to me, the newbie/inexperienced programmer. Ha!

I’m just trying to learn over here guys!. With lots of help and searching around for solutions, I came across Watir Webdriver in these sites:

https://stackoverflow.com/questions/19090032/scraping-ajax-enabled-webpages
https://stackoverflow.com/questions/13789583/html-is-read-before-fully-loaded-using-open-uri-and-nokogiri

#### So here is a Solution:
1. Install Watir
2. Install Homebrew
3. Install chromedriver

#### Install Watir
Follow watir install instructions from this link to set it up:
http://watir.com/guides/installation/
I added it to my gem spec file like this:

```
spec.add_dependency "watir"
```

#### Install Homebrew
https://docs.brew.sh/Installation.html

###### This link will talk about using the appropriate driver, in my case Google Chrome: chromedriver.
http://watir.com/guides/chrome/
*Note* Homebrew will have to be installed before this.

```
brew install chromedriver
```

#### Watir::Wait.until 
Waits until block evaluates to true or times out.
http://www.rubydoc.info/gems/watir-webdriver/Watir/Wait

```
Watir::Wait.until { browser.text_field(name: "new_user_first_name").visible? }
```

#### This is what my final code looks like in my scraper.rb file:

```
require 'watir'

def get_products(url_to_scrape)
		browser = Watir::Browser.new :chrome
		browser.goto url_to_scrape
		Watir::Wait.until { browser.divs(:class => "feed-card").length > 0 }
		webpage = Nokogiri::HTML(browser.html)
end
```

#### This is what my method looked like in case you want to see that too:
```
def fetch_products
      puts "Fetching products from #{@products_list_url}"
      @products.clear
      products = Scraper.new.get_products(@products_list_url)
      products.css(".product-card").each do |product|
        product_name = product.css(".product-details-name span").text
        product_price = product.css(".buy-button-container span").text
        amazon_url = product.css(".buy-button-container a").attr('href').value
        product = Product.new(product_name, product_price, self, amazon_url)
        @products << product
      end
    end
```

#### Success!!
Final results were a list of beautifully displayed items from my chosen category. I must say I will never visit https://canopy.co/ and fondly look at their products with eager eyes. But I learned so so much, I hope this helps someone in the future. Now on to finish the details of my project so I get this sucker in by the end of the week. 











