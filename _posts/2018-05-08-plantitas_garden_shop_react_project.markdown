---
layout: post
title:      "Plantitas Garden Info React Project"
date:       2018-05-08 08:34:11 -0400
permalink:  plantitas_garden_shop_react_project
---



Its here! Its finally here! My project review is pending but I am so excited to have gotten my React Redux project to a presentable state.

For my final project I made an application called Plantitas Garden Info. This app is basically a garden almanac, where you can look for information about flowers and plants, how to plant them and get info about soil  and light requirements. It is still a work in progress because the data was created/seeded by myself in my  Rails 5 API, but I'm hoping to eventually connect it to a more hefty API with all the right info.

This application is comprised of a Rails 5 backend API and a React-Redux frontend. Getting set up with Rails API and React together can be a bit confusing at the beginning but after doing a bit of exploring and reading around I finally got started with setting up my application.. The very first thing I did was creat parent folder that would contain both my back-end (Rails API ) and my frontend (React-Redux).

I started by working with my Rails API. I created my database, models and controller in the backend. I played around in the rails console to make sure my model objects were being created, edited and deleted correctly and then I decided it was time to seed my database. I found a buch of plant photos and then proceeded to troll all the sites about plant information/descriptions to get the info I needed to seed my database. 

Then I moved on to my frontend, the client side. The very first thing I did was getting a collection of plant items to render from my back end. It was tricky because that's where I had to pass around the props and state, create my store and get redux set up to properly make my fetch call to my back end API. After that I decided I wanted to be able to create more plant cards so I composed a form that took me forever to get working, but it now is able to add plant items to my collections. My project needed some structure, so it was time to add in my NavBar with my links. The home Home page, the Plants page and the nested route Add a new Plant, which would display within my main PlantsPage component. 

This projects has been pretty challenging, I must say I still have a ton to learn about React-Redux. My plan is to take this application further and make it really cool. Some things on my list of improvements are adding form validations, connecting it a third party API to get better data, and adding an edit and delete buttons for each card.






