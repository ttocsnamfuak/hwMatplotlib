{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#Dependencies - Import the stuff you need\n",
    "import matplotlib.pyplot as plt\n",
    "import matplotlib.patches as mpatches\n",
    "import numpy as np\n",
    "import pandas as pd\n",
    "import seaborn as sns\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#Read the files\n",
    "# Read CSV and create data frames\n",
    "city_df = pd.read_csv(\"raw_data/city_data.csv\")\n",
    "rider_df = pd.read_csv(\"raw_data/ride_data.csv\")\n",
    "\n",
    "# Merge our two data frames together\n",
    "#combined_unemployed_data = pd.merge(unemployed_data_one, unemployed_data_two, on=\"Country Name\")\n",
    "#combined_unemployed_data.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#merge the data frames on the common id city\n",
    "\n",
    "new_df = pd.merge(left=city_df, right=rider_df,\n",
    "                 how='left', left_on='city',\n",
    "                 right_on='city')\n",
    "fare_sum = new_df['fare'].sum()\n",
    "fare_type = new_df.groupby(['type']).sum()\n",
    "\n",
    "# fare_type.reset_index('type')\n",
    "# type(fare_type)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "new_df.columns"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "rider_df.columns"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "city_df.columns"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "1. Average Fare ($) Per City\n",
    "\n",
    "2. Total Number of Rides Per City\n",
    "\n",
    "3. Total Number of Drivers Per City\n",
    "\n",
    "4. City Type (Urban, Suburban, Rural)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "avgFares_city = rider_df.groupby('city').mean() # for bubble plot\n",
    "\n",
    "avgFares_city.head()\n",
    "\n",
    "totRiders_city = rider_df.groupby(['city']).count() # for bubble plot\n",
    "totRiders_city\n",
    "\n",
    "cityFareSum_df = rider_df.groupby(['city']).sum()\n",
    "\n",
    "totDrivers_city = city_df.groupby(['city']).sum() # for bubble plot\n",
    "\n",
    "totType_city = city_df.groupby(['type']).count() # fort bubble plot\n",
    "\n",
    "cityByType_df = city_df.groupby(['type'])\n",
    "\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "totRiders_city.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "totType_city"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": []
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#create the bubble chart\n",
    "\n",
    "fares = avgFares_city['fare']\n",
    "#len(fares)\n",
    "riders = totRiders_city['ride_id']\n",
    "#len(riders)\n",
    "size = totType_city['driver_count']\n",
    "\n",
    "colors = [\"gold\",\"lightskyblue\",\"lightcoral\"]\n",
    "\n",
    "# create data\n",
    "x = riders # number of riders per city\n",
    "y = fares # average fare per city\n",
    "z = size # nbr drivers per city size of bubble\n",
    "\n",
    "\n",
    "\n",
    "# Size of bubble is equal to nbr of drivers in a city\n",
    "# Change color with c and alpha. I map the color to the X axis value.\n",
    "plt.scatter(x, y, s=z*8, c=colors, alpha=0.75, edgecolors=\"grey\")\n",
    "\n",
    "# Add titles (main and on axis)\n",
    "plt.grid(True)\n",
    "plt.xlabel(\"Total Number of Rides (Per City)\")\n",
    "plt.ylabel(\"Average Fare ($)\")\n",
    "plt.title(\"Pyber Ride Sharing Data\")\n",
    "\n",
    "red_patch = mpatches.Patch(color='lightcoral')\n",
    "blue_patch = mpatches.Patch(color='lightskyblue')\n",
    "gold_patch = mpatches.Patch(color='gold')\n",
    "\n",
    "plt.legend([red_patch, blue_patch, gold_patch], ['Urban', 'Subburban','Rural']) \n",
    "plt.savefig('bubble.png')\n",
    "plt.close('bubble.png')\n",
    "\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "% of total fares by city type\n",
    "\n",
    "% of total rides by city type\n",
    "\n",
    "% of total drivers by city type"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# % of total fares by city type\n",
    "\n",
    "r = new_df.loc[new_df[\"type\"] == \"Rural\",:]\n",
    "rural = r['fare'].sum()\n",
    "\n",
    "u = new_df.loc[new_df[\"type\"] == \"Urban\",:]\n",
    "urban = u['fare'].sum()\n",
    "\n",
    "s = new_df.loc[new_df[\"type\"] == \"Suburban\",:]\n",
    "subburban = s['fare'].sum()\n",
    "\n",
    "print(f\" rural {rural} urban {urban} sub {subburban}\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#Plot first Pie Chart\n",
    "\n",
    "cityType = [\"Rural\", \"Suburban\", \"Urban\"]\n",
    "pie_votes = [rural,subburban,urban]\n",
    "colors = [\"gold\",\"lightskyblue\",\"lightcoral\"]\n",
    "explode = (0,0,0)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Tell matplotlib to create a pie chart based upon the above data\n",
    "plt.pie(pie_votes, explode=explode, labels=cityType, colors=colors,\n",
    "        autopct=\"%1.1f%%\", shadow=True, startangle=140)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Create axes which are equal so we have a perfect circle\n",
    "plt.axis(\"equal\")\n",
    "\n",
    "plt.title(\"% of Total Fares by City Type\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#save and show the graph\n",
    "plt.savefig('pie1.png')\n",
    "plt.close('pie1.png')\n",
    "\n",
    "\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#total fare by city type\n",
    "\n",
    "# total_fares = rider_df['fare'].sum()\n",
    "# total_fares\n",
    "\n",
    "#cityFareSum_df = rider_df.groupby(['city']).sum()\n",
    "\n",
    "#cityFareSum_df.head()\n",
    "#type_df = pd.DataFrame()\n",
    "# cityByType_df = city_df.groupby(['type'])\n",
    "# cityByType_df.columns\n",
    "# type_df = cityByType_df[\"type\", 'city']\n",
    "# type_df.head()\n",
    "\n",
    "# #riderByCity_df = rider_df.groupby(['city'])\n",
    "# #riderByCity_df.head()\n",
    "\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# rider_df['type']=city_df['type']\n",
    "# rider_df"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# #Merge the dataframes into a new df\n",
    "# mCityRide_df = pd.merge(cityByType_df, riderByCity_df)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#Merge dataframes\n",
    "\n",
    "#Merge the dataframes into a new df\n",
    "# merged_df = pd.merge(city_df, rider_df)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# #count of type\n",
    "# totFaresType_df = merged_df.groupby(['type']).count()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# totFaresType_df.head()\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# #count of rides\n",
    "# totridesType_df = merged_df.groupby(['type']).count()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# % of total rides by city type\n",
    "\n",
    "r2 = new_df.loc[new_df[\"type\"] == \"Rural\",:]\n",
    "ruralRider = r2['fare'].count()\n",
    "\n",
    "u2 = new_df.loc[new_df[\"type\"] == \"Urban\",:]\n",
    "urbanRider = u2['fare'].count()\n",
    "\n",
    "s2 = new_df.loc[new_df[\"type\"] == \"Suburban\",:]\n",
    "subburbanRider = s2['fare'].count()\n",
    "\n",
    "print(f\" rural {ruralRider} urban {urbanRider} sub {subburbanRider}\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#Plot second Pie Chart\n",
    "\n",
    "cityType = [\"Rural\", \"Suburban\", \"Urban\"]\n",
    "pie_votes = [ruralRider,subburbanRider,urbanRider]\n",
    "colors = [\"gold\",\"lightskyblue\",\"lightcoral\"]\n",
    "explode = (0,0,0)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Tell matplotlib to create a pie chart based upon the above data\n",
    "plt.pie(pie_votes, explode=explode, labels=cityType, colors=colors,\n",
    "        autopct=\"%1.1f%%\", shadow=True, startangle=140)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Create axes which are equal so we have a perfect circle\n",
    "plt.axis(\"equal\")\n",
    "\n",
    "plt.title(\"% of Total Rides by City Type\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#show the graph\n",
    "plt.savefig('pie2.png')\n",
    "plt.close('pie2.png')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "new_df.columns"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# % of total drivers by city type\n",
    "\n",
    "r3 = new_df.loc[new_df[\"type\"] == \"Rural\",:]\n",
    "ruralDrive = r3['driver_count'].sum()\n",
    "\n",
    "u3 = new_df.loc[new_df[\"type\"] == \"Urban\",:]\n",
    "urbanDrive = u3['driver_count'].sum()\n",
    "\n",
    "s3 = new_df.loc[new_df[\"type\"] == \"Suburban\",:]\n",
    "subburbanDrive = s3['driver_count'].sum()\n",
    "\n",
    "print(f\" rural {ruralDrive} urban {urbanDrive} sub {subburbanDrive}\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#Plot third Pie Chart\n",
    "\n",
    "cityType = [\"Rural\", \"Suburban\", \"Urban\"]\n",
    "pie_votes = [ruralDrive,subburbanDrive,urbanDrive]\n",
    "colors = [\"gold\",\"lightskyblue\",\"lightcoral\"]\n",
    "explode = (0,0,0)\n",
    "\n",
    "# Tell matplotlib to create a pie chart based upon the above data\n",
    "plt.pie(pie_votes, explode=explode, labels=cityType, colors=colors,\n",
    "        autopct=\"%1.1f%%\", shadow=True, startangle=140)\n",
    "\n",
    "# Create axes which are equal so we have a perfect circle\n",
    "plt.axis(\"equal\")\n",
    "\n",
    "# Place a legend on the chart in what matplotlib believes to be the \"best\" location\n",
    "#plt.legend(loc=\"best\")\n",
    "\n",
    "plt.title(\"% of Total Drivers by City Type\")\n",
    "#plt.xlabel(\"Years\")\n",
    "#plt.ylabel(\"Number of Wins/Losses\")\n",
    "\n",
    "\n",
    "\n",
    "\n",
    "#save and show the graph\n",
    "plt.savefig('pie3.png')\n",
    "plt.close('pie3.png')\n",
    "plt.show()\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": []
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.3"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
