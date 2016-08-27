[![Build Status](https://travis-ci.org/Keendly/blog.keendly.svg?branch=master)](https://travis-ci.org/Keendly/blog.keendly)

Keendly Blog
=========================
This is a source of a blog that is available via http://blog.keendly.com.  
It's based on [Hugo](https://gohugo.io/) engine.

Blogging
--------
To create a post, write it using [markdown](https://gohugo.io/content/example/) and put in the `content` directory.  
You can run the *Hugo* server locally and see how the end result is gonna look like:  
`hugo server --theme=beautifulhugo --buildDrafts --watch`

Deployment
----------
Blog is being served directly from Amazon S3.  
Static files are generated and uploaded to an S3 bucket automatically using [Travis CI](https://travis-ci.org/Keendly/blog.keendly).

Contributing
-----------
Want to write a post for our blog?  
Feel free to create a Pull Request!
