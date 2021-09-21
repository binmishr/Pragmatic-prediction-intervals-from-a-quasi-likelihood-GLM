# Pragmatic-prediction-intervals-from-a-quasi-likelihood-GLM-by-ellis2013nz

The details of the codeset and plots are included in the attached Adobe Acrobat reader (.pdf) file in this repository. 
You need to download the same to view the contents. There are referrals to other contents in BLUE colour also to follow.


Rubber-duck debugging
========================

Troubleshooting lesson first. The problem described below has been causing me grief for several days. I have an important and time-critical use case at work where I need to impute lots of complex missing data using the sort of model I’m about to describe. But, due to tiredness causing me to not think straight, and the problem being embedded in a much larger set of complex computer programs, I was struggling to get the answer. After a while I correctly realised I needed to abstract the problem from the use case and make an example with simulated data to get my thinking straight.

After that didn’t work despite an embarrassing couple of hours of effort, come the weekend I decided to take the awesome responsibility of writing a new question on Cross-Validated. I was reluctant to do this because I am clearly meant to know the answer to this problem (outlined below) so it seems an admission of professional failure to have to ask publicly. Only because I was genuinely baffled would I go this far; it seemed clear that what I thought I understood about generalized linear models as implemented in R was wrong in some important respect.

I spent about 30 minutes carefully preparing a minimal reproducible example, explaining clearly what I needed to do and had tried, what my thinking was and why that didn’t seem to work, meticulously re-reading all the documentation. Which made me think, “oh, perhaps it means this…”. Without much hope (for this was about the third time in the process, over several days, I had suddenly thought I saw the light) I made an example to show that I had tried this and why it didn’t work. But (who would have seen this coming) it did work.

So never under-estimate the power of rubber duck debugging. That is, the troubleshooting super-power that is writing a reproducible example and carefully explaining exactly what your problem is in a way that you think someone else can answer it for you. If you explain it clearly enough, that someone else is just as likely to be you.

I actually own four rubber ducks expressly for the purpose of helping me solve stubborn problems that should “surely” be in my own resources to solve – one for each of my work stations at home, one for work (now sitting as a spare at home), and a fourth just for travel. But I hadn’t followed my own advice of explaining the problem to one of the ducks. So, lesson learned.

Documentation: https://en.wikipedia.org/wiki/Rubber_duck_debugging

Cross Validated : https://stats.stackexchange.com/

Multiple Imputation with Chain Equation : https://stats.stackexchange.com/

Prediction intervals for variables with exponential relationships and increasing variance
===========================================================================================

On to the substantive statistical problem. I have data that when log-transformed, has a linear relationship to other data. And my main variable clearly has increasing variance as its expected value increased. In my real world example, I have many of these variables and the motivation is to impute missing values.

I want to model the data in a way that doesn’t throw away individual negative values, so taking a log transformation is sub-optimal. Instead, I want to use a generalized linear model with logarithm link function, which means the expected value of the response will never go below zero but individual values can (the original data includes financial variables which closely follow this pattern). I need the variance of the response variable to increase with the expected value though, so I can’t use family = gaussian(link = log), which assumes constant variance.

The individual-level variance of the response is crucial because I am going to use the model to simulate individual values in a home-made implementation of multiple imputation with chained equations (for various complicated reasons, I can’t just use the mice package, which would have been my preference, and it’s simpler to hand-roll my own system). I’m prepared to pretend the data is normally distributed (conditional on an expected value that changes with the other variables in the system) so long as I have variance increasing proportional to the mean. So an important end result of all this is a vector that I can use for the sd = argument of rnorm().
