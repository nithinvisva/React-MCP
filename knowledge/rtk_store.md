# 🧱 RTK Store & API Guidelines for MCP

This guide defines rules and best practices for using Redux Toolkit (RTK), `@reduxjs/toolkit`, and RTK Query within the MCP (Monorepo Component Platform).

---

## 📁 Folder Structure

Organize store logic inside `packages/store`:

```
/packages/store
  └── slices
      └── auth
          ├── authSlice.ts
          └── authSelectors.ts
  └── api
      └── baseApi.ts
      └── authApi.ts
  └── middleware
      └── loggerMiddleware.ts
      └── errorMiddleware.ts
  └── store.ts
```

---

## ⚙️ Store Setup Rules

- Use `configureStore()` from RTK
- Combine slices and API reducers
- Apply default `middleware` from `getDefaultMiddleware`
- Add custom middleware for logging, errors, etc.

```ts
// store.ts
import { configureStore } from '@reduxjs/toolkit';
import { authApi } from './api/authApi';
import authSlice from './slices/auth/authSlice';
import loggerMiddleware from './middleware/loggerMiddleware';

export const store = configureStore({
  reducer: {
    auth: authSlice.reducer,
    [authApi.reducerPath]: authApi.reducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware()
      .concat(authApi.middleware)
      .concat(loggerMiddleware),
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

---

## 🔌 Custom Middleware Guidelines

### Logger Middleware
```ts
// loggerMiddleware.ts
const loggerMiddleware = (storeAPI) => (next) => (action) => {
  console.log('dispatching', action);
  const result = next(action);
  console.log('next state', storeAPI.getState());
  return result;
};

export default loggerMiddleware;
```

### Error Handling Middleware (Optional)
```ts
const errorMiddleware = (storeAPI) => (next) => (action) => {
  try {
    return next(action);
  } catch (err) {
    console.error('Caught error:', err);
  }
};

export default errorMiddleware;
```

---

## 🧩 Slice Creation Rules

- Use `createSlice()` only
- No manual reducer functions
- Use `createAsyncThunk` for side effects (if not using RTK Query)
- Use Immer-compatible state updates

```ts
const authSlice = createSlice({
  name: 'auth',
  initialState,
  reducers: {
    logout: (state) => {
      state.token = null;
    },
  },
});
```

---

## 🌐 API Layer with RTK Query

- All API logic must use `createApi()`
- Group endpoints by domain
- Always set a `baseQuery`
- Export hooks from each API file

```ts
// baseApi.ts
export const baseApi = fetchBaseQuery({
  baseUrl: process.env.NEXT_PUBLIC_API_URL,
  prepareHeaders: (headers, { getState }) => {
    const token = (getState() as RootState).auth.token;
    if (token) headers.set('Authorization', `Bearer ${token}`);
    return headers;
  },
});
```

```ts
// authApi.ts
export const authApi = createApi({
  reducerPath: 'authApi',
  baseQuery,
  endpoints: (builder) => ({
    login: builder.mutation({...}),
    getProfile: builder.query({...}),
  }),
});

export const { useLoginMutation, useGetProfileQuery } = authApi;
```

---

## ♻️ Caching & Tags

- Use `providesTags` and `invalidatesTags` to manage caching
- Group tags logically per API file

```ts
getProfile: builder.query({
  query: () => '/me',
  providesTags: ['User'],
})
```

```ts
logout: builder.mutation({
  query: () => ({ url: '/logout', method: 'POST' }),
  invalidatesTags: ['User'],
})
```

---

## 🚫 Anti-Patterns to Avoid

- ❌ No API calls inside React components (except RTK Query hooks)
- ❌ No raw axios/fetch in components
- ❌ Don’t store derived data in the Redux store
- ❌ No circular imports between slices

---

## 🧪 Sample Project Scaffold

```
/packages/store
  ├── api
  │   ├── baseApi.ts
  │   ├── authApi.ts
  ├── middleware
  │   ├── loggerMiddleware.ts
  │   └── errorMiddleware.ts
  ├── slices
  │   └── auth
  │       ├── authSlice.ts
  │       └── authSelectors.ts
  ├── store.ts
```

---

## ✅ Summary Checklist

- [x] All slices in `/store/slices/`
- [x] All APIs in `/store/api/`
- [x] Store initialized using `configureStore()`
- [x] RTK Query with `createApi()`
- [x] API baseQuery is centralized
- [x] Export RTK Query hooks
- [x] Use tags for cache control
- [x] Avoid non-RTK network calls in components
- [x] Custom middleware implemented and configured

