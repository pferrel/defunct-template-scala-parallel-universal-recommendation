# template-scala-parallel-multimodal-recommendation

This recommender is designed to take account of a wide range of user behavior, item content, and contextual information to make real time recommendations. It contains highly flexible mechanisms for dealing with events that incorporate any important part of the user's  entire click-stream. It can also mix content based recommendations in several ways to augment collaborative filtering and to account for important context.

It is provided as a [PredicitonIO](http://Prediction.IO) template for easy installation and application integration.

This engine is designed to preform the following functions:

 * Collaborative filtering and content based recommendations
 * Serve recommendations in real time from usage data gathered in real time
 * Based on a "cooccurrence" type recommender
 * Creates predictive models from any number of user interaction types&mdash;full click-stream recommendations
 * Uses real time context to affect recommendations
 * Creates predictive models from content and metadata that deliver personalized recommendations
 * Is responsive as new models are recalculated in the background
 * Creates personalized, similar item, and shopping cart type recommendations
 * Can mix usage, content, context, and metadata in one query for extremely flexible recs targeting
 * Based on highly scalable fault-tolerant technology
 * Queries allow biasing or filtering recs by any metadata field&mdash;get recs filtered or slanted towards categories, tags, subject, or any item attribute.

Near future

 * Streaming input through Kafka
 * Log parsing to extract user click-stream data&mdash;often replacing SDK integration for user interaction data
 * Automatically calculates "trending" items to help solve the cold start problem.

##Architecture

We follow the [lambda architectural model](http://en.wikipedia.org/wiki/Lambda_architecture) as embodies in the DASE architecture and tool set. Streaming input comes through the PreditionIO SDKs (or optionally files), which is turned immediately into user history used to make near real time recommendations. The delay in creating this usable user history is minimal--on the order of a seconds. Recommendations are returned in real time. The predictive models are calculated in the background based on all collected data and is updated as often as requested with no server downtime.

The actual components of the architecture are chosen for their speed, reliability, scalability, and high-level of support. They are the at the forefront in their class:

  * **[Spark-Mahout](http://mahout.apache.org/users/recommender/intro-cooccurrence-spark.html):** provides needed linear algebra calculations for creating the predictive models
  * **[Spark](https://spark.apache.org/):** used for streaming input and batch calculations. Keeping the batch and streaming code automatically in sync.
  * **[PrecitionIO](http://prediction.io/):** Used to scaffold the recommender with data ingestion, model calculation infrastructure and a real time recs server.
  * **[Elasticsearch](http://elastic.org/):** Used as the core of the real time query engine.

![image](http://s6.postimg.org/om34mxdyp/recommender_architecture_2015_03_12_09_50_44.png)

##References

 * A slide deck, which talks about mixing actions or other indicators: [A Multimodal Streaming Recommender](http://occamsmachete.com/ml/2014/10/07/creating-a-unified-recommender-with-mahout-and-a-search-engine/)
 * A free ebook, which talks about the general idea: [Practical Machine Learning](https://www.mapr.com/practical-machine-learning)
 * Two blog posts: [What's New in Recommenders: part #1](http://occamsmachete.com/ml/2014/08/11/mahout-on-spark-whats-new-in-recommenders/)
and  [What's New in Recommenders: part #2](http://occamsmachete.com/ml/2014/09/09/mahout-on-spark-whats-new-in-recommenders-part-2/)
 * A post describing the loglikelihood ratio:  [Surprise and Coinsidense](http://tdunning.blogspot.com/2008/03/surprise-and-coincidence.html)  LLR is used to reduce noise in the data while keeping the calculations O(n) complexity.
 * A [Guide to Online Video](https://guide.finderbots.com) site that demonstrates the use of many of the above techniques

