---
layout:     post
title:      "Integrating a React Application with a Backend API Using Redux Toolkit"
subtitle:   "A easy to folow template for integration"
date:       2024-01-12
author:     "Pasindu"
image:      "https://images.squarespace-cdn.com/content/v1/60794dbc8615125d3ad57026/93c6179d-9540-41c4-aae8-a2ae83f5e850/Frame-37-1.png"
summary: "This is a detailed article on integrating a React application with a backend API using Redux Toolkit and custom hooks. In this article we will give you a easy to follow template to integrate your react front end with a REST backend. This step-by-step guide will help developers set up their applications to fetch data from a backend server and manage it efficiently using Redux Toolkit."
tags:
    - React
    - Middleware
    - Redux

---

Integrating a React application with a backend API is a common requirement in web development. In this guide, we will walk through the process of setting up such an integration using Redux Toolkit and custom hooks in a React application. By the end, you'll have a template you can follow to easily fetch and manage data from your backend server.

###  **Setting Up Your Project**
- **Create a React App**: Use Create React App to set up your project.
  ```sh
  npx create-react-app my-app
  cd my-app
  ```

- **Install Redux Toolkit and React-Redux**:
  ```sh
  npm install @reduxjs/toolkit react-redux
  ```

###  **Project Structure**
Organize your project for scalability. A typical structure might look like this:
```
src/
|-- components/
|   |-- Banner.js
|-- hooks/
|   |-- useTopBannerFetch.js
|-- redux/
    |-- store.js
    |-- topBannerSlice.js
|-- services/
    |--api
        |- fetchBanners.js
|-- App.js
|-- .env
|-- index.js
```

#### Step 1: Setting Up Axios and Async Thunks

First, we need to set up an asynchronous function to fetch data from our backend API using Axios and Redux Toolkit’s `createAsyncThunk`.

```javascript
//fetchBanners.js

import { createAsyncThunk } from "@reduxjs/toolkit";
import axios from "axios";

const fetchBannerMiddleware = createAsyncThunk('fetchTopBanners', async () => {
    const response = await axios.get(process.env.REACT_APP_BANNER_URI);
    return response.data;
});

export default fetchBannerMiddleware;
```

Here, `fetchBannerMiddleware` is an asynchronous thunk that fetches banner data from the backend API defined by the environment variable `REACT_APP_BANNER_URI`.

#### Step 2: Defining Environment Variables

Define your backend URLs in a `.env` file at the root of your project. This helps manage URLs easily across different environments (development, production, etc.). Keep in mind that in order to load these data property names should start with 'REACT_APP_'

```
REACT_APP_BACKEND_URI=http://localhost:8080
REACT_APP_BANNER_URI=http://localhost:8080/banner
```

#### Step 3: Creating a Redux Slice

Next, create a Redux slice to manage the state of the fetched data. This slice will handle the different states of the fetch request (pending, fulfilled, and rejected).

```javascript
// topBannerSlice.js

import { createSlice } from "@reduxjs/toolkit";
import fetchBannerMiddleware from "../services/api/fetchBanners";

export const topBannerSlice = createSlice({
  name: "topBanner",
  initialState: {
    topBanner: false,
    banners: [],
    status: 'init',
    errorData: null
  },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchBannerMiddleware.fulfilled, (state, action) => {
        state.banners = action.payload;
        state.status = 'success';
      })
      .addCase(fetchBannerMiddleware.rejected, (state, action) => {
        state.errorData = action.payload;
        state.status = 'error';
      })
      .addCase(fetchBannerMiddleware.pending, (state) => {
        state.status = 'pending';
      });
  },
});

export default topBannerSlice.reducer;
```

#### Step 4: Configuring the Store

Configure the Redux store to include the new slice. This ensures that the state managed by the slice is part of the global state.

```javascript

// store.js

import { configureStore } from "@reduxjs/toolkit";
import topBannerReducer from "../slices/topBannerSlice"; // Adjust the import path as necessary


export const store = configureStore({
  reducer: { bannerReducer: persistedReducer },
});
```

#### Step 5: Creating a Custom Hook

Create a custom hook to fetch the banner data and return the state values. This hook will be used in your components to access the data.

```javascript
// useTopBannerFetch.js

import { useDispatch, useSelector } from "react-redux";
import fetchBannerMiddleware from "../services/api/fetchBanners";
import { useEffect } from "react";

const useTopBannerFetch = () => {
    const dispatch = useDispatch();
    const bannerData = useSelector((state) => state.bannerReducer.banners);
    const status = useSelector((state) => state.bannerReducer.status);
    const error = useSelector((state) => state.bannerReducer.errorData);

    useEffect(() => {
        dispatch(fetchBannerMiddleware());
    }, [dispatch]);

    return { bannerData, status, error };
};

export default useTopBannerFetch;
```

#### Step 6: Using the Custom Hook in a Component

Finally, use the custom hook in your component to fetch and display the banner data.

```javascript
// Banner.js

import React, { useState } from "react";
import { Link } from "react-router-dom";
import Slider from "react-slick"; // Assuming you're using react-slick for the slider
import Image from "../designLayouts/Image";
import useTopBannerFetch from "../../hooks/useTopBannerFetch";

const Banner = () => {
  const [dotActive, setDocActive] = useState(0);
  const { bannerData, status, errorData } = useTopBannerFetch();

  if (status === 'pending') return <div>Loading...</div>;
  if (status === 'error') return <div>Error: {errorData}</div>;
  // other codes 
  return (
    <Slider {...settings}>
      {bannerData && bannerData.map((banner) => (
        <Link to="/offer" key={banner.id}>
          <div>
            <Image imgSrc={banner.imgSrc} />
          </div>
        </Link>
      ))}
    </Slider>
  );
};

export default Banner;
```

### Conclusion

By following these steps, you can effectively integrate a React application with a backend API using Redux Toolkit. This approach ensures a clean and manageable way to handle asynchronous data fetching and state management. Use this template as a foundation and adjust it according to your project requirements. Happy coding!

---