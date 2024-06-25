# demprepo
just a practice repo for me to be more comfartable 
with github freatures :)

here is nothing spacial or code technical,

you can ignore this repo :)

Create Item: POST /items/

Request Body (JSON):
json
Copy code
{
    "name": "Item 1",
    "description": "Description for item 1",
    "price": 10.5,
    "tax": 1.5
}
Read All Items: GET /items/

Read Single Item: GET /items/{item_id}

Replace {item_id} with the ID of the item you want to retrieve.
Update Item: PUT /items/{item_id}

Replace {item_id} with the ID of the item you want to update.
Request Body (JSON):
json
Copy code
{
    "name": "Updated Item 1",
    "description": "Updated description for item 1",
    "price": 15.0,
    "tax": 2.0
}
Delete Item: DELETE /items/{item_id}

Replace {item_id} with the ID of the item you want to delete.
