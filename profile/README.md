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
      <a href="#c2-container-model">C2 model</a>
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
  
  <h3>Error management</h3>
  
  <h3>UX testing</h3>
  
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

### C2 container model

Each blue box represents a repository in this project:

![C2 model](https://i.imgur.com/34Nvkd4.png)

# Software quality

### Security concerns
Security was among the top non-functional requirements for the Rently. Multiple security aspects were addressed on [OWASP's top 10 list](https://owasp.org/www-project-top-ten/), including: Security Logging and Monitoring Failures, Identification and Authentication Failures, Injection, and Server-Side Request Forgery:

| Risk        | Mitigation  |
| :---        | :------     |
| Security Logging and Monitoring Failures | All services feature a logger class `Broadcaster` that provides rich logging of events that include a readable timestamp prepended to a category label (ex: `ERROR`, `INFO`, `HTTP`...) along with a message. What qualifies as an event includes a request, an error, or a manual log. Each request's action can be traced from start to finish. All logs can be downloaded in CSV format via Heroku. No internal server errors are exposed to bear responses. |
| Identification and Authentication Failures | It was decided early on that no custom logging was going to be introduced on the website and that user credentials were going to be managed on a database. Instead, authentication happens exclusively via OAuth through trusted and popular providers including Twitter, Facebook, or Google. Thus, the website heavily relies on Json Web Tokens. Although there are some [concerns](https://www.loginradius.com/blog/identity/pros-cons-token-authentication/) still with JWTs such as lose expirations blocks, it is a safer to deal with than password hashing. Each service captures requests through a middleware and verifies JWT integrity. |
| Injection | Any incomoing persistent data is verified. Requests are terminated if data does not conform. Rently operates two databases, an SQL and document-based ones. In both instances, object relational mapping, making use of Java's already well established Persistence API. Although in some instances JPA is [prone to injection attacks](https://www.adam-bien.com/roller/abien/entry/preventing_injection_in_jpa_query), using Spring Boot's [named method query](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories.query-methods.query-creation) creation mitigates possible injection. |
| Server-Side Request Forgery | CORS is enabled across services when applicable allowing only one URL to make requests. In cases where no origin is specified, only stripped-down data objects can be retireved. For instance retriving listing data won't feature specific address location nor a leaser's id. |

### OAuth implementation
There are two ways of OAuth can be implemented; either in the frontend or the backend. The project opted a frontend implementation. Before any attempt at posting a listing, a user most login through one of the OAuth providers (Twitter, Google, Facebook). Once logged in, the system checks and fecthes proper user data in the backend. The process is as follow:

<p align="center">
  <img height="500px" src="https://i.imgur.com/GYD4Ss8.jpg" />
</p>

This implementation of OAuth allows for user authorization password-free and secure. Once logged in with proper use data, users can make requests to interact with other services with a properly generated JWT which would be otherwise impossible without login. 

The system essential behaves as if a user had created an account conventionally like other websites.

### JWT implementation
Json Web Tokens allows encryptions of claims for use in authorization between services in a staleless manner and session management for the frontend. Each service has a its own secret and hashing algorithm. For exemple, the user service has for secret `HelloDarknessMyOldFriend` and hasing algorithm the `SHA256`. 

In order to make server-side requests, the services and the frontend need to be aware of other services' secret and hasghing algorithm to generate valid JWTs. A request is rejected if inproper authorization headers are present. 

These parameters are set expernally in a profile enveronment file.

# Release management

### CI/CD
**Dockerhub dockerisation | Heroku deployment | Sonarcloud static code analysis**

Perhaps one of the greatest assets surrounding an IT project is a proper CI/CD enveronment. The project relies on Github actions to perfom various CI/CD tasks. 

Upon any PRs from a development branch to a master branch, usually taking place at the end of sprints, 2 process are performed. To begin with, the code is automatically tested and dockerised to a remote Dockerhub repository and then deployed on Heroku. Should a test fail, the process is terminated and a notification is sent. 

In parallel, static code analysis is performed using Sonarcloud's online tool. A comment to each PRs is appended with the static code results. Generally, the results are good. However, security vulnerabilities within the pushed code are usually immediately addressed for the reminder of the sprint. 

Also, Github Dependabots keep track of any outdated or flawed dependacies in the repository and automatically upgrades them if needed. 

This strategy is replicated across all repositories.

An exemple of CI and CD actions from the User Service can be found [here](https://github.com/rently-io/user-service/blob/V2.0-latest/.github/workflows/ci.yml) and [here](https://github.com/rently-io/user-service/blob/V2.0-latest/.github/workflows/cd.yml) respectively.

### Testing
|Test|Description|Example| 
|---:|---|---| 
| **Unit** | Ensures that multiple aspect of the endpoint work properly within the scope of the endpoint itself, on a *local* level | <ul><li>Correct data gets yield from a request</li><li>No *`null`* values are returned when an empty *`string`* was expected</li></ul> |
| **Integration** | Like the name suggests, integration testing is used to make sure *multiple endpoints* work/interact with one another correctly | <ul><li>`200`/`401`/`400`/etc... reponses are returned when making a request</li><li>Errors are handled correctly, including internal errors (e.i. `Server 500`)</li></ul>
| **E2E**| Some amount of E2E testing has been made on the frontend using Cypress. See reflection for more information. | <ul><li>Testing whether links redirect to proper page or form submission has proper input validation. </li></ul>
| **Regression**| When tests fail, I can address the failures and adjust my code for it to work properly, configure new test cases when and prioritize | <ul><li>Reformatting JSON response data can cause certain tests to fail</li></ul>

# Error management
There are always errors that occur on deployed stuff, even after testing. Any unhandled exception that occur on an service are automatically mailed to a list of first responders and pushed to a Bugsnag dashboard for production level error monitoring. 

Bugsnag allows the ability to view the most frequent errors on a timeline:
<p align="center">
  <img src="https://i.imgur.com/kimM7Sv.png" width=500 /> 
  <img src="https://i.imgur.com/jXmJOyQ.png" width=500 /> 
</p>

Having the ability to view errors overtime allows me to focus on the most reoccuring or most destructive errors.

# UX testing
Hojar is a tool one can implement on his website to perform user experience testing. Every 100 ms, Hotjar records user inputs and website events anonymously. 

Among other things, this tool generates heatmaps of user interaction on a per page basis:

<p align="center">
  <img src="https://i.imgur.com/38AUB0l.jpg" width=450px />
  <img src="https://i.imgur.com/bMkoB62.jpg" width=450px />
</p>

This tool gives me the ability to perform unmoinotered remote UX testing contiously. Testing that reveals design flaws or potential hotspots of user frustration.

For more information regarding some of the findings, please click [here](https://github.com/greffgreff/semester-content/blob/main/ux-testing.md).

# About the repositories
Below is are brief descriptions or particularities about each repository. For more information, click on their cards.

### Frontend
As mentioned ealier, the frontend uses NextJS. To facilitate the implementation of sessions with JWTs, the NextAuth library was used. 

> ⚠️ Given time constraints, not all features have been fully implemented or service fully integrated in the frontend. You may if some flaws on the website.

<a href="https://github.com/rently-io/rently/" >
  <img src="https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=rently&include_all_commits=true&show_owner=true" />
</a>

### User service
In brief, this service is able performs CRUD on user-related data. It uses an MySQL database deployed on Heroku. 

This service was the first one developed. You'll notice there is a `V2-latest` and `V1` branches instead of a `master` or `main` branch. The reason behind this is the first iteration, `V1` of this service was not up to standard in my opinion. Instead of entrusting my code with other, more reliable library to handle certain aspect of the service, I tried to code everything myself. While I believe one should always have authority of ones code, it shouldn't come at the expense of reliability, development time, and security. Upon the first static code analysis, multiple sources of potential SQL injection were possible. 

The second iteration, `V2-latest` aims to fix these issues altogether with the use of JPA. I go into deeper detail under <a href="#general-mindset"> General mindset</a>.

<a href="https://github.com/rently-io/user-service/" >
  <img src="https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=user-service&include_all_commits=true&show_owner=true" /> 
</a>

### Listing service
Much like the User service, this service perfoms CRUD on listing data. Rather than an SQL database, it was decided that the data would be stored in a document-based database, in this case a MongoDB database, as it is easier to work with for query indexes.

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

Some exemples of emails already dispatched:

<p align="center">
  <img width="400px" src="https://i.imgur.com/SYgXt6q.png" />
  <img width="400px" src="https://i.imgur.com/wDKZMIM.png" />
  <img width="400px" src="https://i.imgur.com/kBXMhTV.png" />
  <img width="400px" src="https://i.imgur.com/dH9AN0i.png" />
</p>

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

The images are stored in the same MongoDB database context as the listing data. 

<a href="https://github.com/rently-io/image-service/" >
  <img src="https://github-readme-stats.vercel.app/api/pin/?username=rently-io&repo=image-service&include_all_commits=true&show_owner=true" />
</a>

# Reflection and improvements

### Security
The greatest challenge tackled in the project involved security. Designing the frontend or configuring the backend got me constantly questioning *Am I exposing something here?* I attempted to tackled all concerns about data exposure as much as possible through the intergration of things such as JWTs, CORS, etc. Light security tests were performed using [webtools](https://pentest-tools.com/website-vulnerability-scanning/website-scanner) though, those tests are inadequate. I would have spent more time researching on how to properly and fully test the security of my website.

### Testing

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
