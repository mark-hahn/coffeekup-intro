A Beginners's Intoduction to CoffeeKup 
===

This is a work in progress for a mini-book.  It started out to be a one-page introduction but I found it was easier to write something longer.  It takes less thinking.

This is meant to be a collaborative effort and I will gladly give credit to anyone who helps, even to just point out typos.  To contribute please submit issues or pull requests to this project.

I am not kidding when I say in the book that I am also a beginner, learning CoffeeKup as I write this.  I don't yet understand CoffeeKup very well, especially in the sections that are unwritten.  So please let me know when I am passing along false information or when I am missing something that I've left out that should be said.

The actual book follows.  I have only completed the first section so far which will be the longest and probably the hardest. 

--------

A Beginners's Intoduction to CoffeeKup 
===
By Mark Hahn

KoffeeKup uses a simple scheme to provide a concise, expressive, easy-to-read, and time-saving HTML templating solution. It is based on the CoffeeScript language, with which you will need to be familiar.  If you aren't already hooked on CoffeeScript then visit http://coffeescript.org first to find out what you are missing. Then come back here to also get hooked on CoffeeKup.

This introduction is for CoffeeKup beginners like myself (I'm learning it as I write this). Let's go through this together step by step.  Once you complete this I suggest you go to CoffeeKup's [github page] (https://github.com/mauricemach/coffeekup) to learn more. Currently the only discussion of KoffeeKup is on CoffeeKup's [issues page] (https://github.com/mauricemach/coffeekup/issues?sort=created&direction=desc).

Unlike most tutorials I will not need to help you install KoffeeKup to follow along with the examples. I will give the results of the template with each example. You might also want to bring up http://coffeekup.org in another window and paste these examples into the left pane.  This will allow you to play around with the template and see the results immediately. (This also makes a great tool to use while you are writing your own CoffeeKup code).

Let's Get Started - Hello World
---

First our mandatory friend, Hello World.  In each example the CoffeeKup template code appears first followed by the rendered HTML.

	head ->						
		title 'Hello World'
	body ->
	
	<head>
		<title>Hello World</title>
	</head>
	<body>
	</body>

First of all, note that the template code is real CoffeeScript code. KoffeeKup _is_ CoffeeScript. Except for some important CoffeeScript code added invisibly to the top and bottom of the template, the coffescript you write in the template is executed directly to render the output. This is very different from most template engines and is the reason KoffeeKup offers all the great features mentioned at the beginning.

So how did `head ->` become `<head>`?  And where does the output come from? There is nothing like a `write` function in the template to send out the results. And how did `</head>` appear out of thin air?  The secret ingredient in the coffee recipe is the extra code that was mentioned above.

The top part of the added code defines a lot of things, the most important of which are the functions who share their names with all the possible HTML tags. These functions, when executed, generate the associated HTML code and append it to a buffer, also defined in the invisible code, that accumulates all of the output HTML. When all of your template code has been executed, the buffer containing the complete HTML is then returned as output by an invisible `return buffer` statement added to the bottom of the template.

Let's walk through the execution of the Hello World template code.  First the `head` function is called with the `title` function as an argument. The `head` function adds the `<head>` text to the buffer, then calls the `title` function which adds it's own html to the buffer, and finally adds the closing `</head>`.  

The `title` function was called with `"Hello World"` as its only argument. In a case like this, the function only had to wrap `<title>` and `</title>` around the string it was passed and add the whole thing to the buffer.  The `body` function did the same thing as the `head` function except that the function passed to it added nothing to the output buffer.

So the "tag" functions create all the resulting HTML by adding their arguments to the output buffer while executing the function arguments to create the nesting.  Quite elegant, yes?

If this was all there was to KoffeeKup then it would already be quite useful as a way to write all your HTML in a concise way. No more adding all those nasty closing tags. But wait, there's more ...

Adding Attributes 
---

We know how to put anything we want for the inner html for a tag.  You just include an arbitrary string as an argument to the tag function.  But how do you put attributes inside the tag itself?  Luckily that is very easy.  Check this out ...

	div id:"ugly-box", style:"width:90px, height:90px, background-color: purple, border: 5px green"

	<div id="ugly-box" 
		 style="width:100px, height:100px, background-color: purple, border: 5px green">
	</div>

Any `object` (aka `hash`) used as an argument to a tag function is interpreted as a set of attributes.  The hash keys are the attribute names and the hash values are the attribute values.  In coffescript hashes are created easily and they are perfect for KoffeeKup's attributes.

Let's look at this more complicated example which ties everything we know together ...

	div id:"another-ugly-box", style:"background-color: purple, border: 5px yellow", ->
		span (color:"green"), "And I'm ugly text"
	
	<div id="another-ugly-box" style="background-color: purple, border: 5px yellow">
	  <span color="green">And I'm ugly text</span>
	</div>

Now it is starting to look like real html you'd find on an ugly web page.  

Did you notice the parentheses around `color:"green"`?  In older versions of coffeescript this is needed so that the string after it is not treated as part of the hash.  The newest versions of CoffeeScript have changed the rules so this isn't needed.  I'm going to assume the latest version of CoffeeScript in the following examples to keep them prettier.  As of this writing the KoffeeKup online trial page uses an older version of CoffeeScript.  So add the parentheses before trying them out there.

Lonely Text
---

At this point in my use of KoffeeKup I was starting to think I knew how to generate any html, but then I ran into a stumbling block. I needed to put some text between tags and not inside a tag.  This is a hole in the KoffeeKup logic described so far, but the hole has been filled with a fake tag named `text` ...

	span color:"red", "I'm bright red!"
	text "I'm boring black"
	span color:"blue", "I'm feeling blue"
	
	<span color="red">I'm bright red!</span>
	I'm boring black
	<span color="blue">I'm feeling blue</span>

The `text` tag (function) just adds whatever text is in its string argument to the output buffer.  If we removed `text` from the beginning of the middle line, that line with only the string would be legal CoffeeScript, and the template would execute without error, but the text would be lost because there would be no function to add it to the output buffer.

Before we leave the discussion of general text I'd like to point something out. Whether it is a string argument to a real tag like `div`, or a string argument to the fake `text` tag, a string can contain any text, even html.  We will learn how to use this to our advantage in the section _The Great Escape_.

Variables, Conditionals, Loops, And Functions Galore
---

Running In Context
---

Helpers To The Rescue
---

The Great Escape
---

Options, Options, And Options
---

Running Fast
---

Where To Go From Here
---

Credit Where Credit Is Due
---

