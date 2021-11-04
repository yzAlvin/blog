---
slug: easy-csv-parsing-with-go
title: Transforming csv Files Easily with Go
date: 2021-11-04
---

I wanted to convert a csv file to Markdown. Like my previous post about web scraping, I'd done it before in Python and it was trivial, so I decided to try it with Go.

The thing I liked most was parsing rows into a defined struct.

```go
type Blip struct {
    name        string
    ring        string
    quadrant    string
    isNew       string
    description string
}
```

Apparently in Python 3.7, dataclasses can be used to achieve a similar thing, but I wasn't aware until now.

In Go, there was also much more error handling than Python. I found it to be slightly annoying for my purposes, but perhaps it's actually a blessing in disguise.

My opinion:

* The csv packages in both are really similar
* I really like defining the shape of my rows in Go.
* I prefer Python's string interpolation over Go's string formatting.
* I don't really see a use for the error checking in Go.

This might be the case of me being a noob, not appreciating static typing and robust error handling, but for this use case I reckon Python would have done the same in less lines of code. Perhaps if I was working with more complex data I would prefer using Go.
