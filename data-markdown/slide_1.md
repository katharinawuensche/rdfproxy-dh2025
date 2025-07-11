**RDFProxy: A Model-Centric Approach to Transforming SPARQL Result Sets for Linked Data Clients** <!-- .element: class="font-size-125" -->

Lukas Plank & Katharina Wünsche

<span class="font-size-75">
Austrian Centre for Digital Humanities<br> 
Austrian Academy of Sciences
</span>

+++

## Motivation
How to build a REST API on top of a SPARQL endpoint? <!-- .element: class="fragment" data-fragment-index="1" -->

+++

<div class="flex">
<div>
SPARQL results are returned as flat rows

| Author      | Work    | 
| ----------- | ------- |
| Shakespeare | Hamlet  |
| Shakespeare | Othello |
| Shakespeare | King Lear |
| Goethe      | Faust   |
<!-- .element: class="font-size-75 mx-25" -->

</div>
<div class="fragment bl-1">
API consumers usually expect nested JSON

```json
{
   "authors":[
      {
         "name":"Shakespeare",
         "work":[
            "Hamlet",
            "Othello",
            "King Lear"
         ]
      },
      {
         "name":"Goethe",
         "work":[
            "Faust"
         ]
      }
   ]
}
```
</div>

+++

### What pure SPARQL endpoints cannot offer
<ul class="fragmented-list">
<li class="fragment">Grouping of results <br><span class="font-size-75">supporting nested structures (e.g. authors and their work)<span> </li>
<li class="fragment">Entity-aware pagination <br><span class="font-size-75">returning the requested number of <b>entities</b>, not just rows<span> </li>
<li class="fragment">Documentation standards <br><span class="font-size-75">providing an OpenAPI Description of endpoints and schemas<span> </li>
</ul>

+++

<table class="font-size-75">
  <thead>
    <tr>
      <th>Tool</th>
      <th>Grouping</th>
      <th>Pagination</th>
      <th>OpenAPI specification</th>
      <th>Reusable SPARQL queries</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ramose [5]</td>
      <td class="fragment" data-fragment-index="1">Partial</td>
      <td class="fragment" data-fragment-index="1">❌</td>
      <td class="fragment" data-fragment-index="1">❌</td>
      <td class="fragment" data-fragment-index="1">✅</td>
    </tr>
    <tr>
      <td>Basil [4]</td>
      <td class="fragment" data-fragment-index="1">Partial</td>
      <td class="fragment" data-fragment-index="1">❌</td>
      <td class="fragment" data-fragment-index="1">✅</td>
      <td class="fragment" data-fragment-index="1">✅</td>
    </tr>
    <tr>
      <td>Schröder et al. [11]</td>
      <td class="fragment" data-fragment-index="1">✅</td>
      <td class="fragment" data-fragment-index="1">❌</td>
      <td class="fragment" data-fragment-index="1">❌</td>
      <td class="fragment" data-fragment-index="1">❌</td>
    </tr>
    <tr>
      <td>Grlc [7]</td>
      <td class="fragment" data-fragment-index="1">❌</td>
      <td class="fragment" data-fragment-index="1">❌</td>
      <td class="fragment" data-fragment-index="1">✅</td>
      <td class="fragment" data-fragment-index="1">✅</td>
    </tr>
    <tr>
      <td>SPARQL Transformer [6]</td>
      <td class="fragment" data-fragment-index="1">✅</td>
      <td class="fragment" data-fragment-index="1">✅</td>
      <td class="fragment" data-fragment-index="1">❌</td>
      <td class="fragment" data-fragment-index="1">❌</td>
    </tr>
  </tbody>
</table>


+++

<p><strong>Our solution: RDFProxy</strong></p>

<p class="fragment">A Python library for running SPARQL queries <br/>and mapping SPARQL result sets to Pydantic models.</p>
