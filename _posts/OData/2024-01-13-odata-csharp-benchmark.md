---
layout: post
title: "Benchmarking OData Clients in Dotnet 8"
date: 2024-01-13 04:40:00 -0100
categories: coding odata dotnet d365
tags: odata c# dotnet8 simple.odata.client httpclient d365
image:
  path: /assets/img/headers/odata-benchmark.webp
---

To objectively compare the performance of these OData client libraries, we conducted a benchmarking exercise involving three scenarios:

- <strong>Simple Add Request:</strong> Involving a single entity post-operation.

- <strong>Simple Delete Request:</strong> Involving a single entity delete operation.

- <strong>Simple GET By Id Request:</strong> Involving a single entity read operation by id.

- <strong>Simple GET All Request:</strong> Involving an all entities read operation.

## Device Information

OS Windows 10

Intel i7-9750H, CPU 2.60 GHz, 1CPU, 12 logical and 6 physical core

.Net 8.0.101 SDK

## Benchmark Results

| Operation                  | OData Client                                   | Simple.OData.Client | Custom Http Client                              |
| -------------------------- | ---------------------------------------------- | ------------------- | ----------------------------------------------- |
| <strong>Add</strong>       | <strong style="color:green;">172.9 ms</strong> | 173.5 ms            | 175.2 ms                                        |
| <strong>Delete</strong>    | 517.4 ms                                       | 172 ms              | <strong style="color:green;">171.7 ms </strong> |
| <strong>Fetch</strong>     | 176.2 ms                                       | 178.6 ms            | <strong style="color:green;">175.4 ms </strong> |
| <strong>Fetch One</strong> | <strong style="color:green;">169.3 m</strong>  | 173.6 ms            | 170.2 ms                                        |

The results indicated that ODataClient exhibited a slight performance advantage over Simple OData Client, while custom HttpClient consistently outperformed both libraries. This difference in performance is attributed to the optimization capabilities of custom HttpClient, which allow for fine-tuning of HTTP requests and responses.

## Simple OData Client Benchmark

![simple-odata-benchmark](/assets/img/posts/simpleodataclient.webp "Simple.OData.Client Benchmark")

## OData Client Benchmark

![odata-client-benchmark](/assets/img/posts/odataclient.webp "OData Client Benchmark")

## Http Client Benchmark

![httpclient-benchmark](/assets/img/posts/httpclient.webp "Custom Http Client Benchmark")

## Conclusion

The choice between ODataClient, custom HttpClient, and Simple OData Client hinges on the specific requirements of the project. For straightforward OData interactions and a focus on data manipulation, ODataClient provides a balanced combination of ease of use and performance. In cases demanding granular control over HTTP communication and performance optimization, custom HttpClient emerges as the preferred choice. For beginners or those prioritizing simplicity, Simple OData Client offers a lightweight and approachable entry point into the world of OData development.

## Other

- The benchmarking code is available on [GitHub](https://github.com/manishtiwari25/bites-in-byte-blog/tree/main/src/ODataBenchmark)
- Tools related to OData EDMX or Metadata are available [here](http://edmx.bitesinbyte.com/)
- Other Related [blogs](/categories/odata/)
