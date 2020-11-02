# Hacking my way into creating a free URL shortening service by knitting several free pieces of the Internet

1. The most important part of a URL shortening service is a short domain. It had to be free, so thanks to [freenom.com](freenom.com) for that :)

2. Now a free backend server was required. But there weren't much services that were actually free and provided a good basic service as well. But thanks to [pythonanywhere.com](https://www.pythonanywhere.com/) for that :)
	- But wait, they don't let you have a separate IP. hmmm ...
	- Ok, I will just add the `A` record in my DNS to my `pythonanywhere` app's sub-domain. Problem solved!
	- Not so fast. If only I had known that you can't put domain names in `A` records. hmmm :/
	- Ok, so I found something interesting from `pythonanywhere` forums. You can redirect your naked domain i.e. `example[dot]com` to `www[dot]example[dot]com` by using an IP from a free service called [wwwizer.com](http://wwwizer.com/naked-domain-redirect) and then you can add a `CNAME` record for `www` and redirect that to your pythonanywhere app's sub-domain. Ok, lets do that!
	- :/ hmmm... it seems that `pythonanywhere` guys are clever. They don't let you go to your app's sub-domain from a `www redirect` unless you are a paying client.

3. Ok, it seems that it isn't gonna work, so lets just leave it ...

4. So, I had an idea the next day. What if I used another free service that lets me host a static page, and I could redirect from my DNS to that static page which would then redirect to the actual site hosted at pythonanywhere? Hmmm ... so what platform lets you have static pages? Aha! it's [github.com](https://github.com/).

5. So after experimenting, I came to know that I can't redirect my `CNAME` record to anything that has a path following the actual domain e.g. `username[dot]github[dot]io` is fine but `username[dot]github[dot]io/something` is not. Which means I would need to make a `github organization` to have a separate `username[dot]github[dot]io` domain.

6. Ok, done that, but it seems that `github pages` doesn't support dynamic routing e.g. if you enter `username[dot]github[dot]io/something`, it will look for a repo with the name `something` instead  of serving the page at the root domain and let you parse the `/something` yourself. Which makes sense since it is a static file hosting platform.

7. Thanks to the awesome people who created this [spa-github-pages](https://github.com/rafgraph/spa-github-pages) hack/solution. It lets you handle all dynamic routes on a single static file. Check out their README for more details.

8. Finally! it's done and it works.

> You can try it out on [rout.ml](http://rout.ml)

> Source code is available at [github.com/routml](https://github.com/routml)

> It might not work always due to the limitations of free-ness. So if you are looking at this from the future, thank you for your time :) I hope this has been helpful to you in some way.

**Below is a diagram that shows how the request from the naked public domain goes all the way to the actual app hosting  sub-domain.**

<img src="https://raw.githubusercontent.com/routml/routml-about/main/explanation_diagram.png" width="200px" />

---

I hope this has been interesting for you.
Take Care && Good Bye. 😊
