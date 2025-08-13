## Prometheus
- Install prometheus : brew install prometheus, brew install node_exporter
- Start prometheus: brew services start prometheus, brew services start node_exporter
- Prometheus web url : http://localhost:9090, Node exporter- http:localhost:9100,  Grafana- http://localhost:3000
- **Node exporter**:
   - The Node Exporter is an agent that gathers system metrics and exposes them in a format which can be ingested by Prometheus.
   - **Data Model**:
     - Stores data as time series.
     - Time-series data represents information that changes over time and is sampled at specific points in time (depending on when exactly Prometheus will be pulling metrics from its targets). 
     - Time series has Metric name and Labels.
     - Each label is a Key/Value pair.
     - <metric name>{Key=value, key=value,..}
  - **Data types**:
    - ***Scalar*** - Float or String
       - Store => prometheus_http_request_total{code=200, job=prometheus} (Both values are stored as String)
       - Query - prometheus_http_request_total{code=~"2.*", job="prometheus"} => This will work as both code and job are String.
       - Query - prometheus_http_request_total{code=200, job="prometheus"} => this will not work as code is not Float but String.
    - **Instant Vectors**: (https://medium.com/@ahmed.s.farag96/decoding-promql-unraveling-range-vectors-and-instant-vectors-in-prometheus-c1390f650e5c)
       - From its name, it basically gives you an instant in time for the metrics values (the last scrapped value for each metric to be specific, as shown in the diagram below)
    - **Range Vectors**: https://www.metricfire.com/blog/understanding-the-prometheus-rate-function/
       -  It gives us metrics values within the specified time range.
     
### Artithmetic operators(+, -, /, *, %, ^)
  - **Scalar + instant vector** - Applies to every value of an instant vector. prometheus_sd_updates_total{name=~"^[a-z]*$"} + 10
  - **instant + instant vector**- Applies to every value of left vector and its matching value in right vector(intersection)

### Binary Comparison operators(==, !=, >, <, >=, <=)
  - **Scalar + instant vector** - Returns only the vector rows which passes conparison with scalar prometheus_sd_updates_total{name=~"^[a-z]*$"}  ==  10
  - **instant + instant vector**- Applies to only rows which exists in both instant vector

### Set Binary operator : (And, Or, unless)
   - Can be applied to instant vectors only.
   - and - Return rows fom the two instant vectors such that the metric name, labels and value is matching
   - or - Returns union
   - unless - Returns rows which doesnt exist in second vector.

### Label Matchers and Selectors :  
   -  =: Select labels that are exactly equal to the provided string.
   - !=: Select labels that are not equal to the provided string.
   - =~: Select labels that regex-match the provided string.
   - !~: Select labels that do not regex-match the provided string.
     
## Aggregation operator:
- Aggregates the elemetnts of a single instant vector.
- Syntax: Agg_Operator(instant_vector)
- Syntax: Agg_Operator(instant_vector) by (label1, label2, ....)
- Syntax: Agg_Operator(instant_vector) without (label1, label2, ....)
- Agg operator for instant vector : sum, min, max, avg, count_values, topk, bottomk, stddev, stdvar
- Agg operator for range vector: sum_over_time, min_over_time, max_over_time, avg_over_time, topk_over_time, bottomk_over_time, stddev_over_time, stddev_over_time
- 

## Time Offsets:
- instant_vector offset time

