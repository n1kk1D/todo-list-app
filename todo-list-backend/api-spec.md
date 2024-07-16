# To-do list backend

This backend will provide for all of your to-do item management needs!

## Data Types
### `TodoItem`
`TodoItem` represents a single to-do list item. It contains the following fields:
- id (number): Unique identifier for the to-do, used to access it on the backend
- description (string): The to-do's description
- isDone (boolean): True means checked, false means unchecked

## Endpoints

### `GET /todo-item`
Returns a list of all todos!

#### Response body:
`TodoItem[]`, a list of all the to-do items stored in the backend

### `POST /todo-item`
Create a new to-do!

#### Request body:
```ts
{
    description: string;
}
```
Provide the description of the to-do in the `description` parameter.

#### Response body:
`TodoItem`, the to-do item you have created.

### `POST /todo-item/<todoId>/check`
Check an existing to-do.

#### URL path parameters:
- `todoId` (number), the ID of the to-do you would like to check

### Response body:
`TodoItem`, the to-do you have updated in its new state.

### `POST /todo-item/<todoId>/uncheck`
Uncheck an existing to-do.

#### URL path parameters:
- `todoId` (number), the ID of the to-do you would like to uncheck

### Response body:
`TodoItem`, the to-do you have updated in its new state.

### `PATCH /todo-item/<todoId>/uncheck`
Update a to-do's description!

#### URL path parameters:
- `todoId` (number), the ID of the to-do you would like to update

#### Request body:
```ts
{
    description: string;
}
```
The only thing you are permitted to provide is an updated `description` for the to-do. To update the check status, use the `/check` and `/uncheck` endpoints.

#### Response body:
`TodoItem`, the to-do you have updated in its new state.

### `DELETE /todo-item/<todoId>`
Delete a to-do item.

#### URL path parameters:
- `todoId` (number), the ID of the to-do you would like to check

#### Response body:
`TodoItem`, the to-do you have deleted, with all the data it had before being deleted.
