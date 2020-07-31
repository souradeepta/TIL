# The Central Limit theorem

*Experiments on the fundamental theorem that explains why many distributions tend to be close to the normal distribution.*



The Central Limit theorem, often abbreviated to CLT explains why the bell-shaped curve appears so often in probability.

In words, suppose that you sample from a [population](http://internalpointers.com/post/differences-between-sample-and-population) using a sufficiently large sample size, noted as NN. According to the Central Limit theorem, while the sample size NN grows to infinity, the mean μμ of all those samples will be normally distributed, regardless of the original distribution of the population you sampled from.

## How the Central Limit theorem can be useful

While working with real-world data you might face two main limitations. First, there are many important things one would like to measure and test that do not follow a "pure" normal distribution. Second, working with the full population can be absolutely impractical: just think of a blood sample! How can you make predictions and calculations on that data? Even when full set of raw data does not fit it, there is often a normal distribution underneath. This makes it possible to still use the normal distribution to test ideas about non-normal, incomplete data.

You just have to collect samples of multiple observations, and calculate the average for each sample. As the number of observations in each sample increases, the distribution of these averages becomes more and more similar to the normal distribution. You can apply statistical techniques that assume normality even when you are sampling non-normal populations.

## The central Limit theorem in practice

Let's see if the Central Limit theorem really works. Below is a Python script that generates a fake population and from that population takes `num_samples` random samples of size `sample_size`. Then it computes the mean of those samples and prepares a somewhat primitive histogram made with a Python dictionary. I then took that raw histogram and plotted it into a nicer graph, to visually see what is going on.

You need Python 3 or greater to run that code, give the presence of `statistics` module.

```python
import statistics
import collections
import math
import random


def take_sample(population, sample_size):
  return [random.choice(population) for i in range(sample_size)]


def histogram(data, pop_size):
  out = collections.defaultdict(int)
  for i in range(pop_size):
    out[i] = 0
  for x in data: out[x] += 1
  return out


pop_size   = 64
population = [i for i in range(pop_size)]
num_samples = 50000
sample_size = 32
means = []      

# sample 'num_samples' times from the population: take a random sample of size
# 'sample_size' from it and compute the mean
for i in range(num_samples):
  sample = take_sample(population, sample_size)
  means.append(math.ceil(statistics.mean(sample)))

hist = histogram(means, pop_size)

# you can now plot 'hist'
```

### Plotting the results

Each figure below shows the histograms of several samples mean as I run the script with various sample sizes and the number of samples fixed to 50,000. The x axis contains the mean value, while the y axis tells you how many samples there are with that mean value.



![Frequency distribution, sample size = 1](https://s3.amazonaws.com/images.internalpointers.com/2015/10/frequency-distribution-sample-mean-s-1.svg)

\1. Frequency distribution of the samples mean, with N=1N=1.





![Frequency distribution, sample size = 2](https://s3.amazonaws.com/images.internalpointers.com/2015/10/frequency-distribution-sample-mean-s-2.svg)

\2. Frequency distribution of the samples mean, with N=2N=2.





![Frequency distribution, sample size = 4](https://s3.amazonaws.com/images.internalpointers.com/2015/10/frequency-distribution-sample-mean-s-4.svg)

\4. Frequency distribution of the samples mean, with N=4N=4.





![Frequency distribution, sample size = 8](https://s3.amazonaws.com/images.internalpointers.com/2015/10/frequency-distribution-sample-mean-s-8.svg)

\5. Frequency distribution of the samples mean, with N=8N=8.





![Frequency distribution, sample size = 16](https://s3.amazonaws.com/images.internalpointers.com/2015/10/frequency-distribution-sample-mean-s-16.svg)

\6. Frequency distribution of the samples mean, with N=16N=16.





![Frequency distribution, sample size = 32](https://s3.amazonaws.com/images.internalpointers.com/2015/10/frequency-distribution-sample-mean-s-32.svg)

\7. Frequency distribution of the samples mean, with N=32N=32.





Several interesting facts emerge from these pictures. First of all as the sample size grows, the frequency distribution of the sample mean becomes more similar to a bell-shaped curve: that's exactly what the Central Limit theorem states! The curve is also narrower as the sample size gets bigger: we can say that its standard deviation σσ becomes smaller. The standard deviation as a matter of fact drives the fatness of a bell-shaped curve.

Moreover when the sample size N=1N=1 (figure 1) the frequency distribution looks like a **uniform distribution**, also known as **rectangular distribution**. It means that every sample mean occurs with the same frequency. It happens because you are basically picking single numbers instead of samples mean, since the mean of a sample with size N=1N=1 is just equal to itself.

I also pushed the sample size quite further. Generally in a real-world scenario you rarely need to go further than N=16N=16, but it also depends on how skewed and messed up is the population distribution. The more the population distribution is non-normal, the more the sample size must be large in order to obtain a normal distribution of the samples mean.

## Sources

Atomic Object - *An Introduction to the Central Limit Theorem* ([link](http://spin.atomicobject.com/2015/02/12/central-limit-theorem-intro/))
Khan Academy - *Central limit theorem* ([video](https://www.youtube.com/watch?v=JNm3M9cqWyc))
Harvard University - *Law of Large Numbers and Central Limit Theorem* ([video](https://www.youtube.com/watch?v=OprNqnHsVIA))