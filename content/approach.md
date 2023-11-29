## Approach
{:#approach}

Our approach enables query execution across one or more Solid pods without the need to build any prior indexes.
For this, we build on top of the [Link Traversal Query Processing (LTQP)](cite cites linktraversal) paradigm,
whereby the query engine maintains an internal *link queue* which is initialized through a set of seed URLs.
The query engine then iterates over this link queue, by dereferencing each link,
and adding all discovered RDF triples into an internal triple source that can continuously grow.
Furthermore, for each dereferenced link with resulting RDF triples, the engine finds links to other sources,
which are appended to the link queue.
During the processing of the link queue, the actual query processing happens in parallel over the continuously growing internal triple.
This processing starts by building a logical query plan using the [zero-knowledge query planning technique](cite:cites zeroknowldgequeryplanning).
After that, the plan is executed following the [iterator-based pipeline approach](cite:cites linktraversalsparql),
which considers the execution plan as a [pipeline](cite:cites pipelining) of iterator-based physical operators.

Instead of considering all possible links that are discovered in the RDF triples,
we apply various [Solid-specific](cite:cites solidquery) and [Solid-agnostic](cite:cites linktraversalfoundations) link extraction strategies.
For example, Solid pods can expose a [Type Index](cite:cites spec:typeindex),
which contains a list of RDF classes for which instances exist in this pod,
together with links to RDF documents containing such instances.

A visualization of the architecture of our approach can be seen in [](#figure-link-queue).

<figure id="figure-link-queue">
<img src="img/link-queue.svg" alt="Link queue" class="img-narrow">
<figcaption markdown="block">
Link queue, dereferencer, and link extractors feeding triples into a triple source,
producing triples to tuple-producing operators
in a pipelined query execution.
</figcaption>
</figure>
