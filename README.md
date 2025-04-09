# Potatoes

## Description

Our model reaches an R-squared value of about 0.434, which means about 43% of the variability in the data can be explained by our model. On average, our model has an RMSE of about 0.136.

## Dependencies

Libraries needed:

-   tidyverse

-   spmodel

-   viridis

## Objects

obs - a dataframe containing the locations, CWSI, and related variables for locations with CWSI sensors.

new - a similar dataframe but for locations without the sensors, and therefore with missing values for CWSI.

obs.sp - a spatial linear model trained on the observed data that can be used to predict CWSI for the new data.

## Usage

Run the following code to get predictions for crop water stress index at unobserved locations

```{r}
library(tidyverse)
library(spmodel)
library(viridis)
load('Objects.Rdata')

# Save predictions
new$CWSI <- predict(obs.sp, newdata = new)

all <- rbind(obs, new)

# Plot predictions
all %>%
  ggplot(aes(x=POINT_X,y=POINT_Y, color = CWSI)) +
  geom_point() +
  scale_color_viridis()
```

## Output
