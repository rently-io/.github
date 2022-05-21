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

<details>
  <summary><b>Table of contents</b></summary>
    
  <h3>System overview</h3>
  <ul>
    <li>
      <a href="#a-full-stack-project">A full stack project</a>
    </li>
    <li>
      <a href="#c2-model">C2 model</a>
    </li>
  </ul>
  
  <h3>Software quality</h3>
  <ul>
    <li>
      <a href="#security-concerns">Security concerns</a>
    </li>
    <li>
      <a href="#oauth-implementation">OAuth implementation</a>
    </li>
    <li>
      <a href="#jwt-implementation">JWT implementation</a>
    </li>
  </ul>

  <h3>Release management</h3>
  <ul>
    <li>
      <a href="#cicd">CI/CD</a>
    </li>
    <li>
      <a href="#testing">Testing</a>
    </li>
  </ul>
  
  <h3>About the repositories</h3>
  <ul>
    <li>
      <a href="#frontend">Frontend</a>
    </li>
    <li>
      <a href="#user-service">User Service</a>
    </li>
    <li>
      <a href="#listing-service">Listing Service</a>
    </li>
    <li>
      <a href="#search-service">Search Service</a>
    </li>
    <li>
      <a href="#mailer-service">Mailer Service</a>
    </li>
    <li>
      <a href="#image-service">Image Service</a>
    </li>
  </ul>
  
  Reflection + Works cited
</details>

<br />

# System overview

### A full stack project
The frontend was built in Typescript with the NextJS framework as it offers a number of adantages over ReactJS, the framework on which NextJS was built upon. Not only can it server requests at greater speeds, but also has support for CSS modules, making frontend component development much easier to style. 

More importantly, NextJS has server-side rendering. This is significant as sensitive things such as API keys and Json Web Token generation is not exposed to the client. [...] More on JWTs under <a href="#jwt-implementation">JWT implementation</a>.

The backend services were developed with Spring Boot in Java. They follow industry best practices including expernalised configurations, environment specific variables, along with CORS and JWTs as well. All services are deployed on Heroku PaaS automatically. More on release management under <a href="#cicd">CI/CD</a>.

> ⚠️ Please note that the services are currently deployed on a free Heroku instance and need a few seconds to warm up when first visiting the website!

### C2 model
![C2 model](https://i.imgur.com/CqQbDQA.png)

# Software quality

### Security concerns
Security was among the top non-functional requirements for the Rently. Multiple security aspects were addressed on [OWASP's top 10 list](https://owasp.org/www-project-top-ten/), including: Security Logging and Monitoring Failures, Identification and Authentication Failures, Injection, and Server-Side Request Forgery:

| Risk        | Mitigation  |
| :---        | :------     |
| Security Logging and Monitoring Failures | All services feature a logger class `Broadcaster` that provides rich logging of events that include a readable timestamp prepended to a category label (ex: `ERROR`, `INFO`, `HTTP`...) along with a message. What qualifies as an event includes a request, an error, or a manual log. Each request's action can be traced from start to finish. All logs can be downloaded in CSV format via Heroku. Additioanlly, any unhandled exception that occur are automatically mailed to a list of first responders and pushed to a Bugsnag dashboard for production level error monitoring. |
| Identification and Authentication Failures | It was decided early on that no custom logging was going to be introduced on the website and that user credentials were going to be managed on a database. Instead, authentication happens exclusively via OAuth through trusted and popular providers including Twitter, Facebook, or Google. Thus, the website heavily relies on Json Web Tokens. Although there are some [concerns](https://www.loginradius.com/blog/identity/pros-cons-token-authentication/) still with JWTs such as lose expirations blocks, it is a safer to deal with than password salting and hashing. |
| Injection | Text        |
| Server-Side Request Forgery | Text        |

### OAuth implementation
There are multiple ways of 

### JWT implementation
Session management is done using JWTs as they provide a statless experience on the server side. 
Json Web Tokens allows encryptions of claims for use in authorization between servers.  
This is how I implemented OAuth and manage user data.

# Release management

### CI/CD

### Testing

# About the repositories

### Frontend
![rently](https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=rently&include_all_commits=true&show_owner=true)

### User service
![user-service](https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=user-service&include_all_commits=true&show_owner=true)  

SQL database. Uses Java's JPA ORM for MySQL

### Listing service
![listing-service](https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=listing-service&include_all_commits=true&show_owner=true)  

Document-based database. Uses Java's JPA ORM for MongoDB

### Search service
![search-service](https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=search-service&include_all_commits=true&show_owner=true)  

Uses MongoDB's Text and Geo Indexes. Geos are computed using TomTom API

### Mailer service
![mailer-service](https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=mailer-service&include_all_commits=true&show_owner=true)  

Backend implimentation of sending emails. Uses Gmail's smtp to send automated emails under an offical Rently email `info.rently-io@gmail.com`

### Image service
![image-service](https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=image-service&include_all_commits=true&show_owner=true)  

# Reflection and improvements

# Security
The greatest challenge tackled in the project involved security. Designing the frontend or configuring the backend got me constantly questioning *Am I exposing something here?* I attempted to tackled all concerns about data exposure as much as possible through the intergration of things such as JWTs, CORS, external configurations. Light security tests were performed using [webtools](https://pentest-tools.com/website-vulnerability-scanning/website-scanner) though, those tests are inadequate. I would have spent more time researching on how to properly and fully test the security of my website.

# Code


# Works cited

[oauth](https://supertokens.com/blog/all-you-need-to-know-about-user-session-security?utm_source=Dzone&utm_medium=Company-post&utm_campaign=Oauth&utm_term=Session-Management-1)

[jwt](jwt.io)

[owasp](https://owasp.org/www-project-top-ten/)

[website security](https://pentest-tools.com/website-vulnerability-scanning/website-scanner)

[bugsnag](https://app.bugsnag.com/somethingrandom/rently/overview?release_stage=production)

[jwt cons](https://www.loginradius.com/blog/identity/pros-cons-token-authentication/)
