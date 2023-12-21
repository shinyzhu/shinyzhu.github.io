# Shiny Zhu - Shiny's Blog Powered by Hugo

## What Shiny Zhu Is About?

Welcome to Shiny Zhu's blog 2023 edition.

You can find blog posts and topics on #FullStack, #DevRel, #TravelTech and more now or in the future.

## Tech Stack and Tools of Shiny Zhu Blog

- [Hugo](https://gohugo.io/): One of the most popular open-source static site generators.

- [hugo-theme-diary](https://github.com/AmazingRise/hugo-theme-diary): A Hugo theme ported from [SumiMakito/hexo-theme-Journal](https://github.com/SumiMakito/hexo-theme-Journal). I also [forked one](https://github.com/shinyzhu/hugo-theme-diary) to make my own changes.

- [giscus](https://giscus.app/): A comments system powered by GitHub Discussions.

- [Typora](https://typora.io/): A beautiful and powerful [Markdown](https://daringfireball.net/projects/markdown/) editor.

- [Vercel](https://vercel.com/) Hobby plan. This blog is hosted there. It's free and really fast.

## Run Shiny Zhu Blog Locally

Make sure you have Hugo installed.

I prefer download Hugo Extended from Hugo [Releases](https://github.com/gohugoio/hugo/releases).

```sh
apps % curl -LO https://github.com/gohugoio/hugo/releases/download/v0.121.1/hugo_extended_0.121.1_darwin-universal.tar.gz
apps % tar zxf hugo_extended_0.121.1_darwin-universal.tar.gz -C hugox
apps % export PATH=$PATH:/Users/shiny/apps/hugox
apps % which hugo
/Users/shiny/apps/hugox/hugo
```

Then run `hugo serve` to view it.

```sh
shinyzhu.github.io[main*] % hugo serve
Watching for changes in /Users/shiny/Documents/GitHub/shinyzhu.github.io/{archetypes,content,layouts,static,themes}
Watching for config changes in /Users/shiny/Documents/GitHub/shinyzhu.github.io/config.toml
Start building sites … 
hugo v0.121.1-00b46fed8e47f7bb0a85d7cfc2d9f1356379b740+extended darwin/arm64 BuildDate=2023-12-08T08:47:45Z VendorInfo=gohugoio


                   | EN   
-------------------+------
  Pages            | 141  
  Paginator pages  |   8  
  Non-page files   |  97  
  Static files     |  18  
  Processed images |   0  
  Aliases          |  61  
  Sitemaps         |   1  
  Cleaned          |   0  

Built in 118 ms
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1) 
Press Ctrl+C to stop
```

Enjoy coding and writing. Comments are welcome.