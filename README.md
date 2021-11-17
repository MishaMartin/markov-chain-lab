# Markov Chains Lab

## Markov Chains
## Introduction
First, you’ll create a Python script to procedurally generate original content based on an excerpt from Green Eggs and Ham by Dr. Seuss. Then, you’ll enhance your script so that it works on any corpus (any existing piece of text) and even combinations of corpuses.

## Setup
Download the materials for this exercise here.

## Intro
Procedurally generated data is data that’s created by an algorithm rather than by a human. It’s a popular form of art due to the surprising, unexpectedly sophisticated things that these algorithms will produce. 

People also write scripts to generate amusing parodies of authors and celebrities. My Beautiful Despair: The Philosophy of Kim Kierkegaardashian is a book of tweets generated by combining the works of Soren Kierkegaard with the Twitter feed of Kim Kardashian.

Either way, both procedures — one that generates images and one that generates text — can be represented as Markov chains. By modeling Markov chains with code, you too can write a script to produce your own procedurally generated content.

## Markov Chains
Colloquially, Markov chains are a statistical analysis of frequency patterns in some sequence. Consider the following sequence of words:

### green-eggs.txt
Would you could you in a house?
Would you could you with a mouse?
Would you could you in a box?
Would you could you with a fox?
Would you like green eggs and ham?
Would you like them, Sam I am?

We can process the text and break it down into word pairs. For example, the first line contains the following pairs of words:

(Would you) (you could) (could you) (you in) (in a)
(a house?) (house? Would)
We could generate a new sentence by randomly picking pairs of words, but we’d likely generate something nonsensical. Instead of picking pairs at random, we can arrange words in a weighted Markov chain. That way, we have a higher probability of generating content that makes sense and isn’t gibberish. The Markov chain might look something like this:

Would you → could (66%), like (33%)
you could → you (100%)
could you → in (50%), with (50%)
you in → a (100%)
you with → a (100%)
in a → house? (50%), box? (50%)
with a → fox? (50%), mouse? (50%)
you like → them (50%), green (50%)
To generate a new sentence, start with a random pair of words:

could you
Then, use the Markov chain to choose the next word. After could you, there’s a 50% chance that in is the next word and a 50% chance that with is the next word. Let’s choose in:

could you +in
Finally, we have a new pair of words we can use to repeat the process:

could you in +a
could you in a +house?
…and so on and so forth!

## Modeling a Markov Chain in Python
If only there were some data structure we could use to contain all of these chains, which map word pairs to some list of values. Okay, the preceding sentence was pretty corny — we’ll store the data in a dictionary (they’re also known as hashmaps) where the keys are pairs of words and the values are lists.

chains = {('could', 'you'): ['in', 'in', 'with', 'with'],
          ('you', 'in'): ['a', 'a'],
          # ...
          }
Why are dictionaries, tuples, and lists good data structures to use here?

Write a Text Generator
You are to produce a random text generator using Markov chains. We’ve provided a very bare skeleton of markov.py for you to start your program from.

Open the file in your editor and skim it to get a rough sense of its structure:

First we import modules

Then, we define some functions

…which get called at the bottom of the file

The functions are called in a very specific order. It’s important to think about how we capture data from one function and pass it to the next function.

When you’re finished writing markov.py and run it, it should produce output like this:

### python3 markov.py
box? Would you could you with a mouse? Would you could you
in a house? Would you could you in a house? Would you
could you in a house? Would you could you with a fox?
Would you like green eggs and ham? Would you like them, Sam
I am?

The output will vary, as there is randomness involved, but if you’ve done it right, you’ll always end with “Sam I am?” given the input green-eggs.txt.

## Read the File
Within the skeleton provided, finish the function that opens the file_path for our green-eggs.txt file and spits out its contents as a string.

## Read the Entire File

green-eggs.txt has significant line breaks, so you might be tempted to process the file line-by-line. The problem with that is that it becomes very difficult to have one continuous Markov chain that goes through our entire text. We would end up with lots of disconnected chains for each line. Instead, we want to treat the entire file as one long string, rather than reading it line-by-line.

You can do this by calling the read method on a file object. If you need a hint on how to this, check the hint box below.

How to use file.read

## Make a Markov Chain Dictionary
To be able to construct our dictionary, we need to break up our input string somehow. Once we have it broken up sensibly, we can loop over each piece of our input text (sometimes called a corpus).

In the earlier step, you should be reading the file as a single string, so we’ll use string.split() to separate it by whitespace.

words = contents.split()
Note that we use the argument-less version of .split(), so it breaks on any kind of whitespace, including linebreaks, spaces, and tabs.

As we loop over each piece, we need to keep track of every pair of words which appear next to each other in our text. How can we look at more than one word at a time? Our for item in list loop only looks at one item in a list at a time. Maybe we can get more custom functionality if we loop over the list by index, rather than item directly. Remember how to do this?

## How to loop by index

So now we can print out each word pair. But we also need to keep track of every word that follows each pair of words, wherever the pair appears in the text.

How can we loop over the words in our string, and turn them into the dictionary structure defined above? Think about it, pseudo code it, test as you go, and if you get stuck, review the lecture or check the hint for a walkthrough.

## Pseudocode

Your chains dictionary should look similar to this if you print it:

{('a', 'fox?'): ['Would'],
 ('Sam', 'I'): ['am?'],
 ('could', 'you'): ['in', 'with', 'in', 'with'],
 ('you', 'with'): ['a', 'a'],
 ('box?', 'Would'): ['you'],
 ('ham?', 'Would'): ['you'],
 ('you', 'in'): ['a', 'a'],
 ('a', 'house?'): ['Would'],
 ('like', 'green'): ['eggs'],
 ('like', 'them,'): ['Sam'],
 ('and', 'ham?'): ['Would'],
 ('Would', 'you'): ['could', 'could', 'could', 'could', 'like', 'like'],
 ('you', 'could'): ['you', 'you', 'you', 'you'],
 ('a', 'mouse?'): ['Would'],
 ('them,', 'Sam'): ['I'],
 ('in', 'a'): ['house?', 'box?'],
 ('with', 'a'): ['mouse?', 'fox?'],
 ('house?', 'Would'): ['you'],
 ('a', 'box?'): ['Would'],
 ('green', 'eggs'): ['and'],
 ('you', 'like'): ['green', 'them,'],
 ('mouse?', 'Would'): ['you'],
 ('fox?', 'Would'): ['you'],
 ('eggs', 'and'): ['ham?']
}
Once you think you have your Markov chain dictionary in a usable format,

## Make a Random Text from the Dictionary
So you’ve created a valid dictionary, and it’s getting passed into the function that will create our fake text.

To make a chain of fake text, we need to start with a link. A link in our case is a key from our dictionary and a random word from the list of words that follow it. Put that link in some kind of container (the skeleton file suggests adding each word to a list and joining the list into a string at the end). Once we have our first link, we can add another to it, and we can repeat the following process to add more:

Make a new key out of the second word in the first key and the random word you pulled out from the list of words that followed it.

Look up that new key in the dictionary, and pull a new random word out of the new list.

Keep doing that until your program raises a KeyError.

Well shoot, the KeyError broke our program. How about we keep trying to look for the existence of each key before trying to make a link, and then stop once the newest key is no longer in the dictionary?

Return a string of the words you’ve pulled out. Remember, when using the supplied green_eggs.txt file, if the text generation is working, it will always end with the words “Sam I am?”

Once you’ve got your random text generator working to a somewhat sensible point,

This is a multi-lab exercise, go deep!

## Different Text
We absolutely recommend you use the supplied green-eggs.txt while testing your program, because less text makes it easier to examine your data structures to see if they are working. Later however, it’s more fun to try out the program on a longer, more interesting corpus. We’ve also included gettysburg.txt for the Gettsyburg Address, or, if you’d prefer, you might try Project Gutenberg for some inspiration for your own ideas.

### Congratulations on completing Markov Chains.