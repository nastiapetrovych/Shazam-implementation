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


## Implementation 💻

Shazam’s Music Recognition algorithm can be divided into the following parts: 

1. Fast Fourier Transform of the song/fragment of the song
   - The main part of the Music Recognition Algorithm is the Fast Fourier Transform (further FFT), an algorithm used to transform a time-domain signal into its corresponding frequency-domain representation by using a sequence's computed discrete Fourier transform (DFT).
Let x0,...,xN−1 be complex numbers. Thus, the DFT is defined by the formula:
<p align="center">
 <img width="419" alt="image" src="https://github.com/nastiapetrovych/Shazam-implementation/assets/92577132/fb7f081b-d429-4ba0-b900-9f3b99bf60c9">
</p>


where e^(i2π)/N is a primitive Nth root of 1.
As for the complexity, from the definition, it directly requires O(N2) operations: there are
N outputs Xk, and each output requires a sum of N terms. But FFT algorithms require only O(n log(n)).
In the case of audio signals, the FFT can be used to analyze a sound’s spectral content, allowing us to visualize and manipulate individual frequencies that make up the sound. The basic idea behind the FFT is that any signal can be represented as a sum of sinusoids with different frequencies, amplitudes, and phases. The FFT algorithm takes a time-domain signal as input and produces a frequency-domain representation of that signal as output. The output of the FFT is a set of complex numbers which represent the amplitude and phase of each frequency component of the signal.

2. Finding peaks by applying filtering algorithm
   - Going further into the algorithm, a Maximum filter algorithm is performed on our obtained frequencies. Every pixel of the spectrogram is set to the local maximum by looking at its neighborhood pixels. The example is given in the picture:
   <p align='center'>
   <img width="534" alt="image" src="https://github.com/nastiapetrovych/Shazam-implementation/assets/92577132/f4849d5f-ea05-47c5-9304-bd6061ec3443">
   </p>

Then one has to find the location of the local peaks, as the Maximum filter algorithm only emphasizes them. The concept behind this technique is that the local peaks have been used to replace all the non-peak points in the spectrogram, resulting in a modification of their values. The peaks are the only points in the spectrogram that remain unchanged; therefore, they are candidate peaks for the Constellation map.

<p align='center'>
   <img width="832" alt="image" src="https://github.com/nastiapetrovych/Shazam-implementation/assets/92577132/7c22fcd0-0307-46ff-b0c3-882725ac2513">
   </p>

In case of plotting our filtered peaks together, we will get the kind of Constellation map

<p align='center'>
  <img width="850" alt="image" src="https://github.com/nastiapetrovych/Shazam-implementation/assets/92577132/5164b3c5-0299-451f-b839-9b5fd8c1c2a0">
   </p>


3. Hashing 
  
   - Having a unique fingerprint is crucial as it significantly enhances the speed of searching and enables the identification of songs. Shazam addresses the challenge of uniqueness by generating hashes from pairs of peaks. Pairing points offers the advantage of significantly enhancing uniqueness compared to individual points. When two points are paired together, the resulting combination becomes much more distinct.
As for Shazam’s algorithm of Music Recognition, it suggests the following way of hashing:
     - Take the point from the Constellation map; that will be the anchor point.
     - Find out the size of the target zone for every anchor point.
     - Pair each point from the target zone to the anchor point.

<p align='center'>
  <img width="812" alt="image" src="https://github.com/nastiapetrovych/Shazam-implementation/assets/92577132/36d40eb1-0ff2-4894-8142-01349342bb82">
   </p>


6. Matching 
   -Given the database of grouped fingerprints by song,  now it is possible to perform the matching. After receiving the recorded music's fingerprint, the next step is to iterate through the songs and their fingerprints to check what hashes coincide with those in the  database. However, it is difficult to guarantee that it perfectly lines up due to much noise. Thus, during iteration
through the database, subsequently,  compare the timing of appearing of the hash  in the original track with the timing of when the same hash appears in the sample, simply making the difference between them. Then, the number of coinciding differences is counted and compared to other songs’ differences, and a possible match is shown as the potential name of the song of the given fragment.


# Results 🤯

Solving the problem of Music Recognition, using the approaches of such popular apps as Shazam, we reach our goal of implementation of its algorithm and imitation of the database from which we can find matches to our songs or recorded fragments. The main part of Linear Algebra is actually the Fast Fourier Transform and its application, which helped a lot with the beginning of fingerprinting. Also, it is worth mentioning the filtering algorithm, more accurately maximum-filter, which is used mostly for Image Denoising but was also useful for the Music Recognition algorithm.
Although it is not the full algorithm of Shazam, it is open for further improvements that can lead to better recognition, fingerprinting, and even the extension of the existing  database. It is important to mention that the given algorithm is not 100% efficient, but the chosen dataset works well(almost 90% of accuracy). For better results target zone should be more adaptable and it is better to use noise reduction

# References 🔖

[An Industrial-Strength Audio Search Algorithm](https://www.Ee.Columbia.Edu/~Dpwe/Papers/Wang03-shazam.Pdf.)

[Cameron MacLeod’s Profile Picture](https://www.Cameronmacleod.Com/Blog/How-does-shazam-work)

[Toptal Engineering Blog](www.toptal.com/algorithms/shazam-it-music-processing-fingerprinting-and-recognition)

[Audio Fingerprinting with Python and Numpy](https://willdrevo.com/fingerprinting-and-audio-recognition-with-python/)

