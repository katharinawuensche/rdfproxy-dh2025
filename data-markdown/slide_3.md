**RDFProxy Overview and Features**

<ul style="font-size: 0.7em;">
  <li class="fragment">maps SPARQL <em>bindings</em> to Pydantic model <em>fields</em> (with support for aliasing)</li>
  <li class="fragment">allows arbitrarily nested Pydantic models</li>
  <li class="fragment">features model-controlled grouping and aggregation over SPARQL results</li>
  <li class="fragment">implements purely SPARQL-based pagination</li>
</ul>

+++


**RDFProxy Example 1: Scalar Aggregation**


```sparql
select ?name ?work
where {
  values (?name ?work) {
	("Goethe" "Faust")
	("Shakespeare" "Hamlet")
	("Shakespeare" "Othello")
	("Shakespeare" "King Lear")
  }
}
```

+++

```python
class Author(BaseModel):
	model_config = ConfigDict(group_by="name")

	name: str
	works: Annotated[list[str], SPARQLBinding("work")]


```
```python
adapter = SPARQLModelAdapter(
	target="https://query.wikidata.org/bigdata/namespace/wdq/sparql",
	query=query,
	model=Author,
)

app = FastAPI()

@app.get("/")
def route(
	query_parameters: Annotated[QueryParameters[Author], Query()],
) -> Page[Author]:
	return adapter.get_page(query_parameters)
```

+++

JSON Output

```json
{
  "items":[
	{
	  "name":"Goethe",
	  "works":[
		"Faust"
	  ]
	},
	{
	  "name":"Shakespeare",
	  "works":[
		"Hamlet",
		"Othello",
		"King Lear"
	  ]
	}
  ],
  "page":1,
  "size":100,
  "total":2,
  "pages":1
}
```

+++


**RDFProxy Example 2: Model Aggregation**


```python
class Work(BaseModel):
	name: Annotated[str, SPARQLBinding("work")]

class Author(BaseModel):
	model_config = ConfigDict(group_by="name")

	name: str
	works: list[Work]
```


+++

JSON Output

```json
{
  "items":[
	{
	  "name":"Goethe",
	  "works":[
		{
		  "name":"Faust"
		}
	  ]
	},
	{
	  "name":"Shakespeare",
	  "works":[
		{
		  "name":"Hamlet"
		},
		{
		  "name":"Othello"
		},
		{
		  "name":"King Lear"
		}
	  ]
	}
  ],
  "page":1,
  "size":100,
  "total":2,
  "pages":1
}
```
