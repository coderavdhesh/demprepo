# demprepo
just a practice repo for me to be more comfartable 
with github freatures :)

here is nothing spacial or code technical,

you can ignore this repo :)



Example Code for Each File
app/main.py

python
Copy code
from fastapi import FastAPI, HTTPException
from .schemas import Item, ItemCreate
from .crud import create_item, get_items, get_item, update_item, delete_item

app = FastAPI()

@app.post("/items/", response_model=Item)
def create_item_endpoint(item: ItemCreate):
    return create_item(item)

@app.get("/items/", response_model=List[Item])
def read_items_endpoint():
    return get_items()

@app.get("/items/{item_id}", response_model=Item)
def read_item_endpoint(item_id: int):
    item = get_item(item_id)
    if item is None:
        raise HTTPException(status_code=404, detail="Item not found")
    return item

@app.put("/items/{item_id}", response_model=Item)
def update_item_endpoint(item_id: int, item: ItemCreate):
    updated_item = update_item(item_id, item)
    if updated_item is None:
        raise HTTPException(status_code=404, detail="Item not found")
    return updated_item

@app.delete("/items/{item_id}", response_model=Item)
def delete_item_endpoint(item_id: int):
    deleted_item = delete_item(item_id)
    if deleted_item is None:
        raise HTTPException(status_code=404, detail="Item not found")
    return deleted_item
app/models.py

python
Copy code
from typing import Optional

class Item:
    def __init__(self, id: int, name: str, description: Optional[str], price: float, tax: Optional[float]):
        self.id = id
        self.name = name
        self.description = description
        self.price = price
        self.tax = tax
app/crud.py

python
Copy code
from typing import List, Optional
from .models import Item
from .schemas import ItemCreate

items = []

def create_item(item: ItemCreate) -> Item:
    new_item = Item(id=len(items) + 1, **item.dict())
    items.append(new_item)
    return new_item

def get_items() -> List[Item]:
    return items

def get_item(item_id: int) -> Optional[Item]:
    for item in items:
        if item.id == item_id:
            return item
    return None

def update_item(item_id: int, updated_item: ItemCreate) -> Optional[Item]:
    for index, item in enumerate(items):
        if item.id == item_id:
            items[index] = Item(id=item_id, **updated_item.dict())
            return items[index]
    return None

def delete_item(item_id: int) -> Optional[Item]:
    for index, item in enumerate(items):
        if item.id == item_id:
            return items.pop(index)
    return None
app/schemas.py

python
Copy code
from pydantic import BaseModel
from typing import Optional

class ItemBase(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None

class ItemCreate(ItemBase):
    pass

class Item(ItemBase):
    id: int

    class Config:
        orm_mode = True
tests/test_main.py

python
Copy code
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)

def test_create_item():
    response = client.post("/items/", json={"name": "Item 1", "description": "Description for item 1", "price": 10.5, "tax": 1.5})
    assert response.status_code == 200
    assert response.json() == {"id": 1, "name": "Item 1", "description": "Description for item 1", "price": 10.5, "tax": 1.5}

def test_read_items():
    response = client.get("/items/")
    assert response.status_code == 200
    assert len(response.json()) == 1

def test_read_item():
    response = client.get("/items/1")
    assert response.status_code == 200
    assert response.json() == {"id": 1, "name": "Item 1", "description": "Description for item 1", "price": 10.5, "tax": 1.5}

def test_update_item():
    response = client.put("/items/1", json={"name": "Updated Item 1", "description": "Updated description for item 1", "price": 15.0, "tax": 2.0})
    assert response.status_code == 200
    assert response.json() == {"id": 1, "name": "Updated Item 1", "description": "Updated description for item 1", "price": 15.0, "tax": 2.0}

def test_delete_item():
    response = client.delete("/items/1")
    assert response.status_code == 200
    assert response.json() == {"id": 1, "name": "Updated Item 1", "description": "Updated description for item 1", "price": 15.0, "tax": 2.0}
    assert client.get("/items/1").status_code == 404
Running the Application Locally
Command:

bash
Copy code
uvicorn app.main:app --reload
Summary
You now have a structured FastAPI project with the following features:

CRUD Endpoints: Create, Read, Update, Delete operations for items.
In-Memory Storage: Simple storage for demonstration purposes.
Testing: Basic tests for each endpoint.
This setup provides a foundation to build more complex applications with FastAPI.
