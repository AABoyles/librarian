# librarian

*Because missing packages shouldn't trip up scripts*

How often have you been running a little R script, minding your own business, when out of nowhere...

> Error in library(somePackage) : there is no package called ‘somePackage’

And the whole process stops. You know it's not a huge deal, but it feels like this:

<iframe src="https://giphy.com/embed/1ppRSO8YwlOifaVH9I" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/wyeoak-music-video-merge-records-1ppRSO8YwlOifaVH9I">via GIPHY</a></p>

This isn't a complete solution for that problem, but it's a fist step.

## Installation

```R
devtools::install_github('aaboyles/librarian')
library('librarian')
```

## Usage

```R
load_package('somePackage')
```

## Wait, why?

Loading dependencies in R is terrible. Here's why:

### `library` isn't context-revealing

In R, a chunk of code you can bring into your session is called a *library*. Oh crap, wait, actually it's not. It's called a *package*. You use the `library` function to load a package. This is because \*mumble mumble\* reasons.

### `require` is usually poor form

Since it feels like a misnomer to use `library` to load your R *packages*, what about `require` instead? `require` appeals to intuition, since it doesn't name the thing you're requiring, but it's a perfectly logical choice in any language for bringing in new modular chunks.

Unfortunately, R's `require` is terrible for a much more important reason: [it doesn't stop the script if it fails](https://yihui.name/en/2014/07/library-vs-require/). This is a huge pain in the neck if you require at the top of a script and the thing that didn't get loaded is much later.

### Neither tries very hard

Neither library nor require makes any attempt to rectify a missing package. So, to solve this, lots of R hackers have written some variation of this:

```R
if(!require(somePackage)){
  install.packages(somePackage)
  library(somePackage)
}
```

In other words, if you haven't already, install a package, and attach it. But no more! Use this instead!

```R
library(librarian)
load_package(somePackage)
```

This does it for you!
