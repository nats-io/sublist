# Sublist History

This repo is simply a collection of data structures created during the first port of nats-server to Go, saved here for posterity.

The sublist is a structure used in the nats-server to match subjects from published messages to a result set of subscribers. When originally porting the server from Ruby to Go, Go was at 0.56.
Performance for Go at that point was not great, so I had to create many structures to make the sublist fast. The sublist is essentially a patricia trie that uses hashmaps. I created a collection of
hashing algorithms and my own hashmap with random eviction.

If you want to learn more about the early optimizations for the nats-server, you can find a helpful presentation from the first GopherCon: [High Performance Systems in Go](http://www.slideshare.net/derekcollison/gophercon-2014).

Even as of Go1, this implementation still had some advantages.

```sh
====================
Go version go1.0.3
====================

Benchmark_GoMap___GetSmallKey   50000000                52.20 ns/op       19.17 mops/s
Benchmark_HashMap_GetSmallKey   100000000               15.50 ns/op       64.34 mops/s
Benchmark_GoMap____GetMedKey    50000000                61.60 ns/op       16.24 mops/s
Benchmark_HashMap__GetMedKey    200000000                8.91 ns/op      112.20 mops/s
Benchmark_GoMap____GetLrgKey    20000000                86.90 ns/op       11.51 mops/s
Benchmark_HashMap__GetLrgKey    100000000               25.40 ns/op       39.44 mops/s
```

Once Go hit 1.2, many optimizations came into play, especially around the built-in maps.
These optimizations no longer were necessary.

```sh
====================
Go version go1.2.1
====================

Benchmark_GoMap___GetSmallKey   500000000                7.57 ns/op      132.05 mops/s
Benchmark_HashMap_GetSmallKey   100000000               14.30 ns/op       70.08 mops/s
Benchmark_GoMap____GetMedKey    500000000                4.83 ns/op      207.01 mops/s
Benchmark_HashMap__GetMedKey    200000000                9.54 ns/op      104.82 mops/s
Benchmark_GoMap____GetLrgKey    500000000                4.39 ns/op      227.79 mops/s
Benchmark_HashMap__GetLrgKey    100000000               24.50 ns/op       40.77 mops/s
```

You can see more results here: [results.txt](https://github.com/nats-io/sublist/blob/master/hashmap/results.txt)

## License

(The MIT License)

Copyright (c) 2012-2016 Apcera Inc.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to
deal in the Software without restriction, including without limitation the
rights to use, copy, modify, merge, publish, distribute, sublicense, and/or
sell copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
IN THE SOFTWARE.

[License-Url]: http://opensource.org/licenses/MIT
[License-Image]: https://img.shields.io/npm/l/express.svg
[Build-Status-Url]: http://travis-ci.org/nats-io/gnatsd
[Build-Status-Image]: https://travis-ci.org/nats-io/gnatsd.svg?branch=master
[Release-Url]: https://github.com/nats-io/gnatsd/releases/tag/v0.7.2
[Release-image]: http://img.shields.io/badge/release-v0.7.2-1eb0fc.svg
[Coverage-Url]: https://coveralls.io/r/nats-io/gnatsd?branch=master
[Coverage-image]: https://img.shields.io/coveralls/nats-io/gnatsd.svg
[ReportCard-Url]: http://goreportcard.com/report/nats-io/gnatsd
[ReportCard-Image]: http://goreportcard.com/badge/nats-io/gnatsd
[github-release]: https://github.com/nats-io/gnatsd/releases/
