---
slug: scraping-with-colly
title: Scraping in Go with Colly
date: 2021-10-01
---

I wanted to look back at all my [Codewars](https://www.codewars.com/) solutions, and perhaps create some kind of portfolio frontend for it, so I had a look at the [Codewars API](https://dev.codewars.com/). Unfortunately there was no endpoint to get the actual solution code, so I decided to try scrape it.

I had used BeautifulSoup in Python in the past, but didn't really like it. Maybe because I didn't read the documentation fully or because I didn't understand as much as I do now, but I decided I would try [Colly](http://go-colly.org/), in Go, a self-described 'Fast and Elegant Scraping Framework'.

To my surprise, it was really not too difficult, I was able to find everything I wanted to do easily in the documentation and the examples were very helpful.

I really liked defining a structure for the shape of what I wanted.

```go
type Kata struct {
    Kyu             string   `json:"kyu"`
    KataLink        string   `json:"kataLink"`
    KataTitle       string   `json:"kata"`
    LanguagesSolved []string `json:"languages"`
    Solutions       []string `json:"solutions"`
}
```

It made it real easy to extend the scraper bit by bit, and change the schema without getting confused.

I also really liked the functions a `collector` has, such as `OnHTML()` to match every HTML element by some jquery, rather than a looping over the elements, which is how I would have done it in Python. I also liked the functions to easily get the children elements.

Configuring the scraper was also much easier than with BeautifulSoup. I wanted to set a cookie and in Colly there's a function for it, whereas in BeautifulSoup I would have needed another library.

In the end, I had more issues with the structure of the HTML than scraping itself, but it was so easy I was able to modify the code to scrape a very different website with little code change.

I have built my Codewars solution scraper and you can see the [source code here](https://github.com/yzAlvin/Codewars-Solution-Scraper). I would still like to add more enhancements but that is separate to the scraping part.

I also built a Nestle website scraper for my side project, [source code here](https://github.com/yzAlvin/nestle-proj/tree/master/scraper). Again, I had more issues with the way the website was structured than scraping itself.

I would definitely recommend trying Colly for anyone looking to write any kind of crawler/scraper/spider.
