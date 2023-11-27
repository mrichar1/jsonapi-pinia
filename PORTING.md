# Porting from jsonapi-vuex to jsonapi-pinia

The following is a quick summary of the main changes between jsonapi-vuex and jsonapi-pinia - please see `README.md` for the full documentation.

## Changes

### Setup

The contents of store.js are simplified to just import and instantiate a `jsonapi-pinia` instance:

```js
import axios from 'axios'
import { jsonapiStore } from 'jsonapi-pinia'

const api = axios.create({
  baseURL: 'https://api.example.com/1/api/',
  headers: {
    'Content-Type': 'application/vnd.api+json',
  },
})

const myStore = jsonapiStore(api)

export { myStore }
```

You then use this in your components by importing and instantiating it:

```js
import { myStore } from '../store'

const store = myStore()
```


### Actions

Actions are no longer called with `dispatch` - instead actions are functions called directly from the store:

```js
// jsonapi-vuex
this.$store.dispatch('jv/get', 'widget').then((data) => {})

// jsonapi-pinia
store.get('widget').then((data) => {})
```

### Mutations

Mutations no longer exist. Store modification occurs through `actions` with the same name as the old `mutations`.

### Getters

`Pinia` has a single namespace for all actions and getters. As a result the getter functions have been renamed from `get` to `getData` and `getRelated` to `getRelatedData` to avoid clashes with the equivalent actions.

Like actions, getters are now called directly from the store:

```js
// jsonapi-vuex
this.$store.getters['jv/get']({ _jv: { type: 'Widget' } })


// jsonapi-pinia
store.getData({ _jv: { type: 'Widget' } })
```
