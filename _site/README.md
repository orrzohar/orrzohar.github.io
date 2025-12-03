# Orr Zohar's Website

### Adapted from [Martin Saveski's Website](https://github.com/msaveski/www_personal/tree/master)

## Updates guide
Change one of the files in `_data`, unless you are changing the look of the website.

Test changes with:
```
jekyll serve
```

Push to the ML web directory:
```
rm -rf public_html
mkdir public_html
```
```
./__deploy.sh
```


To update projects, copy the project template under _project/template. Be sure to edit _project/project_name/index.html appropriately. 
This is based off of the [Nerfies](https://github.com/nerfies/nerfies.github.io) website design.

Website icons now included under `-image` in `publications.yaml`. 



## External Libraries
- Framework: [Jekyll](http://jekyllrb.com/)
- CSS
  - [Skeleton](getskeleton.com)
  - Tabs: [Skeleton Tabs](https://github.com/nathancahill/skeleton-tabs)
  - Experience: [Timeline](https://codepen.io/NilsWe/pen/FemfK)
  - Icons: [Font Awesome](http://fontawesome.io/)
- JS
  - [Jquery (3.1.1)](https://jquery.com/)
