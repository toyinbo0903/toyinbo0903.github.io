---
title: Technical Writing Assessment
date: 2017-01-04T15:04:10.000Z
description: >-
  This assessment is to create a technical document in markdown and also share the rendered output. This technical documentation should explain how to calculate the Key Performance Indicator (KPI) Net Promotor Score using SQL
---
# How to Calculate Net Promoter Score (NPS) Using SQL

## Table of Contents

- [Definition of NPS](#definition-of-nps)
- [Business Use Cases of NPS](#business-use-cases-of-nps)
- [Data Sources Required to Implement NPS](#data-sources-required-to-implement-nps)
- [Explanation of the Data Relationships](#explanation-of-the-data-relationships)

## Definition of NPS

The Net Promoter Score (NPS) is a widely used metric that measures customer satisfaction and loyalty. It's calculated based on the responses to a single question:

> "On a scale of 0-10, how likely are you to recommend our company/product/service to a friend or colleague?"

The responses are classified into three categories:

-   Promoters (score 9-10): These are customers who are loyal enthusiasts. They are likely to continue purchasing and refer others, thus fueling growth.

-   Passives (score 7-8): These are customers who are satisfied but unenthusiastic. They are vulnerable to competitive offerings.

-   Detractors (score 0-6): These are customers who are unhappy with their experience. They could potentially damage the brand through negative word-of-mouth.

The Net Promoter Score is calculated by subtracting the percentage of customers who are Detractors from the percentage who are Promoters. It's a simple gauge of customer's overall perception of a brand and an indicator of growth potential.

```m
NPS = %Promoters - %Detractors
```
Net Promoter Score (NPS) is calculated by subtracting the percentage of Detractors from the percentage of Promoters.

## Business Use Cases of NPS

The Net Promoter Score (NPS) is a widely used market research metric that typically takes the form of a single survey question asking respondents to rate the likelihood that they would recommend a company, product, or service to a friend or colleague. The NPS is calculated based on responses to a single question: How likely is it that you would recommend our company/product/service to a friend or colleague?

In terms of business use cases, SQL could be used to calculate the NPS based on survey data stored in a relational database, and then further analyze that data. Here are some use cases:

1.  Calculate NPS Score: With a SQL query, you can calculate the NPS of your company. Promoters are customers who rate your company 9 or 10, passives rate you 7 or 8, and detractors rate you 6 or below. The NPS is the percentage of promoters minus the percentage of detractors.

```sql

SELECT
  (SUM(CASE WHEN score >= 9 THEN 1 ELSE 0 END) / COUNT(*) -
   SUM(CASE WHEN score <= 6 THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS nps
FROM responses;
```
<!-- The HTML -->
<div class="accordion">
  <div class="accordion-item">
    <button id="accordion-button-1" aria-expanded="false"><span class="accordion-title">Sequence Diagram</span></button>
    <div id="accordion-content-1" class="accordion-content">
      <html>
  <body>
    Entity relationship diagram:
    <pre class="mermaid">
            sequenceDiagram
    autonumber
    User->>Database: Request (SQL Query for NPS calculation)
    Database->>Data: Fetch data from "responses" table
    loop Calculate Counts
        Data->>Data: Calculate count of promoters (score >= 9), detractors (score <= 6), and total responses
    end
    Note right of Data: Created promoters_count, detractors_count, and total_responses!
    Data-->>Database: Return result of count calculations
    Database->>Data: Fetch count of promoters, detractors and total responses
    loop Calculate NPS
        Data->>Data: Calculate NPS based on counts (promoters - detractors / total_responses * 100)
    end
    Note right of Data: Calculated NPS!
    Data-->>Database: Return result of NPS calculation
    Database-->>User: Response (NPS Result)
    </pre>

    <script type="module">
      import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
      mermaid.initialize({ startOnLoad: true });
    </script>
  </body>
</html>
    </div>
  </div>

  <div class="accordion-item">
    <button id="accordion-button-2" aria-expanded="false"><span class="accordion-title">Flowchart Diagram</span></button>
    <div id="accordion-content-2" class="accordion-content">
      flowchart LR
    A[Calculate NPS Score] --> B{{SQL Query}}
    B -->|Execute Query| id1[(Database)]
    id1 --> D{Customer's Rating}
    D -->|9 or 10| E[Promoters]
    D -->|7 or 8| F[Passives]
    D -->|6 or below| G[Detractors]
    E --> H[Calculate % of Promoters]
    G --> I[Calculate % of Detractors]
    H --> J[Subtract % of Detractors from Promoters]
    I --> J
    J --> K[Result: NPS Score]
    </div>
  </div>
</div>

<!-- The CSS -->
<style>
.accordion-content {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.2s ease-out;
}

.accordion .accordion-item button[aria-expanded='true'] + .accordion-content {
  max-height: 100px;
}
</style>

<!-- The JavaScript -->
<script>
const buttons = document.querySelectorAll(".accordion .accordion-item button");

buttons.forEach(button => {
  button.addEventListener("click", () => {
    const expanded = button.getAttribute("aria-expanded") === "true" || false;

    button.setAttribute("aria-expanded", !expanded);

    const content = button.nextElementSibling;
    content.style.maxHeight = !expanded ? content.scrollHeight + "px" : 0;
  });
});
</script>


2.  NPS By Product/Service: Calculate NPS for each of your products or services, to understand which ones are performing well and which ones might need improvement.

```sql

SELECT
  product,
  (SUM(CASE WHEN score >= 9 THEN 1 ELSE 0 END) / COUNT(*) -
   SUM(CASE WHEN score <= 6 THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS nps
FROM responses
GROUP BY product;
```

3.  NPS By Customer Segment: Calculate NPS for different customer segments, to understand the customer experience across different segments.

```sql

SELECT
  customer_segment,
  (SUM(CASE WHEN score >= 9 THEN 1 ELSE 0 END) / COUNT(*) -
   SUM(CASE WHEN score <= 6 THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS nps
FROM responses
GROUP BY customer_segment;
```

4.  NPS Over Time: Monitor NPS over time to identify trends and see if customer satisfaction is improving or deteriorating.

```sql

SELECT
  DATE_TRUNC('month', response_date) as month,
  (SUM(CASE WHEN score >= 9 THEN 1 ELSE 0 END) / COUNT(*) -
   SUM(CASE WHEN score <= 6 THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS nps
FROM responses
GROUP BY month;
```
> **NOTE:** These queries are based on the following assumptions:


-   There is a table named `responses` that contains your survey responses.
-   This table has a column named `score` which holds the NPS rating (on a scale of 0-10).
-   The `product` and `customer_segment` columns categorize the responses.
-   The `response_date` column contains the date of each response.

## Data Sources Required to Implement NPS

To calculate NPS, you need specific data that's usually derived from surveys, customer interactions, and customer databases. Here's a more detailed look at the type of data you'll need:

1.  Customer Survey Data: This is typically obtained through a dedicated survey tool or integrated survey module within your CRM or customer experience platform. You'll need to ensure that your survey tool can provide you with the raw score data on a per-response basis, as well as any other metadata or customer attributes that are collected during the survey. These might include timestamps, geographic data, product or service usage details, and so on.

2.  Customer Relationship Management (CRM) Data: CRM systems are a treasure trove of customer data and insights. They can provide historical data about a customer's interactions and transactions with your company, which can be crucial in understanding and interpreting your NPS results. For instance, a low NPS score from a customer who has had several recent support tickets could suggest problems with your support function.

3.  Customer IDs: These unique identifiers are essential to ensure that each survey response is linked to a specific customer. This helps avoid counting multiple responses from the same customer and allows for more detailed analysis, such as tracking NPS changes over time for individual customers or correlating NPS scores with other customer characteristics or behaviors.

4.  NPS Scores: These are the core data for calculating NPS. Each customer is asked to give a score, typically on a scale from 0 to 10, indicating how likely they are to recommend your company, product, or service to others. The actual question can vary slightly, but the key is that it asks about the likelihood of the customer acting as a promoter for your company.

5.  Customer Segmentation Data: This data is useful for breaking down and analyzing your NPS by different customer segments. This might include demographic data (like age, location, industry), behavioral data (like product usage, purchase history), or psychographic data (like customer attitudes or preferences).

Collecting, integrating, and analyzing this data can require significant data infrastructure and expertise. However, the insights gained from a well-implemented NPS program can provide substantial benefits in understanding and improving customer satisfaction and loyalty.

## Explanation of the Data Relationships

<html>
  <body>
    Entity relationship diagram:
    <pre class="mermaid">
            erDiagram
    CUSTOMER ||--o{ SURVEY-RESPONSE : "provides"
    CUSTOMER {
        string customerID PK "The customer's unique ID"
        string firstName "Customer's first name"
        string lastName "Customer's last name"
        string email "Customer's email"
    }
    SURVEY-RESPONSE {
        string surveyID PK "Unique survey ID"
        string customerID FK "Foreign key linking to customer"
        int npsScore "Net Promoter Score"
        date responseDate "Date of the response"
    }
    PRODUCT ||--o{ SURVEY-RESPONSE : "is related to"
    PRODUCT {
        string productID PK "Unique product ID"
        string productName "Name of the product"
        string productCategory "Category of the product"
    }
    </pre>

    <script type="module">
      import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
      mermaid.initialize({ startOnLoad: true });
    </script>
  </body>
</html>

### Entities and their attributes:

1.  CUSTOMER: This entity represents the customers of the business.

    -   `customerID`: The unique ID for each customer.
    -   `firstName`: The first name of the customer.
    -   `lastName`: The last name of the customer.
    -   `email`: The email address of the customer.
2.  SURVEY-RESPONSE: This entity represents the responses received from customers for the surveys conducted by the business.

    -   `surveyID`: The unique ID for each survey.
    -   `customerID`: The unique ID of the customer who provided the response, which links back to the CUSTOMER entity.
    -   `npsScore`: The Net Promoter Score received from the customer. This score is used to gauge the customer's overall satisfaction with a company's product or service and the customer's loyalty to the brand.
    -   `responseDate`: The date when the response was received.
3.  PRODUCT: This entity represents the products of the business.

    -   `productID`: The unique ID for each product.
    -   `productName`: The name of the product.
    -   `productCategory`: The category that the product belongs to.

### Relationships between the entities:

1.  CUSTOMER - SURVEY-RESPONSE: The relationship here is one-to-many, as represented by "||--o{". This means one customer can provide multiple survey responses, but each survey response can be provided by only one customer.

2.  PRODUCT - SURVEY-RESPONSE: The relationship here is also one-to-many, as represented by "||--o{". This means one product can be related to multiple survey responses (a customer may review the same product more than once), but each survey response is related to only one product.



<html>
  <body>
    Here is one mermaid ddiagram:
    <pre class="mermaid">
            sequenceDiagram
    autonumber
    User->>Database: Request (SQL Query)
    Database->>Data: Fetch "customer_surveys"
    loop Calculate Counts
        Data->>Data: Calculate count of promoters, detractors and total responses
    end
    Note right of Data: Created response_counts subquery!
    Data-->>Database: Return result of subquery
    Database->>Data: Fetch from response_counts subquery
    loop Calculate NPS
        Data->>Data: Calculate NPS based on counts
    end
    Note right of Data: Calculated NPS!
    Data-->>Database: Return result of calculation
    Database-->>User: Response (NPS Result)
    </pre>

    <script type="module">
      import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
      mermaid.initialize({ startOnLoad: true });
    </script>
  </body>
</html>

sequenceDiagram
    autonumber
    User->>Database: Request (SQL Query)
    Database->>Data: Fetch "customer_surveys"
    loop Calculate Counts
        Data->>Data: Calculate count of promoters, detractors and total responses
    end
    Note right of Data: Created response_counts subquery!
    Data-->>Database: Return result of subquery
    Database->>Data: Fetch from response_counts subquery
    loop Calculate NPS
        Data->>Data: Calculate NPS based on counts
    end
    Note right of Data: Calculated NPS!
    Data-->>Database: Return result of calculation
    Database-->>User: Response (NPS Result)


<!-- The HTML -->
<div class="accordion">
  <div class="accordion-item">
    <button id="accordion-button-1" aria-expanded="false"><span class="accordion-title">Accordion Item 1</span></button>
    <div id="accordion-content-1" class="accordion-content">
      Content for Accordion Item 1...
    </div>
  </div>

  <div class="accordion-item">
    <button id="accordion-button-2" aria-expanded="false"><span class="accordion-title">Accordion Item 2</span></button>
    <div id="accordion-content-2" class="accordion-content">
      Content for Accordion Item 2...
    </div>
  </div>
</div>

<!-- The CSS -->
<style>
.accordion-content {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.2s ease-out;
}

.accordion .accordion-item button[aria-expanded='true'] + .accordion-content {
  max-height: 100px;
}
</style>

<!-- The JavaScript -->
<script>
const buttons = document.querySelectorAll(".accordion .accordion-item button");

buttons.forEach(button => {
  button.addEventListener("click", () => {
    const expanded = button.getAttribute("aria-expanded") === "true" || false;

    button.setAttribute("aria-expanded", !expanded);

    const content = button.nextElementSibling;
    content.style.maxHeight = !expanded ? content.scrollHeight + "px" : 0;
  });
});
</script>

# Table of Contents

- [Introduction](#introduction)
- [Chapter 1](#chapter-1)
- [Chapter 2](#chapter-2)
  - [Section 2.1](#section-21)
  - [Section 2.2](#section-22)
- [Conclusion](#conclusion)

## Introduction
Your introduction content...

## Chapter 1
Your chapter 1 content...

## Chapter 2
Your chapter 2 content...

### Section 2.1
Your section 2.1 content...

### Section 2.2
Your section 2.2 content...

## Conclusion
Your conclusion content...

<!-- HTML -->
<nav>
  <div class="hamburger-menu">
    <div></div>
    <div></div>
    <div></div>
  </div>
  <ul class="nav-links">
    <li><a href="#link1">Link 1</a></li>
    <li><a href="#link2">Link 2</a></li>
    <li><a href="#link3">Link 3</a></li>
  </ul>
</nav>

<!-- CSS -->
<style>
.hamburger-menu {
  display: none;
  flex-direction: column;
  justify-content: space-around;
  width: 2rem;
  height: 2rem;
  cursor: pointer;
}
.hamburger-menu div {
  width: 2rem;
  height: 0.25rem;
  background: currentColor;
}

.nav-links {
  display: flex;
  justify-content: space-around;
  list-style: none;
}

@media (max-width: 768px) {
  .nav-links {
    display: none;
  }
  .hamburger-menu {
    display: flex;
  }
}
</style>

<!-- JavaScript -->
<script>
const hamburgerMenu = document.querySelector('.hamburger-menu');
const navLinks = document.querySelector('.nav-links');

hamburgerMenu.addEventListener('click', () => {
  navLinks.style.display = navLinks.style.display === 'none' ? 'flex' : 'none';
});
</script>


# Hugo template for Decap CMS with Identity

This is a small business template built with [Hugo](https://gohugo.io) and [Decap CMS](https://github.com/decaporg/decap-cms), designed and developed by [Darin Dimitroff](https://twitter.com/deezel), [spacefarm.digital](https://www.spacefarm.digital).

## Get strted.

Use our deploy button to get your own copy of the repository. 

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/decaporg/one-click-hugo-cms&stack=cms)

This will setup everything needed for running the CMS:

* A new repository in your GitHub account with the code
* Full Continuous Deployment to Netlify's global CDN network
* Control users and access with Netlify Identity
* Manage content with Decap CMS

Once the initial build finishes, you can invite yourself as a user. Go to the Identity tab in your new site, click "Invite" and send yourself an invite.

Now you're all set, and you can start editing content!

## Local Development

Clone this repository, and run `yarn` or `npm install` from the new folder to install all required dependencies.

Then start the development server with `yarn start` or `npm start`.

## Testing

With the development server running, run the tests locally
with `yarn cypress:run` or `npm run cypress:run`.
Or use `yarn cypress:open` or `npm run cypress:open` to run interactively.

Cypress tests also run on deploy with the [Cypress Netlify integration](https://www.netlify.com/integrations/cypress/).

## Layouts

The template is based on small, content-agnostic partials that can be mixed and matched. The pre-built pages showcase just a few of the possible combinations. Refer to the `site/layouts/partials` folder for all available partials.

Use Hugo’s `dict` functionality to feed content into partials and avoid repeating yourself and creating discrepancies.

## CSS

The template uses a custom fork of Tachyons and PostCSS with cssnext and cssnano. To customize the template for your brand, refer to `src/css/imports/_variables.css` where most of the important global variables like colors and spacing are stored.

## SVG

All SVG icons stored in `site/static/img/icons` are automatically optimized with SVGO (gulp-svgmin) and concatenated into a single SVG sprite stored as a a partial called `svg.html`. Make sure you use consistent icons in terms of viewport and art direction for optimal results. Refer to an SVG via the `<use>` tag like so:

```
<svg width="16px" height="16px" class="db">
  <use xlink:href="#SVG-ID"></use>
</svg>
```
