# `<link rel="social-profile">`
A proposal for a `<link>` element:
```
<link rel="social-profile rel-author" title="Alice" href="https://twitter.com/alice">
```
which can declare the relationships between an HTML document and any number of associated Social Media pages or profiles.

It's much less common today in 2020, but in the early to mid 2010s `<link rel="me">` enjoyed a brief burst of popularity, propelled by **Google Authorship** markup.

This proposal communicates the same data but enables tagging of multiple individuals and / or organisations.

Instead of:
```
<link rel="me" href="https://twitter.com/modesty-blaise">
<link rel="me" href="https://github.com/modesty-blaise">
<link rel="me" href="https://facebook.com/modesty-blaise">
```

we may write:
```
<link rel="social-profile rel-author" title="Modesty Blaise" href="https://twitter.com/modesty-blaise">
<link rel="social-profile rel-author" title="Modesty Blaise" href="https://github.com/modesty-blaise">
<link rel="social-profile rel-author" title="Modesty Blaise" href="https://facebook.com/modesty-blaise">
```

______

## Why do we need a `<link rel="social-profile">` ?

Certain social media platforms established proprietary metadata long ago, showing how a web document might meaningfully declare a relationship with an individual page or profile on their platform.

For instance, an article on the *Neverland News* blog might contain amongst its metadata the markup:

```
<meta name="twitter:site" content="@NeverlandNews">
<meta name="twitter:creator" content="@PeterPan">
```

immediately indicating that:

 - `@PeterPan` is the Twitter account of the article's author
 - `@NeverlandNews` is the Twitter account of the website hosting the article
 
 Similarly, the markup
 
 ```
 <meta property="article:author" content="https://www.facebook.com/PeterPan" />
 ```
will indicate that `facebook.com/PeterPan` is the Facebook Personal Profile of the article's author.

The problem with these proprietary formats is that they are:

 - inconsistently formatted
 - limited in scope

That is:

1) there is little similar between `<meta name="twitter:creator">` and  `<meta property="article:author">`; and

2) even when we can indicate the social profiles of an **article author** or **article publisher**, there is no syntax or formatting that enables us to indicate the social profiles of an article's **lead author** or **co-author** or **contributor** or the social profiles of a **guest editor** or of the subject(s) of the article (if, for example, the article was one profiling the work of an **architect** or an **artist** or a **writer/illustrator duo** or some collaborating **musicians** or a **team of app developers**.)

_What if, instead, there was a consistently formatted `<link>` element with a straightforward syntax, which could use any custom descriptor(s) to declare the relationship(s) of any social page(s) or profile(s) on any social media platform(s) to the current web document?_

_______

## Wait a second, can't we already do that with `JSON-LD` + `Schema.org`?

Yes, we absolutely can. But the `Schema.org` turns out to be a little verbose and clunky, due to the number of ***required properties*** needed on at least two separate levels of nesting.

Here's an example of a `JSON-LD` + `Schema.org` describing an **article with four co-authors** (of different levels of seniority), one referencing two of their social media profiles, the others all referencing one each:

```
{
  "@context": "http://schema.org/",
  "@type": "Article",
  "name" : "This Article",
  "headline" : "This Article's Headline",
  "datePublished" : "2020-04-21",
  "image": "/this-article-image.jpg",

  "author": [
    {
      "@type" : "Role",
      "name" : "I don't know what this is supposed to be",
      "roleName" : "Lead Author",
      "author": {
        "@type": "Person",
        "name": "Professor Plum",
        "sameAs": [
          "https://twitter.com/professor-plum"
        ]
      }
    },

    {
      "@type" : "Role",
      "name" : "I don't know what this is supposed to be",
      "roleName" : "Co-author",
      "author": {
        "@type": "Person",
        "name": "Mrs Peacock",
        "sameAs": [
          "https://twitter.com/mrs-peacock"
        ]
      }
    },

    {
      "@type" : "Role",
      "name" : "I don't know what this is supposed to be",
      "roleName" : "Contributing Author",
      "author": {
        "@type": "Person",
        "name": "Colonel Mustard",
        "sameAs": [
          "https://facebook.com/colonel-mustard"
        ]
      }
    },

    {
      "@type" : "Role",
      "name" : "I don't know what this is supposed to be",
      "roleName" : "Contributing Author",
      "author": {
        "@type": "Person",
        "name": "Miss Scarlett",
        "sameAs": [
          "https://linkedin.com/miss-scarlett",
          "https://instagram.com/miss-scarlett"
        ]
      }
    }
  ],

  "publisher": {
    "@type": "Organization",
    "name": "Tudor Mansion Publications",
    "logo": {
      "@type": "ImageObject",
      "url": "/tudor-mansion-logo.jpg"
    }
  }  
}
```
Leaving aside for the moment the `name` property of the `"@type" : "Role"` (since I have no idea what it's supposed to represent), our first impression might be that the `JSON-LD` is rather long when all we want to do is declare relationships between the current web document and several social profiles.

With that in mind, what can we remove from the `JSON-LD`, above?

 - We can't remove the `"publisher"` from the top level, because it's a required property of `"@type": "Article"`
 - We can't remove `@type": "Organization` because that's the expected `@type` for `"publisher"`
 - We can't remove `"logo"` or `"name"` from  `"publisher"` because they're both required for `@type": "Organization`
 - We can't remove `"name"` from the top level, because it's a required property of `"@type": "Article"`
 - We can't remove `"headline"` from the top level, because it's a required property of `"@type": "Article"`
 - We can't remove `"datePublished"` from the top level, because it's a required property of `"@type": "Article"`
 - We can't remove `"image"` from the top level, because it's a required property of `"@type": "Article"`
 - We can't remove `"name"` from above each `"roleName"`, because it's a required property of `"@type": "Role"`  
 
Essentially, we can't remove anything from the `Schema.org` above.

Not only that, but Google's [Structured Data Testing Tool](https://search.google.com/structured-data/testing-tool) gives additional warnings (not errors) for **two _further_ top-level fields** which it recommends ought to be included in the `Schema.org` above:

 - the `dateModified` field is recommended. Please provide a value if available.
 - The `mainEntityOfPage` field is recommended. Please provide a value if available.

That's a lot of extra structured data when we're _only_ trying to communicate:

 - a Relationship
 - a Social Media URL

So it's worth asking again:

_What if, instead, there was a consistently formatted `<link>` element with a straightforward syntax, which could use any custom descriptor(s) to declare the relationship(s) of any social page(s) or profile(s) on any social media platform(s) to the current web document?_

________

## Introducing `<link rel="social-profile">`

What would the data in the section immediately above look like, if it were formatted using `<link rel="social-profile">`?

It would look like this:

```
<link rel="social-profile rel-lead-author" title="Professor Plum" href="https://twitter.com/professor-plum" />
<link rel="social-profile rel-co-author" title="Mrs Peacock" href="https://twitter.com/mrs-peacock" />
<link rel="social-profile rel-contributing-author" title="Colonel Mustard" href="https://facebook.com/colonel-mustard" />
<link rel="social-profile rel-contributing-author" title="Miss Scarlett" href="https://linkedin.com/miss-scarlett" />
<link rel="social-profile rel-contributing-author" title="Miss Scarlett" href="https://instagram.com/miss-scarlett" />
```

This represents the entirety of the data we wish to declare and no more.

_____

## The anatomy of `<link rel="social-profile">`

The `<link rel="social-profile">` element comprises **4 Parts**:

1) The `href` attribute is **compulsory** and its value gives the `URL` of the related social media page or profile.

2) The `rel` value `social-profile` is **compulsory** and indicates that the URL value of the `href` attribute points to a *social media page or profile*. This could be pretty much any resource with a URL that might be recognised as part of the social web:

 - a Facebook Page
 - a Facebook Group
 - a LinkedIn Profile
 - a Twitter Account
 - a Strava Profile
 - a GitHub Developer Profile etc.

3) The second `rel` value, prefixed with `rel-`, is **optional** and indicates the *type of relationship* that the owner of the social-profile has to the current document.

There are no predefined values which follow `rel-*` (much like the `data-*` custom attribute in HTML5, any word or series of hyphenated words will suffice), though conventions may arise over time, such that where the following are used:

 - `rel="social-profile rel-blogger"`
 - `rel="social-profile rel-writer"`
 - `rel="social-profile rel-me"`
 - `rel="social-profile rel-author"`

eventually, the first three may all come to be regarded as secondary aliases of the quasi-standard:

 - `rel="social-profile rel-author"`

4) The `title` attribute is **optional** but where it is included, it indicates which `<link rel="social-profile">` elements refer to the same entity and which refer to distinct entities.

______

## Further thoughts on `<link rel="social-profile">`

 - `<link rel="social-profile">` may only appear in the `<head>` of an HTML document
 - There is no limit to the number of `<link rel="social-profile">` elements in one document
 - There is no limit to the number of times a `rel-*` value may be reused
 
