Alright, I am going to talk about putting PostCSS into a Sass based workflow. They come together and wow! It’s pretty awesome. But I’m also going to do it in the context of how I, as a developer, deal with things like that weird combination of fear, laziness and pride that makes us unwilling to try new things, or to improve certain parts of our workflow.

I’ll start that story I guess two and half or three years ago, where I was a developer and I began seeing all sorts of great articles about how to use Sass, and hearing my developer friends talk about how to use Sass. And this was all fine and good – I sat there and thought ‘nah, my CSS is great! It doesn’t need any help, it’s fine all by itself, in its 2,000 line files’. 

I resisted it for a while. And then I went to a conference, and I heard some good talks about it. They gave me enough information for me to push past the fear and the lack of knowledge, as well as a boost of encouragement to get past some of that laziness and pride. 

I started using it, and it was fantastic – I dove in completely and learned a lot about it. And then a year later, I found a tweet from myself through one of those little time machine apps. 

I remember tweeting ‘Why would I need Grunt? I have a .config Ruby file that Compass runs that compiles my Sass AND does autoprefixer... I have literally everything I could want’. And then I figured out all the other things I could do with Grunt – I got through the learning curve and overcame fear, put away my pride and pushed through laziness. 

Then I changed jobs a few months later and everything was based on Gulp. I didn’t even have the option at this point to decide “do I want another tool or not?”. It was “here’s our workflow, here’s the tool. Use it. This is your new job”, and so I did. And you would think that by the time I got through all of that, I’d finally be at the point where when a new tool comes along, I can immediately and calmly evaluate it, integrate it into my workflow and test it. But sure enough, I spent months sitting on PostCSS thinking “nah, I don’t want to add that, my current Gulp flow is really good and it does all the style things I need”. 

### Why did I feel that way?

I’m going to talk through some of the reasons here today, and by way of doing that, I’ll show you the outline:
<ul>
<li>Why is there a mindset of Sass “versus” PostCSS? And when I say Sass, I mean any of your traditional preprocessors like Stylus, Less or Sass – that’s just the one that I happen to use. Why did I have that mindset? Why did I have that concern that they were against one another?</li>
<li>How can Sass and PostCSS work together nicely?</li>
<li>And then just practical tips for PostCSS – what are some awesome, practical things we can do with this tool?</li>
</ul>

### So why is there a mindset of Sass versus PostCSS?

You may have seen a tweet saying something like ‘PostCSS is a Sass killer’ or ‘Now I’m breaking up with Less because I have PostCSS’, and these awesome one-against-the-other, dichotomy-type statements. Where does that come from?

And it’s not just a PostCSS thing. We do this with Grunt versus Gulp, React versus all the other things… and you know, Angular and Ember and whatever camp we fall in. We keep, in our minds, dividing ourselves and separating ourselves from people that use ‘that other thing’. 

### So let’s talk about Sass versus PostCSS!

PostCSS is a preprocessor and Sass is a preprocessor… But what do I mean by a preprocessor? Essentially, it does stuff to your CSS before it ever gets to a browser. If you want a literal, actual post processor, look up prefixfree.js – that actually runs client-side in the user’s browser and adds, in this case, browser prefixes where they’re needed on CSS3 properties. But PostCSS is a preprocessor – it runs on your styles before they ever get to a user’s browsers. Of sorts. It doesn’t do the exact same thing, but it is still preprocessing in that workflow.

It modifies your styles before it gets to a browser, but it doesn’t replace preprocessors. Some people will use it to do so, and I’m not saying they’re bad or they’re wrong, or in any way denigrating their work. That’s a choice you can make, and there are plugins for PostCSS that will mimic all those things if you want them to. But it doesn’t do the exact same thing – it doesn’t modify CSS in the same way that preprocessors do.

PostCSS is our $10 ‘Word of the Day’

### An Abstract Syntax Tree Parser. What in the world is that?

By way of answering, let’s look at what CSS actually is – what kind of code is this? Well, it’s a text file, right? But it’s actually an associative array full of nested arrays. Right? Okay.

You’ve got a key (.element), and then you’ve got it’s value, which is an array of it’s own. And it’s a whole bunch of key value pairs. They key is ‘display’ and the value is ‘inline-block’. They key is ‘padding’ and the values are ‘1em’ and ‘2ems’.

CSS is, by definition, a whole bunch of nested objects or associative arrays – however you want to look at that. But you can’t parse it as such in your workflow easily, can you? I mean, browsers know how to do that – they’re really smart – but if you want to, like, do find and replace, or automate CSS modifications, you want to set up a bunch of really complex regx’s. And that’s really time consuming and kind of fragile. But PostCSS converts your CSS (again, it’s just a string in a file on your system) into a giant, actual iterable object. And with that, it provides you JavaScript APIs to loop through that object, and do whatever you want to with the object’s contents – add things, remove things, edit things.

It should be noted first, before we go much farther, that PostCSS, by itself, does absolutely nothing to your stylesheet. It just turns it into an object, and turns that object back into a file for you at the end. All the plugins you can get for PostCSS are what make the actual changes to your file. We’ll talk about some examples of those a little bit later.

So that’s what PostCSS does. By now, I’m hoping you can see how different that is from Sass or Less. 

Let’s talk about the individual strengths and weaknesses of these two tools. What are the advantages of Sass, Less, Stylus, and those traditional preprocessors (if I can use that word to describe something that’s only been around for a few years)? They come with a bunch of prebuilt features, and they come with a great bunch of community support that’s years old.

### What features am I talking about? 

<ul>
<li>Variables: Who loves variables in their Sass? That was, for me, the one cool things that I could do that got me over the hurdle of not using any Sass to using a little bit – I could put all my colours into variables and it was magical!</li>
<li>Conditions and loops: If this is true about a current environment variable or situation, then do this. Or even better, loops! Right? Stick all your social media colours in an array in a map, and loop through them all and create your share buttons.</li>
<li>Functions and mixins: These are all really, really awesome.</li>
<li>Community support: If you go to your preprocessor that’s had years of documentation and open source work, it’s got a really great docs page. How to set it up, how to install it, how to use it’s syntax.</li>
<li>Conferences and meetups: Certain preprocessing languages have their own conferences every year or two. </li>
<li>Blog posts and tutorials</li>
</ul>

I want to be careful here – I don’t want to imply that these things are not present with PostCSS. They’re just less developed because it’s a younger technology. And in some ways, sometimes, they are a little more fragmented – if you want to use Sass with your project, you can find all the Sass documentation in one place. If you want to add optional toolkits like Susy for grids, or something else, then you can find documentation on those toolkit author websites. 

With PostCSS, as I mentioned, PostCSS does nothing to your code. The plugins for PostCSS do everything, and each one of those is at it’s own stage in maturity in terms of documentation and community support. So by being less monolithic than an older preprocessor, you end up with a little less documentation and support. 

I guess that can be a call to action here – start using PostCSS, learn some stuff and write about it. Contribute to the blog posts and articles, post a talk at a conference, help document some things. 

### But what are the advantages of PostCSS over Sass, for example? Or Less?

It can manipulate CSS – Sass can’t go through and actually manipulate and exchange existing spec CSS that you’ve written. It can only handle it’s own syntax, and do things with it.

And custom syntax which I’ll show you an example of shortly.

Let’s talk about manipulation in a little more detail.

Things like vendor prefixes and syntax clean-up, sorting and shortening your CSS properties – those can all be handled very manageably by PostCSS plugins. There’s no option in Sass to ‘sort all my CSS in the same property order in every selector block’. This is an advantage that you get from having that Abstract Syntax Tree Parser.

Take an example real quick: 

Vendor Prefixes – if you’re using Sass you may remember this syntax, if you use Bourbon or if you saw the old Compass library it had almost the exact same syntax. Here’s how you do a transform, rotating something by 90 degrees. That’s kind of clunky, and it uses an awful lot of parenthesis. I never type the word ‘include’ correctly on the first try… Does anyone? 

But what if you could write this instead?

That spec CSS – that’s just plain, old, boring CSS. And what does autoprefixer do? It doesn’t care what you’re doing – in the background, it quietly goes through, takes your list of browser specifications for your support requirements, and says “okay, based on what the developer told me and their config, here are the exact prefixes I need to add next to that property”. It’s all automated, it all runs in the background. It’s very awesome.

How about custom syntax? This is one of those ‘your mileage may vary’ things. I haven’t found a custom syntax thing that I’ve considered worth implementing in my workflow, but should you have a need for this, here’s a great way to go:

You can make up your own custom CSS propertie, and then use JavaScript to parse the values you’ve given them, and put out real, live, browser comprehensible CSS. If you’re using Neat or Singularity or Susy (or a similar Sass grid mixin system), you might be familiar with an include mixin like this.

Again, who types ‘include’ right on the first try? ‘Extend’ is even worse!

What if you could just write this? It looks a lot more CSSy. It doesn’t really conflict with anything in spec (because I don’t think that’s a valid property at the moment). And you can prefix it with your own thing – make it your initials or your company name.You can get the same output on both of them.

The Sass library you’re using for your grids has all the math written into it’s mixins and functions, or if you prefer JavaScript and writing all your math in there instead, you can do that. They’ll both come up with the exact same output. Whenever PostCSS hits a grid-span property, it goes through and generates these mathematical values and plugs them into the correct properties. 

### Now how can they work together in real life? You say “this is cool, but does it work for real? How so?”

I like Gulp right now, so I’m going to show you a quick sample in Gulp. This part is very boilerplate – if you’re using Gulp to compile Sass, you probably have this in your Gulp glow already. You’re acquiring packages that you install with NPM install and giving them a variable name (aliases) in your Gulp file.

Then here’s a task to do that:

We start the source maps, we run a Sass compiler, and here’s the output style. We write the source maps and then we spit it all out in the right destination directory.

Now let’s add a few more packages that run PostCSS things. 

This first one is the big, obvious one – PostCSS itself. Then three really helpful tools I’ve found are ‘autoprefixer’, ‘cssnano’ and ‘postcss-sorting’.

By the way, cool thing (and this might be a secret), if you’re running Gulp autoprefixer or Grunt autoprefixer already, you’re secretly running PostCSS. It’s actually just a wrapper around PostCSS, running nothing but autoprefixer.

So if you’re using that, you’re secretly using PostCSS already. 

And here’s a new task for that:

First thing we’re going to do is we’re going to create an array (postcss_tools)that has instructions for how to use autoprefixer – these are the browsers I have to support because my boss said so, or because that’s what’s worth the money. 

And then we’re going to run the sorting one as well. And then if the production variable is true, because we’ve told it that with a flag, while we’re running Gulp, then we’re going to add ‘cssnano’, which is a fantastic minifier to the process as well.

And then, same thing – we take the source file as a CSS file we just made with the Sass task, we run postcss_tools on it, and we spit it back out in the same directory. That’s all it takes. 

If you’re not familiar with Gulp, I’m sorry – this may be confusing, boring code to look at. But the point is that it’s very simple in principle: you bring in the right packages to Grunt or Gulp, give them the right variable names, put them in an array and spit it through a new task. In fact, to optimise this, you wouldn’t need to run two different tasks, you could run the PostCSS process at the end of your Sass task to save one file right on your system.

So you can check that out. There is a Gist I’ve got up with sample code in there if you’d like to go there. These slides are live and clickable and you can get further information from the links in there.

### So what are some practical tips for using PostCSS?

We’re getting into the part of the slides that are just my opinion – there are other developers who use PostCSS in ways that I don’t recommend and that’s fine. If you can justify it, it fits your use case, if makes your workflow better, use it that way. Here are the things that i prefer to do:

### Avoid non-CSS input

I prefer to keep my non-CSS in the Sass in the older preprocessor, and just use PostCSS to turn CSS into better CSS. So you’ll notice I didn’t have a package installed in the sample Gulp file that took made up, custom CSS properties. The grid span I showed you in the example, there is actually a good library called Lost Grids that does provide a way to use custom CSS-looking properties and put your grid syntax into there.

And I should clarify, I’m not saying ‘grid syntax’ like we heard from Justin earlier, and not hte actual CSS Grid layout, just float-based stuff, but it does the math for you. That kind of grid – columns is really all it is, most of the time.

But anyway, that was a little rabbit trail.

I like to keep my PostCSS workflow just turning CSS into better CSS. If a stylesheet gets to the user’s browser without all the right vendor prefixes, some things might break. But for the most part, it’s all valid CSS and there’s not a property in there that the browser can’t understand. Unless it doesn’t have a prefix yet.

If you run something like a custom property or some CSS that doesn’t exist yet into a browser, that somehow misses your PostCSS workflow, the browser just won’t know what to do with it. So I prefer to use PostCSS to just turn CSS into optimised, faster, lighter, better compressed, whatever you want to call it CSS.

### And then I want to recommend that you be very, very careful with “Prolly-Fill” code. It’s a fun little word.

You’ve probably used a polyfill at some point – how many of you remember sticking HTML5 shivs/shim into the header of all your sites? That’s polyfill! It tricks old IE into rendering HTML5 elements correctly.

Polyfills, typically in JavaScript, are really good in that they’re testable and they’re stable. You use them to turn browser APIs that don’t yet exist into valid JavaScript functions that do  exist and do run, and when those aren’t supported by the browser, this extra bit of JS makes it happen. 

CSS doesn’t have anything like that. If you want to actually polyfill CSS in a browser, you can’t do anything with… Let me put it this way:

If we could write a whole bunch of older, existing CSS properties that did the exact same thing as the new, next-coming, non-spec CSS, we wouldn’t need the new, next, coming soon CSS. You kind of have to write that in JavaScript.

So there’s a great little PostCSS library called CSSNext. If you want to get your feet wet with new CSS that’s coming down the pipeline that’s been approved by one browser and behind the flag in the other, and not yet in the other three, and we may never see it Internet Explorer or very late Safari, that’s cool. CSSNext is a great little PostCSS plugin that will let you write that brand new, not-even-approved everywhere CSS (I’m talking custom properties – you may call them variables, grid layouts, some of these things), and will write the next best thing with existing CSS. But be aware, it’s not identical.

So if you’re using something like CSSNext, or another way to implement future CSS that’s not specd, not approved and not implemented, just be really aware that you need to go back and test everything that existed beforehand. Make sure you check it out in not just old browsers, but your new browsers that may not have that implemented correctly, because the best PostCSS can do in that, is write the next-best CSS to what you’re really aiming for with the new property.

Not to say don’t use it – just be careful. And test it really, really well as you go.

### Now, I have a list I’ll go through quickly for you, of cool plugins I think you might enjoy in PostCSS. 
<ul>
<li>You can manage your prefixes (as has already been mentioned) with ‘autoprefixer’.</li>
<li>You can sort your properties with ‘postcss-sorting’ – you say “Why do I care what order they’re in?” or “Doesn’t my linter already do that for me in my IDE or in Sublime Text? And that’s cool if it does.
If you’re using server-side gZip compression, and I hope you are (if you don’t know what that means, talk to somebody in the Back End team at work, or talk to your hosting company to see what needs to be enabled), if you put the properties in the same order every time, gZip compression can compress more text – it gives a server-side optimiser more room to work with. So whenever you can, sort your properties consistently.</li>
<li>You can minify your styles with ‘cssnano’. And this is a lot bigger and better than just running the minified output flag on Sass, or running ‘cssminify’ in an existing Gulp or Grunt plugin. The number of config options you have with ‘cssnano’ is remarkable and really well documented. Highly worth sticking in there.
Most of the time, minifying is just collapsing white space – this does more than that. If you have a 0 pixel measurement, it’ll take the px for you and remove it, and make things even lighter everywhere that needs to happen.</li>
<li>Who likes Flexbox? Who hit a bug with Flexbox, ever? Yeah, Ishould see about the same hands up there.
Phillip Walton has a fantastic repo on GitHub called Flexbugs, where he documents and provides workarounds in many cases, for a lot of common, but not yet fixed Flexbox bugs in modern and major browsers. 
Here’s a PostCSS plugin that implements those fixes automatically in the background, without you having to think about them, whenever that’s possible to automate.</li>
<li>Inheriting legacy code with like, 9 different versions of the same blue hex colours, all one decimal off, or the proverbial joke of ‘50 shades of different grey colours’? Yeah we’ve all heard that one. This (‘csscolorguard’) will tell you where they are and help you clean them up.</li>
<li>If you’re using Google fonts, you can just stick in the name of the font, and ‘font magician’ will automatically create the @font-face import and all that for you.</li>
<li>You can analyse your CSS – and this is really fantastic for working on teams where you have code reviews – and find out great data about what you’re actually doing. ‘Cssstats’ dumps a tonne of data into your console (or wherever things are going during your workflow), about many different selectors you’re using, how many different properties you’re using, and all sorts of great things.</li>
<li>‘List-selectors’ does the same thing, but with just one DL selector.</li>
<li>You can lint your styles with ‘stylelint’</li>
<li>And if you’re on a team that uses Bem, then you can go ahead and make sure that you’re doing that correctly with a Linter for that too – ’postcss-bem-linter’.</li>
</ul>

All this is possible with PostCSS plugins! And then there’s a list of further reading in these slides – I won’t read all the titles. In fact, I’ll go ahead and finish up a couple minutes early. 

Thanks so much for listening, it’s been a great joy to share this with you. I’m James Steinbach. You can find me almost everywhere on the internet at @jdsteinbach.

If you have any further questions or feel like I owe you four more minutes worth of content, catch me later this week and ask me more questions. Thank you very much!

