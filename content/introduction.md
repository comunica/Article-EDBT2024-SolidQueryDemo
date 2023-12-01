## Introduction
{:#introduction}

Most research on the optimization and execution of [SPARQL queries](cite:cites spec:sparqllang) over [Knowledge Graphs](cite:cites knowledgegraphs)
focuses on centralized use cases,
in which queries are to be executed over a single dataset, in which queries can be optimized using traditional [*optimize-then-execute* techniques](cite:cites sparqlqueryoptimization, sparqlbgpoptimization).

Due to issues in recent years involving personal data exploitation on the Web,
there has been increasing interest in the decentralization of such personal data.
This has materialized into various decentralization initiatives,
such as [Solid](cite:cites solid), [Bluesky](cite:cites bluesky), and [Mastodon](cite:cites mastodon).
What is common among these initiatives, is that they distribute personal and permissioned data across a large number of authoritative Web sources,
which leads to the emergence of Decentralized Knowledge Graphs (DKGs).
For this work, we limit our scope to RDF-based initiatives such as Solid,
as the use of IRIs in RDF enable convenient integration of data across multiple sources.
Solid enables users to host and control *personal data pods*, which are data sources into which hierarchies of RDF documents can be stored.

In order to *find* data across such massive distributions of data across the Web,
there is a need for efficient query processing techniques.
While techniques have been introduced that enable the execution of [SPARQL federated queries](cite:cites fedx, hibiscus, splendid),
they are [optimized for handling a *small* number (~10) of *large* sources](cite:cites fedshop),
whereas DKGs such as Solid are characterized
by a *large* number (>1000) of
<span class="placeholder printonly">
<span style="display: block; height: 9em;"></span>
<!-- This is a dummy placeholder for the ACM first page footnote -->
</span>
*small* sources.
Additionally, federated SPARQL query processing assumes sources to be known prior to query execution,
which is not feasible in DKGs due to the lack of a central index.
Hence, these techniques are ill-suited for the envisaged scale of distribution in DKGs.

To cope with these problems, alternative techniques have been introduced recently.
[ESPRESSO](cite:cites espresso) was introduced as an approach that builds distributed indexes for Solid pods
which can be accumulated in a single location.
This accumulated index can then be queried using keyword search to find relevant pods to a query,
after which these relevant pods can be queried using federated SPARQL processing techniques.
This approach depends on placing trust over personal data in this single accumulated indexer.
[POD-QUERY](cite:cites podquery) is another approach that involves placing a SPARQL query engine agent in front of a Solid pod,
which enables full SPARQL queries to be executed over single Solid pods. This approach does not consider query execution across multiple pods.
In [recent work](cite:cites solidquery), we proposed making use of [Link Traversal Query Processing (LTQP)](cite cites linktraversal)
for querying across one or more Solid pods.
LTQP is derived from the idea of [*SQL-based query execution over the Web*](cite:cites queryingwwwsql, infogatheringwwww3ql)
and the concept of [*focused crawling*](cite:cites focusedcrawling, focusedcrawlingimproving).
LTQP starts query execution given a set of seed sources,
from which links are recursively followed between RDF documents to discover additional data to consider during query execution,
In this previous work, we introduced various LTQP techniques that understand the structural properties of Solid pods,
and use this to optimize LTQP in terms of the number of links that need to be followed.
Such a traversal-based approach does not rely on prior indexes over Solid pods,
and can query over live data that is spread over multiple pods.

The focus of this article is on demonstrating the implementation of a query engine
that can execute SPARQL queries across Solid pods using the LTQP techniques introduced in [](cite:cites solidquery).
This query engine is open-source, and is implemented in a modular way so it can serve future research in this domain.
We demonstrate this query engine through a Web-based user interface,
in which preconfigured or manually created SPARQL queries can be executed across both simulated or real-world Solid pods.
In the [next section](#approach), we explain our approach, after which we discuss our implementation in [](#implementation).
In [](#demonstration), we describe our Web-based demonstration interface,
after which we conclude in [](#conclusions).
