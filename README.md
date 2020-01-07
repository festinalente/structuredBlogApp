# structuredBlogApp
On my backend(s) I generally use 'contenteditable' on elements rather than a rich text editor. However, the time came to make this feature available to clients and sanitizing, storing and rendering this output is tedious.

I have tried a few rich text editors but found them wanting, most recentely the beautiful app 'quill.js'. I was very impressed by this till the time I had to render the data outside of quill.js, where picking through the post data was difficult and loading a massive script on the client to render a post seemed over the top. 

Inspired by Quill.js, this app works by producing an array with an object representing a node element and it's content.

A blog post is stored as an array of objects: 

`[{el: 'h1', cont:'Hello'}, {el:'p', cont:'world'}]; `

Once sanitized, this can be popped directly into MongoDB or a JSON field in MySQL.

This can easilly be rendered, here's an example in pug:

```
block bodycontent
  h1 #{post.title}
  .post
    each val, index in post.content  
      case val.el
        when 'p'
          p #{val.cont}
        when 'h1'
          h1 #{val.cont}
        when 'h2'
          h2 #{val.cont}  
        when 'h3'
          h3 #{val.cont}
        when 'h4'
          h4 #{val.cont}      
        when 'img'
          +imgMixin(val.cont, post.content[index+1].cont, val)
```
Note that for images, cont refers to the source, 'src', however this is dealt with within the code.
