# Chapter 4 - Comments

*Don't comment bad code - rewrite it.*

Comments are not "pure good". The proper use of comments is to compensate for our failure to express ourselves in code. Note that I used the word "failure". I meant it. Comments are always failures. We must have them because we cannot always figure out how to express ourselves without them, but their use is not a cause for celebration.

When you are in the position where you need to write a comments, think if there is a way to turn the tables and express your self on code.

The problem with comments that they lie too often. They get out of date. They were correct when they were written, but the code has been changed, and the comments did not change with it. Comments are not executed by the compiler, unlike the code, comments doesn't have bug or tests or linter checks.

```java
// Checks if the password length is greater than 10 
if (password.length() > 8) {
    return true;
}
```

Inaccurate comments are worse than no comments at all. They are worse than no comments because they are misleading. They lead you to believe a lie and never be able to meet an expectation.

Truth can only be found in one place, code, only the code can truly tell you what it does.

## Comments Do Not Make Up for Bad Code

One of the more common motivations of writing comments is bad code. We write a module and we know it is not clear. We know it is not well written. So we say to ourselves, "Ooh, I'd better comment that." No! You'd better clean it!

## Explain Yourself in Code

Instead of writing comments, write code that explains itself.

```java
// Check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65))
```

This is much better:

```java
if (employee.isEligibleForFullBenefits())

boolean isEligibleForFullBenefits() {
    return (flags & HOURLY_FLAG) && (age > 65);
}
```

## Good Comments

Sometimes you need to write comments. Here are some examples of good comments:

### Legal Comments

Sometimes you need to comment the legal information about the code. For example, the license information.

```java
// Copyright (C) 2003,2004,2005 by Object Mentor, Inc. All rights reserved.
// Released under the terms of the GNU General Public License version 2 or later.
```

### Informative Comments

It is sometimes useful to provide a basic information with a comment, for example, the purpose of a function or a variable.

```java
// Returns an instance of the Responder being tested.
protected abstract Responder responderInstance();
```

But a better way is to use the name of the function or variable to express the same information.

```java
protected abstract Responder responderBeingTested();
```

Here is another example:

```java
// format matched kk:mm:ss EEE, MMM dd, yyyy
Pattern timeMatcher = Pattern.compile("\\d*:\\d*:\\d* \\w*, \\w* \\d*, \\d*");
```

The comments let's us know that the regular expression is supposed to match a time and date that is being formatted. But note if the code is moved to a special class that converted the formats of dates and times, the comment would be unnecessary.

### Explanation of Intent

Sometimes it is useful to provide the intent behind a decision. You know that how the author of the code was thinking and why he made the decision.

### Clarification

Sometimes it is just helpful to translate the meaning of some obscure argument or return value into something that’s readable. In general it is better to find a way to make that argument or return value clear in its own right; but when its part of the standard library, or in code that you cannot alter, then a helpful clarifying comment can be useful.

```java
public void testCompareTo() throws Exception
{
    WikiPagePath a = PathParser.parse("PageA");
    WikiPagePath ab = PathParser.parse("PageA.PageB");
    WikiPagePath b = PathParser.parse("PageB");
    WikiPagePath aa = PathParser.parse("PageA.PageA");
    WikiPagePath bb = PathParser.parse("PageB.PageB");
    WikiPagePath ba = PathParser.parse("PageB.PageA");
    assertTrue(a.compareTo(a) == 0); // a == a
    assertTrue(a.compareTo(b) != 0); // a != b
    assertTrue(ab.compareTo(ab) == 0); // ab == ab
    assertTrue(a.compareTo(b) == -1); // a < b
    assertTrue(aa.compareTo(ab) == -1); // aa < ab
    assertTrue(ba.compareTo(bb) == -1); // ba < bb
    assertTrue(b.compareTo(a) == 1); // b > a
    assertTrue(ab.compareTo(aa) == 1); // ab > aa
    assertTrue(bb.compareTo(ba) == 1); // bb > ba
}
```

But there are a risk of the clarifying comment is incorrect, go through the previous example and see how difficult it is to verify if some of the comments are correct.

So before writings a comments like this, make sure that there are no better ways. if no consider taking care into making these comments accurate.

### Warning of Consequences

Sometimes it is useful to warn other programmers about certain consequences. For example:

```java
public static SimpleDateFormat makeStandardHttpDateFormat() {
    // SimpleDateFormat is not thread safe, 
    // so we need to create each instance independently.
    SimpleDateFormat df = new SimpleDateFormat("EEE, dd MMM yyyy HH:mm:ss z");
    df.setTimeZone(TimeZone.getTimeZone("GMT"));
    return df;
}
```

It helps prevent other programmers from making mistakes.

### TODO Comments

It is sometimes useful to leave "to do" notes in the code. This can be done by using `TODO` comments.

```java
// TODO: Need to make sure that the name is set
```

`TODO`s are jobs that the programmer thinks should be done, but for some reason can't do at the moment. It might be a reminder to delete a deprecated feature, a reminder to finish an implementation or to look for a problem or potential optimization.

Nowadays, most IDEs have a feature to list all the `TODO`s in the project. Still, you don't want your code to be littered with `TODO`s. They are like broken windows that will lead to more broken windows. Try whenever possible to scan them regularly and remove eliminate the ones you can.

### Amplification

A comment amy be used to amplify the importance of something that may otherwise seem inconsequential.

```java
String listItemContent = match.group(3).trim();
// the trim is real important. It removes the starting 
// spaces that could cause the item to be recognized
// as another list.
new ListItemWidget(this, listItemContent, this.level + 1);
return buildList(text.substring(match.end()));
```

### Javadocs in Public APIs

If you are writing a public API, then you should certainly write good javadocs or code documentation. Javadocs are a great way to communicate the intent of an API. but keep in mind the rest of advice discussed here, javadocs can also be misleading and dishonest as any other comments.

## Bad Comments

Most comments fall into this category. Usually they are crutches or excuses for poor code or justifications for insufficient decisions. They are not to be taken lightly.

### Redundant Comments

The following code contains a comment that is completely redundant with the code.

```java
// Utility method that returns when this.closed is true. Throws an exception
// if the timeout is reached.
public synchronized void waitForClose(final long timeoutMillis)
    throws Exception {
    if (!closed) {
        wait(timeoutMillis);
        if (!closed)
            throw new Exception("MockResponseSender could not be closed");
    }
}
```

What is the purpose does this comment serve? It is certainly not more informative than the code. Actually, reading the actual code is more informative and easier than reading the comment.

### Misleading Comments

The following comment is misleading:

```java
// Check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65))
```

Sometimes, the author itself mistakenly writes a comment that is misleading or not representing the real behavior of the code. And the poor programmer spending some debugging time to figure out why the code is not working as expected.

### Mandated Comments

It is just plain silly to have a rule that says that every function or variable in the code must have javadoc, comments like this just clutter up the code, propagate lies, and lend to general confusion and disorganization.

For example, this what happens when you require java doc comments for every function:

```java
/**
 * 
 * @param title The title of the CD
 * @param author The author of the CD
 * @param tracks The number of tracks on the CD
 * @param durationInMinutes The duration of the CD in minutes
 */
    public void addCD(String title, String author, 
    int tracks, int durationInMinutes) {
    CD cd = new CD();
    cd.title = title;
    cd.author = author;
    cd.tracks = tracks;
    cd.duration = duration;
    cdList.add(cd);
}
```

### Journal Comments

Sometimes people add a comment to the start of a module every time they edit it. These comments accumulate as a kind of journal, or log, of every change that has ever been made.

```java
* Changes (from 11-Oct-2001)
 * --------------------------
 * 11-Oct-2001 : Re-organised the class and moved it to new package 
 * com.jrefinery.date (DG);
 * 05-Nov-2001 : Added a getDescription() method, and eliminated NotableDate 
 * class (DG);
 * 12-Nov-2001 : IBD requires setDescription() method, now that NotableDate 
 * class is gone (DG); Changed getPreviousDayOfWeek(), 
 * getFollowingDayOfWeek() and getNearestDayOfWeek() to correct 
 * bugs (DG);
 * 05-Dec-2001 : Fixed bug in SpreadsheetDate class (DG);
 * 29-May-2002 : Moved the month constants into a separate interface 
 * (MonthConstants) (DG);
 * 27-Aug-2002 : Fixed bug in addMonths() method, thanks to N???levka Petr (DG);
 * 03-Oct-2002 : Fixed errors reported by Checkstyle (DG);
 * 13-Mar-2003 : Implemented Serializable (DG);
 * 29-May-2003 : Fixed bug in addMonths method (DG);
 * 04-Sep-2003 : Implemented Comparable. Updated the isInRange javadocs (DG);
 * 05-Jan-2005 : Fixed bug in addYears() method (1096282) (DG);
```

Long ago there was a good reason to add these before the existence of source control systems. But now, this is unnecessary and just adds noise to the code.

### Noise Comments

Sometimes you see comments that are just noise. They restate the obvious and provide no new information.

```java
i++; // increment i
```

```java
/**
 * Default constructor.
 */
protected AnnualDateRule() {
}
```

```java
/** The day of the month */
private int dayOfMonth;
```

### Scary Noise

```java
/** The name */
private String name;
```

```java
/** The version */
private String version;
```

```java
/** The licenseName */
private String licenseName;
```

### Don't Use a Comment When You Can Use a Function or a Variable

Consider this code:

```java
// does the module from the global list <mod> depend on the
// subsystem we are part of?
if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem()))
```

Can be rewritten as:

```java
ArrayList moduleDependees = smodule.getDependSubsystems();
String ourSubSystem = subSysMod.getSubSystem();
if (moduleDependees.contains(ourSubSystem))
```

### Position Markers

Sometimes you see comments that are used as markers. They are used to mark a particular position in the code.

```java
// Actions //////////////////////////////////
```

### Closing Brace Comments

Sometimes programmers will put special comments on closing braces. Although this might make sense for long functions with deeply nested structures, it serves only to clutter the kind of small and encapsulated functions that we prefer. So if you find yourself wanting to mark your closing braces, try to shorten your functions instead.

```java
public void testCompareTo() throws Exception
    try {
        while ((line = in.readLine()) != null) {
            lineCount++;
            charCount += line.length();
            String words[] = line.split("\\W");
            wordCount += words.length;
        } //while
    } //try
    catch (IOException e) {
        reportError("IOException while reading from file.");
    } //catch
}//testCompareTo
```

### Attributions and Bylines

Sometimes you see comments that attribute the author of the code. This is a bad practice. The information should be in the source control system.

```java
/* Added by Rick */
```

### Commented-Out Code

Just don't do it.

```java
InputStreamResponse response = new InputStreamResponse();
response.setBody(formatter.getResultStream(), formatter.getByteCount());
// InputStream resultsStream = formatter.getResultStream();
// StreamReader reader = new StreamReader(resultsStream);
// response.setContent(reader.read(formatter.getByteCount()));
```

Others who see that commented-out code won’t have the courage to delete it. They’ll think it is there for a reason and is too important to delete. So commented-out code gathers like dregs at the bottom of a bad bottle of wine.

There was a time, back in the sixties, when commenting-out code might have been useful. But we’ve had good source code control systems for a very long time now. Those systems will remember the code for us. We don’t have to comment it out any more. Just delete the code. We won’t lose it. Promise.

### Nonlocal Information

Sometimes you see comments that have nothing to do with the code at hand, but rather give some general information about the system or far code away from this code.

```java
/**
 * Port on which fitnesse would run. Default is 8082
 * 
 * @param fiteessePort
 */

public void setFitnessePort(int fiteessePort) {
    this.fitnessePort = fiteessePort;
}
```

Besides being a very redundant comment, there is no guarantee that when the port will be changed, the comment will be updated.

## Too Much Information

Don’t put interesting historical discussions or irrelevant descriptions of details into your comments. The comment below was extracted from a module designed to test that a function could encode and decode base64. Other than the RFC number, someone reading this code has no need for the arcane information contained in the comment.

```java
/*
 RFC 2045 - Multipurpose Internet Mail Extensions (MIME) 
 Part One: Format of Internet Message Bodies
 section 6.8. Base64 Content-Transfer-Encoding
 The encoding process represents 24-bit groups of input bits as output 
 strings of 4 encoded characters. Proceeding from left to right, a 
 24-bit input group is formed by concatenating 3 8-bit input groups. 
 These 24 bits are then treated as 4 concatenated 6-bit groups, each 
 of which is translated into a single digit in the base64 alphabet. 
 When encoding a bit stream via the base64 encoding, the bit stream 
 must be presumed to be ordered with the most-significant-bit first. 
 That is, the first bit in the stream will be the high-order bit in 
 the first 8-bit byte, and the eighth bit will be the low-order bit in 
 the first 8-bit byte, and so on.
 */
```

## Inobvious Connection

The connection between a comment and the code it describes should be obvious. If you will really write a comment, at least make sure that the reader will understand the connection between the comments and the code.

Consider, for example, this comment drawn from apache commons:

```java
 /*
  * start with an array that is big enough to hold all the pixels
  * (plus filter bytes), and an extra 200 bytes for header info
 */
 this.pngBytes = new byte[((this.width + 1) * this.height * 3) + 200];
```

What is a filter byte? Does it relate to the +1? Or to the*3? Both? Is a pixel a byte? Why 200? The purpose of a comment is to explain code that does not explain itself. It is a pity when a comment needs its own explanation.

### Function Headers

Short functions don't need much description. A well-chosen name for a small function that does one thing is usually better than a comment header. So don't write a comment header for every function.

### JavaDoc in Nonpublic Code

javadocs are useful for public APIs. But for nonpublic code, they are just noise and not useful. They are just a distraction.

### Appendix

Watch the following youtube video: [Don't Write Comments](https://youtu.be/Bf7vDBBOBUA)
