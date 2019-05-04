# Coping with troubled backend

If the backend on your project keeps shutting down unexpectedly or changes the data shape without any preliminary notice and it breaks your frontend, here is what you can do:

1. **Duscuss it with the backend team**

    Even though it's obvious for you, the backend team may not know that you can't work when the servers are shut down or what it takes time to fix frontend after each data shape change. It is very important to explicitly let guys on the other side know this before you take any preventive steps. Read further only if this advice doesn't help.

2. **Build mock data, never delete it, always update it**

    This allows your frontend to work independently, even when the servers are shut down. This makes behaviour of your app more predictable during development and makes you able to postpone any bug fixing related to data shape change. When (or if) the backend becomes stable, it's still useful to have mocks by your side to test some edge cases or to be sure that some bug doesn't come from backend side.

3. **Adapt the data before using it in components**

    Sometimes responses from server look like this:

    ```json
    {
      'dt_month': 201801,
      'dev_id': 5,
      'geo_id': 1,
      'rch': 49819.5,
      'rch_p': 100,
    }
    ```

    We can figure out what `dt_month`, `dev_id`, `geo_id` mean but it's hard to get what is the purpose of `rch` and `rch_p` and we sertainly don't want names like this in our codebase. This and frequent data shape changes are good reasons to _adapt_ the data in a dedicated funtion:

    ```typescript
    interface Adapter {
      (data: SomeRawDataFromBackend): AdaptedData
    }
    ```

    This way all the data transformation is contained in a single function which makes it very convenient to fix when changes on backend happen.

4. **Validate the data with JSON Schema or decode it with io-ts**

    [JSON Schema](https://json-schema.org/) allows you to describe what data you expect to get from backend and validate that data before using it so if any unexpected data shape changes happen in production you are able to show nice error message instead of a corrupted view. One of the most popular validating libraries is [Ajv](https://github.com/epoberezkin/ajv).

    But this has a downside if you use Typescript. Because your static types and JSON Schemas are independent of each other, how you describe your data using each may differ, so the data validated by the schema may still break your statically typed code if you don't keep them in sync. In this case, you may want to use [io-ts](https://github.com/gcanti/io-ts), it provides Typescript integration that infers the types instead of making you define them manually.