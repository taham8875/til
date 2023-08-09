# Technical Writing Course by Google

This cheat sheet is a quick reference for the [technical writing course by Google](https://developers.google.com/tech-writing).

# Chapter 1: Just Enough Grammar

| Grammar      | Description                                                            | Example                           |
| ------------ | ---------------------------------------------------------------------- | --------------------------------- |
| Noun         | A person, place, thing, or idea                                        | `cat`, `desk`, `New York`, `love` |
| Pronoun      | A word that replaces a noun                                            | `he`, `she`, `it`, `they`         |
| Adjective    | A word that describes a noun                                           | `red`, `big`, `tasty`             |
| Verb         | An action word                                                         | `run`, `eat`, `write`             |
| Adverb       | A word that describes a verb                                           | `quickly`, `loudly`, `well`       |
| Preposition  | A word that describes a relationship between other words in a sentence | `in`, `on`, `at`, `to`            |
| Conjunction  | A word that connects words, phrases, or clauses                        | `and`, `but`, `or`                |
| Interjection | A word that expresses emotion                                          | `wow`, `ouch`, `yikes`            |
| Transition   | A word that connects ideas                                             | `first`, `next`, `finally`        |

**Examples:**

- Noun: The `cat` is on the `desk`.
- Pronoun: `It` is on the desk.
- Adjective: The `red` cat is on the `big` desk.
- Verb: The cat `runs` on the desk.
- Adverb: The cat runs `quickly` on the desk.
- Preposition: The cat runs quickly `on` the desk.
- Conjunction: The cat runs quickly on the desk `and` on the ground, `but` it is not tasty.
- Interjection: `Wow`, the cat runs quickly on the desk and on the ground, but it is not tasty.
- Transition: `First`, the cat runs quickly on the desk and on the ground, but it is not tasty. `Next`, the cat sleeps on the desk and on the ground, and it is still not tasty. `Finally`, the cat eats the desk and the ground, and it is tasty. `However`, the cat is now sleeping.

# Chapter 2: Words

## Define new or unfamiliar terms

If a word might be unfamiliar to your reader, define it, use one of the following methods:

- If the term already exists, link to the definition (e.g. from Wikipedia) and don't reinvent the wheel.
- If the term is new (your documentation introducing it), define it in the text.

## Use terms consistently

If you change the name of a variable midway through a method, your code won’t compile. Similarly, if you rename a term in the middle of a document, your ideas won’t compile (in your users’ heads).

## Use acronyms properly

On the initial use of an unfamiliar acronym within a document or a section, spell out the full term, and then put the acronym in parentheses. Put both the spelled-out version and the acronym in boldface. For example:

> The **application programming interface (API)** is a set of routines that an application uses to request and carry out lower-level services performed by a computer’s operating system. To use an API, you make a request in a predefined format.

Do not cycle back-and-forth between the acronym and the expanded version in the same document.

### Use the acronym or the full term?

- If the acronym is more familiar than the full term, use the acronym.
- If the full term is more familiar than the acronym, use the full term.

Although acronyms are often shorter than the full term, they can be harder to read and take longer time for the reader to process. Unless they are very familiar like `API` or `HTML`, use the full term.

## Recognize ambiguous pronouns

Many pronouns point to a previously introduced noun. Such pronouns are analogous to pointers in programming. Like pointers in programming, pronouns tend to introduce errors. Using pronouns improperly causes the cognitive equivalent of a null pointer error in your readers’ heads. In many cases, you should simply avoid the pronoun and just reuse the noun. However, the utility of a pronoun sometimes outweighs its risk (as in this sentence).

Consider the following guidelines for using pronouns:

- Use a pronoun only after you have used the noun it refers to. Not before.
- Place the pronoun as close as possible to the noun it refers to. (Not more than five words away.)
- If a new noun is introduced between the pronoun and the noun it refers to, repeat the noun.

### It and they

These cause the most confusion in technical documentation:

For example, consider the following sentence:

> Python is interpreted, while C++ is compiled. It has an almost cult-like following.

> Be careful when using Frambus or Carambola with HoobyScooby or BoiseFram because a bug in their core may cause accidental mass unfriending.

### This and that

These are less confusing than it and they, but they can still cause problems. For example:

> Running the process configures permissions and generates a user ID. This lets users authenticate to the app.

**This** could refer to the user ID, to running the process, or to all of these.

To help your readers, avoid using these confusing pronouns. Instead, use the noun that the pronoun refers to.

# Chapter 3: Active voice vs. passive voice

In an active sentence, the subject is the doer of the action. In a passive sentence, the subject is the receiver of the action.

> Active voice sentence: The cat ate the mouse.
> Passive voice sentence: The mouse was eaten by the cat.

> Note that some passive voice sentences don’t include the doer of the action. For example: The mouse was eaten. In this case, the doer of the action is unknown or unimportant.

## Prefer active voice over passive voice

Active voice is usually clearer and more concise than passive voice. Use it whenever possible.

# Chapter 4: Clear sentences

- Choose strong verbs over weak verbs. (e.g. The system `generates` this error message when instead of This error message `happens` when )
- Reduce `there is` / `there are`
- Minimize certain adjectives and adverbs (optional) Adjectives and adverbs perform amazingly well in fiction and poetry. Not in technical documentation.

So instead of writing:

> Using Rust makes the application run blazingly fast.

Write:

> Using Rust makes the application run 225-250% fast.

**Note**: Don't confuse educating your readers (technical writing) with publicizing or selling a product (marketing writing). When your readers expect education, provide education; don't intersperse publicity or sales material inside educational material.

# Chpaters 5: Short sentences

- Shorter documentation reads faster than longer documentation.
- Shorter documentation is typically easier to maintain than longer documentation.
- Extra lines of documentation introduce additional points of failure.

## Focus each sentence on a single idea

Bad example:

> The late 1950s was a key era for programming languages because IBM introduced Fortran in 1957 and John McCarthy introduced Lisp the following year, which gave programmers both an iterative way of solving problems and a recursive way.

Good example:

> The late 1950s was a key era for programming languages. IBM introduced Fortran in 1957. John McCarthy introduced Lisp the following year. Consequently, by the late 1950s, programmers could solve problems iteratively or recursively.


# Convert some long sentences to lists

For example, consider the following sentence:

> To get started with the Frambus app, you must first find the app at a suitable store, pay for it using a valid credit or debit card, download it, configure it by assigning a value for the `Foo` variable in the `/etc/Frambus` file, and then run it by saying the magic word twice.

You can convert this sentence to a list:

> To get started with the Frambus app, you must:
> - Find the app at a suitable store.
> - Pay for the app using a valid credit or debit card.
> - Download the app.
> - Configure the app by assigning a value for the `Foo` variable in the `/etc/Frambus` file.
> - Run the app by saying the magic word twice.


## Eliminate or reduce extraneous words

i.e. Remove words that don't add meaning to the sentence.

For example, consider the following sentence:

> An input value greater than 100 causes the triggering of logging.


You can refactor this sentence to:

> An input value greater than 100 triggers logging.


Another example:

> This design document provides a detailed description of Project Frambus.

Can be refactored to:

> This design document describes Project Frambus.

The following table suggests replacements for a few common bloated phrases:

| Wordy | Concise |
| --- | --- |
| at this point of time | now |
| determine the location of | find |
| is able to | can |
| is going to | will |

## Reduce subordinate clauses 

Keep it `one sentence = one idea`.

Examples :

> Python is an interpreted language, *which means that the language can execute source code directly.*


> Bash is a modern shell scripting language *that takes many of its features from KornShell 88*, *which was developed at Bell Labs.*

## Distinguish that from which

n some countries, the two words are pretty much interchangeable. But in USA `which` is for nonessential subordinate clauses, and `that` is for an essential subordinate clause that the sentence can't live without.

For example :

> Python is an interpreted language, `which` Guido van Rossum invented.

> Fortran is perfect for mathematical calculations `that` don't involve linear algebra.

# Chapter 6: Lists and tables


Good lists can transform technical chaos into something orderly. Technical readers generally love lists. Therefore, when writing, seek opportunities to convert prose into lists.

## Choose the correct list type

- Bullet lists (for unordered lists)
- Numbered lists (for ordered lists)
- Embedded lists (for inline lists) (e.g. `The following are the most popular programming languages: Python, C++, and Rust.`)

Generally speaking, the embedded lists are poor style. They are hard to read and hard to maintain. Instead, use a numbered list.

Exercise : Convert the following into one or more lists:

> Today at work, I have to code three unit tests, write a design document, and review Janet's latest document. After work, I have to wash my car without using any water and then dry it without using any towels.

Answer :

> Today at work, I have to:
> - Code three unit tests.
> - Write a design document.
> - Review Janet's latest document.
>
> After work, I have to:
> 1. Wash my car without using any water.
> 2. Dry my car without using any towels.


## Keep list items parallel

What separates effective lists from defective lists? Effective lists are parallel; defective lists tend to be nonparallel. All items in a parallel list look like they "belong" together. That is, all items in a parallel list match along the following parameters:

- grammar
- logical category
- capitalization
- punctuation


For example, the following list is parallel:

- carrots
- potatoes
- cucumbers


The following list is nonparallel:

- carrots
- potatoes
- The summer light obscures all memories of winter.


Exercise : Is the following list parallel? If not, how would you make it parallel?

- The red dots represent sick trees.
- Immature trees are represented by the blue dots.
- The green dots represent healthy trees.


Answer :

The list is nonparallel. The first and third items are in active voice, but the second item is in passive voice. To make the list parallel, you could rewrite the second item as follows:

- The blue dots represent immature trees.


## Start the numbered list with imperative verbs

An imperative verb is a command, such as open or start.

For example, the following numbered list starts with an imperative verb:

1. Download the Frambus app.
2. Configure the Frambus app.
3. Run the Frambus app.

## Punctuate list items correctly

If the list items are complete sentences, use sentence-style capitalization and punctuation.

For example, the following list items are complete sentences:

- The Frambus app is available for download.

Otherwise, don't use capitalization or punctuation for non complete sentences.

For example, the following list items are not complete sentences:

- the color of lemons

## Create useful tables

Consider the following guidelines when creating tables:

- Label each column with a meaningful header. Don't make readers guess what each column holds.
- Avoid putting too much text into a table cell. If a table cell holds more than two sentences, ask yourself whether that information belongs in some other format.
- Although different columns can hold different types of data, strive for parallelism within individual columns. For example, the cells within a column should all be nouns or all be numbers.

## Introduce each list and table

Introduce each list and table with a short paragraph that explains the purpose of the list or table. (Give the reader the context).

Although not a requirement, we recommend putting the word **following** into the introductory sentence. For example :

> The following table lists the most popular programming languages.

| Language | Number of users |
| --- | --- |
| Python | 8 million |
| C++ | 6 million |
| Perl | 1 million |

