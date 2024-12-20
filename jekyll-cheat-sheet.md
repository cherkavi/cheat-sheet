# [jekyll cheat sheet](https://jekyllrb.com/)
> translate yaml/markdown/liquid to html pages
> Transform your plain text into static websites and blogs.

## Other static sites generators, blogging tool
- [Hexo](https://hexo.io/docs/)
- [Hugo](https://gohugo.io/documentation/)
- [Gatsby](https://www.gatsbyjs.com/docs/)
- [Pelican](https://docs.getpelican.com/)
- [MkDocs](https://www.mkdocs.org/#documentation)
- [Lektor](https://www.getlektor.com/docs/)
- [Eleventy/11ty](https://www.11ty.dev/docs/)
- [Gridsome](https://gridsome.org/docs/)
- [Docusaurus](https://docusaurus.io/docs)
  - [docusaurus playground](https://docusaurus.io/docs/playground)
- [Metalsmith](https://metalsmith.io/#documentation)
- [Middleman](https://middlemanapp.com/basics/install/)
- [Sculpin](https://sculpin.io/documentation/)
- [Publish](https://github.com/JohnSundell/Publish) 
- [VuePress](https://vuepress.vuejs.org/guide/)

## Jekyll Templates
### Template language Liquid
* [liquid + jekyll](https://jekyllrb.com/docs/liquid/)
* [liquid shopify doc](https://shopify.github.io/liquid/)

### Templates for dynamic web page creation
* [https://jekyllthemes.io/](https://jekyllthemes.io/theme)
* [cv theme](https://github.com/sharu725/online-cv)
* [business landing page, agency page](https://jekyllthemes.io/jekyll-business-themes)
* https://jekyllthemes.dev/

## How to start with Jekyll
```sh
git clone https://github.com/sharu725/online-cv.git
cd online-cv
# Start Jekyll like: docker run ...
x-www-browser localhost:9090
```


## Start Jekyll

### Manual installation 
```sh
sudo apt install bundler jekyll
bundle add webrick

### check versions 
ruby --version
# ruby 3.1.1p18
gem --version
# 3.3.25
```

### [Docker container start](https://github.com/envygeeks/jekyll-docker/blob/master/README.md)
```sh
DOCKER_IMG_NAME=jekyll/jekyll
DOCKER_IMG_TAG=3.8
# DOCKER_IMG_TAG=3.9.3
# DOCKER_IMG_TAG=latest
DOCKER_JEKYLL=jekyll

# start server with caching gem's
docker run --rm --name $DOCKER_JEKYLL --volume="$PWD:/srv/jekyll:Z" --volume="$PWD/vendor/bundle:/usr/local/bundle:Z" --publish [::1]:4000:4000 $DOCKER_IMG_NAME:$DOCKER_IMG_TAG jekyll serve --force_polling
# connect to running container 
# docker exec -it `docker ps | grep jekyll/jekyll | awk '{print $1}'` /bin/sh
x-www-browser http://localhost:4000


wkhtmltopdf 
```

docker-compose start
```yaml
version: "2"
services:
  jekyll:
      image: jekyll/jekyll:3.9.3
      command: jekyll serve --force_polling
      ports:
          - 4000:4000
      volumes:
          - .:/srv/jekyll
          - ./vendor/bundle:/usr/local/bundle
      environment:
        JEKYLL_UID: 1001
        JEKYLL_GID: 1001
```

### jekyll commands
```sh
# create new source folder
jekyll new my-cv

# build web pages
jekyll build

# bundle exec jekyll serve
jekyll serve
jekyll serve --force_polling --livereload
x-www-browser localhost:4000
```

## possible issues
* `require': cannot load such file -- webrick
  ```sh
  # gem install webrick # didn't work
  bundle add webrick
  bundle exec jekyll serve
  ```

* ‘ruby3.0’: No such file or directory
  ```sh
  ❯ jekyll build
  /usr/bin/env: ‘ruby3.0’: No such file or directory
  ```
  ```sh
  ll /usr/bin/ruby3.0
  ln -s /usr/bin/ruby3.2 /usr/bin/ruby3.0
  ```
