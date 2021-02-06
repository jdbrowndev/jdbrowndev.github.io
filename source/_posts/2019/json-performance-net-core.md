---
title: System.Text.Json Performance in .NET Core 3
date: 2019-10-12 17:24:08
tags:
 - .NET
 - C#
---

### Overview

Microsoft released .NET Core 3 last month. One feature I’ve been eager to try is the new System.Text.Json API, a minimal, built-in replacement for Json.NET.

For many years, Json.NET has been the go-to library for serializing and deserializing JSON in the .NET ecosystem. Json.NET works well but requires a third-party dependency and fails to take advantage of performance enhancements recently made available in .NET Core.

System.Text.Json corrects both these issues. It’s available by default in all .NET Core projects, and it prioritizes high performance with a low memory footprint. System.Text.Json uses Span<T\> and ReadOnlySequence<T\>, constructs that were introduced into .NET Core last year. It also supports UTF-8 natively so that JSON does not have to be transcoded into UTF-16, the format used by .NET’s string class.

I wrote a short console app to compare Json.NET and System.Text.Json performance on my laptop. The console app has two inputs--file path and number of iterations. File path is a path to a JSON file. Number of iterations is the iterations count where an iteration is a single deserialize-serialize run on the JSON file. The console app then outputs min, max, and average runtimes for both JSON libraries.

Below are some sample runs on 3 datasets from Kaggle.com. Run times are given in milliseconds (ms).

### Laptop Specs

Ubuntu 18.04 LTS
Intel Core i5-3230M CPU @ 2.60GHz × 4
8GB RAM / 256 GB SSD

### Deserialize

![](/images/json-performance-2019-deserialize.png)

### Serialize

![](/images/json-performance-2019-serialize.png)

For deserialize, System.Text.Json performs 4-5x faster. For serialize, System.Text.Json performs only 1.25x faster at most, with the Food.com recipes dataset actually serializing slower. Further testing could be done with datasets having different structure (e.g. record count, nesting depth, size) to determine where System.Text.Json performs its best.

The console app source code and Kaggle datasets are linked below. Give it a try yourself!

### Resources

[Console App Source Code](https://github.com/jdbrowndev/dotnet-core-json-test)

[Food.com Recipes](https://www.kaggle.com/shuyangli94/food-com-recipes-and-user-interactions)

[UFC Fight Data](https://www.kaggle.com/rajeevw/ufcdata)

[Wine Reviews](https://www.kaggle.com/zynicide/wine-reviews)

### Further Reading

[.NET Core 3 Release Notes](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-0/)

[System.Text.Json Overview](https://devblogs.microsoft.com/dotnet/try-the-new-system-text-json-apis/)

[Scott Hanselman on System.Text.Json](https://www.hanselman.com/blog/SystemTextJsonAndNewBuiltinJSONSupportInNETCore.aspx)