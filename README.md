# Shazam implementation via Linear Algebra 🎵


## Table of contents
* [Introduction](#introduction)
* [Problem setting](#problem-setting)
* [Review of possible approaches](#review-of-possible-approaches)
* [Implementation](#implementation)


## Introduction 📖

As music is an important and progressive part of the modern world, the need for a tool that could recognize it was only growing over time which is confirmed by the popularity of Shazam. Suppose the person hears a familiar song that has listened to a thousand times long ago, and the one captures 20 seconds of a song or even less. In that case, no matter if it’s intro, verse, or chorus, it will create a fingerprint for the recorded sample, consult the database for a match, and use its music recognition algorithm to tell you exactly which song you are listening to.


## Problem setting ❔

As was said before, the aim is to develop  a user-friendly application similar to Shazam, allowing  to find the song's name by its recorded fragment. The main parts of the implementation are the Fast Fourier Transform of the music piece, applying filtering algorithms for detecting the more valuable information for fingerprinting the song, and its hashing and matching algorithms.

## Review of possible approaches 📝

Indeed, many solutions propose Music Recognition, as Shazam and other apps can do (e.g., SoundHound, Genius, Musicxmatch, Google Assistant & Siri). But the basis of it is transforming audio fragments into the frequency domain by using Fast Fourier Transform. It will help to extract further the fingerprint or signature for the audio fragment providing a static representation of a dynamic signal. From that point, there is a difference between the algorithm of Shazam and some attempts to imitate it. There are many variants, but it is better to highlight two of them.

1. Hashing Intervals
   - To make Music Recognition more efficient, those algorithms use some kind of sliding window, as frequency domain representation does not provide the most important information about the timing of peak frequency. It is important to  make Fast Fourier Transform on every chunk of the music (i.e., 44 chunks in one second, given a 44100 sample rate) and then, having intervals of frequencies, detect the peaks with the biggest magnitude. This information forms a signature for this chunk of the song, and this signature becomes part of the fingerprint of the song as a whole. Hashing the data is then stored in the database for further matching.

2. Hashing with Anchors and target zone (Shazam)
   -  As Fast Fourier Transform has been performed on the song, one needs to find the strongest frequencies in that signal by applying filtering. It is important that frequencies be evenly spaced through the signal's spectrogram to allow fingerprinting of all fragments. Also, it is common knowledge that the recorded song fragment may contain noise; thus, one has to be confident that those peaks are evenly spaced in frequency so that the algorithm can deal with noise and frequency distortion. After detecting those strongest frequencies (anchors), the next step is to create the target zone and hash it.


## Implementation
