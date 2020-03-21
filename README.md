thist - a go package for calculating online histograms with plotting to the terminal and images
===============================================================================================
[![Documentation](https://godoc.org/github.com/bsipos/thist?status.svg)](http://godoc.org/github.com/bsipos/thist)
[![Go Report Card](https://goreportcard.com/badge/github.com/bsipos/thist)](https://goreportcard.com/report/github.com/bsipos/thist)
[![Join the chat at https://gitter.im/bsiposkj/thist](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/bsiposkj/thist?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Check out the `watch` subcommands from the [csvtk](https://github.com/shenwei356/csvtk) and [seqkit](https://github.com/shenwei356/seqkit) tools to see the code in action.

Example
-------

```go
package main

import (
        "fmt"
        "github.com/bsipos/thist"
        "math/rand"
        "time"
)

// randStream return a channel filled with endless normal random values
func randStream() chan float64 {
        c := make(chan float64)
        go func() {
                for {
                        c <- rand.NormFloat64()
                }
        }()
        return c
}

func main() {
        // create new histogram
        h := thist.NewHist(nil, "Example histogram", "auto", -1, true)
        c := randStream()

        i := 0
        for {
                // add data point to hsitogram
                h.Update(<-c)
                if i%50 == 0 {
                        // draw histogram
                        fmt.Println(h.Draw())
                        time.Sleep(time.Second)
                }
                i++
        }
}

```

[![demo video](http://img.youtube.com/vi/7mrs1QGDyys/0.jpg)](http://www.youtube.com/watch?v=7mrs1QGDyys)

TODO
----

- Add more details on online histogram generation.
- Add separate object for online estimation of moments.
- Maybe add tcell as a back-end?
