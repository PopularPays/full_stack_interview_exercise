# RAILS REST API -- Interview Practice Project
The objective of this practice project is to build an API only Rails application. This application should run locally and consist of several API endpoints and Rails models.

## Models

There are several models: **creators**, **gigs**, and **gig payments**. All relationships should be defined in your Rails models.

### Creators

_Attributes_: `first_name` and `last_name`

_Relationships_: A `creator` has many `gigs`

### Gigs

_Attributes_: `brand_name` and `state`

_Relationships_: A `gig` has one `gig payment`

The `brand_name` can be any arbitrary string. The initial value of `state` is `applied`. The other possible states for a `gig` are `accepted`, `completed` and `paid`

When the `state` is set to `completed` a `gig payment` should automatically be created for this `gig`.

### Gig Payments

_Attributes_: `state` and `amount_in_cents`

The initial value of `state` is `pending`. The other possible value is `complete`. When the `state` is set to `complete` the state of the related `gig` should be set to `paid`.

The `amount_in_cents` can be an integer that with division by 100 could be converted from cents to dollars. The value can be arbitrary.

## API Endpoints

### Creators resource

**POST** `/creators` - saves a `creator` record to the database. It should provide values for the `first_name` and `last_name` attributes in the request body.

---

**GET** `/creators` - returns a list of creators. It accepts `sort`, `sort_direction`, `limit` and `offset` query params.

The `sort` param determines which attribute of `creator` to sort by. The `sort_direction` param should accept either `asc` (ascending) or `desc` (descending)

The `limit` param defines the max number of records returned.

The `offset` param defines how many records to skip in the results set. E.G. if `limit` was 5 and `offset` was 5 this would return records 6 through 10.

---

**GET** `/creators/<id>` - returns a single creator with the given ID

### Gigs resource

**POST** `/gigs` - saves a `gig` record to the database. It should provide value for the `brand_name` attribute in the request body. The created gig should also be linked it to a given creator.

---

**GET** `/gigs` - returns a list of gigs. The request has the option to specify a brand name or creator. In this case the endpoint should only return gigs with the given brand name or creator.

---

**GET** `/gigs/<id>` - returns a given gig. The request has the option to specify a relationship to include with the response. E.G. If the `gig payment` relationship is specified it returns the `gig` **and** its `gig payment`

---

**PUT** `/gigs/<id>` - updates the attributes for a given gig based on the ID.

## CRON

A CRON job should run every 2 minutes. This job changes the state of any `pending` `gig payments` to `completed`. You can use the [whenever](https://github.com/javan/whenever) gem to [schedule rake tasks](https://dev.to/risafj/cron-jobs-in-rails-a-simple-guide-to-actually-using-the-whenever-gem-now-with-tasks-2omi) to accomplish this behavior.

## Serialization

All endpoints should return data using the [JSON API format](https://jsonapi.org/). You can use the [active model serializers gem](https://github.com/rails-api/active_model_serializers) to automatically serialize data using this format. This gem can also be used to include relationships of records in the response.

## Changing state

There should be some mechanism for changing the state for `gigs` and `gig payments`. You can optionally use the [AASM gem](https://github.com/aasm/aasm).

## Database

Your application should be using ActiveRecord so the underlying database shouldn't matter much. We will be testing using the PostgreSQL database.

## Authentication & Authorization

The API should ensure that not anyone can access it. Implement a basic system of authorization, we can assume that the user has authenticated with us previously and has the proper credentials.

## Error Monitoring

The API should have proper error monitoring and return errors that make sense downstream for person or application making the request. Internally, we want to monitor errors, especially 500s so we can implement bug fixes. Externally, we want to render common errors (ie. rate limited, not authorized, bad request, the 400's) in a way to users so they can uunderstand why their requests are failing.

## Rate limiting

The API should implement some basic form of rate limiting based on the request. Based on how you implement authorization, we'll also need to check that a particular user is not abusing our endpoints.

## Scalability and Searchability

Imagine the creators database is large - At least 1 million records. While the `GET /creators` endpoint supports some basic functionality - ie. pagination, sort, and limiting, it does not support searchability.

If you had to design a scalable search on this endpoint, how would you go about doing so? This is a critical thinking question that we want you to include as part of the exercise, but do not expect you to implement. Think about issues of scalability and how you would solve them as well as design of the search feature.

For example, the different attributes on a creator you might search (what would we know about a social media creator, imagine we have access to all the information on their public feed.) What if you could search by the types of images they post? Maybe they have a profile where they tell us more about them? Those are all examples of searchable criteria.

The other part is the scalability. How would you architect a system to handle that quantity of records to query assuming it's only going to grow larger and larger (maybe exponentially)? This could include a plan to implement monitoring, solutions at the DB layer as well as monitoring layer.

## Testing

Add whatever test coverage you deem necessary. Please use the [rspec](https://github.com/rspec/rspec-rails) testing framework.

## UI/UX

Our web clients are written in Ember.js - a javascript framework that we to organize business logic on the front end as well as do some of the heavy lifting associated with the request/response cycle and interactive behavior.

Based on the API you have created from the requirements above, we could build a UI that renders each of the gigs eligible for payment.  Explain how you would layout the UI based on [Ember's 5 core concepts](https://emberigniter.com/5-essential-ember-concepts/) - Routes, controllers, models, components (you can ignore services for this exercise).  It's ok if you're not familiar with Ember and there is exact wrong or correct answer. This is an exercise in how you think about laying out a UI given a set of requirements.

These requirements:

1.) A header titled 'Unpaid Gigs'

2.) A main content area with a table

3.) A table header within the main content area with columns: `Creator Name`, `Brand Name`, `Amount in Dollars Owed`

4.) Table rows within the main content area with the data to fill in the various columns from the header


What endpoints would you need to hit to fill in the data?  How would you lay out the components within the top-level route/controller/template - assuming we would use 1 route, controller, and template to fill the full screen and render components within UI to fill in the requirements listed above.

## Deliverables

To submit your project please archive the project folder in a ZIP and email it to `justin@lightricks.com` with the subject `Interview Practice Submission - <your name>`.

If the file size is too large to be attached to an email then upload it to Dropbox or Google Drive and email the link of the ZIP file.
