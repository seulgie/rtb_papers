Smart Pacing for Effective Online Ad Campaign Optimization by Jian Xu et al. KDD 2015.

## Good to Know
The objective of an advertiser can be divided as follows:
* Branding campaigns : to spend budget as much as possible to reach an extensive audience
* Performance campaigns : to meet the performance goal (e.g. CPC no more than 2 dollars) & spend as much budget as possible.
Advertisers expect their ads to be shown smoothly throughout the purchased period to reach a wider range of audience.
At the same time, advertisers want to reduce the creative serving cost and deliver impressions to right users efficiently.

## Objective
This paper focuses on two most prevailing campaign types : (1) branding campaigns, and (2) performance campaigns.
Also, this paper assumes that the bid is already given by a preceding bid optimization module
(because the bid win-rate curve is usually not smooth so modifying bid can cause significant changes in budget spending).

1) Formulated the multi-objective optimization problem and applied smart pacing control
2) Developed a control-based method to optimize budget pacing and campaign performance at the same time
3) Conducted experiments with extensive online / offline circumstances

## Problem Formulation
* Ad : ad campaign
* B : budget of Ad
* G : performance goal of Ad if there is
* spending plan : B = (B(1), ..., B(k))
* Req(i) : Ad request
* s(i) : whether Ad participants the auction for Req(i) or not (1 or 0)
* r(i) : probability that Ad participates the auction for Req(i) => the only thing Yahoo can have full control
* w(i) : variable indicating whether Ad wins Req(i) if the ad participates in the auction => depends on bid(i)
* c(i) : advertiser's cost if ad is served to Req(i) => sum(inventory cost, creative serving cost)
* q(i) : whether user performs desired responses (e.g. click) (1 or 0)
* p(i) : Pr(respond | Req(i)), probability of such response
* C : sum(s(i) x w(i) x c(i)), total cost of ad campaign o
* P : C / sum(s(i) x w(i) x q(i)), performance (e.g. CPC if the desired response is click) of ad campaign i
* (C(1), ..., C(k)) : spending pattern over K time slots, where C(t) is the cost in time slot t
* ε : tolerance level on deviating from the spending plan
* Ω : rmse penalty (error) function of deviation between spending plan and actual spent

## Smart pacing without performance goals
Determine the value of r(i) which minimizes P (performance of ad campaign i)
Focus on CPM campaign cases. In this case, advertisers are charged a fixed amount of money for each impression.
As long as budget can be spent and spending pattern is aligned with the plan, high responding ad requests should have a 
higher point pacing rate than low responding ones.

## Steps
1. Response Prediction : Learn from offline serving logs to build a response prediction model to estimate Pr(respond|Req(i))
   * helps distinguish high responding ad requests from low responding ones
   * leverage the hierarchy in the data to collect features at different granularities
2. Grouping Ad Requests
   * reduce solution space by grouping similarly responding ad requests together
   * requests in the same group share the same grouping pace rate
   * original problem of solving the point pacing rate of each r(i) is reduced to solving a set of group pacing rates
3. Develop a control-based method
   * learn from online feedback data and dynamically adjust group pacing rates to approximate the optimal solution
  
## Interesting points
- AvgErr, (K/B) x Ω quantifies relative deviation of actual spent and planned spend per time slot (K: number of time slots, B: total budget)
- Yahoo introduced an adaptive controller whose parameters are adaptive to spendings in t and planned budget in (t+1)
  this allowed to have much smaller deviations (AvgErr : 18%)
- Traffic pattern was further divided into 1 minute time slots
- This paper used average traffic of previous 7 days as the forecasted traffic to determine budget allocation
