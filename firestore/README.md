# React Firebase Hooks - Cloud Firestore

React Firebase Hooks provides convenience listeners for Collections and Documents stored with
Cloud Firestore. The hooks wrap around the `firebase.firestore.collection().onSnapshot()`
and `firebase.firestore().doc().onSnapshot()` methods.

In addition to returning the snapshot value, the hooks provide an `error` and `loading` property
to give a complete lifecycle for loading and listening to Cloud Firestore.

There are 2 variants of each hook:

- `useX` which subscribes to the underlying Collection or Document and listens for changes
- `useXOnce` which reads the current value of the Collection or Document

All hooks can be imported from `react-firebase-hooks/firestore`, e.g.

```
import { useCollection } from 'react-firebase-hooks/firestore';
```

List of Cloud Firestore hooks:

- [useCollection](#usecollection)
- [useCollectionOnce](#usecollectiononce)
- [useCollectionData](#usecollectiondata)
- [useCollectionDataOnce](#usecollectiondataonce)
- [useDocument](#usedocument)
- [useDocumentOnce](#usedocumentonce)
- [useDocumentData](#usedocumentdata)
- [useDocumentDataOnce](#usedocumentdataonce)

### useCollection

```
const [snapshot, loading, error] = useCollection(query, snapshotListenOptions);
```

Returns a `firebase.firestore.QuerySnapshot` (if a query is specified), a `boolean` to indicate if the data is still being loaded and any `firebase.FirebaseError` returned by Firebase when trying to load the data.

The `useCollection` hook takes the following parameters:

- `query`: (optional) `firebase.firestore.Query` for the data you would like to load
- `snapshotListenOptions`: (optional) `firebase.firestore.SnapshotListenOptions` to customise how the query is loaded

#### Full example

```js
import { useCollection } from 'react-firebase-hooks/firestore';

const FirestoreCollection = () => {
  const [value, loading, error] = useCollection(
    firebase.firestore().collection('hooks'),
    {
      includeMetadataChanges: true,
    }
  );
  return (
    <div>
      <p>
        {error && <strong>Error: {error}</strong>}
        {loading && <span>Collection: Loading...</span>}
        {value && (
          <span>
            Collection:{' '}
            {value.docs.map(doc => (
              <React.Fragment key={doc.id}>
                {JSON.stringify(doc.data())},{' '}
              </React.Fragment>
            ))}
          </span>
        )}
      </p>
    </div>
  );
};
```

### useCollectionOnce

```
const [snapshot, loading, error] = useCollectionOnce(query, getOptions);
```

As `useCollection`, but this hook will only read the current value of the `firebase.firestore.Query`.

Returns a `firebase.firestore.QuerySnapshot` (if a query is specified), a `boolean` to indicate if the data is still being loaded and any `firebase.FirebaseError` returned by Firebase when trying to load the data.

The `useCollectionOnce` hook takes the following parameters:

- `query`: (optional) `firebase.firestore.Query` for the data you would like to load
- `getOptions`: (optional) `firebase.firestore.GetOptions` to customise how the collection is loaded

### useCollectionData

```
const [values, loading, error] = useCollectionData<T>(query, options);
```

As `useCollection`, but this hook returns a typed list of the `firebase.firestore.QuerySnapshot.docs` values rather than the
`QuerySnapshot` itself.

The `useCollectionData` hook takes the following parameters:

- `query`: (optional) `firebase.firestore.Query` for the data you would like to load
- `options`: (optional) `Object` with the following parameters:
  - `idField`: (optional) name of the field that should be populated with the `firebase.firestore.QuerySnapshot.id` property.
  - `snapshotListenOptions`: (optional) `firebase.firestore.SnapshotListenOptions` to customise how the collection is loaded

### useCollectionDataOnce

```
const [value, loading, error] = useCollectionDataOnce<T>(query, options);
```

As `useCollectionData`, but this hook will only read the current value of the `firebase.firestore.Query`.

Returns a typed list of the `firebase.firestore.QuerySnapshot.docs` values rather than the
`QuerySnapshot` itself.

The `useCollectionDataOnce` hook takes the following parameters:

- `query`: (optional) `firebase.firestore.Query` for the data you would like to load
- `options`: (optional) `Object` with the following parameters:
  - `getOptions`: (optional) `firebase.firestore.GetOptions` to customise how the collection is loaded
  - `idField`: (optional) name of the field that should be populated with the `firebase.firestore.QuerySnapshot.id` property.

### useDocument

```
const [snapshot, loading, error] = useDocument(reference, snapshotListenOptions);
```

Returns a `firebase.firestore.DocumentSnapshot` (if a document is specified), a `boolean` to indicate if the data is still being loaded and any `firebase.FirebaseError` returned by Firebase when trying to load the data.

The `useDocument` hook takes the following parameters:

- `reference`: (optional) `firebase.firestore.DocumentReference` for the data you would like to load
- `snapshotListenOptions`: (optional) `firebase.firestore.SnapshotListenOptions` to customise how the document is loaded

#### Full example

```js
import { useDocument } from 'react-firebase-hooks/firestore';

const FirestoreDocument = () => {
  const [value, loading, error] = useDocument(
    firebase.firestore().doc('hooks/nBShXiRGFAhuiPfBaGpt'),
    {
      includeMetadataChanges: true,
    }
  );
  return (
    <div>
      <p>
        {error && <strong>Error: {error}</strong>}
        {loading && <span>Document: Loading...</span>}
        {value && <span>Document: {JSON.stringify(value.data())}</span>}
      </p>
    </div>
  );
};
```

### useDocumentOnce

```
const [snapshot, loading, error] = useDocumentOnce(reference, getOptions);
```

As `useDocument`, but this hook will only read the current value of the `firebase.firestore.DocumentReference`.

Returns a `firebase.firestore.DocumentSnapshot` (if a reference is specified), a `boolean` to indicate if the data is still being loaded and any `firebase.FirebaseError` returned by Firebase when trying to load the data.

The `useDocumentOnce` hook takes the following parameters:

- `reference`: (optional) `firebase.firestore.DocumentReference` for the data you would like to load
- `getOptions`: (optional) `firebase.firestore.GetOptions` to customise how the document is loaded

### useDocumentData

```
const [values, loading, error] = useDocumentData<T>(reference, options);
```

As `useDocument`, but this hook returns the typed contents of `firebase.firestore.DocumentSnapshot.val()` values rather than the
`DocumentSnapshot` itself.

The `useDocumentData` hook takes the following parameters:

- `reference`: (optional) `firebase.firestore.DocumentReference` for the data you would like to load
- `options`: (optional) `Object` with the following parameters:
  - `idField`: (optional) name of the field that should be populated with the `firebase.firestore.DocumentSnapshot.id` property.
  - `snapshotListenOptions`: (optional) `firebase.firestore.SnapshotListenOptions` to customise how the document is loaded

### useDocumentDataOnce

```
const [value, loading, error] = useDocumentDataOnce<T>(reference, options);
```

As `useDocumentData`, but this hook will only read the current value of the `firebase.firestore.DocumentReference`.

Returns the typed contents of `firebase.firestore.DocumentSnapshot.data()` rather than the `DocumentSnapshot` itself.

The `useDocumentDataOnce` hook takes the following parameters:

- `reference`: (optional) `firebase.firestore.DocumentReference` for the data you would like to load
- `options`: (optional) `Object` with the following parameters:
  - `getOptions`: (optional) `firebase.firestore.GetOptions` to customise how the document is loaded
  - `idField`: (optional) name of the field that should be populated with the `firebase.firestore.DocumentSnapshot.id` property.