# State mutation workflow

**Question:** When to use API and when should API be skipped? Should use API heavily or should use state directly and use API to refetch data as less as possible, only when required?

!!! info
    You have to evaluate which approach should be used based upon your app. **OR** you can also go for mixed approaches.

## Rely on API ONLY

> Logic is **HIGHLY DEPENDS** upon the api response.

- **Benefit:** Reducer's logic will be simpler with less code.
- **Drawback:** Increase API usage, that is, increase number of request to API.

### Get

For get request it does not matter because the data has to be fetched from the API.

### Create

1. Record is created successfully.
2. There could be 3 scenarios based on API response.
     1. API sends newly created record in response. Then we can directly add that record in our state.
     2. API sends ID of newly created record. Then we can send request to API to fetch the data of single record by ID rather than whole list and then add it to the state.
     3. API does not send anything. Then we have to refetch whole list.

### Update

1. Record is updated successfully.
2. There could be 2 scenarios based on API response.
     1. API sends updated record in response. Then we can directly update that record in our state.
     2. API does not send anything. But we have ID with us. Then we can send request to API to fetch the data of single record by ID rather than whole list and then update the state.

### Delete

1. Record(s) are deleted successfully.
2. There could be 2 scenarios.
      1. API deos not send back anything. But we already have list of deleted IDs. On complete success we can simply remove entries from state. But if some of them are failed and some of them are succeeded then in this case we can re-fetch whole list to remove failed entries from state.
      2. API sends back ID of deleted records. In this case, handling scenario when some of them are failed and some of them are succeeded can be easy. Because we can have list of succeeded ids that we can use to remove entries from state and let the failed one untouched. We do not need to fetch whole list again.

## Use state as proxy and send API request when required

!!! note "2 places to make decision"
    1. In saga. But it should be avoided unless no other option. Because in this case, we have to handle the `loader` which is set `on` on request. Or we can use initial action say `getPostsInitialize` which dermine if request be sent or skipped.
    2. In component. This is recommended approach. You can make that decision in `useEffect` hook. And avoid creating extra `Initialize` action.

> Steps below are conceptualized in **BROWSER ENVIRONMENT**.

### Get (all/partial)

Tha data has to be fetched from API when action is triggered.

### Get (single record)

Tha data has to be fetched from API when action is triggered. But in success's reducer function you have to handle two scenarios.

1. When record is created. You add new record to the list.
2. When record is updated. You update record already existed in the list.

### Create

1. Either open page by url directly or by clicking on link.
2. Record is created successfully.
3. Based on API response.
    - In component, In success function. If get an id back in response and list has been fetched already, then fetch the created record and add it to the list. Basically, **trigger get single record request action**, which will also handle the logic of adding record to the list.
    - In component, In success function. If does not get an id back in response then re-fetch the whole list. Basically, **trigger get all (or partial-based on pagination) records request action.**

### Update

> You can have refresh button somewhere on header/top which fetches the fresh copy from API.

1. Either open page by url directly or by clicking on link. In `useEffect` check if you are already recieving a record from selector then do not send request otherwise send the request.
2. Record is updated successfully.
3. In component, In success function. You already have an id of updated record. So send a request to fetch updated data from API and update the existed record. Basically, **trigger get single record request action**, which handle the record update operation too.

### Delete

1. Either single or multiple records are deleted successfully.
2. Delete the records from the state's list only. In most cases, you do not have to re-fetch list from API. Deleting from state will do. But for some complex list you may need to re-fetch the list after perfoming delete operation.
    - In case of multiple records, if any request is failed then re-fetch the list from API to keep it synchronised rather than perfoming logic of removing only records that are successfully removed.

## Questions

### On create and update, if API does not send anything back then instead of re-fetching whole data. Can we use the post data of request to update the state?

This can be done if the post data model in request is exactly identical with reponse data model from API when using `GET`. Not only the keys' name but values too. For example, if you get `createdOn` field in response which has value in milliseconds then in request we have to send `createdOn` field in milliseconds. But this is absurd and not feasible. If you go by this then you are just **tightly couple** the API with frontend.

Also, this approach is not feasible in real world applications. Because we transform response/request and the reponse that we get from API can have additional fields that are attached to reponse on backend like `createdOn`, `createdBy` etc. Also, the value send in request is computed on backend and then on reponse you will get different value.

!!! important
    I mean this is stupid, if you try to build your logic like that because you do not want to re-fetch whole list. This appraoch invites all kind unknown scenarios to handle in order to keep data synchronise with backend. Which is more costly than fetching whole list again and process it again. Also, by this way, you will be handling all kind of conditions in your code rather than focusing on implementing business logic correctly.
