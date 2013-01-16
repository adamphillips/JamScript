JamScript
=========

What is JamScript?
------------------

JamScript is a simple markup language for musicians to use to describe songs quickly and easily.  It is designed to be both human and machine readable.  The easiest way to think of it is [Real Book](http://www.amazon.com/Real-Book-Hal-Leonard-Corporation/dp/0634060384) meets [Markdown](http://daringfireball.net/projects/markdown/).

In the same way that a markdown document should be publishable as-is, it should be possible to read and use a JamScript document without any additional software.  The symbols used in JamScript should be familiar to musicians.

It is also designed to be as concise as possible aiming to give musicians a high-level overview of a song rather than a detailed note-by-note score.

For example

    An Example Jam
    by Adam Phillips
    tempo 120bpm

    # Chords
    * Verse
    C F | G G :x3
    - extended
    G | F G

    * Chorus
    F | C | F G | C

    # Beats
    * Verse
    k . s . | {k k} . s {. s}

    ...

    # Structure
    Verse, Verse, Chorus
    Verse (keys solo), Verse - extended, Chorus, Chorus

This would be a valid JamScript document yet straight away most musicians would have a reasonable idea of the structure of the piece and how it should sound (not great in this case but hey, it's just an example!)

The complete spec is available at [http://github.com/adamphillips/JamScript/blob/master/spec.md](http://github.com/adamphillips/JamScript/blob/master/spec.md).

What's the point?
-----------------

Most musicians have their own shorthand for describing pieces like this, so you may wonder what the point is of using JamScript instead.

Well firstly, JamScript goes beyond most musician's shorthand by defining ways of specifying specific chord voicings, complex timings and so on.  Have a defined spec means that when you come back to your notes in 6 months time, you will be able to look up any terms you have forgotten in the mean time.  It also means that anyone that you share your notes with can use the JamScript spec to interpret them in the same way.

Then there is the machine-readable aspect.  We are working hard building tools that developers can use to read and write JamScript documents in their applications along with tools for converting JamScript documents to other common formats such as HTML (for publishing on the web) and PDF (for printing).

So what can I do with it now?
-----------------------------

Well it's very early days at the moment.  The first tool that will be available will be a Ruby parser that will allow Ruby application developers to read JamScript documents after which we will be creating a number of convertors.  All of this is probably a few months away however and it is probable that the first versions will not support the entire spec.

However the nature of JamScript is such that you can start using it straight away and the very least you will get is some tidy, readable, share-able notes which is a pretty good start!  You can also help by giving us feedback on the language.  If you have sufficient geek factor, you can even clone this repository and send us pull requests with your proposed amends to the spec so that they can be discussed.

Alternatives
------------

So far as we know, there aren't any real alternatives to JamScript - that's why we're making it!  There is [Lilypond](http://www.lilypond.org/) which provides a plain-text way of specifying complex scores.  Whilst it is pretty awesome, it is not really designed for the same job as JamScript - it is not human-readable and is doesn't support a high level overview of a song in the same way that JamScript does.  We don't see the two as incompatible however and at some point we may even consider adding a way of using Lilypond syntax inside a JamScript for specifying particularly complex parts in traditional notation.


Contributing
------------

If you want to get involved just send a message or clone the repository in Github, make your proposed changes and then send us a pull request.
