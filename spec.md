JamScript Spec
==============
Version 0.1

For more information on JamScript please see

[https://github.com/adamphillips/JamScript](https://github.com/adamphillips/JamScript)


Control Characters
------------------

\# : command. Used to switch between modes.

    # Chords (default)
    # Structure
    # Lyrics

\* : define a new section or part

    * Verse
    * Chorus

\- : defines a variation within the current set of chords

    * Verse
    ...
    - short
    ...
    - long
    ...

\! - notes (textual not musical!) to be included with the current set of chords

    * Verse
    ! Stop on the 4 before each chorus

Chords
------

### Defining sections

A section is prefixed by a * followed by a space.  The text after the * is the name of the section.

    * Verse
    * Chorus
    * Bridge

It can have variations:

    * Verse
    - 1st time
    - 2nd time

### Adding chords to a section

Each section consists of one or more bars which in turn contain one or more chords. Bars are separated by the | character.  The | at the beginning and end of the line is optional

    * Verse
    C F | G G7

This defines a 2 bar verse where the chords C, F, G and G7 are to be played for 1/2 a bar each.  Since by default, songs are in 4/4 this would be 2 beats per chord.

### Variations

Sections can also have variations.  These are declared in a similar manner to sections but prefixed with a - . 
For example

    * Verse
    C F | G G7
    - normal
    C F | C
    - fancy
    C F | G C

is the same as

    * Verse - normal
    C F | G G7
    C F | C

    * Verse - fancy
    C F | G G7
    C F | G C

Sections can also have notes.  Notes are prefixed with a !

    * Bridge
    ! Bass drops out until bar 2 but then comes back in heavy
    C F | G G7

As can variations

    * Verse
    C F | G G7
    - normal
    ! when there's vocals
    C F | C
    - fancy
    ! when it's a solo
    C F | G C

### Repeats

Each line is considered to be a subsection of the current section.  This is particularly important when working with repeats.  To specify that a section should repeat, add a colon to the end of the line

    C F | G G7 :

is the same as

    C F | G G7 | C F | G G7

which is the same as

    C F | G G7
    C F | G G7

You can repeat multiple times. For example

    C F | G G7 :x3
    C F | C

is the same as

    C F | G G7
    C F | G G7
    C F | G G7
    C F | C

You can also have 1st and 2nd time bars

    C F |1 G G7 :|2 C

is the same as

    C F | G G7
    C F | C

### Voicings

There are times when the voicing of chords is important or where the name of a chord becomes a bit convoluted. In these cases you can specify exactly which intervals should be played by specifying in paretheses the additional notes to be played. Some examples should make things clearer:

    D7      = D(7)        = d f# a c
    Cmin7   = Cmin(b7)    = c eb g bb
    Fmaj7   = F(7)        = f a c e
    Fmin13  = Fmin(b7 13) = f ab c eb bb

You can remove notes by prefixing them with a *. Alternatively, you can specify the exact intervals to be played by using square brackets.  In either case, JamScript will also allow for lazy note numbering by assuming that intervals are ordered once an interval has been specified, subsequent intervals will by be higher in pitch.  Some more examples:

    G(*5 7)                     = g b f
    G9add13 = G(9 13) = G(9 6)  = g b d a e
    G(*3)   = G[1, 5]           = g d

Usually the root note is assumed from the tonic of the chord. Otherwise, the root note can be specified using / notation. In this case the tonic is assumed not to be present.  If you want to add it back in use (1) or [1 ...]

    D             = d f# a
    D(*3 4)       = d g a
    D(*3 4)/E     = e g a
    D(1 * 3 4)/E  = e d f# a

Timing
------

### Note lengths

Sometimes it is not enough to know which chords are to be played, it is important to know how long each chord is to be played for.  There are 2 ways of specifying timing in JamScript that can be used independently or combined as necessary.

By default chords are evenly spaced within a bar. So, assuming we are in 4/4

    C F | G G7

will be interpreted as 2 beats per chord. We can use a . followed by a number of beats to assign a specific length to a chord. For example

    C F | G.3 G7.1

would mean play the G chord for 3 beats and the G7 for 1 beat. Where possible, JamScript will try and intelligently guess unspecified chord lengths.  For example

    C F | G.3 G7.1

can also be written as

    C F | G.3 G7

  or

    C F | G G7.1

Lengths can either be whole numbers or fractions of a beat.

    C.2 F.1 G.1/2 G7.1/2

If all the lengths are specified they must be equal to the number of beats specified in the time signature. (4 for 4/4, 3 for 3/4, 6 for 6/8 etc). If some chords don't have lengths specified, there must be beats available in the bar. For example

    C.2 F.1 G.1 G7
    C.2 F.1 G.1 G7.1

would cause an error.


The other way to divide the beats in a bar is to use { } to define groups of beats.  These are then evenly spaced within the bar.  For example

    C.2 F.1 G.1 | Am.2 F.1 G.1

can also be written as

    C {F G} | Am {F G}

Lengths specified with a . still work in the same way and can be applied to a group of notes

    C {F G}.1 | Am.3 {F G}

is the same as

    C.3 F.1/2 G.1/2 | Am.3 F.1/2 G.1/2

You can also use brackets to do things like triplets

    C.3 {F C G} | C {F G F G}

would produce tripleted quavers in the first bar and semi-quavers in the second.

It is also possible to specify a section within a section
For example in

    * Intro
    C C# D D#

    * Verse
    E | E | A | C C# D D#

the Verse could also be written as

    E | E | A | Intro

Structures
----------

In order to specify the song's structure start by typing

    # Structure

You can then list the different sections in the order they appear using the names you gave them. List them either one per line or separated by commas.

    * Verse
      ...

    * Chorus
      ...

    # Structure
    Verse, Verse, Chorus
    Verse, Verse, Chorus, Chorus

If a section has variations, you can specify them using a -

    Verse - long, Chorus
    Verse - long, Chorus
    Verse - short, Chorus, Chorus

You can also use x when repeating sections.  For example the last line of the above example could also be written as

    Verse - short, Chorus x 2

You can use ( ) to add comments or notes

    Verse - long, Chorus
    Verse - short (keys solo), Chorus (sax solo)
    Verse - short, Chorus, Chorus

Lyrics
------

To start entering lyrics first type

    # Lyrics

You can then enter the lyrics. To match sections of chords with sections of lyrics, use *.
For example

    * Verse
    C F | G G7

    # Lyrics
    * Verse
    Blah blah de blah

Would be interpreted as the 'Blah blah' to be sung over the first 2 bars and the 'de blah' over the second. Lines are important

For example if we have

    * Verse
    C F | G G7
    C F | G G7

Then

    # Lyrics
    * Verse
    Blah blah de blah
    Yadda yadda yey yey

will spread the lyrics across all 4 bars in the verse where as

    # Lyrics
    * Verse
    Blah blah de blah, Yadda yadda yey yey

would put all the lyrics over the first 2 bars and have nothing over the 2nd 2 bars.

If we need to we can use timings in the same way as we do for chords to specify individual beat lengths
For example

    Blah.3 blah de blah

would mean that the Blah should be held for 3 beats and the remaining syllables sung as tripleted quavers. So does

    Blah {blah de blah}

Usually, a song has multiple verses and a repeated chorus.  To specify these just repeat the '* Verse' or '* Chorus' command as needed.  If there are multiple definitions for the same section, they are taken in order and used in order.  If the structure defines more repetitions of a section than are specified in the lyrics, the lyrics will repeat.

For example

    # Structure
    Verse, Verse, Chorus
    Verse, Verse, Chorus

    # Lyrics
    * Verse
      ...
    * Chorus
      ...
    * Verse
      ...
    * Verse
      ...

Would mean that the same chorus is repeated twice in the song and that the first verse repeats again at the end. Sometimes this is not desirable as there maybe instrumental verses part way through the song.  In this case, including 'instrumental' or 'solo' as a comment in the structure will fix this.  So if in the above example, there should be a solo the third time the verse is repeated and before the final verse is sung, we would specify the structure as

    # Structure
    Verse, Verse, Chorus
    Verse (solo), Verse, Chorus

would work with the lyrics specified in the same way.  We can use the word as part of a sentence for more natural notes.

    # Structure
    Verse, Verse, Chorus
    Verse (keys solo), Verse, Chorus

Beats
-----

The beat behind the tune is as important as the chords being played. JamScript supports a quick and simple way of expressing beats.

To start entering beats type

    # Beats

Sections are defined the same way as with chords. Since most beats are driven by a kick / snare pattern, that's how JamScript interprets them.

    # Beats
    * Verse
    k . s . | k k s .

Here we have defined a simple kick / snare pattern to be played over a verse.  The kick drum should be played on the 1st beat of each bar and also on the 2nd beat every second bar. The snare should be played every 3rd beat.  The . indicicates nothing should be played for the 4th beats and the 2nd beat of the 1st bar.  The symbols that you can use to represent beats are as follows:

    .       nothing
    k       kick
    s       snare
    lt      low tom
    ht      high tom
    r       ride
    cr      crash
    h       hi-hat (closed)
    ho      hi-hat (open)

You can combine instruments using +

    k+h h k+h h
    h+(k . s .)

or if you want hi-hats every quaver instead

    h.1/2+(k . s .)

That said, you may just want to use notes (textual not musical) instead

    ! hi-hat every quaver
    k . s .

{} brackets can be used to group notes (musical not textual) together.

    k . s {. s} | k . s {ht lt ht}


## References

- The section on voicings builds on ideas taken from this document [http://ismir2005.ismir.net/proceedings/1080.pdf]