# A/B-Test
In this project I will demonstrate how to properly design an A/B test and measure the results by testing for statistical significance. To provide context, I will use Acme Corp as a fictional company that has a common real world business problem.
## Business Problem
Here's a 10,000-foot view of the Acme Corp product:
A consumer posts a request for a service needed. Every request is in some category (e.g., Catering, Personal Training, Interior Design) and some location (e.g., New York, San Francisco).
We match the request up with appropriate service providers and send each of those providers an invite to quote on the request.
Providers view the invite and some choose to send a quote to the consumer expressing interest.

I've just concluded a test of our quote form. After receiving an invite, service providers come to the quote form to view the consumer request and choose whether or not to pay to send a quote. Over the course of a week, I divided invites from about 3000 requests among four new variations of the quote form as well as the baseline form we've been using for the last year. Here are my results:

Baseline: 32 quotes out of 595 viewers
Variation 1: 30 quotes out of 599 viewers
Variation 2: 18 quotes out of 622 viewers
Variation 3: 51 quotes out of 606 viewers
Variation 4: 38 quotes out of 578 viewers

I would like to know the proper way to design this experiment in order to avoid confounding results. My goal is to then determine if certain variations of the form would cause more providers to send a quote after coming to the page, making Acme Corp more profitable $$ 

## Business Solution
The fundamental components of a statistically sound experiment is 
  1. Randomization
  2. Test and Control Groups
  3. Repeadability and Reproducibility of the experiment
  
Service providers should be selected randomly when inviting them through different treatment variations to minimize statistical differences between the providers. For example, personal trainers may like Variation 3 the best which can introduce bias to the calculation if we specifically handpicked them. However, that biased is minimized if Variation 3 is sent out to different categories of contractors, giving our experiment an unbiased result.
The control group is used as a benchmark to compare results to. The test group only has one treatment differece. A treatment is the thing you are testing. In this example, the treatment is the design. Hence, the only difference between the Baseline and Variations should be one change in the design and nothing else. The header, font size, and words of the email should be exactly the same. If not, then we have no way of determining which factor contributed to a different conversion rate. This is called confounding results.
The test needs to be repeated to ensure results are consistent and thus reliable. In this case, the test should be run the same way the week after as close as possible by the day, hour, and minute, reducing confouding results as much as possible. Reproducibility ensures that other areas of the business can benefit from this test. In this case, if Acme corp had another website that sells widgets using different designs, they can easily run the same test. 
