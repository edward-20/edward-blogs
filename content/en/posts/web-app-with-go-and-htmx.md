+++
date = '2025-01-25T10:27:07+11:00'
draft = false
title = 'Web App With Go and Htmx'
+++

I want to make a minimalist web app to help me practice for HSK 1. I'd been
hearing alot about Htmx and the trend has been to pair that with Go. So let's
see if I can do it one day. 

## Requirements
1. local server (no need to deploy)
2. randomly pulls up one of 500 words from HSK 1 3.0
3. user inputs their answer in pinyin
4. web page tells user if their answer is correct, if not it'll show the word
   and pinyin
5. web page then brings up another word

The source code is [here](https://github.com/edward-20/chinese-learning-app).

## Results and My Take
After knowing Htmx a bit better after learning its gist, I can say that it is
honestly a really interesting, unique and potentially powerful way to look at
frontend.

Let's briefly compare how something in this app would be done in React vs HTMX.

The thing being done: Show a chinese character and have the user input their
answer for the pinyin of that character. Once the submit their answer, change
the UI to provide feedback to their answer. Ask them if they want to see another
chinese character. Repeat.

I just want to say this is not a well thought out analysis on the differences
between React and HTMX and of frontend-JS libraries in general. It's just my
opinion after learning HTMX literally in one day to do something. This is more
for me to record my own initial thoughts and if you derive some use from this,
I'm grateful.

As someone who's a little fatigued of how much JS is encroaching on my web
development (NextJS, React, Angular, Node), I just wanted to see what else is
out there.

### React
 On the test page I would have used `useEffect` to initiate an ajax request for
 some chinese characters and their corresponding correct pinyin. Then I'd
 initialise state with the payload which is an array of objects, each containing
 the chinese character and the corresponding pinyin. Let's call it
 `chineseWords` I'd also have a state
 variable which corresponds to a random index of the array. Let's call it
 `current`.

 Display the chinese character corresponding to that index. Then give them an
 input for their guess at the pinyin on which I would trigger an event handler
 whenever they press enter. The event handler checks the input's value and
 change the UI to represent feedback based on whether the pinyin was correct or
 not. This feedback UI will also have a prompt for them to get another character
 which will just change `current`. I'm now realising I need a state variable to
 determine whether I want to show feedback or the input, call it
 `feedbackOrQuestion`. This feedback UI needs to know what the user input. So I
 need another state variable `userPinyinAnswer`. As I'm writing this I'm
 realising how non-trivial the amount of client-side work needs to be done. I
 haven't even accounted for needing to fetch more chinese characters.

With HTMX I'm using `hx-get` to constantly ajax request html rendered templates.
I will say that right now currently the way I use it doesn't seem effective. My
concern primarily being the nature of the AJAX requests I'm making. For this
app, the requests I'm making don't seem to make sense in that I'm requesting
with data I already have in order to manipulate DOM. For example, when
presenting the question, I already previously have the chinese character and the
correct pinyin loaded in the template, and yet when I want to show feedback it's
easier to put it all back in the request along with the user's answer to show
the feedback UI.

I do believe that things like manipulating DOM should be done client-side if
the new UI components are contingent on data that already exists client-side.
And yet it seems so bloated.

This was a rough iteration, and I intend to explore HTMX further and understand
better the advantages and disadvantages of this HATEOAS style of web development
as opposed to client-side DOM manipulation.
