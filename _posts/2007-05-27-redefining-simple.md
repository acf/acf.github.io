---
layout: post
title: Redefining Simple
date: 2007-05-27 22:06:05.000000000 +01:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- design
tags: []
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '1044099662'
author: Alan Francis
permalink: "/2007/05/27/redefining-simple/"
---
One of the main tenets of _Extreme Programming_ is that we should strive to produce 'simple code'. Sometimes code is written simply from the outset, other times code needs to be refactored until it is simple. In either case, it's necessary to have a working definition of simple to code or refactor to.

Luckily, XP provides us with a definition.

'Simple Code' meets the following criteria (this is an ordered list).

1.  Passes all the tests
2.  Communicates it's intent
3.  Contains no duplication
4.  Has the fewest possible number of classes and methods

It's probably worth discussing each of these in turn.

First off, we're not done if the code doesn't pass our tests. Since we're Extreme Programmers, we code _test-first_, and have a suite of unit tests for any particular piece of code we're about to check in. If we can't get here, we shouldn't even bother looking at numbers 2-4.

Secondly, communication. This is somewhat subjective, but in essence we're looking for _good names_ for methods and variables. Descriptive, and in keeping with any system of named we have in place. Perhaps all our controllers are called XXXController, perhaps the return variable from any method is always called result. The other thing I think is important here is reasonably short methods.

Number 3, like our first item seems pretty black and white. Either there's duplication or there isn't, right ? But discussing the different kinds of duplication is a whole other article. We're looking for both straight _textual_ duplication, but also _duplication of intent_. Duplication is a bad smell in our code and it's important that we stamp it out whenever we can.

The last item in the list is almost an tiebreaker. Given two different approaches that meet the first 3 criteria, choose the one with the least amount of code. It's an **ordered list**, so communication trumps size here.

Now, this (I think) is a pretty decent working definition. It has some fairly concrete guidelines (not rules) to code against and I do like the results I get when I work consciously with them.

But it's only one definition. Over time I've run a few experiments, trying out different 'rules'. Seeing what happens to code if they're followed religiously. One that I want to talk about here is named for it's Java implementation : No braces.

What would happen if the only braces we allowed in our Java source was to scope classes and methods. What would the code look like ?

Here's a simple little example, somewhat adapted from Martin Fowlers canonical Refactoring example.


{% highlight java %}
public void statement( RentalList rentals, User user, PrintWriter output, boolean headerRequired )
{
	if( headerRequired )
	{
		output.println( "========== STATEMENT ===============" );
		output.println( " Username: " + user.getName() );
		output.println( " Date: " + new Date().toString() );
		output.println( "====================================" );
	}

	while( Iterator i = rentals.iterator(); i.hasNext() )
	{
		Rental r = (Rental)i.next();
		output.println( "Title:  " + r.getTitle() );
		output.println( "On:     " + r.getRentDate() );
		output.println( "Cost:   " + r.getPrice() );
		if( r.earnsPoints() )
		{
			output.println( "Points: " + r.getPoints() );
			totalPoints += r.getPoints();
		}
	}

	output.println( "====================================" );
	output.println( "Total Points: " + totaPoints );
	output.println( "====================================" );
}
{% endhighlight %}


Here's what I ended up with following my rule.


{% highlight java %}
public void writeStatement(PrintWriter output, RentalList rentals, User user, boolean headerRequired)<br />
{
	writeHeaderIfNecessary( output, user, headerRequired );
	writeRentals( output, rentals );
	writeFooter( output, rentals );
}

private void writeStatementHeaderIfNecessary(  PrintWriter output, User user  )
{
	if( headerRequired )
		writeHeader( output, user )
}

private void writeHeader( PrintWriter output, User user )
{
	output.println( "========== STATEMENT ===============" );
	output.println( " Username: " + user.getName() );
	output.println( " Date: " + new Date().toString() );
	output.println( "====================================" );
}

private void writeRentals( PrintWriter output, RentalList rentals )
{
	while( Iterator i = rentals.iterator(); i.hasNext() )
		writeRental( (Rental)i.next() );
}

private void writeRental( PrintWriter output, RentalList rental )
{
	writeRentalDetails( output, rental );
	writePointsIfNecessary( output, rental );
}

private void writeFooter( PrintWriter output, RentalList rentals )
{
	output.println( "====================================" );
	output.println( "Total Points: " + rentals.getTotalPoints() );
	output.println( "====================================" );
}

private void writePointsIfNecessary( PrintWriter output, Rental rental )
{
	if( rental.earnsPoints() )
		output.println( "Points: " + rental.getPoints() );
}
{% endhighlight %}

There are, overall more lines of code, but what we end up with is interesting in a few ways.

The first interesting thing, I think, is the method length. A lot of _very_ small methods, none really over 2 or 3 lines long. Each method does exactly one thing. It's a decision, a loop or a small set of sequential steps bundled into a 'job'.

The second interesting thing is that those 'job' methods are written at the same level of abstraction. We don't see writeHeader() followed by all the guts of writing the body. Instead we see header, body, footer.

The third interesting thing is that we've eliminated all _temporary variables_. A useful side effect is that it becomes much more apparent when methods can or should be moved between objects. The total points in the above example seems like it needed moved to the RentalList. One more temporary variable eliminated.

Now, there's obviously a few criticisms to be levelled at code like this. Where's the work done? Why do I have click all over the place to figure out the flow of the application? I've heard it described as _'ravioli code'_ (as opposed to spaghetti). Interestingly, large chunks of Smalltalk are written like this. It's quite a common coding style in dynamic languages, although the loop method calling a singular method would most likely be done with a block.

The advantages, however all depend on good names for the methods in question. With good names, someone reading the code shouldn't have descend the method hierarchy very far to get the answers they seek.

If we're interested in something in the statement footer, we can click into writeStatementTo() and into writeFooterTo() we don't need to be bothered with all the implementation detail of the header and body. It's not relevant except to understand that they are printed before the footer. Or inside the writeHeaderIfNecessaryTo() we can see a decision is made on whether to print the header. If all we care about right now is how that decision is made, and not the actual printing we can see that clearly.

One of the design techniques I was taught in college was Jackson Structured Programming. This was focussed mostly on COBOL, and it's strategy was to turn a Data Definition into a Program Definition. The diagrams for these definitions were box-and-arrow style, with boxes representing **SEQUENCE**, **SELECTION** and **ITERATION**. Sound familiar?

I like this code style. I know it's a bit extreme, but I'm an extreme programmer so that doesn't frighten me. It makes my code really, really simple and obvious. Each piece laid out in terms of others. Iteration or Selection logic separate from what happens based on that logic.

Extreme Programming is based on turning the dials up to 10 on a lot of practices and then dialling down only where appropriate, so I like the idea of challenging myself by trying a coding style all the way. I'm almost certainly not going to write all my production code that way, as sometimes code gets seriously contorted to meet the 'rule', but it's a useful exercise.

Perhaps you have your own ideas of what **simple** might mean? What would happen if you tried to put those ideas into practice ? What would happen if you tried to completely eliminate all temporary variables? Or set a hard limit on method length of, say, 5 lines? Try it. You may not want to code like that all the time, but I guarantee you'll learn something about your code and maybe even yourself.