# Test-Driven-Development with React & Redux: Thunk, Slices & Requests Mocking - DEV Community 👩‍💻👨‍💻
![](https://res.cloudinary.com/practicaldev/image/fetch/s--tyZQMKck--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://img.bloggu.io/ipfs/bafkreigckhma7r5rcumchxeixtnpy3hlcitop35rvfz7wg4ewiq6xvqiea)

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--glvnkl59--/c_fill,f_auto,fl_progressive,h_50,q_auto,w_50/https://dev-to-uploads.s3.amazonaws.com/uploads/user/profile_image/282902/4f864134-f853-4f7e-84c5-3edcb1650361.jpg)
](https://dev.to/koladev)

_If you want to read more of these articles, don't hesitate to subscribe to my [newsletter](https://www.getrevue.co/profile/koladev32).😁_

Writing tests in Redux may definitely sound counter-intuitive. It may seem even more complex if you are working with Redux.🥶

However, writing tests before adding features helps write better code because you think upfront about the design patterns, the architecture, and the variable's name that will be used.🚀

## [](#project)Project

We are building a user management dashboard. Basically, using Redux and thinks, we want to perform CRUD actions.  
Then, the user can:

-   Create a user.
-   Update a user.
-   Delete a user.
-   Get a user or the list of users.

Users in this small project will have four attributes:

-   An id
-   A name
-   An username
-   An email

For sake of simplicity, we won't write UI code. We'll mostly focus on creating a testing environment, writing tests, and making sure we have slices and thunk handling what we want.

## [](#setup-the-project)Setup the project

First of all, create a simple React project.  

    yarn create react-app react-redux-test-driven-development 

Enter fullscreen mode Exit fullscreen mode

Once the project is created, make sure everything works by running the project.  

    cd react-redux-test-driven-development
    yarn start 

Enter fullscreen mode Exit fullscreen mode

And you'll have something similar running at [http://localhost:3000](http://localhost:3000/).

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--oCwdrGFE--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn.hashnode.com/res/hashnode/image/upload/v1649575625302/8wYFebD1Y.png)
](https://res.cloudinary.com/practicaldev/image/fetch/s--oCwdrGFE--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn.hashnode.com/res/hashnode/image/upload/v1649575625302/8wYFebD1Y.png)

Next, we want to install redux packages but also a mock adapter. The mock adapter will help us simulate requests on a server.  

    # Yarn
    yarn add @reduxjs/toolkit axios-mock-adapter axios 

Enter fullscreen mode Exit fullscreen mode

Great! Once it's installed, let's move to write mock data for the tests first.🍔

## [](#mocking-data-for-the-tests)Mocking data for the tests

In the src directory, create a new directory called `utils`. Then, create a file called `tests.data.js`.

This file will contain the following methods and variables:

-   mockNetWorkResponse: create the mock adapter on the default instance and mock any GET or POST request to the required endpoints
-   getCreateUserResponse: return the response of a POST request on `/user/`
-   getUserListResponse: return the response of a GET request on `/user/`

Let's write these methods.  

    import axios from "axios";
    import MockAdapter from "axios-mock-adapter";

    const getCreateUserResponse = {
      id: 3,
      name: "Clementine Bauch",
      username: "Samantha",
      email: "Nathan@yesenia.net"
    };

    const getUserListResponse = [
      {
        id: 1,
        name: "Leanne Graham",
        username: "Bret",
        email: "Sincere@april.biz"
      },
      {
        id: 2,
        name: "Ervin Howell",
        username: "Antonette",
        email: "ervin@april.biz"
      },
    ];

    // Adding mock network response that is used in tests

    const mockNetWorkResponse = () => {
      const mock = new MockAdapter(axios);

      mock.onGet(`/users/`).reply(200, getUserListResponse);
      mock.onPost(`/users/`).reply(200, getCreateUserResponse);
    };

    export {
      mockNetWorkResponse,
      getCreateUserResponse,
      getUserListResponse,
    }; 

Enter fullscreen mode Exit fullscreen mode

Great! With the mock adapter ready, we can focus on initializing the store and writing tests for the slices.

## [](#writing-tests)Writing tests

This is the most interesting part. Let's go TDD.🔥  
First of all, let's create the store and configure it. In the src directory, create a new directory called `index.js`. In this file, initialize the store.  

    import { configureStore } from "@reduxjs/toolkit";
    import { combineReducers } from "redux";

    const rootReducer = combineReducers({
      // Adding the reducers
    });

    export const store = configureStore({
      reducer: rootReducer,
    }); 

Enter fullscreen mode Exit fullscreen mode

### [](#writing-userslice)Writing userSlice

A "slice" is a collection of Redux reducer logic and actions for a single feature in your app, typically defined together in a single file. The `userSlice` will have actions and reducers to perform CRUD actions.  
The default state for the slice should be an empty array, after all, we are dealing with `users`.  
Let's get into it by writing a test and making it fail. Create a new directory in the `src/store` called `slices`.  
Inside this directory, add a file called `user.test.js`. This file will contain the tests we'll write for the `userSlice`.

The first test is to make sure that the store is empty or undefined. The initial state will probably look like this.  

    const initialState = {
      users: [],
      loading: false,
      error: null
    }; 

Enter fullscreen mode Exit fullscreen mode

Let's write the first test.

#### [](#testing-the-initial-state)Testing the initial state

In the `user.test.js` file, add the following test:  

    import reducer, {
        initialState,
      } from "./user";
      /**
       * Testing the initial state
       */

      test("Should return initial state", () => {
        expect(
          reducer(undefined, {
            type: undefined,
          })
        ).toEqual(initialState);
      }); 

Enter fullscreen mode Exit fullscreen mode

Now run the `yarn test` command. The test will fail.❌  
Totally normal. We haven't defined the `userSlice`, the reducer, and the initial state.

Inside the slice directory, create a file called user.js.  

    export const initialState = {
      users: [],
      loading: false,
      error: null
    };

    export const userSlice = createSlice({
      name: "users",
      initialState: initialState,
      extraReducers: () => {
      },
    });

    export default userSlice.reducer; 

Enter fullscreen mode Exit fullscreen mode

And also, register the slice reducer in the store in `store/index.js`.  

    import { configureStore } from "@reduxjs/toolkit";
    import { combineReducers } from "redux";
    import { userSlice } from "./slices/user";

    const rootReducer = combineReducers({
      users: userSlice.reducer,
    });

    export const store = configureStore({
      reducer: rootReducer,
    }); 

Enter fullscreen mode Exit fullscreen mode

And run the tests again.✅

#### [](#testing-the-user-creation)Testing the user creation

For this, we need to write a thunk. A thunk is a function that takes the store's dispatch method as the argument and which is afterward used to dispatch the synchronous action after the API or side effects has been finished.

First of all, let's write the test for this feature.  

    import reducer, {
        initialState,
        addUser
      } from "./user";
      import {
        mockNetWorkResponse,
        getCreateUserResponse,
      } from "../../utils/tests.data";

     /**
       * Testing the createUser thunk
       */

      describe("Create a new user", () => {
        beforeAll(() => {
          mockNetWorkResponse();
        });

        it("Should be able to create a new user", async () => {
          // Saving previous state
          const previousState = store.getState().users;

          const previousUsers = [...previousState.users];
          previousUsers.push(getCreateUserResponse);

          // Dispatching the action

          const result = await store.dispatch(addUser(getCreateUserResponse));

          const user = result.payload;

          expect(result.type).toBe("users/addUser/fulfilled");
          expect(user).toEqual(getCreateUserResponse);

          const state = store.getState().users;

          expect(state.users).toEqual(previousUsers);
        }); 

Enter fullscreen mode Exit fullscreen mode

In this test, we are:

-   Saving the previous state and modifying the `users` property to the expected state before making updates. This will help when we are comparing the next state.
-   Dispatching an action and making sure that it's fulfilled and that we compare the expected state and the actual state.

Again, the tests will fail. Let's add the thunk and the reducer for the create user feature.  

    import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";
    import axios from "axios";

    const addUser = createAsyncThunk("users/addUser", async (user) => {
      const res = await axios.post(`/users/`, user);
      return res.data;
    });

    export const initialState = {
      users: [],
      loading: false,
      error: null
    };

    export const userSlice = createSlice({
      name: "users",
      initialState: initialState,
      extraReducers: () => {
        /*
         * addUser Cases
         */

        builder.addCase(addUser.pending, (state) => {
          state.loading = true;
        });
        builder.addCase(addUser.rejected, (state, action) => {
          state.loading = false;
          state.error = action.error.message || "Something went wrong";
        });
        builder.addCase(addUser.fulfilled, (state, action) => {
          state.loading = true;
          state.users.push(action.payload);
        });
      },
    });

    export default userSlice.reducer;
    export { addUser }; 

Enter fullscreen mode Exit fullscreen mode

And run the tests again and it should pass.✅

#### [](#writing-tests-for-getting-a-list-of-users)Writing tests for getting a list of users

First of all, let's write the test for this feature.  

    import reducer, {
        initialState,
        addUser,
        fetchUsers
      } from "./user";
      import {
        mockNetWorkResponse,
        getCreateUserResponse,
        getUserListResponse
      } from "../../utils/tests.data";

    ...
      /**
       * Testing the fetchUsers thunk
       */

      describe("List all users", () => {
        beforeAll(() => {
          mockNetWorkResponse();
        });

        it("Shoudl be able to fetch the user list", async () => {
          const result = await store.dispatch(fetchUsers());

          const users = result.payload;

          expect(result.type).toBe("users/fetchUsers/fulfilled");
          expect(users).toEqual(getUserListResponse);

          const state = store.getState().users;

          expect(state.users).toEqual(getUserListResponse);
        });
      }); 

Enter fullscreen mode Exit fullscreen mode

And make sure the tests fail.  
Let's add the reducer and the thunk.  

    import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";
    import axios from "axios";

    const fetchUsers = createAsyncThunk(
      "users/fetchUsers",
      async () => {
        const response = await axios.get(`/users/`);
        return response.data;
      }
    );

    const addUser = createAsyncThunk("users/addUser", async (user) => {
      const res = await axios.post(`/users/`, user);
      return res.data;
    });

    export const initialState = {
      users: [],
      loading: false,
      error: null
    };

    export const userSlice = createSlice({
      name: "users",
      initialState: initialState,
      extraReducers: () => {
        /*
         * addUser Cases
         */

        builder.addCase(addUser.pending, (state) => {
          state.loading = true;
        });
        builder.addCase(addUser.rejected, (state, action) => {
          state.loading = false;
          state.error = action.error.message || "Something went wrong";
        });
        builder.addCase(addUser.fulfilled, (state, action) => {
          state.loading = true;
          state.users.push(action.payload);
        });

        /*
         * fetchUsers Cases
         */

        builder.addCase(fetchUsers.pending, (state) => {
          state.loading = true;
        });
        builder.addCase(fetchUsers.fulfilled, (state, action) => {
          state.loading = false;
          state.users = action.payload;
        });
        builder.addCase(fetchUsers.rejected, (state) => {
          state.loading = false;
        });
      },
    });

    export default userSlice.reducer;
    export { addUser, fetchUsers }; 

Enter fullscreen mode Exit fullscreen mode

And the tests should pass.✅

Great! We've just written some tests using Redux, thunk, and axios mock.🤩

A little bit of a challenge for you? Add features such as deletion of a user, modification, and also the possibility of retrieving a user.

You'll find the code with all these features [here](https://github.com/koladev32/react-redux-test-driven-development).

## [](#conclusion)Conclusion

In this article, we made a quick introduction to TDD with Redux. If you are looking to write React components using TDD, you can check this [article](https://dev.to/koladev/introduction-to-test-driven-development-in-react-for-beginners-260f) I've written.

And as every article can be made better so your suggestion or questions are welcome in the comment section.

_Article posted using [bloggu.io](https://bloggu.io/). Try it for free._ 
 [https://dev.to/koladev/test-driven-development-with-react-redux-thunk-slices-requests-mocking-585i](https://dev.to/koladev/test-driven-development-with-react-redux-thunk-slices-requests-mocking-585i)
