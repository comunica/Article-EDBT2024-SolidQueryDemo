## Conclusions
{:#conclusions}

In this article, we discussed the implementation of an open-source SPARQL query engine
that is able to query over DKGs within Solid,
a permissioned decentralization environment.
Our system is based on LTQP techniques
that were specifically designed for exploiting the structural properties of the Solid environment.

To demonstrate this system, we provide a Web-based user interface that can be accessed and used
by anyone with a modern Web browser.
We provide several default SPARQL queries that users can execute across simulated Solid pods,
or users may write their own SPARQL queries for execution against real-world Solid pods.
Besides this Web-based user interface, our system can be used within any TypeScript or JavaScript application,
or via the command-line.

Through this demonstration, we show the effectiveness of traversal-based SPARQL query execution across DKGs.
Many queries start producing results in less than a second,
which is below the [threshold for obstructive delay in human perception](cite:cites uiresponsetime) in interactive applications.
Our system and its demonstration provide groundwork for future research in traversal-based query processing over DKGs.
In future work, we will investigate further optimizations,
which may involve [adaptive query planning techniques](cite:cites adaptive_book) or enhancements to the [link queue](cite:cites linkqueueevolution).
