# Website

- My Research Website at [balaje.github.io](https://balaje.github.io).
- Based on the minimal theme by [orderedlist](https://orderedlist.com/minimal/).

# Install notes

Installing this Unix based operating system:

1) First install Ruby, Jekyll and Bundler by typing

``` shell
sudo apt-get install ruby-full jekyll
```

2) Then clone this repository by typing

``` shell
git clone git@github.com:Balaje/balaje.github.io.git
```

and then type

``` shell
bundle install
```

to install the necessay gems for building the Github pages. Then type

``` shell
bundle exec jekyll serve
```

to test if the webpages are built. If it works, Good! If not Google the error.
