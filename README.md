# Potatoes

## Description

Our model predicts the crop water stress index based on the slope of the terrain, topographic wetness index, direction of terrain, electrical conductivity of the soil, the normalized difference vegetation index, and several spatial features based on the X and Y spatial coordinates.

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

Based on the output of the model, it seems like most of the farm appears to have a crop water stress index between 2 and 0, which means most of the crops are receiving enough water. However, there is a part of the crops that do have a CWSI value of 3 and 4, meaning they need to be watered. This part appears to be around the northwest part of the farm, slightly southwest of the coordinates (209850, 626375). We recommend that area to be watered as soon as possible.
