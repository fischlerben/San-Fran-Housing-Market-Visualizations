# Visualizations with Python

This project utilizes Python visualizations packages, such as Plotly Express, HVPlot and PyPlot/Matplotlib to create an interactive dashboard exploring the 2010-2016 San Francisco housing market.  Uses MapBox API to grab data and create visualizations.
![san_fran](https://www.fortunebuilders.com/wp-content/uploads/2015/04/san-francisco-real-estate-market.jpg)

## Visualizations of San Francisco Real Estate Housing Market:

### Average Housing Units/Year
![av_housing_units_per_year](/Pics/av_housing_units_per_year.png?raw=true)

### Average Price/Neighborhood (with Interactive Dropdown Selector for Neighborhood)
First, group by year and neighborhood and then create a new dataframe of the mean values:

    mean_values = sfo_data.groupby([sfo_data.index, "neighborhood"]).mean()
    mean_values.reset_index(inplace=True)
    
Then use HVPlot to create an interactive line chart of the Average Price/Sq. Foot, with a dropdown selector for the neighborhood:

    mean_values.hvplot.line(x="year", y="sale_price_sqr_foot", xlabel= "Year", ylabel="Average Price/Square Foot", groupby="neighborhood")

The above code results in the following interactive line chart:
![av_price_per_neighborhood](/Pics/av_price_per_neighborhood.png?raw=true)

### Top 10 Most Expensive Neighborhoods
![top_10_most_expensive](/Pics/top_10_most_expensive.png?raw=true)

### Parallel Coordinates Plot to Interactively Filter and Explore Various Factors Related to Sales Price

    # Parallel Coordinates Plot
    px.parallel_coordinates(ten_most_expensive_df, color="sale_price_sqr_foot", color_continuous_scale=px.colors.sequential.Inferno, title='Average House Value/Neighborhood', labels={'neighborhood': "Neighborhood", 'sale_price_sqr_foot':'Sales Price/Square Foot', 'housing_units':'Housing Units', 'gross_rent':'Gross Rent'})

The above code results in the following interactive parallel coordinates plot:
![parallel_cats](/Pics/parallel_cats.png?raw=true)

### Parallel Categories Plot to Interactively Filter and Explore Various Factors Related to Sales Price

    # Parallel Categories Plot
    px.parallel_categories(ten_most_expensive_df, color="sale_price_sqr_foot", color_continuous_scale=px.colors.sequential.Inferno, title='Average House Value/Neighborhood', labels={'neighborhood': "Neighborhood", 'sale_price_sqr_foot':'Sales Price/Square Foot', 'housing_units':'Housing Units', 'gross_rent':'Gross Rent'})

The above code results in the following interactive parallel coordinates plot:
![parallel_two](/Pics/parallel_two.png?raw=true)

### Average Value/Neighborhood utilizing MapBox API
First, calculate the mean values for each neighborhood:

    mean_neighborhoods = sfo_data.groupby("neighborhood").mean()
    mean_neighborhoods = mean_neighborhoods.reset_index()
    
Then join average values with the neighborhood locations (Lat/Long loaded in with MapBox API):
    
    values_and_locations_df = pd.concat([df_neighborhood_locations, mean_neighborhoods['sale_price_sqr_foot'], mean_neighborhoods['housing_units'], mean_neighborhoods['gross_rent']], axis=1).dropna()

Lastly, create a scatter plot through mapbox to analyze neighborhood info:

    map_plot = px.scatter_mapbox(values_and_locations_df, lat="Lat", lon="Lon", size="sale_price_sqr_foot", color="gross_rent", color_continuous_scale=px.colors.cyclical.IceFire, size_max=15, zoom=3, width=1000, hover_name="Neighborhood", title="Average Price/Square Foot and Gross Rent in San Francisco")

The above code results in the following interactive MapBox visualization:
![mapbox](/Pics/mapbox.png?raw=true)