# Phase 4 React Project

You've made it! You're ready to build a React application! Before you start
ideating, think about some of the project requirements.

## Requirements

You've been through quite a few Project Modes by now and should have some idea
how to think about scoping a project, what you can accomplish in the designated
time, and what is expected of you in terms of meeting complexity requirements.

The guidelines here are minimal but be sure that you:

1. Use a _Rails API backend_ with a separate _React frontend_ that are created
   in two different Github repositories.
2. Have at least three resources (three DB tables) on the backend. Your
   application must have full CRUD actions for at least one resource.
3. Have at least two different _client-side routes_ (i.e. use `react-router`).
   Ex: even if your whole app is mostly a single page app, have the form to
   signup be found at `/signup`
4. **Optional:** Your application can have authentication/authorization. You are
   welcome to use an auth template as discussed in class.

## Backend Setup

To create your Rails API, run:

```
rails new <my-project> --api -T --database=postgresql
```

Let's go through this in detail:

- `--api`: Make a [Rails API](http://edgeguides.rubyonrails.org/api_app.html),
  basically you're telling Rails you don't want any of the stuff you wouldn't
  need for an application where Rails is not rendering views. Think the
  ActionView library (`form_for`, `link_to`, etc..), ERB, Security protections
  that ensure forms were rendered by the Rails app, things like that.
- `-T`: don't generate tests for this app
- `--database=postgresql`: Set this up to use a Postgres (as opposed to SQLite)
  database. If you ever want to push this to Heroku, Heroku requires a Postgres
  database. There won't be too much difference in how you have to write your
  code. You'll have to be sure to run `rails db:create` and make sure you have
  Postgres running (i.e you can see the elephant)

Be sure to do the necessary setup for the [rack-cors-gem](https://github.com/cyu/rack-cors)!

You may also want to use
[active-model-serializers](https://github.com/rails-api/active_model_serializers/tree/0-10-stable).

## Frontend Setup

To create your React project, you should use
[Create React App](https://create-react-app.dev/). To use this, run:

```sh
npx create-react-app my-app
```

It will generate your project structure along with all the necessary
dependencies for working with React. It will _also_ initialize a Git repository
in your project directory.

We'd recommend to begin by removing any of the default stuff given to you by
Create React App that you do not understand or won't be using.

## Working with APIs

It is highly suggested that any calls to 3rd party APIs are made _through your
backend_.

Example: A user clicks a button that says 'Get Gifs'

- React makes a request to Rails
- Rails makes a request to the Giphy API
- Rails receives the response from Giphy and sends to React
- React receives the response from Rails and you do something with it on the client

```js
// src/components/GifSearch.js
fetch("http://localhost:3000/gifs?q=dogs")
  .then((r) => r.json())
  .then((gifs) => console.log(gifs));
```

```rb
# app/controllers/gifs_controller.rb
def search
  response = RestClient.get "giphy.com/search?q=#{params[:q]}"
  gifs = JSON.parse(response.body)
  # you could add serialization here if you like!
  render json: gifs
end
```

This is so you can avoid any _CORS_ issues. If you are unable to hit an API from
your React app due to a CORS restriction, it is very likely that it is
impossible to do so.

> Brief Refresher on CORS: the idea is that from one domain (the port your
> webpack development server is running on) you are not allowed to access
> another domain. You must make the request from a server (i.e. Rails), so the
> request is exempt from the Same-Origin Policy restriction.

## Running Client and Server Code

By default, both your React app and your Rails app will run on port 3000. You'll
have to specify one or the other to run on a separate port.

- Rails: `rails s -p <some_number_thats_not_3000>`
- React: Check out this [issue](https://github.com/facebookincubator/create-react-app/issues/1083)

## Resources

- [How DHH organizes his controllers in Rails](http://jeromedalbert.com/how-dhh-organizes-his-rails-controllers/)
- [React Docs on File Structure](https://reactjs.org/docs/faq-structure.html)
- [The 100% Correct Way to Structure a React App (or why there’s no such thing)](https://hackernoon.com/the-100-correct-way-to-structure-a-react-app-or-why-theres-no-such-thing-3ede534ef1ed)
