---
title: "Effective Elasticsearch Mapping"
date: "2015-11-22"
categories: 
  - "computing"
  - "projects"
tags: 
  - "distributed-computing"
  - "elasticsearch"
---

Finally, after more than a year of hiatus, I start writing again on this blog! This time, I'll write about Effective Elasticsearch Mapping, i.e. some tips on how to define our mapping in Elasticsearch 1.7.x. We just started using Elasticsearch 2.0 in LARC (my current workplace :)), so I'll do my best to update this list accordingly as we grow our experience in Elasticsearch 2.0.

# 1\. Use appropriate data type for an ES field

Elasticsearch will try its best to determine the data type of an unknown field when we index a document. However, it’s better to use appropriate data type for ES field, hence define your mapping early (i.e. before you start to index the documents) and use index template (( [https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-templates.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-templates.html))).

Some of the data types that you should use:

1. `date` type for timestamp. Don’t use `long` or `string` type although they may look promising. Using `date` type allow us to use the index easily in Kibana and support time-aggregation without the needs of scanning.
    
2. `geo_point` type for geo location (i.e. latitude-longitude)
    

# 2\. Analyse and store the field only when it’s needed

By default, Elasticsearch will analyse and store all the fields. It’s not harmful when you start using Elasticsearch. But once your indices and data grow, you need to selectively analyse and store the field, so you can effectively use your disk space and you don’t waste resource to store the analysis results for unused fields. Example:

1. We have some indices that store the `raw_html` of a webpage, and so far we never do search query on it but we display this field in our web application. So, we should store but NOT analyse this field.
    
2. Some fields in Tweet’s JSON are not useful for us, such as `retweeted` (([https://dev.twitter.com/overview/api/tweets](https://dev.twitter.com/overview/api/tweets))). We should not analyse and store this field.
    
3. URL field is another example. We want to do exact search using the URL, and it’s not possible when you analyse the field (because the analysis will tokenise the URL hence we can't search using exact URL)
    

# 3\. Use raw field if you need both analysis and aggregation on the field

For example `username` field. Sometimes we want to search the `username` that closes to our input (i.e. I want to do autocomplete search on `username`). In this case, we need analysis on `username` field. On the other hand, we want to do aggregation based on exact `username`, then we need to perform aggregation query on the `raw` field of `username`, i.e. not-analysed field. The final result of the `username` will be like this in the mapping (([https://www.elastic.co/guide/en/elasticsearch/reference/2.0/\_multi\_fields.html](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/_multi_fields.html))):

\[caption id="attachment\_1180" align="aligncenter" width="582"\][![Raw field in Elasticsearch](images/Screenshot-2015-11-22-21.21.11.png)](http://www.otnira.com/wp-content/uploads/2015/11/Screenshot-2015-11-22-21.21.11.png) Raw field in Elasticsearch\[/caption\]

# 4\. Set the number of shards and replicas judiciously

We can’t change the number of shards unless we reindex the data. Estimate how your data will grow and prepare to reindex if needed. Sometimes for small data we only need 1 shard!

We can change the number of replicas when we need to balance the load (([https://www.elastic.co/guide/en/elasticsearch/guide/current/replica-shards.html](https://www.elastic.co/guide/en/elasticsearch/guide/current/replica-shards.html))), increase availability (([https://www.elastic.co/guide/en/elasticsearch/guide/current/replica-shards.html](https://www.elastic.co/guide/en/elasticsearch/guide/current/replica-shards.html))) and scale up (([https://www.elastic.co/guide/en/elasticsearch/guide/current/\_scale\_horizontally.html](https://www.elastic.co/guide/en/elasticsearch/guide/current/_scale_horizontally.html))). It’s a good practice to start with 1 replica and then increase it accordingly.

Qbox suggests that recommended maximum shard size is 30 GB (([https://qbox.io/blog/optimizing-elasticsearch-how-many-shards-per-index](https://qbox.io/blog/optimizing-elasticsearch-how-many-shards-per-index))).

Use time-based index and index template, if possible. We can set smaller shards first , and increase the number of shard in the next index creation when we observe increasing volumes of data.

# 5\. Limit the number of fields in an index

Otherwise, it may kill your cluster (([https://www.elastic.co/blog/found-crash-elasticsearch#mapping-explosion](https://www.elastic.co/blog/found-crash-elasticsearch#mapping-explosion))).

# 6\. Set \`doc\_values\` to true for unanalysed string for ES 1.x

Therefore, we don’t waste Java heap to store unanalysed-string’s `fielddata` (([https://www.elastic.co/guide/en/elasticsearch/guide/current/fielddata-intro.html](https://www.elastic.co/guide/en/elasticsearch/guide/current/fielddata-intro.html))). For ES 2.0, the default value for `doc_values` is true.

# 7\. Prefer \`reindex\` than \`delete-and-recreate\` mapping

Because deleting mapping is much more expensive than deleting index. Quoting from the Delete API documentation (([https://www.elastic.co/guide/en/elasticsearch/reference/1.7/indices-delete-mapping.html](https://www.elastic.co/guide/en/elasticsearch/reference/1.7/indices-delete-mapping.html))):

> Note, most times, it make more sense to reindex the data into a fresh index compared to delete large chunks of it.

When we delete a document, the underlying Lucene will set a delete flag into the document (hence the disk space is not reclaimed immediately). Only after the periodical segment merging, then we can reclaim the disk space.

For ES 2.0, "it is no longer possible to delete the mapping for a type. Instead you should delete the index and recreate it with the new mappings" (([https://www.elastic.co/guide/en/elasticsearch/reference/2.0/indices-delete-mapping.html](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/indices-delete-mapping.html))).

# Wrap up

That's all folks! Although you can easily index a document to Elasticsearch and let Elasticsearch do auto mapping (i.e. determines its field types), it's better to define your mapping upfront.

Thanks to [Philips](https://www.linkedin.com/in/philips) for his initial feedback for this post.

Feel free to write your comments and suggestions :)
