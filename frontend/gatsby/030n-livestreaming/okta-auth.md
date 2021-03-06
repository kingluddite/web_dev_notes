# Add authentication to your apps with Okta
https://www.youtube.com/watch?v=7b1iKuFWVSw&list=PLz8Iz-Fnk_eTpvd49Sa77NiF8Uqq5Iykx&index=9&t=2775s


## Aaron Parecki (Okta developer Advocate)
Teacher: Aaron Parecki (https://twitter.com/aaronpk)

## Two parts of Okta House
* Famous Part - IT, Enterprise SSO
    - You get your one account for your business and then you can log into all your companies internal apps using Okta
    - Which is way better than having passwords for each individual one
* Developer Side
    - https://developer.okta.com/
    - This is the other side of Okta, and completelys separate from the SSO part
    - This is a tool that helps you manage authentication in your own applications
        + That would cover:
            * If you need to build a sign up form
            * Or you have your users log into stuff and you don't want to deal with it as dealing with passwords is not fun/and sometimes dangerous and this is why you would let someone like Okta deal with it for you
    - Cost
        + Developer account is free forever for up to 1000 active users a month
        + Makes app dev a lot faster
        + Good for any size company
        + no time tier
        + If > 1000 users are logging in per month it will cost money

## What is Gatsby
* Web app framework that follows the static site generator
* We'll build a series of React files that we will build ahead of time
* All the data that we can will get loaded early and get put into the template but with authentication we would not want to do that
    - We would not want to put user information into these templates and publish those somewhere because then anyone who knew the pathname would be able to theoretically get them
    - Because Gatsby is built with React we are able to

## Install Gatsby
`$ npx gatsby new okta-gatsby-auth https://github.com/gatsbyjs/gatsby-starter-hello-world`

* We'll use the gatsby-starter-hello-world
* This is the default from Gatsby and all code was developed by Gatsby developers

## Build a home page that gives us access to a dashboard
* We won't build any backends where the authenticated content would be today
* But we will set up the protected dashboard

`components/layout.js`

```
import React from 'react'

const Layout = ({ children }) => (
  <>
    <header
      style={{
        background: '#333399',
        color: 'white',
        padding: '1rem 5%',
      }}
    >
      My Sweet App
    </header>
    <main
      style={{
        margin: '5rem auto',
        width: '90%',
        maxWidth: 600,
      }}
    >
      {children}
    </main>
  </>
)

export default Layout

```

`pages/index.js`

```
import React from 'react'
import Layout from '../components/layout'

export default () => (
  <Layout>
    <div>Hello world!</div>
  </Layout>
)
```

## Add links
`layout.js`

```
import React from 'react'
import { Link } from 'gatsby'

const Layout = ({ children }) => (
  <>
    <header
      style={{
        background: '#333399',
        color: 'white',
        padding: '1rem 5%',
      }}
    >
      <Link style={{ color: 'white', marginRight: '1rem' }} to="/">
        My App
      </Link>
      <Link style={{ color: 'white' }} to="/dashboard">
        Dashboard
      </Link>
    </header>
    <main
      style={{
        margin: '5rem auto',
        width: '90%',
        maxWidth: 600,
      }}
    >
      {children}
    </main>
  </>
)

export default Layout

```

## Add client only route
* What is a client only route?
* One route takes us home
* One route takes us to dashboard

### We could do this
`pages/dashboard.js`

```
import React from 'react'
import Layout from '../components/layout'

export default () => (
  <Layout>
    <div>Dashboard</div>
  </Layout>
)
```

* Above will route and show the dashboard page but this is not what we want because this page will be built to static HTML so we're not protecting anything

## We could do this a couple of ways
1. We could just authenticate the component that gets passed through to Dashboard
2. Client only routes
    * This lets us say "I only want Anything that is under `/app` I want to be passed to a custom react router"
    * This would be if your dashboard had:
        - an accounts section
        - and a settings section
        - analytics
        - whatever you want on your private dashboard that would be user data
        - [This would be the way](https://www.gatsbyjs.org/docs/building-apps-with-gatsby/) you would solve that problem so anything that is under `/app` will be routed by react route and it will not be server rendered
            + so we can protect that content
            + we are never saying here is user data so put this into a static file that someone can touch

```
// Implement the Gatsby API “onCreatePage”. This is
// called after every page is created.
exports.onCreatePage = async ({ page, actions }) => {
  const { createPage } = actions

  // page.matchPath is a special key that's used for matching pages
  // only on the client.
  if (page.path.match(/^\/app/)) {
    page.matchPath = "/app/*"

    // Update the page.
    createPage(page)
  }
}
```


`dashbaord.js`

```
import React from 'react'
import Layout from '../components/layout'

export default () => (
  <Layout>
    <div>TODO: require authentication to see this</div>
  </Layout>
)
```

## What is an OpenID Connect Client
* This is an open standard that deals with authentication and that is what Okta is based on
* OpenID Connect is a way for the OpenID Server to provide for to log people in and provide a token that the app can use to verify that the user did indeed log in
* The job of the Gatsby app is going to be to request one of these tokens, start the flow and then verify at the end so that it knows that the user did log in and knows who they are

## Than in Okta
* Add new App
* Single-Page App
    - name: Okta + Gatsby Auth
    - Change port to 8000 (gatsby uses this by default)
* Login redirect URIs
    - This is where you want to redirect back to after we are logged in
    - http://localhost:8000/callback
    - We'll create a page called `callback.js`
        + We can make a route called callback or do it on the index page
* Click `Done`
* Now that you created the app and enabled OpenID Connect
* We now need to plugin the `Client ID` to the app which will identify our app to the whole system
* Copy the Client ID
* [react quick start for Okta](https://developer.okta.com/quickstart/#/react/nodejs/express)
* Also have your Org URL

### What this workflow looks like
* Why does the redirect exist? What does it do?
    - When you have a login link in Gatsby it's going to send the user over to the Okta login page and then the user is going to type in their password there, create the account, type in the password and that will all happen outside of our application, that will happen "off domain" on something that Okta manages and then after the user logs in okta is going to send the user back to the Gatsby app and it has to send them somewhere that is ready to cat
    - It has to send them somewhere that is ready to catch that token and somewhere that will be able to verify the token and remember it and sign them in
* Does it send as a post or a get? Or what comes back?
    - Yes it will actually redirect the user's browser to that URL if we are doing the normal OpenID Connect flow

#### The Normal OpenID Connect flow using Gatsby
1. The user will be on Gatsby
2. Then the user will end up on Okta (the address in the browser will be Okta)
3. After the user finishes logging in, Okta sends them back to Gatsby with just a GET request to that callback URL

* Need to double check is whether the clicks that we are going through for react is going to do that or is it going to do it built-in where it all just kind of happens silently on the Gatsby site in the frontend code where that whole exchange happens behind the scenes
* Okta can do it either way

`$ npm install @okta/okta-react --save`

## Configuring the Okta SDK
* [scroll to Configuration heading](https://developer.okta.com/quickstart/#/react/nodejs/express)
* It will need this for everything
* This will need to be available for all components
* It should be in the intitiation of the app

## Here is the React Okta documentation
```
import React, { Component } from 'react';
import { BrowserRouter as Router, Route } from 'react-router-dom';
import { Security, ImplicitCallback } from '@okta/okta-react';
import Home from './Home';

const config = {
  issuer: 'https://dev-414986.oktapreview.com/oauth2/default',
  redirect_uri: window.location.origin + '/implicit/callback',
  client_id: '{clientId}'
}

class App extends Component {
  render() {
    return (
      <Router>
        <Security issuer={config.issuer}
                  client_id={config.client_id}
                  redirect_uri={config.redirect_uri}
        >
          <Route path='/' exact={true} component={Home}/>
          <Route path='/implicit/callback' component={ImplicitCallback}/>
        </Security>
      </Router>
    );
  }
}

export default App;
```

## For Gatsby we will use wrapRootElement
* [link to docs](https://www.gatsbyjs.org/docs/browser-apis/#wrapRootElement)
* For simplicity we spread the `config` because it needs all 3
* The implicit callback will be set elsewhere because that will live in the component that uses it not in the route here
* We use `wrapRootElement` because we are telling Gatsby to put something around the whole site
  - By default we take the pages and wrap them in a general wrapper div so the `Layout` would get unmounted and remounted on every page
  - If you didn't want to do that you could use wrapRootElement to put that around everything on the page

`utils/wrap-root-element.js`

```
import React from 'react'

import { Security, ImplicitCallback } from '@okta/okta-react'

const config = {
  issuer: 'https://ironcove-guide.oktapreview.com/oauth2/default',
  // TODO - make this production ready
  redirect_uri: window.location.origin + '/implicit/callback',
  client_id: '0oajl9xmnh64GtTIS0h7',
}

const wrapRootElement = ({ element }) => {
  ;<Security {...config}>{element}</Security>
}

export default wrapRootElement
```

`gatsby-browser.js`

```
export { default as wrapRootElement } from './src/utils/wrap-root-element.js'
```

* We can remove the ImplicitCallback as we will use it in `pages/callback.js`
  - Because the route will be `localhost:8000/callback` to match what we have in Okta

```
import React from 'react'

import { Security } from '@okta/okta-react'

const config = {
  issuer: 'https://ironcove-guide.oktapreview.com/oauth2/default',
  // TODO - make this production ready
  redirect_uri: window.location.origin + '/implicit/callback',
  client_id: '0oajl9xmnh64GtTIS0h7',
}

const wrapRootElement = ({ element }) => {
  ;<Security {...config}>{element}</Security>
}

export default wrapRootElement
```

* The SDK is going to be dealing with all of this so we just need to make sure that the SDK is available at that page
* It will handle all the verification and eventually redirect back where we started from

## Houston we have a problem
* The Okta SDK is using React Router and Gatsby using React Router
* We could install React Router but that would be several steps
* We can instead fallback and do this manually
  - Two options using plain JavaScript:
    1. Install a library that can do JWT validation, this is the format of OpenID tokens (that is what is happening under the hood that the SDK deals with - so the end result of the OpenID Connect flow there is an ID token sent back and that is a JWT and it needs to be verified, et certa..., thankfully the SDK handles this for us)
    2. The other option (if we stay in front end only - and don't do anything server side) we can use the Okta sign in widget because that will be a small form that gets embedded into the webpage we're making and that will do everything on the page without redirecting, without page reloads and the end result of that in JavaScript the user information is returned back to us and then we can use it
        * This will work because anything we do with auth in gatsby will be completely limited to client side
        * We can treat it as a fully client side app in terms of how we build it, we just need to make sure it is properly escaped so that when we do the build it doesn't break for wanted the `window` to be available
        * This is a good solution because it keeps us out of "router land"

## Roll things back
* Delete `utils` folder and `wrap-root-element.js`
* Delete `callback.js`
* Delete `gatsby-browser.js`
* Delete `gatsby-ssr.js`
* Remove the react library `@okta/okta-react`

`package.json`

```
// MORE CODE

  "dependencies": {
    "gatsby": "^2.1.19",
    "react": "^16.8.3",
    "react-dom": "^16.8.3"
  },

// MORE CODE
```

`$ npm i` to update dependencies

## Back to a clean slate
* Test to make sure all works

`$ gatsby develop`

## Test in browser
* Page should be working

## Move onto the sign in widget
* [link](https://developer.okta.com/quickstart/#/widget/nodejs/express)
* Put in Okta hosted code
* Okta has a build step if you want to host it locally
* But the easy way is to:

1. Paste in the JS and CSS
2. Put that on the page where you want the log in form to appear
3. And the JS code initiates it (this is pure frontend JavaScript code which will tell the widget "hey you should appear now", and ask the user to log in, and instead of a redirect it will happen all on the page without a reload and the result of this function is here is the `id` token that corresponds to the user that logs in)

### Review Code
```
<script type="text/javascript">
// we do this sign in
  var oktaSignIn = new OktaSignIn({
    baseUrl: "https://dev-414986.oktapreview.com",
    clientId: "{clientId}",
    authParams: {
      issuer: "https://dev-414986.oktapreview.com/oauth2/default",
      responseType: ['token', 'id_token'],
      display: 'page'
    }
  });
  // this is the callback - checks for tokens in URL
  if (oktaSignIn.token.hasTokensInUrl()) {
    // okta will grab info
    oktaSignIn.token.parseTokensFromUrl(
      function success(res) {
        // The tokens are returned in the order requested by `responseType` above
        var accessToken = res[0];
        var idToken = res[1]

        // Say hello to the person who just signed in:
        console.log('Hello, ' + idToken.claims.email);

        // Save the tokens for later use, e.g. if the page gets refreshed:
        // sets things so we know someone is logged in
        oktaSignIn.tokenManager.add('accessToken', accessToken);
        oktaSignIn.tokenManager.add('idToken', idToken);

        // Remove the tokens from the window location hash
        window.location.hash='';
      },
      function error(err) {
        // handle errors as needed
        console.error(err);
      }
    );
  } else {
    // if they don't have that it will run session
    oktaSignIn.session.get(function (res) {
      // Session exists, show logged in state.
      if (res.status === 'ACTIVE') {
        // we get user's log in status
        console.log('Welcome back, ' + res.login);
        return;
      }
      // No session, show the login form
      // no we show user the log in button
      oktaSignIn.renderEl(
        { el: '#okta-login-container' },
        function success(res) {
          // Nothing to do in this case, the widget will automatically redirect
          // the user to Okta for authentication, then back to this page if successful
        },
        function error(err) {
          // handle errors as needed
          console.error(err);
        }
      );
    });
  }
</script>
```

* We need to execute the above page only on our dashboard page (the page you want to redirect to when the user signs in)
* First chunk is on callback page (if there are tokens in URL)
* And the bottom part says: 
  - if they are logged in welcome back
  - if they are not logged in, then redirect them out
* This is the URL for redirect
* There is a different URL for inline

## Inline sign in widget
* [link](https://developer.okta.com/code/javascript/okta_sign-in_widget)
* We don't want to use the CDN because importing 3rd party scripts is not what we want to do

### Install sign in widget
`$ npm install @okta/okta-signin-widget --save`
 
## Wrap every component
`$ touch gatsby-browser.js gatsby-ssr.js`

### What is the difference between gatsby-browser.js and gatsby-ssr.js?
* The difference is when they are run
  - `gatsby-browser.js` runs on the client side
  - `gatsby-ssr.js` runs during `$ gatsby build` on the server side
  - In a majority of cases the code in them will be the same but there are cases when they are not the same and that is why they are separate

## Problem with Gatsby and okta
* This is Okta code

```
const config = {
  issuer: 'https://ironcove-guide.oktapreview.com/oauth2/default',
  redirect_uri: window.location.origin + '/implicit/callback',
  client_id: '{clientId}'
}
```

* This is a problem for Gatsby because during builds we don't have a window object to reference
* It it just a way to get the base URL of the app so if there is another way where Gatsby can know what port it is running on
* We will create `gatsby-browser.js`
* And we'll need `gatsby-ssr.js` because we'll need it true in both places
* But the code will be identical so

### Nope - change of plans
* Remove the react library `@okta/okta-react` from Okta

`$ npm i` to remove it from node-modules

`layout.js`

```
import React from 'react'
import { Link } from 'gatsby'
// Okta stuff
import OktaSignIn from '@okta/okta-signin-widget'
import '@okta/okta-signin-widget/dist/css/okta-sign-in.min.css'
import '@okta/okta-signin-widget/dist/css/okta-theme.css'

const Layout = ({ children }) => (

// MORE CODE
```

* We need to get `<div id="widget-container"></div>` into the DOM

`gatsby-browser.js`

```
import React from 'react'
import '@okta/okta-signin-widget/dist/css/okta-sign-in.min.css'
import '@okta/okta-signin-widget/dist/css/okta-theme.css'
import OktaSignIn from '@okta/okta-signin-widget'

class AuthWrapper extends React.Component {
  componentDidMount() {
    var signIn = new OktaSignIn({
      baseUrl: 'https://dev-414986.oktapreview.com',
    })
    signIn.renderEl(
      {
        el: '#okta',
      },
      function success(res) {
        if (res.status === 'SUCCESS') {
          console.log('Do something with this sessionToken', res.session.token)
        } else {
          // The user can be in another authentication state that requires further action.
          // For more information about these states, see:
          //   https://github.com/okta/okta-signin-widget#rendereloptions-success-error
        }
      }
    )
  }

  render() {
    return <>{this.props.children}</>
  }
}

export const wrapRootElement = ({ element }) => (
  <AuthWrapper>{element}</AuthWrapper>
)

```

`layout.js`

```
// MORE CODE
    <div id="okta" />
    <main
      style={{
        margin: '5rem auto',
        width: '90%',
        maxWidth: 600,
      }}
    >
      {children}
    </main>
  </>
)

export default Layout
```

https://developer.okta.com/blog/2018/06/05/authentication-vanilla-js#install-the-widget-for-secure-authentication

```
new OktaSignIn({
  baseUrl: 'https://ironcove-guide.oktapreview.com',
  clientId: '{clientId}',
  redirectUri: window.location.origin,
  authParams: {
    issuer: 'default',
    responseType: ['id_token','token']
  }
})
```

* issues with access token

`gatsby-browser.js`

```
import React from 'react'
import '@okta/okta-signin-widget/dist/css/okta-sign-in.min.css'
import '@okta/okta-signin-widget/dist/css/okta-theme.css'
import OktaSignIn from '@okta/okta-signin-widget'

class AuthWrapper extends React.Component {
  constructor(props) {
    super(props)

    this.state = {
      user: false,
    }

    this.handleLogout = this.handleLogout.bind(this)
  }

  handleLogout = () => {}

  componentDidMount() {
    var signIn = new OktaSignIn({
      baseUrl: 'https://dev-414986.oktapreview.com',
      clientId: '0oajl9xmnh64GtTIS0h7',
      redirectUri: window.location.origin,
      authParams: {
        issuer: 'default',
        responseType: ['id_token', 'token'],
      },
      logo: '//logo.clearbit.com/gatsbyjs.org',
      features: {
        registration: true, // Enable self-service registration flow
        rememberMe: true, // Setting to false will remove the checkbox to save username
      },
    })

    const showSignIn = () => {
      signIn.renderEl(
        {
          el: '#okta',
        },
        function success(res) {
          if (res.status === 'SUCCESS') {
            window.location.reload()
            // console.log('Do something with this sessionToken', res.session.token)
            console.log('Do something with this sessionToken', res)
          } else {
            // The user can be in another authentication state that requires further action.
            // For more information about these states, see:
            //   https://github.com/okta/okta-signin-widget#rendereloptions-success-error
          }
        }
      )
    }

    signIn.session.get(async res => {
      if (res.status === 'ACTIVE') {
        signIn.tokenManager.add('access_token', res[1])
        console.log(signIn.tokenManager.get('access_token'))
        // console.log('active')
        // console.log(res)
        this.setState({ user: res.login })
      } else {
        showSignIn()
      }
    })

    this.handleLogout = event => {
      event.preventDefault()
      signIn.session.close(err => {
        if (err) {
          console.error(err)
          return
        }

        showSignIn()
      })
    }
  }

  render() {
    return (
      <>
        {this.state.user ? (
          <>
            <p>
              Logged in as {this.state.user}.{' '}
              <a href="#logout" onClick={this.handleLogout}>
                log out
              </a>
            </p>
          </>
        ) : null}
        {this.props.children}
      </>
    )
  }
}

export const wrapRootElement = ({ element }) => (
  <AuthWrapper>{element}</AuthWrapper>
)
```

* make sure to add localhost:8000 as trusted origin and CORS and Redirect
* directory > self-service registration
  - Enabled
  - Show Sign Up link
  - Custom URL (only will work if you add localhost:8000 as trusted origin)
* Add API Key
* Add SPA app and assign it a user that will log into it
