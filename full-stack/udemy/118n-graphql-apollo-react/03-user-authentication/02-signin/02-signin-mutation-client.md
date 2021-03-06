# Implement Signin Mutation on client
* Copy graphql code you were using and paste into `index.js`

### First create your shell
`queries/index.js`

```
// MORE CODE

// User Mutations
export const SIGNIN_USER_MUTATION = gql`
 // playground code paste here
`;

export const SIGNUP_USER_MUTATION = gql`

// MORE CODE
```

### Next Paste in Playground code

```
// MORE CODE

// User Mutations
export const SIGNIN_USER_MUTATION = gql`
 mutation SIGNIN_USER_MUTATION ($username: String!, $password: String!) {
    signinUser(username: $username, password: $password) {
      token
    }
  }
`;

export const SIGNUP_USER_MUTATION = gql`

// MORE CODE
```

## Save time
* Copy all `Signup.js` code and paste into `Signin.js`

`Signin.js`

```
import React, { Component } from 'react';
import { Mutation } from 'react-apollo';
import { SIGNIN_USER_MUTATION } from '../../queries/index';

// custom components
import Error from '../Error';

const initialState = {
  username: '',
  password: '',
};

class Signin extends Component {
  state = {
    ...initialState,
  };

  clearState = () => {
    this.setState({
      ...initialState,
    });
  };

  handleChange = event => {
    const { name, value } = event.target;
    // console.log(name, ':', value);
    this.setState({
      [name]: value,
    });
  };

  handleSubmit = (event, signinUser) => {
    event.preventDefault();
    // console.log(signinUser);
    signinUser().then(({ data }) => {
      // console.log(data);
      this.clearState();
    });
  };

  validateForm = () => {
    const { username, password } = this.state;
    const isInvalid = !username || !password;

    return isInvalid;
  };

  render() {
    const { username, password } = this.state;

    return (
      <div className="App">
        <h2 className="App">Signin</h2>
        <Mutation mutation={SIGNIN_USER_MUTATION} variables={{ username, password }}>
          {(signinUser, { data, loading, error }) => (
            <form className="form" onSubmit={event => this.handleSubmit(event, signinUser)}>
              <label htmlFor="username">
                Username
                <input
                  type="text"
                  name="username"
                  id="username"
                  placeholder="Username"
                  onChange={this.handleChange}
                  value={username}
                />
              </label>
              <label htmlFor="password">
                Password
                <input
                  type="password"
                  name="password"
                  id="password"
                  placeholder="Password"
                  onChange={this.handleChange}
                  value={password}
                />
              </label>
              <div>
                <button type="submit" disabled={loading || this.validateForm()} className="button-primary">
                  Signin
                </button>
                {error && <Error error={error} />}
              </div>
            </form>
          )}
        </Mutation>
      </div>
    );
  }
}

export default Signin;  
```

* `Signin.js` will be very similar to `Signup.js`
* Make the logical changes (see above)

## Visit route
`http://localhost:3000/signin`

* Log in and you will see the data object returned from our **console.log()** and you'll see token if successfully logged in

## Git stuff

### Add to staging with git
`$ git add -A`

### Commit with git
`$ git commit -m 'Add signin mutation client`

## Push to github
`$ git push origin signin`

## Next - Add token to localStorage




