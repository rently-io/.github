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
  
  <h3>Reflection and improvements</h3>
  <ul>
    <li>
      <a href="#frontend">Security </a>
    </li>
    <li>
      <a href="#user-service">Code</a>
    </li>
    <li>
      <a href="#user-service">General mindset</a>
    </li>
  </ul>
  
  <h3>Works cited</h3>
</details>

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
| Security Logging and Monitoring Failures | All services feature a logger class `Broadcaster` that provides rich logging of events that include a readable timestamp prepended to a category label (ex: `ERROR`, `INFO`, `HTTP`...) along with a message. What qualifies as an event includes a request, an error, or a manual log. Each request's action can be traced from start to finish. All logs can be downloaded in CSV format via Heroku. No internal server errors are exposed to bear responses. Additioanlly, any unhandled exception that occur are automatically mailed to a list of first responders and pushed to a Bugsnag dashboard for production level error monitoring. |
| Identification and Authentication Failures | It was decided early on that no custom logging was going to be introduced on the website and that user credentials were going to be managed on a database. Instead, authentication happens exclusively via OAuth through trusted and popular providers including Twitter, Facebook, or Google. Thus, the website heavily relies on Json Web Tokens. Although there are some [concerns](https://www.loginradius.com/blog/identity/pros-cons-token-authentication/) still with JWTs such as lose expirations blocks, it is a safer to deal with than password hashing. Each service captures requests through a middleware and verifies JWT integrity. |
| Injection | Any incomoing persistent data is verified. Requests are terminated if data does not conform. Rently operates two databases, an SQL and document-based ones. In both instances, object relational mapping, making use of Java's already well established Persistence API. Although in some instances JPA is [prone to injection attacks](https://www.adam-bien.com/roller/abien/entry/preventing_injection_in_jpa_query), using Spring Boot's [named method query](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories.query-methods.query-creation) creation mitigates possible injection. |
| Server-Side Request Forgery | CORS is enabled across services when applicable allowing only one URL to make requests. In cases where no origin is specified, only stripped-down data objects can be retireved. For instance retriving listing data won't feature specific address location nor a leaser's id. |

### OAuth implementation
There are multiple ways of 

<p align="center">
  <img height="500px" src="https://i.imgur.com/GYD4Ss8.jpg" />
</p>
  
### JWT implementation
Session management is done using JWTs as they provide a statless experience on the server side. 
Json Web Tokens allows encryptions of claims for use in authorization between servers.  
This is how I implemented OAuth and manage user data.

# Release management

### CI/CD
**Dockerhub dockerisation | Heroku deployment | Sonarcloud static code analysis**

Perhaps one of the greatest assets surrounding an IT project is a proper CI/CD enveronment. The project relies on Github actions to perfom various CI/CD tasks. 

Upon any PRs from a development branch to a master branch, usually taking place at the end of sprints, 2 process are performed. To begin with, the code is automatically tested and dockerised to a remote Dockerhub repository and then deployed on Heroku. Should a test fail, the process is terminated and a notification is sent. 

In parallel, static code analysis is performed using Sonarcloud's online tool. A comment to each PRs is appended with the static code results. Generally, the results are good. However, security vulnerabilities within the pushed code are usually immediately addressed for the reminder of the sprint. 

Also, Github Dependabots keep track of any outdated or flawed dependacies in the repository and automatically upgrades them if needed. 

This strategy is replicated across all repositories.

### Testing

# About the repositories

Below is are brief descriptions or particularities about each repository. For more information, click on their cards.

### Frontend
As mentioned ealier, the frontend uses NextJS. To facilitate the implementation of sessions with JWTs, the NextAuth library was used. 

> ⚠️ Given time constraints, not all features have been fully implemented or service fully integrated in the frontend. You may if some flaws on the website.

<a href="https://github.com/rently-io/rently/" >
  <img src="https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=rently&include_all_commits=true&show_owner=true" />
</a>

### User service
SQL database. Uses Java's JPA ORM for MySQL. V1 vs V2

<a href="https://github.com/rently-io/user-service/" >
  <img src="https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=user-service&include_all_commits=true&show_owner=true" /> 
</a>

### Listing service
Document-based database. Uses Java's JPA ORM for MongoDB

<a href="https://github.com/rently-io/listing-service/" >
  <img src="https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=listing-service&include_all_commits=true&show_owner=true" /> 
</a>

### Search service
A service whose endpoints returns a collection of stripped-down listings. Depending on the parameters sent in the request's URL, a request query can be qualified as either `RANDOM`, `QUERRIED` if a keyword(s) is supplied, `QUERRIED_LOCATION` if a keyword(s) is supplied with an address (country and/or city and/or street), `QUERRIED_NEARBY` if a range is present with an address and a keyword(s). `LOCATION` and `NEARBY` queries are also possible for none keyword-specific searches.

Each type has its own endpoint, though, a dedicated endpoint accepts all parameters and redirects according the combination of query paramters making searches easier.

Searches are made possible with a combination of database-side data indexes for blazing fast data retrieval operations using text and geo indexes.

<a href="https://github.com/rently-io/search-service/" >
  <img src="https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=search-service&include_all_commits=true&show_owner=true" />
</a>

### Mailer service
A dedicated solution that is capable of sending various emails. The mailer uses Gmail's smtp to send automated emails under an offical Rently email under `info.rently-io@gmail.com`.

Pre-made html templates already exists for things like sending a greeting mail for new users or a new listing mail, though, an already formated generic message exists for other kinds of emails. Also, a dedicated endpoint is used for sending dev errors whenever an unhandled exception occurs in one of the services.

<a href="https://github.com/rently-io/mailer-service/" >
  <img src="https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=mailer-service&include_all_commits=true&show_owner=true" />
</a>

### Image service
A very basic service whose sole purpose is to save image data in base64 to useable URL links. Upon each GET requests, a "*Rently*" watermark is stamped onto the bottom left corner of images:

<p align="center">
  <img height="200px" src="https://i.imgur.com/SmS5WmQ.png" />
  <img height="200px" src="https://i.imgur.com/scIquXW.jpg" />
  <img height="200px" src="https://i.imgur.com/HXTqtA0.png" />
</p>

The images are stored in the same MongoDB database as the listing data. 

<a href="https://github.com/rently-io/image-service/" >
  <img src="https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=image-service&include_all_commits=true&show_owner=true" />
</a>

# Reflection and improvements

### Security
The greatest challenge tackled in the project involved security. Designing the frontend or configuring the backend got me constantly questioning *Am I exposing something here?* I attempted to tackled all concerns about data exposure as much as possible through the intergration of things such as JWTs, CORS, etc. Light security tests were performed using [webtools](https://pentest-tools.com/website-vulnerability-scanning/website-scanner) though, those tests are inadequate. I would have spent more time researching on how to properly and fully test the security of my website.

### Code

### General mindset

# Works cited

[oauth](https://supertokens.com/blog/all-you-need-to-know-about-user-session-security?utm_source=Dzone&utm_medium=Company-post&utm_campaign=Oauth&utm_term=Session-Management-1)

[jwt](jwt.io)

[owasp](https://owasp.org/www-project-top-ten/)

[website security](https://pentest-tools.com/website-vulnerability-scanning/website-scanner)

[bugsnag](https://app.bugsnag.com/somethingrandom/rently/overview?release_stage=production)

[jwt cons](https://www.loginradius.com/blog/identity/pros-cons-token-authentication/)

[prone to injection attacks](https://www.adam-bien.com/roller/abien/entry/preventing_injection_in_jpa_query),

[named method query](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories.query-methods.query-creation)
