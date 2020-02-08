## microtask-3

Based on the JSON documents produced by Perceval and its source code, try to answer the following questions:

- What is the meaning of the JSON attribute `timestamp`?
- What is the meaning of the JSON attribute `updated_on`?
- What is the meaning of the JSON attribute `origin`?
- What is the meaning of the JSON attribute `category`?
- How many categories do the GitHub and GitLab backends have?
- What is the meaning of the JSON attribute `uuid`?
- What is the meaning of the JSON attribute `search_fields`?
- What is stored in the attribute data of each JSON document produced by Perceval?

---

Based on the JSON documents which are extracted using the Backends of Perceval, we will try to define a few terms and answer the above questions.

#### `timestamp`

The `timestamp` attribute has the information about the datetime when the item was fetched using Perceval.

```python
'timestamp': datetime_utcnow().timestamp(),
```

Example:  `'timestamp': 1581087390.214627,`

- It is a simple function which returns the date and time in UTC format.
    
  [grimoirelab-toolkit/datetime.py#L59](https://github.com/chaoss/grimoirelab-toolkit/blob/562047df21f4bb3b76e14f1a5d70993be4a94013/grimoirelab_toolkit/datetime.py#L59)


#### `updated_on`

The  `updated_on`  attribute is used to know the datetime when the particular item was last updated in the source.

```python
'updated_on': self.metadata_updated_on(item),
```

Example: `'updated_on': 1570609866.0,`

- It extracts the updated time from the data item of the data source. In GitHub, it is extracted from 'updated_at' field. 
This date is converted to UNIX timestamp format. 

  [grimoirelab-perceval/github.py#L186](https://github.com/chaoss/grimoirelab-perceval/blob/805d73122b871c29146a70601d8f3d78267b41e1/perceval/backends/core/github.py#L186)


#### `origin`

The  `origin`  attribute is URL of the repository (in GitHub). Usually, it the URL of the data source.

```python
'origin': self.origin,
```

Example:  `'origin': 'https://github.com/amfoss/gitlit',`

- In the GitHub backend, `origin` is initialized in the file framing a URL using the arguments passed (OWNER, REPOSITORY).

  [grimoirelab-perceval/github.py#L95](https://github.com/chaoss/grimoirelab-perceval/blob/805d73122b871c29146a70601d8f3d78267b41e1/perceval/backends/core/github.py#L95)
  and [grimoirelab-toolkit/uris.py#L28](https://github.com/chaoss/grimoirelab-toolkit/blob/562047df21f4bb3b76e14f1a5d70993be4a94013/grimoirelab_toolkit/uris.py#L28)


#### `category`

The  `category`  attribute is the to know which type of item is to be fetched from the data source.

```python
'category': self.metadata_category(item),
```

Example: `'category': 'issue',`

- In GitHub, it extracts the category from a GitHub item.

  [grimoirelab-perceval/github.py#L206](https://github.com/chaoss/grimoirelab-perceval/blob/805d73122b871c29146a70601d8f3d78267b41e1/perceval/backends/core/github.py#L206)


#### How many categories do the GitHub and GitLab backends have?

- GitHub has three categories, 'issue', 'pull_request', 'repository'. (check microtask-2.ipynb, In [6] cell)
- GitLab has two categories, 'issue', 'merge_request'. (check microtask-2.ipynb, In [17] cell)

  [grimoirelab-perceval/github.py#L87](https://github.com/chaoss/grimoirelab-perceval/blob/805d73122b871c29146a70601d8f3d78267b41e1/perceval/backends/core/github.py#L87)
  and [grimoirelab-perceval/gitlab.py#L86](https://github.com/chaoss/grimoirelab-perceval/blob/805d73122b871c29146a70601d8f3d78267b41e1/perceval/backends/core/gitlab.py#L86)


#### `uuid`

The `uuid`  attribute is a universal unique identifier of every item extracted with the help of Perceval backends.

```python
'uuid': uuid(self.origin, self.metadata_id(item)),
```

Example: `'uuid': '110373d3cf943343cabe2a1cb5bcd77caf01c4f2'`

- In GitHub, it is the SHA1 of the concatenation of the origin and the identifier from an item.

  [grimoirelab-perceval/backend.py#L427](https://github.com/chaoss/grimoirelab-perceval/blob/805d73122b871c29146a70601d8f3d78267b41e1/perceval/backend.py#L427)


#### `search_fields `

The `search_fields` attribute will add a few search fields to an item of the data source.

```python
'search_fields': self.search_fields(item),
```

Example: `'search_fields': {'item_id': '504496022', 'owner': 'amfoss', 'repo': 'gitlit'},`

- In GitHub, it returns a dictionary with the details like id of the item, owner and name of the repository.

  [grimoirelab-perceval/backend.py#L277](https://github.com/chaoss/grimoirelab-perceval/blob/28d4b13fdeddaa43c89fd69a530242b1396208d6/perceval/backend.py#L277)
  and [grimoirelab-perceval/github.py#L144](https://github.com/chaoss/grimoirelab-perceval/blob/28d4b13fdeddaa43c89fd69a530242b1396208d6/perceval/backends/core/github.py#L144)


#### What is stored in the attribute `data` of each JSON document produced by Perceval?

The `data` has the information regarding the the items which are extracted from the data source using the Perceval Backends.

If you are extracting an 'issue' details using GitHub Backend, you could see the below keys in the `data` dictionary.

```
['url', 
'repository_url', 
'labels_url', 
'comments_url', 
'events_url', 
'html_url', 
'id', 
'node_id', 
'number', 
'title', 
'user', 
'labels', 
'state', 
'locked', 
'assignee', 
'assignees', 
'milestone', 
'comments', 
'created_at', 
'updated_at', 
'closed_at', 
'author_association', 
'body', 
'reactions', 
'user_data', 
'assignee_data', 
'assignees_data', 
'comments_data', 
'reactions_data']
```