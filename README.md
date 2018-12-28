# Game Review Feed JSON Specification

This documentation is published at [game-review-specification.dev](https://game-review-specification.dev).



## Table of index

- [Introduction](#introduction)
- [Feed example](#feed-example)
- [Specification](#specification)
    - [Feed structure](#feed-structure)
        - [Top level](#top-level)
        - [Review item](#review-item)
    - [Character encoding](#character-encoding)
    - [MIME](#mime)
- [Suggestions for Review publishers](#suggestions-for-review-publishers)
    - [Publishing, access and update frequency of the feed](#publishing-access-and-update-frequency-of-the-feed)
    - [Faulty data](#faulty-data)
- [Suggestions for Content aggregators](#suggestions-for-content-aggregators)
    - [Fetching the review title and thumbnail image](#fetching-the-review-title-and-thumbnail-image)
- [Improving the documentation](#improving-the-documentation)


# Introduction

This document specifies an Internet web feed standard tailored for [electronic video game](https://en.wikipedia.org/wiki/Video_game) reviews.

The goal of this specification is to create a simple, but universal, structure on how to expose game reviews by _Reviewer Publishers_
so that any it can be programmatically crawled, aggregated and exposed by _Content aggregators_ on the world wide web.

This specification shares the similarities to both the [RSS](https://en.wikipedia.org/wiki/RSS) and the [Atom](https://en.wikipedia.org/wiki/Atom_(Web_standard))
web standards in regards to exposing published content, and [Review Microdata](https://schema.org/Review) in regards to
providing web crawlers with review markup data, however the _Game review specification_ is specifically tailored for single resource feed of game reviews, but still being
open for extension.

Standardization would aid both;

- _Review publishers_, as it makes it easier to expose their review content to a larger audience programmatically, hence draw in traffic to their site where the
  full length of the review resides.

- _Content aggregators_, as it makes feed aggregator services able to programmatically fetch reviews from different publishers for a certain
game, giving the reader a broader selection of different game opinions, critiques and reviews.



# Feed example

Below is an example of a complete feed.

Resource: https://www.reviewpublisher.com/reviews.json

```json
{
  "version": 1,
  "reviews": [
    {
      "link": "http://reviewpublisher.com/2019/sekiro/",
      "title": "Sekiro is Dark Souls perfected",
      "lead": "Clearly inspired by the Dark Souls serie, Sekiro improves both the game play and the difficulty curve for new and old players.",
      "thumbnail": "http://static.reviewpublisher.com/sekiro.png",
      "score": 4.5,
      "maxScore": 5,
      "published": "2019-03-24 16:43",
      "game": {
        "name": "Sekiro: Shadows Die Twice",
        "platform": "PlayStation 4",
        "releaseYear": 2019
      }
    },
    {
      "link": "https://www.reviewpublisher.com/review/diablo-3-latest-smash-hit-from-blizzard",
      "thumbnail": "https://www.reviewpublisher.com/review/diablo.jpg",
      "score": 78,
      "maxScore": 100,
      "published": "2018-10-22T10:45:43+00:00",
      "game": {
        "name": "Diablo 3",
        "platform": "PC",
        "releaseYear": 2012
      }
    },
    {
      "link": "http://oldreviews.reviewpublisher.com/doom.html?utm_source=reviewfeed&utm_medium=reviewlink",
      "score": 4,
      "maxScore": 10,
      "published": "1994-02-12",
      "language": "en",
      "game": {
        "name": "Doom",
        "platform": "PC",
        "releaseYear": 1993
      }
    }
  ]
}
```



# Specification

## Feed structure

### Top level

Specification of the top level JSON object and its properties:

#### Mandatory fields

- `version` (mandatory) (number) - Should be the current specification version the feed is using, currently version `1`.
- `reviews` (mandatory) (array of objects) - An array of Review items.

### Review item

Specification of each review item object and its properties:

#### Mandatory fields

- `link` (required) (string) - The canonical web URI (`https://` or `https://`) to where the full length review is published, and which the content readers
should be directed to.

- `published` (mandatory) (string) - An [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601) formatted date when the review was published by the _Review publisher_.

- `game.name` (mandatory) (string) - The canonical name of the game, according to the game's publisher and / or developer.

- `game.platform` (mandatory) (string) - The English name of the gaming platform where the game was released on, such as `PC`, `Nintendo Switch`,
`Playstation 4`, `NES`, `XBOX 360`, `iOS`, etc. In order to keep the standard future proof, we only ask the _Review publishers_ to be clear and
non-ambiguous with what platform they mean, as it will help the _Content aggregators_ to identify and organize the reviews appropriately to their own systems.

- `game.releaseYear` (mandatory) (number) - An [ISO-8601 formatted year](https://en.wikipedia.org/wiki/ISO_8601) (`YYYY`) year of when the game was released.
This will help  _Content aggregators_ differentiate between games that share the same name and platform, but at different times (such as 1993's and 2016's
"Doom", which was both released on same platform (`PC`) as well). 

#### Optional fields

- `title` (optional) (string) - The title of the review.

- `lead` (optional) (string) - The lead paragraph of the review that opens or summarizes the review article.

- `thumbnail` (optional) (string) - A canonical web URI (`https://` or `https://`) to any associated thumbnail image. Needs to point to a 
[common image format]( https://en.wikipedia.org/wiki/Image_file_formats) for the web, such as [JPEG](https://en.wikipedia.org/wiki/JPEG),
[PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics) or [WebP](https://en.wikipedia.org/wiki/WebP).

- `language` (optional) (string) - The language of the review according to [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639). For example English is
`en`, German is `de` and French is `fr`, etc. A full list of language codes can be found [here](http://www.loc.gov/standards/iso639-2/php/code_list.php).
If `language` is omitted the default is English `en`.

- `score` and `maxScore` (optional) (number) - If the _Review publisher_ has given any numeric review score and what the possible max score is according
to their review policies.



## Character encoding

The feed should be character encoded in [UTF-8](https://en.wikipedia.org/wiki/UTF-8).

## MIME

The feed resource should be served using the MIME type `application/json`, the same content type as any other JSON data. 


# Suggestions for Review publishers

## Publishing, access and update frequency of the feed

There are no requirements regarding how the feed is published. For example `reviewpublisher.com/reviews.json`, `reviewpublisher.com/reviews` and
`reviews.reviewpublisher.com` are all perfectly legal URIs in order access the feed, as long as the JSON contents is delivered back to the HTTP client.

Access constraints may be applied on the feed by the _Review publisher_ (such as with rate limits, API keys, etc), however the argument against such
policies would be that the feed have a decreased public exposure.

There are no requirements regarding update frequencies of the feed of the _Review publisher_.

## Faulty data

It would be in the _Review publisher_'s_ own interest to keep the feed free from any (JSON) errors, follow the specification, and have clean and high quality content,
with working out-bounding links. If a feed has too many associated problems, a _Content aggregator_ might consider it is not worth the hassle to collect the reviews
from that particular _Review publisher_.



# Suggestions for Content aggregators

## Fetching the review title and thumbnail image

If either the review `title`, `lead` or the `thumbnail` is omitted, the _Content aggregator_ could potentially fetch this by parse the review page's HTML.
The `title` could be fetched by reading the [HTML Title element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/title), `lead` could be taken from
the [description](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta), while the `thumbnail` could potentially be fetched by looking at
the [Open Graph image](http://ogp.me/). Is it also possible that the page might have [Review Microdata](https://schema.org/Review).


# Improving the documentation

Discussions and improvements of both the specification and the documentation is done by creating issues and pull requests over at the GitHub repository.

Any changes will be appended according to [semantic versioning](https://semver.org/).
