[<<< Previous](Part5.md) | [Next](Part7.md)

## Part 6: Exercise

Since the focus of this workshop is spatial R rather than how well the prediction is, I'll simply use linear regression to build the model: predicting AGB (aboveground biomass) based on MAR (mean anuual rainfall) and MAT (mean annual temperature).

```
mod = lm(agb ~ MAR + MAT, data = site_df)

agb = predict(mod,amz_df[,c('MAR','MAT')])

amz_agb = cbind(amz_df[,c('lat','lon')],agb)
```

Now follow the prompt to finish the rest of analysis!

```diff
# convert dataframe to simple feature


# rasterization


# plot
```

