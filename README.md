# A/B-Test
In this project I demonstrated how to properly design an A/B test and measure the results by testing for statistical significance using R. To provide context, Acme Corp was used as a fictional company that has a common real world business problem.
## Business Problem
Here's a 10,000-foot view of the Acme Corp product:
A consumer posts a request for a service needed. Every request is in some category (e.g., Catering, Personal Training, Interior Design) and some location (e.g., New York, San Francisco).
We match the request up with appropriate service providers and send each of those providers an invite to quote on the request.
Providers view the invite and some choose to send a quote to the consumer expressing interest.

I've just concluded a test of our quote form. After receiving an invite, service providers come to the quote form to view the consumer request and choose whether or not to pay to send a quote. Over the course of a week, I divided invites from about 3000 requests among four new variations of the quote form as well as the baseline form we've been using for the last year. Here are my results:

| Bucket       | Quotes | Viewers | Conversion Rate |
| -------------|:-------| -------:| --------------: |
| Baseline     | 32     | 595     | .05             |
| Variation 1  | 30     | 599     | .05             |
| Variation 2  | 18     | 622     | .03             |
| Variation 3  | 51     | 606     | .08             |
| Variation 4  | 38     | 578     | .07             |



My goal is to then determine if certain variations of the form would cause more providers to send a quote after coming to the page, making Acme Corp more profitable $$. I would also need to know the proper way to design this experiment in order to avoid confounding results. 

## Solution
The fundamental components of a statistically sound experiment is 
  1. Equal Randomization
  2. Test and Control Groups
  3. Repeadability and Reproducibility of the experiment
  
Invites should equally amongst the four new variations and the baseline form of the quote. From a statistical perspective, this is important because both groups need to satisfy the assumptions of equal variance and a normal distribution in order to test for statistical significance using the t test. The highest probabilistic way to accomplish this is through equal size groups since variance negatively correlates to sample size and a large sample size directly correlates to a normal distribution. Equally size groups maximizes the sample sizes for each group to satisfy the assumptions. Luckily, this violation will not have a statistical impact in the calculation because a biomnial test does not rely on these assumptions. However, from a practical perspective most websites use LRU caches which means that the variation with the most quotes will have the highest speed, introducing user bias since users prefer higher speed despite the design of the quote. Invites should also be randomly divded to minimize statistical differences and bias between the providers. For example, personal trainers may like Variation 3 the best which can introduce bias to the calculation if we specifically handpicked them. This would be a problem since personal trainers do not represent other service providers such as Caterers who would also get invites from variation 3 and may not like that particular design. 

The control group is used as a benchmark to compare results to. The test group only has one treatment differece. A treatment is the thing you are testing. In this example, the treatment is the design. Hence, the only difference between the Baseline and Variations should be one change in the design and nothing else. The header, font size, and words of the email should be exactly the same. If not, then we have no way of determining which factor contributed to a different conversion rate. This is called confounding results.

The test needs to be repeated to ensure results are consistent and thus reliable. In this case, the test should be run the same way the week after as close as possible by the day, hour, and minute, reducing confouding results as much as possible. Reproducibility ensures that other areas of the business can benefit from this test. In this case, if Acme corp had another website that sells widgets using different designs, they can easily run the same test. 

Looking for statistical significance leads us to two more questions. Did the variation of quotes occur from design differences or strictly by chance? How large is the variation if it did not occur by chance? In statistics, there are no guarantees, so we have to change the word “strictly” to “unlikely” in the first question. Statisticians came to the consensus that an event is unlikely if it occurs less than 5% of the time. We ask the second question because everything in the real world will naturally vary in small amounts. Hence, we only care about large variations that are not expected. We can combine the first two questions to test for statistical significance, which means the variations of the designs were unlikely to have occurred by chance if the variations were expectantly small. A binomial test will be used since we are comparing two different proportions. For instance, the ratio between the number of quotes and total number of views differ for each variation. In addition, we are going to test with 95% confidence for the reason mentioned above. The p value represents the probability that the variations occurred by chance when there are no significant differences between them. Hence, a p value below .05 is statistically significant. Additionally, the ratio of quotes per views vary positively and negatively so a two tailed hypothesis test needs to be used. The R function prop.test() is a function used for the binomial test and will be used to run our analysis and is shown below.  

Assuming that speed did not effect users behavior and that the experiment was repeated at least one more time, one can conclude that Variation 2 and Variation 3 have a p value of 0.04153 and 0.04983 respectively. This means that about 4% of the time, the number of quotes from variation 2 and variation 3 will occur by chance alone, when variance is at its expected levels. Since those numbers are below .05, we can conclude that variation 2 will generate less quotes, and variation 3 will generate more quotes 95% of the time when compared to it's baseline. 

## Business Action
Use Variation 2 and Variation 3.

```{r}

prop.test(x = c(32,30), n =c(595,599))

	2-sample test for equality of proportions with continuity
	correction

data:  c(32, 30) out of c(595, 599)
X-squared = 0.024814, df = 1, p-value = 0.8748
alternative hypothesis: two.sided
95 percent confidence interval:
 -0.02314956  0.03054564
sample estimates:
    prop 1     prop 2 
0.05378151 0.05008347 

> prop.test(x = c(32,18), n =c(595,622))

	2-sample test for equality of proportions with continuity
	correction

data:  c(32, 18) out of c(595, 622)
X-squared = 4.1541, df = 1, p-value = 0.04153
alternative hypothesis: two.sided
95 percent confidence interval:
 0.0007906984 0.0488945133
sample estimates:
    prop 1     prop 2 
0.05378151 0.02893891 

> prop.test(x = c(32,51), n =c(595,606))

	2-sample test for equality of proportions with continuity
	correction

data:  c(32, 51) out of c(595, 606)
X-squared = 3.847, df = 1, p-value = 0.04983
alternative hypothesis: two.sided
95 percent confidence interval:
 -0.060627950 -0.000125856
sample estimates:
    prop 1     prop 2 
0.05378151 0.08415842 

> prop.test(x = c(32,38), n =c(595,578))

	2-sample test for equality of proportions with continuity
	correction

data:  c(32, 38) out of c(595, 578)
X-squared = 0.54968, df = 1, p-value = 0.4584
alternative hypothesis: two.sided
95 percent confidence interval:
 -0.04081128  0.01688642
sample estimates:
    prop 1     prop 2 
0.05378151 0.06574394 
``` 
 
