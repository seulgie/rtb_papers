Budget Pacing for Targeted Online Advertisements at Linkedin by Deepak Agarwal et al. KDD 2014.

# Good to know
Generalized second-price auction (GSP) is one where at each opportunity of an impression, eligible campaigns are ranked
based on bids and the winner plays the bid of the second highest bidder.

# Goal
Allocate budget during each time window proportional to the forecasted volume of traffic for the campaign during that window.
1) Obtain a forecast for the traffic pattern of eligible impressions during the day
2) based on forecast, determine an allocation plan proportional to the forecasted traffic volume
3) at runtime, monitor the spending of each ad campaign and throttle campaigns which spend faster than planned,
   and do not allow them to participate in next slot auctions

# Algorithm
* S : set of all members
* S_i : subset of S in which ad campaign i targets
* d_i : daily budget
* s_i_t : cumulative spend of campaign i from the beginning of the day till the start of t
* f_i_t : forecast for volume of eligible impressions for campaign i during
* a_i_t = (f_i_t / f_i_T) * d_i
* PTR (Pass-through-rate) is being updated for each campaign at 1 minute intervals.
  This is a dynamic method of choosing PTR where the value at the current time window is set
  as a small perturbation of the value at the previous time window.

1) Forecasting the number of eligible impressions for campaigns (from segment S_i)
2) Adjustment Rate (Set the adjustment rate r_i at a constant 10%)
3) Slow Start (always start with a PTR of 10%)
4) Fast Finish (modify the allocation curve slightly to spend all budget within 22 hours)

# Metrics
1) Advertiser centric : Unique impressions per spend, Number of campaigns served
2) Publisher centric : Cost per request, Over delivery
3) Member centric : Unique campaigns served => we can use it

# Engineering Design
Online linkedin DSP dataset (A/B test)
1) Forecast of eligible impression is computed and pushed to the pacing module daily.
2) The pacing module recalculates PTR for each campaign using the latest budget/spend data and allocation curve,
   and pushes it into campaign index.
3) Campaign index is scanned at ad matching and auction time.
4) PTR is used in a simple probability tes to determine whether a given campaign should participate in the auction or not.


# Interesting points
* Logic was simple but robust and practical
* This paper set various metrics in diverse point of views
* Linkedin applied PTR (simple probability to determine whether a given campaign should participate in the auction or not) every 7 seconds.
