# Shazam implementation via Linear Algebra 🎵


## Table of contents
* [Introduction](#introduction)
* [Problem setting](#problem-setting)
* [Review of possible approaches](#review-of-possible-approaches)
* [Implementation](#implementation)


## Introduction 📖

As music is an important and progressive part of the modern world, the need for a tool that could recognize it was only growing over time which is confirmed by the popularity of Shazam. Suppose the person hears a familiar song that has listened to a thousand times long ago, and the one captures 20 seconds of a song or even less. In that case, no matter if it’s intro, verse, or chorus, it will create a fingerprint for the recorded sample, consult the database for a match, and use its music recognition algorithm to tell you exactly which song you are listening to.


## Problem setting ❔

As was said before, our aim is to develop our own user-friendly application similar to Shazam, which allows us to find the song's name by its recorded fragment. The main parts of the implementation are the Fast Fourier Transform of the music piece, applying filtering algorithms for detecting the more valuable information for fingerprinting of the song, and its hashing and matching algorithms.

## Review of possible approaches

## Implementation
