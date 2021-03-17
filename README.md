# Working with APIs

## Objectives

1. Understand why programmers need to work with APIs
2. Learn some of the basic ways in which programmers work with APIs

## Define API

An API, or application programming interface, is a manner in which companies and
organizations, like Twitter or the New York City government, expose their data
and/or functionality to the public (i.e. talented programmers like yourself) for
use. APIs allow us to add important data and functionality to the applications
we build. Here's just a few examples of some of the cool things you can do by
using APIs:

* Create an app that allows users to sign up/sign in via
  Facebook/Google/Twitter/Github/etc.
* Use the NYC Open Data API to get and map data––everything from Health
  Department restaurant ratings to public park locations and hours to New York
  City Public Housing repair issues to noise complaints to public school
  construction, you name it!
* Use the Yelp API to find and deliver popular local spots to your users.
* Use the Weather Underground API to give your users up-to-date weather alerts.
* Use the Ticket Master API to inform your users if their favorite musician has
  an upcoming show.
* So much more!

This is just a small sample of what working with APIs allows us to do as
developers. Throughout the course of your programming life, you'll likely be
exposed to working with many different APIs. You'll even learn how to build your
own API later on in this course. This reading seeks to introduce the topic,
emphasize some of the benefits of getting comfortable working with APIs and
offer a brief intro into some of the common methods of working with APIs.

## How to Work with APIs

Different APIs expose their data and functionalities in different ways. However,
there are commonalities among them and there are common approaches that we'll
discuss here. Generally speaking, there are two main uses for APIs––getting data
and adding functionality (i.e. signing in with Facebook or posting to
Instagram). We'll be discussing the "getting data" part of working with APIs
here. 

Many APIs are built on what is referred to as a REST-ful framework. That means
that the "endpoints", or URLs to which we can send a request for data, follow
certain conventions. These URLs should allow you to request information, send
information, update information and delete information. Let's focus on the
"getting information" request.

### Retrieving Data from an API

For this walk-through, we'll be working with a mock API. Most of the APIs you'll be
interacting with during this course will serve JSON data to you. 

It is important to note - The APIs we're accessing data from are run on servers in some form or
another. What language those servers are running isn't actually relevant to our needs
on the front-end. The server could be JavaScript, Ruby, Python, etc... The language doesn't
matter because all of these languages are able to serve up JSON data. From the perspective
of the front-end, we just need to know the endpoint we should to access to get the JSON data we need. 

For this lesson, we've set up a [JSON file on GitHub][json-api] that contains some example data. In
GitHub, this file can be [viewed like any other file][regular]. However, GitHub gives us the option to
access files as raw data, which serves up the contents of the file directly. In essence, by serving
the raw file data, any JSON file on GitHub can be turned into a static API.

### Scenario

Let's say we've been hired by a school to create an app that will help students sign their
up for after-school activities. To connect studentst to after-school
activities around the school, we need a data set of those activities. Luckily for us, the
school has collected that information and allows the public to access it via a specific 
[API endpoint][json-api].

[json-api]: https://raw.githubusercontent.com/learn-co-curriculum/json-api-exampless/main/school-programs.json
[regular]: https://github.com/learn-co-curriculum/json-api-exampless/blob/main/school-programs.json

**Top Tip:** Once you find the right URL for retrieving your data, test it out
directly *before* you try to request the data from inside your
program. You can do this in a few ways - for public APIs, you can probably just paste
the URL into your browser directly. For APIs that require Authentication, an app like Postman
can be used to structure our a request. By doing this first, once you try to request
the data from within your program, if it doesn't work, at least you'll know it's
something wrong with your code, as opposed to something wrong with the API.

#### Identifying the API Endpoint

In this example, we've provided the endpoint, but you'll often have to figure out the correct endpoint
on your own. Most APIs include documentation on how they can be used - make sure to take time to
familiarize yourself using whatever documentation is provided. The documentation will oftten include
what _endpoints_ are available on the API. **Endpoint** refers to the URL we can submit a request to and that will return
to us the desired data.

Open up a new tab in your browser and paste in the following URL:
`https://raw.githubusercontent.com/learn-co-curriculum/json-api-exampless/main/school-programs.json`

The page brings you to the desired set of data! Notice that the data is laid
out in what looks like an array of nested objects. This is actually a
[JSON](http://json.org/) object (or, at least, your browser's representation of it).

#### Sending a Request to an API from a Program or Application

Now that we understand what an API is and have an endpoint example that serves up some data,
let's use that same URL to send a request for data from a JavaScript program.

Open up `lib/nyc_api.rb`. Let's take a look at the code here:

```js
function getData(url) {
  fetch(url)
    .then(response => {
      return response.json()
    })
    .then(data => {
      return data
    })
}

getData('http://data.cityofnewyork.us/resource/uvks-tn5n.json')
```

We have a `getData` method that sends an HTTP GET request from our application
via `fetch`.

Now, in your terminal in the directory of this lab, run `node app/nycAPI.js`. It
should output the response from the NYC Open Data API!

### Working with API Data

Now that we have all of our data back from the API, we need to be able to
collect and present it within the context of our app. Since we've been practicing
working with nested data structures, we shouldn't have too much trouble. Let's
write a function, `programSchool`, that just returns a list of the schools or
organizations that are running our after school programs.

Copy and paste the following code into our GetPrograms class:

```js
function programSchool() {
  //we use .json() to parse JSON into a useable object
  getData('http://data.cityofnewyork.us/resource/uvks-tn5n.json')
  
}
# 
  programs = JSON.parse(self.get_programs)
  programs.collect do |program|
    program["agency"]  
  end
end
```

At the bottom of the file, comment out:

```ruby
programs = GetPrograms.new.get_programs
puts programs
```

Let's `puts` out a unique list of the schools. Paste in the following two lines
right underneath where you commented out the above two lines:

```ruby
programs = GetPrograms.new
puts programs.program_school.uniq
```

Now, run the program with `ruby lib/nyc_api.rb` in your terminal. You should
see something like this:

```bash
Rockaway Artist Alliance, Inc.
CAMBA  
Sports and Arts In Schools Foundation, Inc.
New York Junior Tennis League
Arthur Ashe Institute for Urban Health, Inc
Citizens Advice Bureau, Inc.
United Activities Unlimited, Inc.
Kips Bay Boys & Girls Club
National Society For Hebrew Day Schools
School Settlement
The Door - A Center of Alternatives
Community Counseling and Mediation
Dream Yard Project, Inc.
The Childrens Aid Society
Just Us, Inc.
Bushwick Community Action Association, Inc.
Greater Ridgewood Youth Council, Inc.
Brooklyn Bureau of Community Services
...
```

So far we've used the NET::HTTP and URI to send a request for data to an API
endpoint, a URL. We've operated on the data that was returned to us, making sure
it was formatted properly as JSON and iterating over that JSON to retrieve the
name of the school hosting each after-school program. That was a lot of work! I
wonder if there is an easier way to work with popular APIs...

### Using Gems to Work with APIs

Many popular APIs have wrappers, i.e. gems that are code libraries that do a lot
of the heavy lifting for you. In fact, we'll use a gem to interact with the
Twitter API in the next lab.

## Conclusion

To recap: APIs generally either provide a user with data or added functionality.
We can use APIs that serve data to get information for our own applications and
projects. To get this data, we need to send a request to the URL of the API and
know how to work with the response we receive. Many APIs serve data in JSON
format, which needs to be parsed before we can use it. Once parsed, it becomes
a hash we can work with and extract data from.

In our example, we were able to retrieve remote information from an API using
the built-in Ruby classes `NET::HTTP` and `URI`. By putting this implementation
inside a class, we can develop highly reuseable code that lets us access all
sorts of information remotely.

The `GetPrograms` class used a hard-coded URL, stored as a class constant, and
included an instance method called `get_programs`:

```ruby
URL = "http://data.cityofnewyork.us/resource/uvks-tn5n.json"

def get_programs
  uri = URI.parse(URL)
  response = Net::HTTP.get_response(uri)
  response.body
end
```

But we could easily adapt this code to be flexible and accept _any_ URL we pass
in using an `initialize` method and an instance variable. The `get_programs`
method is really just getting the response body from the requested URL, so we
could name this `get_response_body` to be more accurate. We could replace the
custom `program_school` method with a general `parse_json` method, as well.
Instead of a specific class, we would instead have a class that retrieves
JSON from any provided URL!

## Resources

* [NET::HTTP][`net/http`]
* [Open URI][`open-uri`]

[`net/http`]: https://ruby-doc.org/stdlib-2.6.3/libdoc/net/http/rdoc/Net/HTTP.html
[`open-uri`]: https://ruby-doc.org/stdlib-2.6.3/libdoc/open-uri/rdoc/OpenURI.html
