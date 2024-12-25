[Sequential A/B Testing Keeps the World Streaming Netflix
Part 1: Continuous Data](https://netflixtechblog.com/sequential-a-b-testing-keeps-the-world-streaming-netflix-part-1-continuous-data-cba6c7ed49df)

Canary Deployment a.k.a regression-driven deployment - just deploy to a limited set of users and observe for errors or regressions. The group receiving the new software is known as the "treatment group" and the group using the existing software is called as the "control group"

Play delay - the delay between the user pressing the play button and the stream starting.

In this blog, they want to quickly and confidently identify any difference in the distribution of play delay using statistical methods.

There are two purposes of canary deployment:
1. To catch bugs beforehand
2. To measure the performance of the new software

Netflix measures play delay as one of the metrics in its canary deployments.

The following graphs show the distribution of play delay in a control group(left) against that in the treatment group(right)
![Play delay distribution](https://miro.medium.com/v2/resize:fit:828/format:webp/1*yDCF303-R9uqqH_zo7F4ug.gif)

To detect the increase in play delay, they cannot just rely on means and medians, as they don't paint a complete picture. For example, they don't consider the variance in the data, and thus some some members of the control group can experience higher play delays. So, they have the following requirements from their canary deployment system to able to quickly detect any upward shift in the play-delay distribution:
1. Identify bugs and performance regressions as quickly as possible.
2. Minimise false alarms, so as to not disrupt software deployment velocity
3. The system should be able to detect any change in distribution.

### Sequential testing
They employ fixed-n or fixed-time tests, which is nothing but collecting data until `n` numbers are received or `t` time has passed. They've mentioned two tests here: Kolmogorov-Smirnov test, and a Mann-Whitney test. Here's a small description about these tests:
1. [Kolmogorov-Smirnov test](https://en.wikipedia.org/wiki/Kolmogorov%E2%80%93Smirnov_test): A non parametric test(it doesn't assume the probability distribution from the start) used to test whether a sample came from a given distribution or not. In other words, if we were to draw a sample from a given reference distribution, how likely is it to match our collected sample?
2. [Mann-Whitney U test](https://en.wikipedia.org/wiki/Mann%E2%80%93Whitney_U_test): Given two groups samples, this test determines if one group tends to have higher values than others.

The above are fixed-n tests, and can be applied only once when a canary deployment is done(as n is fixed), but Netflix wants to test frequently to detect differences super fast. Also, doing multiple tests can increase chances of hitting false positives. This is how:
> If there's 5% chance of false positive in one test, then in two tests, the chances of not hitting false positives = 0.95*0.95 = 0.9025. That means, the chance of getting a false positive is 1-0.9025 = 8.75% > 5%

They go more deep into statistics and talk about p-values and stuff. To understand that, we must learn about hypothesis testing! Here's some notes on thst:

## Hypothesis testing
Statistical method to make decisions or inferences about a population based on a sample data. It helps determine whether there is enough evidence to support a specific claim or hypothesis about the population.