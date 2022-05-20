<br />

<p align="center"> 
  <a href="https://rently-io.herokuapp.com/">
    <img src="https://i.imgur.com/QA6KWPy.png" alt="Logo" width="auto" height="70">
  <a/>
</p>

<h4 align="center">All you can rent on <a href="https://rently-io.herokuapp.com/">Rently</a>!</h4>
<p align="center">Rently is a second year university full stack project that <a href="https://github.com/greffgreff">I developed</a> over the course of a few months. I have no genuine intention of deploying this application for real-life use. 🌈 Feel free to contribute to the project!</p>

<br />

<p align="center" style="align: center;">
  <a href="https://github.com/rently-io/user-service/" >
    <img src="https://github.com/rently-io/user-service/actions/workflows/ci.yml/badge.svg" />
  </a>
  <a href="https://github.com/rently-io/listing-service/" >
    <img src="https://github.com/rently-io/listing-service/actions/workflows/ci.yml/badge.svg" />
  </a>
  <a href="https://github.com/rently-io/search-service/" >
    <img src="https://github.com/rently-io/search-service/actions/workflows/ci.yml/badge.svg" />
  </a>
  <a href="https://github.com/rently-io/image-service/" >
    <img src="https://github.com/rently-io/image-service/actions/workflows/ci.yml/badge.svg" />
  </a>
  <a href="https://github.com/mailer-io/image-service/" >
    <img src="https://github.com/rently-io/mailer-service/actions/workflows/ci.yml/badge.svg" />
  </a>
</p>

<br />

# About

This project consists of 5 micro-services. Together with a frontend, they make Rently a simple yet complete platform on which users can quickly create and post items for rent to the public. The whole process seriously takes a few seconds! 

Registration does not require users to create a dedicated account; simply login with your favorite social provider instead. The rest is taken care of in the backend. In fact, the site does not handle any sensitive user data such as passwords.

The site is also equiped with an extensive search engine able to perform simple keyword searches to geolocation and nearby searches through the use of database indices and external APIs such Tomtom's [fuzzy](https://developer.tomtom.com/search-api/documentation/search-service/search-service) and [geo](https://developer.tomtom.com/search-api/documentation/geocoding-service/geocoding-service) endpoints along with Google's [javascript interactive map](https://developers.google.com/maps/documentation/javascript/overview) API.

> ⚠️ Please note that the services are currently deployed on a free Heroku instance and need a few seconds to warm up!
> 
<details>
  <summary><b>Table of contents</b></summary>
    
  <h3>System overview</h3>
  <ul>
    <li>
      <a href="https://github.com/rently-io/.github/blob/main/profile/README.md#a-full-stack-project">
        A full stack project
      </a>
    </li>
    <li>
      <a href="https://github.com/rently-io/.github/blob/main/profile/README.md#c2-model">
        C2 model
      </a>
    </li>
  </ul>
</details>

<br />

# System overview

### A full stack project

The frontend was built in Typescript with the NextJS framework as it offers a number of adantages over ReactJS, the framework on which NextJS was built upon. Not only can it server requests at greater speeds, but also has support for CSS modules, making frontend component development much easier to style. 

More importantly, NextJS has server-side rendering. This is significant as sensitive things such as API keys and Json Web Token generation is not exposed to the client. More on JWTs [here]().

The backend uses Spring Boot

### C2 model
![C2 model](https://i.imgur.com/CqQbDQA.png)

![rently](https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=rently&include_all_commits=true&show_owner=true)
![user-service](https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=user-service&include_all_commits=true&show_owner=true)  
![listing-service](https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=listing-service&include_all_commits=true&show_owner=true)  
![search-service](https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=search-service&include_all_commits=true&show_owner=true)  
![mailer-service](https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=mailer-service&include_all_commits=true&show_owner=true)  
![image-service](https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=image-service&include_all_commits=true&show_owner=true)  

# OAuth implementation 

This is how I implemented OAuth and manage user data.
