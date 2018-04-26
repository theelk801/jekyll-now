---
layout: post
title: Making chords machine-readable
---

For my capstone at General Assembly, I wanted to explore using deep learning to generate chord progressions with Long Short-Term Memory networks (detailed very well in [this well-known article by Andrej Karpathy](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)). However, I decided I would try to do something different. In most character-based RNNs, the inputs are one-hot encoded, meaning that there’s a vector with exactly one entry equal to 1 and the rest equal to 0. In the case of character encoding, we would have 26 (or more) of these vectors, and each one would represent a different vector.

The first advantage to this is that it’s very easy to encode text in this way. You don’t have to do many tricky things besides remove potentially unnecessary characters. The second advantage to this is that it means that when our network learns the sequences, it outputs a vector which acts as a probability distribution from which to sample. We’ll get back to this second point later on.

The problem with these approaches for encoding is that they don’t properly express the underlying structure of musical chords. When we look at the text, we only see letters. Sure, there are things that are communicated, but not at the level we may be looking for. What if instead we could communicate the harmonies themself? Well, first we have to talk about what harmonies actually are.

The western chromatic scale has 12 notes, A through G, with sharps and flats in between, and a standard diatonic scale contains 7 of those 12 notes. We can play multiple of these notes at once to create a chord. For example, Dmaj7 consists of the notes D, F#, A and C#. Additionally, D is the *root* of this chord, and is always played as the lowest. The rest of the notes can be played in different octaves and this will change the *voicing*, but if we change the root, we’ll have an *inversion*. All of this is to say that when characterising a chord, we need to capture two things: the set of notes in the chord and which of those notes is the root.

Let’s go back to Dmaj7, but with a helpful visual:

	C C# D D# E F F# G G# A A# B | C C# D D# E F F# G G# A A# B
	Root 						Entire Chord

	     [0,0,1,0,0,0,0,0,0,0,0,0,0,1,1,0,0,0,1,0,0,1,0,0]


Up above we have the chromatic scale twice with the root highlighted on the left and the notes highlighted on the right, and below we have this represented as a vector. If we invert the chord to Dmaj7/F#, we instead get the following:

	C C# D D# E F F# G G# A A# B | C C# D D# E F F# G G# A A# B
	Root 						Entire Chord

	     [0,0,0,0,0,0,1,0,0,0,0,0,0,1,1,0,0,0,1,0,0,1,0,0]

The notes are the same, but the root is different.

So anyway, this was the idea. Next time we’ll talk more about how we implemented it, and then where everything went wrong…
