**Pydantic Crash Course**

<div class="fragment">Pydantic is a data modeling and validation library based on Python's type annotation system</div>

<br/>
<ul style="font-size: 0.7em;">
  <li class="fragment">Standard Python classes and class-level attributes as modelling language</li>
  <li class="fragment">Handles parsing, validation and serialization of data</li>
  <li class="fragment">Features a Rust core for high-performance operations over models</li>
</ul>


+++

```python
from pydantic import BaseModel, Field, model_validator

class Point(BaseModel):
	x: int
	y: int

class ConstrainedPoint(BaseModel):
	x: int
	y: int = Field(ge=0)

	@model_validator(mode="after")
	def _check_not_origin(self):
		if self.x == 0 and self.y == 0:
			raise ValueError("Point at origin (0, 0) is not allowed.")
		return self
```

```python
p1 = Point(x=1, y=2)    # Point(x=1, y=2)
p2 = Point(x=1, y=2.1)  # ValidationError: Input should be a valid integer...
```

```python
p3 = ConstrainedPoint(x=1, y=0)   # ConstrainedPoint(x=1, y=0)
p4 = ConstrainedPoint(x=1, y=-1)  # ValidationError: Input should be ge=0
p5 = ConstrainedPoint(x=0, y=0)   # ValidationError: Point at origin...
```
