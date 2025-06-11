---
title: "The FFT: Finished Code"
layout: default
date: 2025-6-10 04:00:00 -0000
categories: posts
tags: ['dsp', 'maths']
---

# Finished FFT Example in Code

Alright, finally finished with my example FFT program. 
It took a while, there's been a lot going on in my life 
recently: I got married, I'm starting a new job, and my 
wife and I are moving to a new house! Needless to say, 
I've been a little too distracted to consistently work on 
projects. 

But I now have a finished code example for the FFT. 
The code can be found [here](https://github.com/bgmichelsen/BGM_FFT).

This project was written to be a VST plugin, which basically means 
it can be from a Digital Audio Workstation. Go ahead and try it in 
your favorite DAW!

The program implements a custom FFT forward transform based on 
a BASIC program written by Dr. Steven W. Smith in his book 
*The Scientist and Engineer's Guide to Digital Signal Processing (see 
[Chapter 12](https://www.dspguide.com/ch12.htm)). 

It also implements Juce's implementation of the FFT, as part of the DSP 
library. This is just to compare how a profressional library compares to 
my implementation. 

(NOTE: The application only shows an amplitude/magnitude graph, but the 
FFT does produce enough information for a phase-vs.-frequency graph as 
well. Both a magnitude graph and phase graph together is referred to as a 
Bode plot.) 

## Results

Below are the results of both the Juce FFT and my custom FFT. Both of these 
were tested with a sine wave at the note A4 (440 Hz). 

Juce FFT results:
![juce_fft](/img/fft/fft_example_juce.PNG)

My custom FFT results:
![bgm_fft](/img/fft/fft_example_bgm.PNG)

As you can see, my custom FFT is a little blockier. This is because the 
FFT breaks the frequency domain into buckets, or discrete frequency samples. 
My implementation just doesn't have the buckets reduced to a very fine detail. 

Overall though, my implementation matches the Juce implementation, showing a peak 
in magnitude at 440 Hz.

## The Code

Below is the C++ code for my custom FFT: 

```cpp
void BGM::FFT::forwardTransform(float *const data, size_t size)
{
    constexpr double PI = 3.14159265;
    size_t N = (size);
    int M = (std::log(N) / std::log(2));

    float *re = new float[N];
    float *im = new float[N];
    std::memcpy(re, data, (N * sizeof(float)));
    std::memset(im, 0.0f, (N * sizeof(float)));

    bitReversal(re, im, N);

    // Loop each FFT stage
    for (int l = 1; l < M; l++)
    {
        int le = std::pow(2, l);
        int le2 = (le / 2);
        float ur = 1.0f;
        float ui = 0.0f;
        float sr = std::cos(PI / le2);
        float si = -1.0f * std::sin(PI / le2);

        // Loop for each sub DFT
        for (int j = 1; j < le2; j++)
        {
            // Loop for each butterfly calculation
            for (int i = (j - 1); i < (N - 1); i += le)
            {
                int ip = i + le2;

                // Butterfly calculation
                float tr = re[ip] * ur - im[ip] * ui;
                float ti = re[ip] * ui + im[ip] * ur;
                re[ip] = re[i] - tr;
                im[ip] = im[i] - ti;
                re[i] = re[i] + tr;
                im[i] = im[i] + ti;
            }

            float tr = ur;
            ur = tr * sr - ui * si;
            ui = tr * si + ui * sr;
        }
    }

    // Set the new data
    for (int i = 0; i < (N / 2); i++)
    {
        data[i * 2] = re[i];
        data[i * 2 + 1] = im[i];
    }

    delete[] re;
    delete[] im;
}
```

There are two parts to this: the bit-reversal sorting and 
the three nested for-loops that run the algorithm. 

Below is the function for the bit-reversal sorting: 

```cpp
void BGM::FFT::bitReversal(float *const re, float *const im, size_t size)
{
    size_t N = size;
    int j = (N / 2);
    for (int i = 1; i < (N - 2); i++)
    {
        if (i < j)
        {
            float tr = re[j];       // Real part
            float ti = im[j];       // Imaginary part
            re[j] = re[i];
            im[j] = im[i];
            re[i] = tr;
            im[i] = ti;
        }
        int k = (N / 2);
        while (k <= j)
        {
            j = j - k;
            k = k / 2;
        }
        j = j + k;
    }
}
```

Bit-reversal sorting basically works by reversing the bits of the 
index. For example, if the index could be represented by a 4-bit 
number, bit-reversal would result in address 0x0001 becoming 0x1000. 

This innately sorts the data into even and odd groups, even 
at the front of the data and odd at the back. Once it's all sorted, 
we can continue on to the actual FFT.

The FFT algorithm is composed of three loops. The outermost loop is 
for each FFT stage. Basically, this loops through each stage that breaks 
the samples down into 1-point signals. This takes $$log_2(N)$$ samples, 
where N is the total number of samples. For example, to break a 16-point 
signal into 16 single point signals, it would take four stages ($$log_2(2^4) = 4$$). 

The next loop goes through the DFT calculation for each single point signal at the given 
stage. This is also where the twiddle factors are calculated, by the *ui* and *ur* 
variables. 

The final loop performs the butterfly calculation on each single point DFT to 
recombine them into a frequency spectrum. 

## Improvements

Most of the issues I had with this project had to do with the fact that it 
was a VST plugin. Juce architects the plugin application so that it has a 
separate portion for processing audio and showing graphics: called the plugin 
processor and the plugin editor. And they do not make it easy to communicate 
data between the two. 

Because of this, there was a lot of copying data from one array to another. 
Most of my issues may have come from jumping in to a VST application without 
fully understanding how it is structured and thus architecting the application 
poorly, but if I had to do it over again I probably would've used a standalone 
application.

For the FFT implementation itself, I would have passed the *re* and *im* arrays 
directly, and not allocated them on the heap and then copied data into them. 
This would've been a much more efficient use of memory, using the same two 
arrays for both sampling the time-domain data and calculating the FFT.

## Conclusion

Overall, this project was more of an academic exercise so that I could understand 
the FFT better. There are plenty of DSP libraries out there that are much better 
than the algorithm I implemented. You can see that by how much smoother the Juce 
library results were. For embedded applications, there is also the [CMSIS DSP](https://arm-software.github.io/CMSIS_5/DSP/html/index.html) library for ARM applications. These libraries 
will be faster and more efficient, and would take less time to get to your end application 
than anything you could write yourself. 

But I hope you enjoyed the learning process as much as I did! See you in the next post.