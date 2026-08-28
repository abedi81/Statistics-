## Streaming Bootstrap Standard Error

In a regular bootstrap, we usually keep the full dataset and repeatedly resample from it with replacement.

With streaming data, this is difficult because old observations may need to be deleted before new data arrive. We therefore cannot go back and resample the full dataset later.

To solve this, we use a Poisson bootstrap.

For each incoming observation, we generate a random weight from a Poisson distribution with lambda = 1.

The weight tells us how many times that observation should be included in a particular bootstrap replicate:

- 0 means do not use the observation
- 1 means use it once
- 2 means use it twice
- 3 means use it three times

This works because in a regular bootstrap, each original observation appears about once on average in a bootstrap sample.

For every bootstrap replicate, we keep only two running values:

- the weighted sum of the observations
- the weighted number of observations

After a batch is processed, the original observations can be deleted. The running bootstrap sums and counts are kept and updated when the next batch arrives.

In this task, the data arrive in 6 separate batches.

Each batch contains 100,000 observations randomly drawn from a Uniform distribution between 20 and 40.

We maintain 1,000 bootstrap replicates across all batches.

For each new observation, we generate 1,000 Poisson(1) weights, one for each bootstrap replicate, and update the corresponding running sums and counts.

After all 6 batches have been processed, we calculate the mean for each of the 1,000 bootstrap replicates.

Finally, the standard error is calculated as the standard deviation of these 1,000 bootstrap means.

The main advantage of this approach is that we can estimate the bootstrap standard error without storing all previous raw observations in memory.
