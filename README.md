# www

Source for [tony.software](https://www.tony.software).

Built with [Hugo](https://gohugo.io/) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

## Local development

Clone with submodules (the theme lives in `themes/PaperMod`):

```sh
git clone --recurse-submodules https://github.com/apancorb/www.git
cd www
hugo server --bind 0.0.0.0 -p 8001
```

## Adding content

```sh
hugo new posts/my-post.md
hugo new books/my-book.md
hugo new papers/my-paper.md
```
