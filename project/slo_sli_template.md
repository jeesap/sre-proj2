# API Service

| Category     | SLI                                                               | SLO                                                                                                         |
|--------------|-------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| Availability |  Total number of successful requests / Total number of requests   | 99%                                                                                                         |
| Latency      |  90% latency for requests over 5 min                              | 90% of requests below 100ms                                                                                 |
| Error Budget |  No. of error requests/Total no. of requests                      | Error budget is defined at 20%. This means that 20% of the requests can fail and still be within the budget |
| Throughput   |  Total number of successful requests over 5 minutes               | 5 RPS indicates the application is functioning                                                              |
