# `<link rel="social-profile">`
A proposal for a `<link>` element, which can declare the relationships between an HTML document and any number of associated Social Media pages or profiles

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
