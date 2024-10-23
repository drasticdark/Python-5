import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import linregress

# Load the data from epa-sea-level.csv
def load_data(filepath):
    return pd.read_csv(filepath)

# Create a scatter plot and regression lines
def plot_sea_level(data):
    # Scatter plot
    plt.figure(figsize=(10, 6))
    plt.scatter(data['Year'], data['CSIRO Adjusted Sea Level'], color='blue', label='Data')
    
    # Line of best fit for the entire dataset
    slope, intercept, r_value, p_value, std_err = linregress(data['Year'], data['CSIRO Adjusted Sea Level'])
    years_extended = pd.Series(range(1880, 2051))  # Extend years to 2050
    plt.plot(years_extended, intercept + slope * years_extended, color='red', label='Best Fit Line (1880-2050)')
    
    # Line of best fit for data from 2000 onward
    data_2000 = data[data['Year'] >= 2000]
    slope_2000, intercept_2000, _, _, _ = linregress(data_2000['Year'], data_2000['CSIRO Adjusted Sea Level'])
    plt.plot(years_extended, intercept_2000 + slope_2000 * years_extended, color='green', label='Best Fit Line (2000-2050)')
    
    # Labels and title
    plt.xlabel('Year')
    plt.ylabel('Sea Level (inches)')
    plt.title('Rise in Sea Level')
    plt.legend()
    
    # Save the figure
    plt.savefig('sea_level_plot.png')
    plt.show()

# Main function to execute the analysis
if __name__ == "__main__":
    data = load_data('epa-sea-level.csv')  # Make sure to provide the correct path to the CSV file
    plot_sea_level(data)
