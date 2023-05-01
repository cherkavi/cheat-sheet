# [jekyll](https://jekyllrb.com/)

## installation 
```sh
sudo apt install bundler jekyll
bundle add webrick
```
## versions
```sh
ruby --version
# ruby 3.1.1p18 (2022-02-18 revision 53f5fc4236) [x86_64-linux-musl]

gem --version
# 3.3.25
```


## start

## [docker start](https://github.com/envygeeks/jekyll-docker/blob/master/README.md)
```sh
docker run jekyll/jekyll:latest -it /bin/sh

export JEKYLL_VERSION=3.8
docker run --rm \
  --volume="$PWD:/srv/jekyll:Z" \
  -it jekyll/jekyll:$JEKYLL_VERSION \
  jekyll build
```

### docker container start 
```sh
# build with caching gem's
export JEKYLL_VERSION=3.8
docker run --rm \
  --volume="$PWD:/srv/jekyll:Z" \
  --volume="$PWD/vendor/bundle:/usr/local/bundle:Z" \
  -it jekyll/jekyll:$JEKYLL_VERSION \
  jekyll build

# start server
docker run --rm \
  --volume="$PWD:/srv/jekyll:Z" \
  --volume="$PWD/vendor/bundle:/usr/local/bundle:Z" \
  --publish [::1]:4000:4000 \
  jekyll/jekyll \
  jekyll serve

docker run --rm \
  --volume="$PWD:/srv/jekyll:Z" \
  --volume="$PWD/vendor/bundle:/usr/local/bundle:Z" \
  --publish [::1]:4000:4000 \
  -it \
  jekyll/jekyll /bin/sh 
```

## possible issues
* `require': cannot load such file -- webrick (LoadError)
```sh
# gem install webrick # didn't work
bundle add webrick
bundle exec jekyll serve
```


## docker-compose start
```
version: "2"
services:
  jekyll:
      image: jekyll/jekyll:3.9.3
      command: jekyll serve --force_polling
      ports:
          - 4000:4000
      volumes:
          - .:/srv/jekyll
      environment:
        JEKYLL_UID: 1001
        JEKYLL_GID: 1001
```

### start locally
```sh
# create new source folder
jekyll new my-cv

# bundle exec jekyll serve
jekyll serve
jekyll serve --livereload
x-www-browser localhost:4000
```

## build
```sh
jekyll build
```

## Templates
* [https://jekyllthemes.io/](https://jekyllthemes.io/theme)
* [cv theme](https://github.com/sharu725/online-cv)