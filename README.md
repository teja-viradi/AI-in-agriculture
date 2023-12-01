# AI-in-agriculture
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Importing the Libraries"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {},
   "outputs": [],
   "source": [
    "# for manipulations\n",
    "import numpy as np\n",
    "import pandas as pd\n",
    "\n",
    "# for data visualizations\n",
    "import matplotlib.pyplot as plt\n",
    "import seaborn as sns\n",
    "plt.style.use('fivethirtyeight')\n",
    "\n",
    "# for interactivity\n",
    "import ipywidgets\n",
    "from ipywidgets import interact"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Reading the Dataset"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Shape of the Dataset : (2200, 8)\n"
     ]
    }
   ],
   "source": [
    "# lets read the dataset\n",
    "data = pd.read_csv('data.csv')\n",
    "\n",
    "# lets check teh shape of the dataset\n",
    "print(\"Shape of the Dataset :\", data.shape)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>N</th>\n",
       "      <th>P</th>\n",
       "      <th>K</th>\n",
       "      <th>temperature</th>\n",
       "      <th>humidity</th>\n",
       "      <th>ph</th>\n",
       "      <th>rainfall</th>\n",
       "      <th>label</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>90</td>\n",
       "      <td>42</td>\n",
       "      <td>43</td>\n",
       "      <td>20.879744</td>\n",
       "      <td>82.002744</td>\n",
       "      <td>6.502985</td>\n",
       "      <td>202.935536</td>\n",
       "      <td>rice</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>85</td>\n",
       "      <td>58</td>\n",
       "      <td>41</td>\n",
       "      <td>21.770462</td>\n",
       "      <td>80.319644</td>\n",
       "      <td>7.038096</td>\n",
       "      <td>226.655537</td>\n",
       "      <td>rice</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>60</td>\n",
       "      <td>55</td>\n",
       "      <td>44</td>\n",
       "      <td>23.004459</td>\n",
       "      <td>82.320763</td>\n",
       "      <td>7.840207</td>\n",
       "      <td>263.964248</td>\n",
       "      <td>rice</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>74</td>\n",
       "      <td>35</td>\n",
       "      <td>40</td>\n",
       "      <td>26.491096</td>\n",
       "      <td>80.158363</td>\n",
       "      <td>6.980401</td>\n",
       "      <td>242.864034</td>\n",
       "      <td>rice</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>78</td>\n",
       "      <td>42</td>\n",
       "      <td>42</td>\n",
       "      <td>20.130175</td>\n",
       "      <td>81.604873</td>\n",
       "      <td>7.628473</td>\n",
       "      <td>262.717340</td>\n",
       "      <td>rice</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    N   P   K  temperature   humidity        ph    rainfall label\n",
       "0  90  42  43    20.879744  82.002744  6.502985  202.935536  rice\n",
       "1  85  58  41    21.770462  80.319644  7.038096  226.655537  rice\n",
       "2  60  55  44    23.004459  82.320763  7.840207  263.964248  rice\n",
       "3  74  35  40    26.491096  80.158363  6.980401  242.864034  rice\n",
       "4  78  42  42    20.130175  81.604873  7.628473  262.717340  rice"
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# lets check the head of the dataset\n",
    "data.head()"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "## Description for each of the columns in the Dataset\n",
    "\n",
    "N - ratio of Nitrogen content in soil\n",
    "P - ratio of Phosphorous content in soil\n",
    "K - ration of Potassium content in soil\n",
    "temperature - temperature in degree Celsius\n",
    "humidity - relative humidity in %\n",
    "ph - ph value of the soil\n",
    "rainfall - rainfall in mm"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "N              0\n",
       "P              0\n",
       "K              0\n",
       "temperature    0\n",
       "humidity       0\n",
       "ph             0\n",
       "rainfall       0\n",
       "label          0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# lets check if there is any missing value present in the dataset\n",
    "data.isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "pomegranate    100\n",
       "lentil         100\n",
       "coffee         100\n",
       "mothbeans      100\n",
       "grapes         100\n",
       "orange         100\n",
       "muskmelon      100\n",
       "watermelon     100\n",
       "blackgram      100\n",
       "banana         100\n",
       "cotton         100\n",
       "kidneybeans    100\n",
       "jute           100\n",
       "pigeonpeas     100\n",
       "papaya         100\n",
       "coconut        100\n",
       "mango          100\n",
       "apple          100\n",
       "rice           100\n",
       "maize          100\n",
       "chickpea       100\n",
       "mungbean       100\n",
       "Name: label, dtype: int64"
      ]
     },
     "execution_count": 6,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# lets check the Crops present in this Dataset\n",
    "data['label'].value_counts()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Descriptive Statistics"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Average Ratio of Nitrogen in the Soil : 50.55\n",
      "Average Ratio of Phosphorous in the Soil : 53.36\n",
      "Average Ratio of Potassium in the Soil : 48.15\n",
      "Average Tempature in Celsius : 25.62\n",
      "Average Relative Humidity in % : 71.48\n",
      "Average PH Value of the soil : 6.47\n",
      "Average Rainfall in mm : 103.46\n"
     ]
    }
   ],
   "source": [
    "# lets check the Summary for all the crops\n",
    "\n",
    "print(\"Average Ratio of Nitrogen in the Soil : {0:.2f}\".format(data['N'].mean()))\n",
    "print(\"Average Ratio of Phosphorous in the Soil : {0:.2f}\".format(data['P'].mean()))\n",
    "print(\"Average Ratio of Potassium in the Soil : {0:.2f}\".format(data['K'].mean()))\n",
    "print(\"Average Tempature in Celsius : {0:.2f}\".format(data['temperature'].mean()))\n",
    "print(\"Average Relative Humidity in % : {0:.2f}\".format(data['humidity'].mean()))\n",
    "print(\"Average PH Value of the soil : {0:.2f}\".format(data['ph'].mean()))\n",
    "print(\"Average Rainfall in mm : {0:.2f}\".format(data['rainfall'].mean()))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.jupyter.widget-view+json": {
       "model_id": "22a562b9d7c843abbf5d86853fd10092",
       "version_major": 2,
       "version_minor": 0
      },
      "text/plain": [
       "interactive(children=(Dropdown(description='crops', options=('pomegranate', 'lentil', 'coffee', 'mothbeans', '…"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "# lets check the Summary Statistics for each of the Crops\n",
    "\n",
    "@interact\n",
    "def summary(crops = list(data['label'].value_counts().index)):\n",
    "    x = data[data['label'] == crops]\n",
    "    print(\"---------------------------------------------\")\n",
    "    print(\"Statistics for Nitrogen\")\n",
    "    print(\"Minimum Nitrigen required :\", x['N'].min())\n",
    "    print(\"Average Nitrogen required :\", x['N'].mean())\n",
    "    print(\"Maximum Nitrogen required :\", x['N'].max()) \n",
    "    print(\"---------------------------------------------\")\n",
    "    print(\"Statistics for Phosphorous\")\n",
    "    print(\"Minimum Phosphorous required :\", x['P'].min())\n",
    "    print(\"Average Phosphorous required :\", x['P'].mean())\n",
    "    print(\"Maximum Phosphorous required :\", x['P'].max()) \n",
    "    print(\"---------------------------------------------\")\n",
    "    print(\"Statistics for Potassium\")\n",
    "    print(\"Minimum Potassium required :\", x['K'].min())\n",
    "    print(\"Average Potassium required :\", x['K'].mean())\n",
    "    print(\"Maximum Potassium required :\", x['K'].max()) \n",
    "    print(\"---------------------------------------------\")\n",
    "    print(\"Statistics for Temperature\")\n",
    "    print(\"Minimum Temperature required : {0:.2f}\".format(x['temperature'].min()))\n",
    "    print(\"Average Temperature required : {0:.2f}\".format(x['temperature'].mean()))\n",
    "    print(\"Maximum Temperature required : {0:.2f}\".format(x['temperature'].max()))\n",
    "    print(\"---------------------------------------------\")\n",
    "    print(\"Statistics for Humidity\")\n",
    "    print(\"Minimum Humidity required : {0:.2f}\".format(x['humidity'].min()))\n",
    "    print(\"Average Humidity required : {0:.2f}\".format(x['humidity'].mean()))\n",
    "    print(\"Maximum Humidity required : {0:.2f}\".format(x['humidity'].max()))\n",
    "    print(\"---------------------------------------------\")\n",
    "    print(\"Statistics for PH\")\n",
    "    print(\"Minimum PH required : {0:.2f}\".format(x['ph'].min()))\n",
    "    print(\"Average PH required : {0:.2f}\".format(x['ph'].mean()))\n",
    "    print(\"Maximum PH required : {0:.2f}\".format(x['ph'].max()))\n",
    "    print(\"---------------------------------------------\")\n",
    "    print(\"Statistics for Rainfall\")\n",
    "    print(\"Minimum Rainfall required : {0:.2f}\".format(x['rainfall'].min()))\n",
    "    print(\"Average Rainfall required : {0:.2f}\".format(x['rainfall'].mean()))\n",
    "    print(\"Maximum Rainfall required : {0:.2f}\".format(x['rainfall'].max()))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.jupyter.widget-view+json": {
       "model_id": "10ad6ccf7598439e98e15c2b06be683d",
       "version_major": 2,
       "version_minor": 0
      },
      "text/plain": [
       "interactive(children=(Dropdown(description='conditions', options=('N', 'P', 'K', 'temperature', 'ph', 'humidit…"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "## Lets compare the Average Requirement for each crops with average conditions\n",
    "\n",
    "@interact\n",
    "def compare(conditions = ['N','P','K','temperature','ph','humidity','rainfall']):\n",
    "    print(\"Average Value for\", conditions,\"is {0:.2f}\".format(data[conditions].mean()))\n",
    "    print(\"----------------------------------------------\")\n",
    "    print(\"Rice : {0:.2f}\".format(data[(data['label'] == 'rice')][conditions].mean()))\n",
    "    print(\"Black Grams : {0:.2f}\".format(data[data['label'] == 'blackgram'][conditions].mean()))\n",
    "    print(\"Banana : {0:.2f}\".format(data[(data['label'] == 'banana')][conditions].mean()))\n",
    "    print(\"Jute : {0:.2f}\".format(data[data['label'] == 'jute'][conditions].mean()))\n",
    "    print(\"Coconut : {0:.2f}\".format(data[(data['label'] == 'coconut')][conditions].mean()))\n",
    "    print(\"Apple : {0:.2f}\".format(data[data['label'] == 'apple'][conditions].mean()))\n",
    "    print(\"Papaya : {0:.2f}\".format(data[(data['label'] == 'papaya')][conditions].mean()))\n",
    "    print(\"Muskmelon : {0:.2f}\".format(data[data['label'] == 'muskmelon'][conditions].mean()))\n",
    "    print(\"Grapes : {0:.2f}\".format(data[(data['label'] == 'grapes')][conditions].mean()))\n",
    "    print(\"Watermelon : {0:.2f}\".format(data[data['label'] == 'watermelon'][conditions].mean()))\n",
    "    print(\"Kidney Beans: {0:.2f}\".format(data[(data['label'] == 'kidneybeans')][conditions].mean()))\n",
    "    print(\"Mung Beans : {0:.2f}\".format(data[data['label'] == 'mungbean'][conditions].mean()))\n",
    "    print(\"Oranges : {0:.2f}\".format(data[(data['label'] == 'orange')][conditions].mean()))\n",
    "    print(\"Chick Peas : {0:.2f}\".format(data[data['label'] == 'chickpea'][conditions].mean()))\n",
    "    print(\"Lentils : {0:.2f}\".format(data[(data['label'] == 'lentil')][conditions].mean()))\n",
    "    print(\"Cotton : {0:.2f}\".format(data[data['label'] == 'cotton'][conditions].mean()))\n",
    "    print(\"Maize : {0:.2f}\".format(data[(data['label'] == 'maize')][conditions].mean()))\n",
    "    print(\"Moth Beans : {0:.2f}\".format(data[data['label'] == 'mothbeans'][conditions].mean()))\n",
    "    print(\"Pigeon Peas : {0:.2f}\".format(data[(data['label'] == 'pigeonpeas')][conditions].mean()))\n",
    "    print(\"Mango : {0:.2f}\".format(data[data['label'] == 'mango'][conditions].mean()))\n",
    "    print(\"Pomegranate : {0:.2f}\".format(data[(data['label'] == 'pomegranate')][conditions].mean()))\n",
    "    print(\"Coffee : {0:.2f}\".format(data[data['label'] == 'coffee'][conditions].mean()))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.jupyter.widget-view+json": {
       "model_id": "51bc5659482b4da7b2cd760db25e20ba",
       "version_major": 2,
       "version_minor": 0
      },
      "text/plain": [
       "interactive(children=(Dropdown(description='conditions', options=('N', 'P', 'K', 'temperature', 'ph', 'humidit…"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "# lets make this funtion more Intuitive\n",
    "\n",
    "@interact\n",
    "def compare(conditions = ['N','P','K','temperature','ph','humidity','rainfall']):\n",
    "    print(\"Crops which require greater than average\", conditions,'\\n')\n",
    "    print(data[data[conditions] > data[conditions].mean()]['label'].unique())\n",
    "    print(\"----------------------------------------------\")\n",
    "    print(\"Crops which require less than average\", conditions,'\\n')\n",
    "    print(data[data[conditions] <= data[conditions].mean()]['label'].unique())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Analyzing Agricultural Conditions"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAA30AAAHfCAYAAADkwbF1AAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADh0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uMy4yLjEsIGh0dHA6Ly9tYXRwbG90bGliLm9yZy+j8jraAAAgAElEQVR4nOzdeZxcVZn4/89TXV29Jb1lTzqQhECAoGwJ4IZRRCBfkRm/EYMrgiLfgREdZ0Zl+CoOLnxdxsGJo78GGWQGCYw4yoyAItAgQggkIQFCQkISk87WSXpL77U8vz/OrU6luqq7umvvft6vV72669577j33VlV3Pfec8xxRVYwxxhhjjDHGjE++fFfAGGOMMcYYY0z2WNBnjDHGGGOMMeOYBX3GGGOMMcYYM45Z0GeMMcYYY4wx45gFfcYYY4wxxhgzjlnQZ4wxxhhjjDHjmAV9xpiME5EmEcnbfDAico+IqIjMi1k2z1t2T77q5dUjr9cmGRF5v4g8JyJt3nX6db7rlE/eNWjK8TEL8r2RChHZJSK78l2PbEv0GonIMu/9cuso9zUhrpkxpjBY0GeMScj7EhP76BeRQyKyXkTuEpHLRKQkS8cu2i9DiQLOQufV9TfAfODfgG8Aq/NYJURkroiEvWv57XzWJZ/GGlAUC+91vl1E1nk3HIIi0iIifxCRm0SkJt91HKtiDuKNMeOPP98VMMYUvG94P0uAWmAx8AngWuAlEfmYqr4RV+aTQGXuqjjEV4Hbgb15rEMy+b42ibwPKAe+pKq/yHdlPJ/B3ZhU4NMi8jVVDeXw+KcBPTk83oQjIp8BVgFlwEbgfqANmAK8E/hn4P8CU/NVxxStxb1fDo+y3EVZqIsxxiRkQZ8xZliqemv8MhGZAfwL8GHgDyKyRFVbYsrszl0Nh1LV/cD+fNYhmXxfmyRmez/35bUWHq8F+RqgE7gP+D/AB4Ff5aoOqrolV8eaiETko8CduCDvf6vqbxNs8w7gx7mu22ipag8w6veLqr6ZheoYY0xC1r3TGDNqqnoQWAk0AXOBm2PXJxn3IiLyKW/c2CER6RORPSLyOxH5iLfNMq/cicCJcd1L74nZl3rHmOl1Nd3rdQW82ls/bBdLETlVRH4tIq0i0i0iz4rI+xNsd6u3n2UJ1g0ZI+jV/VPe050xdd813LXxlvtE5HoReVFEurx6vSgi/0dEhvytjrkGU0WkUUT2e11wXxORTyc67wT7iF7vaGvuUzF1Xhaz3ckicq93nQdEZJ/3/OThrpmIfFREXvDOZ1f8tsO4DGgAHgD+1Vv22WHOQ7yugJu999VeEVklIjWJugqLyNVeHa8WkUu969gR+7pIkjF9IlLivU5/8sr0ish27314csx2Sd+DkmKXTe+99ZT39Otxn4dl3jajeo/G1W2BiPy1iGzyzqPJWx8QkRtF5BER+bP3vmoV1+XysuHqnAoRmYy7aQSwMlHAB6CqfwLOT1D+IhF5zKtTn4i8Ia6L6JCuoNHPm4j4ReRmEdnmnc8eEfl/IhJIUseV4rqc9orrbvrvIjI7ybbHvZ7R6w6823se+7o1xZRL2I1dRMpE5Cve69IjIp0i8kcRuTLBtoOvsff7ahE57F2Xl0TkAwnKBETk8+K66rd5x9glIr8RkfclOkdjTPGzlj5jzJioakREvgksA64SkS+q6nDjV76F63a5E3gQ6ABmAUtxLYYPALtwAcgXvDL/HFP+5bj91QNrgC5cC1AEOJhC1ecDzwOvAv+fV4ePAI+KyEdV9YEU9pHMN4C/AM4E7gDaveXtSUsc8+/AR4E9wF24bo1/iQt63gl8LEGZWuBPwADwS1wXzRXA3SISUdWfj3DMXV6dl+G+oP7cWxZdh4gsBf4ATAYeBjYDp3r1uUJELlLVlxLs+0vAxcB/4wKX0YzNus77eY+qvioi64H3i8iJqvrnBNv/GNcauA9oxF2PDwLnAaVAMMlxVgCXAo8CPwXmDVcpL0D4La477B7gF7jWyHm41+pZYFtKZ5iaaDKdTwFP426yRO3KwP7vAN6FO6dHgLC3vN5b9xzwOHAI9zm5HHhERD6rqnelcdwV3jHWqOrvh9tQVftjn4vI54CfAN3AfwItuPfvl4HLReQdqpro8/YL3Lk+invNlgN/D0wHjrtJIiJfBP4J97m91/t5Ce56dKRwfu24z9XVuBtY34hZt2u4gt577He4z+MW3Hu7EnfNHhCRs1T15gRFT8R1M92B+1tSj/u79hsReZ+qPhWz7T3AVbi/gfcCvbjW/nfiPg9/SOEcjTHFRlXtYQ972GPIAxd06AjblOG+UCswP2Z5U3xZ4AjQDFQm2M/UuOe7gF0j1Q33hcWfYP093vp5McvmxZT7Xtz2S7zzaAOqY5bf6m2/LMExovu7Z6Rjx61PdG2u8sqsBybFLK8CXvLWfTTJNbgLKIlZfjoQAjaP4rVOeJ6AAK976z4Wt+4j3vItgC/BvrqBs8fwvpvj1X9rzLK/9vZ5W4Lt3+Wt2wrUxiwPAM9463bFlbnaWx4BLh3mPdYUt+zb3vKHgbIEn4VpqbwPcEGKArem8N5IuG2G3qN7ifncxp1LQ4LlNbhAoRWoGM1nNm7bn3nH/+Yo3xsnAv24oO3UuHX/6u2zMdE1BdYB9XGfre24QHdm3DXr984x9u+HD3iIBH8XR/N6jnTNcDfGFBeE+2OWT/e2V+DtCV5jBb4et69LovuKew0juL8rJQnqNGU0r4k97GGP4nlY905jzJipuwt/xHs6LYUiQY61JsTuZ7QJEMC15vytjj65Rwfwj3HHfwk3dqwW12KTa9d4P7+iql0x9erGtWCAS2wSrwf4G1UNx5TZjGv9O83rRpeOt+Na9Z5X1ftiV6hrEX0WWIRrIYjXqKobxnDMa3FJg+6JWfYL3Ot9jQzNGBvtTvstjWnhUdUB3Bfo4fxGVR9LpVLecf8K1ypyvca1QKlqv6oeSmVfBeS7qrozfqF3Ls0JlncAdwN1uBb6sZrl/RxyjBF8HBfMr9KhYy7/ATgKfEJEyhKU/bKqtkafeJ+t+3DB3JKY7T7mHeNfVHVXzPYR4O9wAVM2XYML1P4m9m+bujHTt3lPE/0t+DPwzdgFqvo7YDeuxXtwMe5mTj8JzkVVj8QvM8aMDxb0GWPSJd7PkVKT34e7K/2aiHzHG0uVTjr2XRqTPGYU1qvq0QTLm7yfZ4+9SmN2Du4LWFOCdU/jAuVE9dqmqp0Jlu/xftZmoF4ATyZZH12eqG5rR3swcWMXr8Fdi3ujy70vov+D64L2v+KKRY/9bIJdrsG1GiYzmjqeimsl2aSqBZHwJgOSnr+ILPbGie3wxrWpN07tB94mc9I4bqp/M+IlfT+qahuwAdfF+dQEZRN1QY5+TuoSHOPpBMfYEVMm47ybNAuBfQmCWhj+8/Zy7M2fGHuIOT/v78V/427ovCwiXxOR94hIoWUUNsZkmAV9xpgxE5Fy3NgRcON+hvNF3Fi9buAruLE1h73kAQvHcPgDYygDycf9RfeXj3nBaoBWr3XqON7d/sMkrleysYLRQCfdeRSjx0yWCTW6PFFwOZbX5xJcF77HVTV+uo1/835eF7c8Wschr6v3JXi4lovR1DF6joU4DchYJTx/EbkAeBE3xnQrbuzrbbixab/xNkvUmpaqaNDcMMpyY34/auJxfok+J0nfT56x/t1JRTqft+H+FsR/1/sI7rWs8H4+CRzxktXMSL26xphiYkGfMSYd78QlhDoY2xUqEVUNq+odqnomMAP438B/4RJuPJakS9awuxxDffGOnchM72dsooZo96dESa/SbUWL1QHUi0hp/AoR8ePmKUvUopdt0WsxM8n6WXHbxRrL6xMN6C6Jy3iouNYJgEtFZG5Mmeh1GfK6el0ypwxzvNHUMfqlOtUWrly9d9I5TrLzvwUXELxfVS9T1S+o6tfUTd/ywtiqeZxoq+xo56lL5/042mOM9HciG3Jxfqhqr6reqqqnACfgus0+6/38ZTr7NsYULgv6jDFj4nXF+wfv6agm9FbVFlX9lapeibvLfBJwRswmYdJvpUrmnCRj3ZZ5P2PHobV5P+cy1JIEy+DYmMXR1H8D7u/xhQnWXejta/0o9pcp0WuxLMn66PK06yYiM4EP4IK4nyV5/Al3La6JKRqtY6JxhReQuSzVW3CB31uTpe6PM5b3TiIjvZ8ydZxYC3Etz00J1r17jPuM9UtcopS3jTRFQNzNoKTvRxGpBc4C+nDJh8Yq+l4ecp4isoDE1zmZsFcupb8FXrfzN4E5kmA6FOA9cXVMm6ru8cbrXoLLPPtOERnuRokxpkhZ0GeMGTURmQ6sxn352o3Lajjc9mXe3FoSt7yUY91De2JWHQGmiUhFxip9TA3wtbh6LMElcOjAtT5GRcc8fdprcYtuPzd+HzGi3QlPGEWd7vZ+fid2bI33++3e05+NYn+Z8idc9753isiK2BXe8wuBN0g8nm60rsEFaPep6mcSPTiWdfNaOTZ3YXTs3z/EjhH1Ut8P+74cDa+r6L/iWsB+Gt8y7c19FpvMKPre+Wzcdm8BbhrFoUd6P43lPTqSXbiW57fGLhSRa3HBQVq84Obz3tMHRCThPr1ups/HLPoPXDKov07QJfw2oBr4j/gkO6N0X8wx5sXUxQd8j9F9bxrr3wIBvhcbLIrIVOD/xmwzJiIyTUSGzH2Iy2Y6GdcddEg3c2NM8bN5+owxw5JjE0j7cN3FFuNaVQK4L5wfSyH7ZgVu7qddIvICLtNcOW4et9OAh1U19u78E7jsgI+JyDO4THMbVfW/Sd8zwGe8Lz5/4tg8fT7gc7GJUVT1Be/4FwJrReRJXLevy3FzaSW66/8ELsvfnSLyS9w8gu2quipZhVT1FyJyBXAlLtHNr3HBzV/g5hV8MD57Zi6oqorIp3BztT0gIr/BtXgt8up2FPikl9lwzLybAdd6T5PO/6aq20XkadzNhsuA36rq0yLSiOsa+pqIPIT70n45LojfR+YyLn4DN1n45cAbIvI/uGswF3g/7nW/x9v2N7iWk6tEpAHXLfIE4Apv3ZCJtpPYihtHuFJEBnA3WRT4d1X98xjfoyP5Z1xw96yIROfUXIL73P8SN2dcWlT1Pu+mzirc5/xl3Dx4bbguuW/DzXd5OKbMLhH5Am7uuvVe3Q7hWuXehntvfpk0eMf4Ci5hzQYReQB3/pfg/v5tAt46zC5iPYGbg/RXIvIILvPrn1X134cp833ce/sKYKNXrtLbz3RcxtV0brLMAdaIyOu4FsM9uGD5A7hupT9KkujKGFPs8j1nhD3sYY/CfHBs7qfoox/3BWwdcCduEl9fkrJNxMxPhZsg++9xyVt247pgHcJlV7weCMSVr8JNwNyMu/N83FxjJJhDLa78PSSfp+8eXKD5G9wXzB5c8HdJkn3Veufb4l2DV3EBxrz4esWU+RtcF7N+4uaJi782Mct9uCkBXvLq1ONd6xsSXefhrkGi8x/htb6VJHO9eesX4SZ83o8LqPbjWl0WjXZfSfZ/sVdmfQrbftTb9jdx1+6LuC/9/bhA78e4Vt2juMyGsfu42tvH1SO8/4dcX9zN0htxNzy6cImJtuEmhV8Yt+1c4AFcV8ZeXHKUDzHKed1wN0CewAUfkfjrO9r3aCrvD1wQsMa7fu3A73GBZcJrxyjm6Utwjf4fLgBp995fh4CncImfqhOUeb9XnzbvfLcD3yVmnsaRrulI7wPc3JnrOfa36j9w2WOH7G+Y17ME19q8g2PzmTbFrE94zXA3xG72Xsde7zV4FrgqwbYJX+Nk5++9V76G61a/17t++73trgJktK+hPexhj+J4iOpYcyEYY4wxhcsbF/UGsFpVr8p3fYwxxph8sTF9xhhjipqIzIwZ4xddVonrpgjHj9M0xhhjJhwb02eMMabYfQE3dq4J11VtJm46gAZcl+L/zF/VjDHGmPyzoM8YY0yxexyX9OP9uGywIVy3zh8B/6w2jsEYY8wEZ2P6jDHGGGOMMWYcszF9xhhjjDHGGDOOWdBnjDHGGGOMMeOYBX3GGGOMMcYYM45Z0GeMMcYYY4wx45gFfcYYY4wxxhgzjlnQZ4wxxhhjjDHjmAV9xhhjjDHGGDOOWdBnjDHGGGOMMeOYBX3GGGOMMcYYM45Z0GeMMcYYY4wx45gFfcYYY4wxxhgzjlnQZ4wxxhhjjDHjmAV9xhhjjDHGGDOOWdBnjDHGGGOMMeOYBX3GGGOMMcYYM45Z0GeMMcYYY4wx45gFfcYYY4wxxhgzjlnQZ4wxxhhjjDHjmAV9xhhjjDHGGDOOWdBnjDHGGGOMMeOYBX3GGGOMMcYYM45Z0GeMMcYYY4wx45gFfcYYY4wxxhgzjlnQZ4wxxhhjjDHjmAV9xhhjjDHGGDOOWdBnjDHGGGOMMeOYBX3GGGOMMcYYM45Z0GeMMcYYY4wx45gFfcYYY4wxxhgzjlnQZ4wxxhhjjDHjmD+VjUTkUuAOoAS4S1Vvj1sv3vrlQA9wtaqu99bdDXwAaFHVM2LKPAAs8p7WAu2qepaIzANeB7Z669ao6vXD1W/q1Kk6b968VE4lo7q7u6mqqsr5cceqmOpbTHWF7NR33bp1h1V1WkZ3mqZsfdaK7fWOsnrnTjbrXGiftXQ+Z4X22lp9hldo9YHs1Wk8fc5SVYivbzyrY/oKqX7Dfs5UddgHLtB7E1gABICNwOlx2ywHHgUEuAB4IWbdhcA5wKvDHOMHwNe83+cNt22ix7nnnqv58NRTT+XluGNVTPUtprqqZqe+wEs6is9BLh7Z+qwV2+sdZfXOnWzWudA+a+l8zgrttbX6DK/Q6qOavTqNp89Zqgrx9Y1ndUxfIdVvuM9ZKt07zwO2q+oOVR0AVgNXxG1zBXCvd7w1QK2IzPKCymeA1mQ791oJrwTuT6EuxhhjjDHGGGNGIZWgbw6wJ+Z5s7dstNsk8y7goKpui1k2X0Q2iMjTIvKuFPdjjDHGGGOMMSZOKmP6JMEyHcM2yVzF8a18+4ETVPWIiJwL/FpEFqtq53EHFLkOuA5gxowZNDU1pXi4zOnq6srLcceqmOpbTHWF4quvMcYYY4yZOFIJ+pqBuTHPG4B9Y9hmCBHxAx8Czo0uU9V+oN/7fZ2IvAmcArwUW1ZVG4FGgCVLluiyZctSOJXMampqIh/HHatiqm8x1RWKr77GGGNMoUkhceCpwL/hckX8g6p+P9Wyxkx0qXTvfBE4WUTmi0gAWAk8HLfNw8AnxbkA6FDV/Sns+33AFlVtji4QkWkiUuL9vgA4GdiRwr6MMcYYY0wR8r77/Ri4DDgduEpETo/brBX4PPD9MZQ1ZkIbMehT1RBwI/A73FQKD6rqayJyvYhEp1J4BBeYbQfuBP4qWl5E7geeBxaJSLOIXBuz+5UMTeByIbBJRDYCvwSuV9WkiWCMMcYYY0zRGzFxoKq2qOqLQHC0ZY2Z6FKap09VH8EFdrHLfhrzuwI3JCl71TD7vTrBsoeAh1KplzHGGGOMGRcSJQU8PwdljZkQUgr6TP60tg7fyFlfX5+jmhhjhvPy4b6Ey8+aWp7jmhgzssbGjUnXXXfdmTmsiTGD0kkKmFLZXCcBLIYkb5mq4+HDh497PnXq1LT3GVXo17HQ6xdlQZ8xxhhjjMm3MSUFHE3ZXCcBLIYkb5mqY2Nj43HPV6xYkfY+owr9OhZ6/aJSSeRijDHGGGNMNqWSODAbZY2ZECzoM6ZAPPbYYyxatIiFCxdy++1DM0172XF/JCLbRWSTiJwTs+5uEWkRkVfjyjwgIi97j10i8rK3fJ6I9Mas+2n88YwxxphcSSVxoIjMFJFm4G+AW7wEgdXJyubnTIwpTNa905gCEA6HueGGG3j88cdpaGhg6dKlAPGDwS7DTWFyMm6A+k84NlD9HmAVcG9sAVX9SPR3EfkB0BGz+k1VPSuT52GMMcaMVQqJAw/gum6mVNYYc4y19BlTANauXcvChQtZsGABgUCAlStXAtTGbXYFcK86a4BaEZkFoKrP4OYvSkhEBLiSoVOkGGOMMcaYcc6CPmMKwN69e5k799gY9IaGBoBA3GaJUlLPSfEQ7wIOquq2mGXzRWSDiDwtIu8afa2NMcYYY0wxsO6dxhQAN9Xl0MVxz9NJZ30Vx7fy7QdOUNUjInIu8GsRWayqnfEFc5HiuljSHceLrXdvKPFL0e5P9LLlVzFe72KsszHGGFMoLOgzpgA0NDSwZ8+xRrzm5maAYNxmY0pnLSJ+4EPAudFlqtoP9Hu/rxORN4FTgJfiy+cixXWxpDuOF1vvYpqnrxivdzHW2RhjjCkU1r3TmAKwdOlStm3bxs6dOxkYGGD16tUA7XGbPQx80svieQHQoar7U9j9+4AtqtocXSAi00SkxPt9AS45zI6MnIwxRUpELhWRrV6G3K8kWJ8wg66IlIvIWhHZKCKvicg3YsrUi8jjIrLN+1mXy3Myxph8aGxsHHyYwmBBnzEFwO/3s2rVKi655BJOO+00rrzySoC+2FTVuKxkO4DtwJ3AX0XLi8j9wPPAIi+F9bUxu1/J0AQuFwKbRGQj8EvgelVNmgjGmPHOuwnyY1yW3NOBq0Tk9LjNYjPoXofLoAuu1fy9qnomcBZwqXdjBuArwBOqejLwhPfcGGOMySnr3mlMgVi+fDnLly8ffH7LLbfEp6pW4IZEZVX1qmT7VdWrEyx7CHgonfoaM86cB2xX1R0AIrIalzF3c8w2gxl0gTUiUisis7wW9y5vm1LvoTFllnm//xxoAr6cxfMwxhhjhrCgzxhjjEmcHff8FLaZA+z3WgrXAQuBH6vqC942M6LdsFV1v4hMz0bljTGmEDz55JP88Y9/pLKykuuuu46ampp8V8l4LOgzxhhjUsuOm3QbVQ0DZ4lILfBfInKGqr6a8sEzlCU31Syn9fW9Sdc1NbWN6djp1CdXrD4jK8Q6meLw2muv8cADDzB//nz27NnDd7/7XW6++eZ8V8t4LOgzxhhjUsuOO+I2qtouIk3ApcCrwMFoF1ARmQW0JDp4prLkpprltLFxY9J1K1acOaZjp1OfXLH6jKwQ62QKXzAYZPXq1UyfPp0vfelL7N69m+9973s8+uijfPGLX8x39QyWyMUYY4wBeBE4WUTmi0gAlwDp4bhtEmbQ9bLh1gKISAVextyYMp/yfv8U8Jtsn4gxxuTaunXraGlp4corr6S0tJSTTjqJt7/97Tz11FPs3Lkz39UzpBj0jTWNtbfubhFpEZFX48rcKiJ7ReRl77E8Zt1XvX1tFZFL0jlBY4wxZiSqGgJuBH4HvA48qKqvpZhBdxbwlIhswgWPj6vq/3jrbgcuFpFtwMXec2OMGVdeeukl6uvrWbx48eCyyy+/HBHhO9/5Th5rZqJG7N4Zk8b6YlzXlhdF5GFVjc1oFpvG+nxcGuvoAPh7gFXAvQl2/0NV/X7c8U7H3WFdDMwG/iAip3jjJYwxxpisUNVHcIFd7LIRM+iq6ibg7CT7PAJclNmaGmNM4Thy5AivvfYa73vf+/D5jrUn1dXV8fa3v52f//znfOMb32DWrFl5rKVJpaVvMI21qg4A0TTWsQbTWKvqGqDWG7uAqj4DjGb+ryuA1arar6o7cXdUzxtFeWOMMcYYY0wOPPTQQ0QiEZYuXTpk3cUXX0woFOJHP/pRHmpmYqWSyCWtNNYj7PtGEfkk8BLwJVVt88qtSbCv42Qq01k6cpHhKhwevoGzpKQk5X0VU0auYqorFF99jTHGGGMy4be//S1Tp05l7ty5Q9ZNmzaNFStW8JOf/ISvfvWrVFdX56GGBlIL+tJKYz2MnwC3edvdBvwAuCbVfWUq01k6cpHhqrV1+EbS+vr6lPdVTBm5iqmuUHz1NcYYY4xJVzgc5umnn+Ytb3kLIom+wsPf//3f8+CDD9LY2Mjf/u3f5riGJiqV7p0ZSWMdT1UPqmpYVSO4AfHRLpyj3pcxxhhjjDEmtzZs2EBHRwennnpq0m3OPfdcLrroIn74wx/S25t8jlCTXakEfWNOYz3cTqNj/jx/iZvPKLqvlSJSJiLzcclh1qZQT2OMMcYYY0yOPPnkkwCccsopw253yy23sG/fPu64445cVMskMGLQl2Yaa0TkfuB5YJGINIvItd6q74rIK16K6/cAX/SO9xrwILAZeAy4wTJ3GmOMMcYYU1iefPJJTj/9dGpqaobdbtmyZXzwgx/k29/+NgcPHsxR7UysVMb0jTmNtbfuqiTLPzHM8b4FfCuVuhljjDEmPX19IdatO8jb3jY731UxxhSJcDjMc889x8c//vGUtv/ud7/LW9/6Vq6//np+9atfJR0DaLIjpcnZjTHGGDN+3Xff69x772a2bWvLd1WMMUVi69atHD16lAsuuCCl7RctWsS3v/1tfv3rX/Ozn/0sy7Uz8VJq6TPZM1J2TjNxPPbYY9x0002Ew2E+85nPDFkv7pbYHcByoAe4WlXXe+vuBj4AtKjqGTFlbgU+CxzyFt3stdwjIl8FrgXCwOdV9XfZOjdjTOFat+4ga9ceAGDHjo4818YYUyzWrnUpN8477zyeeeaZpNs1NjYO/l5VVcVFF13ETTfdxLvf/W5OPvnkrNfTONbSV4DC4TDd3d0cPXqUUCiU7+qYHAiHw9xwww08+uijbN68mfvvvx+gPG6zy3CJjU7GzVH5k5h19wCXJtn9D1X1LO8RDfhOxyVlWuyV+1cRSX3SR2PMuPHiiweYMqWc6dMr2LnTgj5jTGrWrl1LdXX1iElcYvl8Pu655x7Kysr4+Mc/PuJ81CZzLOgrMMFgkEOHDtHR0cHRo0dpaWmx9LYTwNq1a1m4cCELFiwgEAiwcuVKgNq4za4A7lVnDVAbzYKrqs8Ao2k2vgJYrar9qroTl4TpvBHKGGPGoSNHepk1q4qTTqplx44O3DB9Y4wZ3m9/+1tmz57NXXfdNapyDQ0NrFq1irVr11o3zxyy7p0FJBQKcfjwYXw+H1OnTqWkpITW1lba29vx+/2Ulpbmu4omS/bu3cvcucemp2xoaAAIxG02B9gT87zZWzbs9CjAjSLySeAl4Euq2uaVW5NgX0OIyHW4lkVmzJhBU1PTCIcbva6urqzsN9ti690bSvxFud5KKF8AACAASURBVN1feAPVi/F6F2Odi8Xhw73Mn1/DnDmTeP75/XznOy8wdWoFANddd2aea2cmEhG5FDeMoQS4S1Vvj1s/3DCHLwKfARR4Bfi0qvblsPoTSl9fH3v37uXiiy8eddnGxkZUlVNOOYWbb76ZD3/4w9TV1WWhliaWBX0FQlVpb28HYMqUKfj97qWpr6+npaWFzs5OpkyZks8qmixKcmc9fmGi6GGkW/I/AW7ztrsN+AFwzWj2paqNQCPAkiVLdNmyZSMccvSamprIxn6zLbbeLx9O/N3irKnxvXTzrxivdzHWuRj09gbp6QkxZUoFCxa4zgU7drQPBn3G5Io3xODHwMW4G5EvisjDqro5ZrPYYQ7n4/7HnS8ic4DPA6eraq+IPIgbwnBPDk9hQnnllVcIh8OceOKJYyovIlx55ZV885vf5K677uLv/u7vgOPH/wFcd911adfVONa9s0B0d3czMDBATU3NYMAHUFJSwqRJk+jv7ycYDOaxhiabGhoa2LPnWCNec3MzQPwL3gzMjS0G7Btuv6p6UFXDqhrBzaEZ7cI56n0ZY8afw97NiqlTK5g9uwqfT9i7tyvPtTIT1HnAdlXdoaoDwGrcUIRYSYc54BoyKkTED1Ri/9OyauPGjQDH9VIarblz5/LOd76TxsZGIpFIpqpmkrCgrwBEIhGOHj1KWVkZFRVD765WVlYiInR3d+ehdiYXli5dyrZt29i5cycDAwOsXr0aoD1us4eBT4pzAdChqsN27Yz5Zwjwl8CrMftaKSJlIjIfd9d0bUZOxhhTNI4ccWPGp0wpp6TER11dGW1t/XmulZmgkg1hGHEbVd0LfB/YjRvy0KGqv89iXSe8TZs2UVZWlnYvtM997nNs376dp556KkM1M8lY984C0NXVhapSXV2dcKLKkpISKioq6O3tpaamxiazHIf8fj+rVq3ikksuIRwOc80117Bp06Y+EbkeQFV/CjyCG8ewHTeW4dPR8iJyP7AMmCoizcDXVfVnwHdF5Cxc181dwOe8/b3mdX/ZDISAG1TVUmhlQE8owt7uID4R5lTZn1hT2A4fdkFftDtnXV05ra2WPMzkRSrDDhJuIyJ1uFbA+bgbpv8pIh9X1f84rnAOxqjHKoaxyGOt4zPPPMOJJ57I1KlT0zp+dXU1kyZN4vvf/z4lJSXU19cft76pqangr2Oh1y/KvpHkWSQSobu7m4qKimETtVRUVNDT00NfX1/C1kBT/JYvX87y5csHn99yyy3RYA8AdQP/bkhUVlWvSrL8E8mOp6rfAr415gqbIdr7w6w73EvQ66XS3BXk1NoyastsNoxiMNYkEiIyF7gXmAlEgEZVvcMrcytJ5sosBEeO9FJWVkJVlfv/U19fbnP1mXxJZdhBsm3eB+xU1UMAIvIr4O3AcUFfLsaoxyqGscijqWN0vJ2qsm3bNpYsWZL2fNMrVqzg0ksvZc2aNbz73e/mzjvvHLK+0K9jodcvyrp35ll3dzeqyqRJk4bdLhAIICL09VkiKmMKUSiibDjSR6lPeNfMSt42o4KQKr/a2UnEUuAXvJgkEpcBpwNXefNZxko2V2YIlxn3NOAC4Ia4skPmyiwUR470MXVqxWAPkrq6ctra+ohE7D1rcu5F4GQRmS8iAVwilofjtkk2zGE3cIGIVHo3Zy4CXs9l5SeStrY2enp6opnG09LY2EhZWRnNzc384z/+YwZqZ5KxoC+Poq18ZWVlI07HICKUl5fT19dncygZU4C2dQzQH1beWl9OVamPmkAJp9eV0dIb5pVWGyNVBMacREJV90fTxqvqUdyXzYRToBSaw4d7mTLlWIbZurpywmHl6NGBPNbKTESqGgJuBH6H+ww96A1FuD461AE3zGEHbpjDncBfeWVfAH4JrMdN1+DDa9Ezmbd3714A5szJzJ+5009398hef93i9Gyy7p151NHRQSQSGbGVL6q8vJze3l4GBgYoKyvLcu2MmTjSnW6hvT/M7q4gc6v8x3XlnFnh53BVmD/u62FxXRl+n43HLWCJEkScn8I2x82VKSLzgLOBF2K2SzRXZkFoa+vj5JOPzY9VX+/e862tfdTU2P8Zk1teS/gjcctSHebwdeDrWa2gAQYzjGcs6Js6dSrTp09n8+bNvPe9783IPs1QFvTliapy+PBh/H4/gUD8HNyJRQM9C/qMKSxrW1zii5Oqj/8siwjvnFnJA292srW9n8X1hTdnnxk05iQSgytFJgEPAV9Q1U5vcbK5Mo/fcYYSTKSaUKC+vpdQSOnpCTF9eh/19W7o1Lx57r0cDO6nvr6bpqb04tNCS3Bg9RlZIdbJFJYDBw5QW1ub0RwTp5xyCuvXr0dVLWFhlljQlyddXV309/dTW1ub8pvb5/Ph9/sZGLBuN8YUip5ghE1H+phd5afcP7TH/LzJpdQGfGw43GdBX2FLJ4kEIlKKC/juU9VfRTdQ1YPR30XkTuB/Eh08UwkmUk0o0Ni4kc5O1+3Y55tCa+tsAEpKBoAd7N5dwSmnzGbFijPHVI/R1idXrD4jK8Q6mcKyf/9+Zs2aNfKGo3DCCSfw7LPPcuTIkbQzgprEUhrTJyKXishWEdkuIl9JsF5E5Efe+k0ick7MurtFpEVEXo0r8z0R2eJt/18iUustnycivSLysvf4afzxCkU4HKa1tXXYRzKtra34/f5R3yUpKytjYGDAxvUZUyA2HukjpDB/cuIWexHhrKnlNHeHONQbynHtzCiMOYmElzjiZ8DrqvpPsQWGmSsz77q6ggBMmnRsTHlVVSmlpT7a2ixpmDFmKFXlwIEDzJw5M6P7PeGEEwDYs2fPCFuasRox6EszoxnAPcClCXb9OHCGqr4VeAP4asy6N2MynV2foGxRGxgY4OjRo9TV1Y26CTsQCKCqBIPBLNXOGJMqBTYc6eOESaVMKk3+5/Qt9eUIsNkmvS5Y6SSRAN4BfAJ4b8wNy+j8K98VkVdEZBPwHuCLOTqlESUK+kSE+vpyWlst6DPGDNXe3k5/f3/Gg745c+YgIhb0ZVEq3TsHM5oBiEg0o9nmmG0GM5oBa0QkNqPZM97A9uOo6u9jnq4BVozxHIpOW5sbI1FXV0dXV9eoykbH//X396c8FtAYkx3t/kl0DkR4z+wq+sOJW9+jSWLqy0p4+XAf1aU+zp5mc20WorEmkVDVZ0k83m/YuTLzrbs7GvQd/7/ETdtgNyiMMUMdOHAAIOPdOwOBADNnzrSgL4tSCfoyktFsBNcAD8Q8ny8iG4BO4BZV/WN8gUwNek9HT08PGzZsGHabkpKhkzLX1dURDod57rnnCIfDoz7ulClTOHToENu3b0+4/2SKaXB2MdUViq++JjMOBuqp8gun1ARGnJZhZqWf19r66YzO3G5MnnV1ufHh0YnZo2pry3jjjYJJMGqMKSD797uv9plu6QOYO3cu27Zty/h+jZNK0Jd2RrNhdy7yD7iJbe/zFu0HTlDVIyJyLvBrEVkckwnN7TxDg97T8cQTT3D22WcPu019ff1xzzs7O9m9ezfz58+nurp62HF/ybS2thIMBlmwYMGQ/Q+nmAZnF1Ndofjqa9LX3h+m3T+Zt08ppySFqRhmVPjZ3NbPgR4b12cKw7GWvuODvurqMjo7+23suDFmiAMHDlBRUUF1dXXG933CCSewdu1aurq6Up7OzKQulUQuaWU0G46IfAr4APAxr9sMqtqvqke839cBbwKnpFDPohBN4DJ58uQx7yMQCBAOh8fUSmiMyYyXj7hum6nO5RcoEerLSjhoyVxMgejqClJa6iMQOL7HSE1NgFBIB4NCY4yJiiZxyca0CtF5//btGzGEMGOQStA35oxmw+1URC4Fvgx8UFV7YpZP85LHICILcMlhdqR8RgUsGAzS1dU1pgQusUpLSwf3Z4zJvVBE2Xikj7rQUaoDqXexnl5RQk9Iae2zGzYm/7q6gkPG8wGDk7J3dNj0QMaY47W0tDBjxoys7Hv69OmDxzCZN2LQl2ZGM0TkfuB5YJGINIvItd6qVcBk4PG4qRkuBDaJyEbgl8D1qjr6PpAFqKOjA4Da2tq09mNBnzH5tbW9n96QMmPgyKjKTatwPeq3d9qXaZN/XV0DQ7p2QmzQZ8lcjDHHDAwM0NbWNhicZVp9fT1+v9+CvixJaXL2sWY089ZdlWT5wiTLH8JNcDvutLe3U1FRQVlZWVr7sUnax6fHHnuMm266iXA4zGc+85kh6725wO4AlgM9wNWqut5bdzeuq3SLqp4RU+Z7wOXAAK6r9KdVtd3LqPs6sNXbdM14nB4lW9Yf7qOuzEdNR/eoylX6fUwq9bG9Y4DzplsGT5Nf3d3BIUlcwHXvBAv6jDHHO3z4MADTpk3Lyv59Ph9Tp07l4MGDWdn/RJfS5Owmff39/fT19VFTU5OR/ZWWlhIK2dig8SIcDnPDDTfw6KOPsnnzZu6//36A+MFiNh9mAdjfE2Rvd4izp1YkztE/gunlJezpCtIXsiyeJr+6u4PW0meMSVm0BS5bLX3RfVtLX3ZY0Jcj7e3tABkL+vx+vyVzGUfWrl3LwoULWbBgAYFAgJUrVwLE9wMenA9TVdcAtSIyC0BVnwGGdINW1d97XbTBzYfZkL2zmBieO9BLWYlw5pSxtdhPq/CjwI5O655t8qurK3FLX3m5n7KyEhvTZ4w5zqFDh4DstfSBC/oOHTpEJGI3RjPNgr4cUFXa29upqqoaHI+Xruh++vr6MrI/k1979+5l7txjCXAbGhoA4jMsJJsPM1XXAI/GPJ8vIhtE5GkRedfoalxYXj7cN+SRDQd7QmzrGGDptArKSsb257M24KPSLzauz+RVJKL09CRO5AKutc9a+owxsVpaWqiqqqKqqiprx5g+fTrBYHCwscRkTkpj+kx6ent7CQaDGW0Ojw36svnhM7mRZD6svM+H6ZW9DtedlBkzZmRlEvp0J7fvDQ29DO3+1DtfJiofvw8FtlTOo8RfQd/Wl2jaGjmu3sn2kUjV5Aa2Bqt5cte64+68JdpHxSjOI1XpXu98KMY6F7KeniCqQ+foi6qpCVjQZ4w5zqFDh7LaygcMZgY9ePDgqOaiNiOzoC8HOjo6EJGMTmTp8/kQEWvpGycaGhrYs+dYI15zczNAfP+/dOfDvCh2Pkyg3/t9nYhE58N8Kb68qjYCjQBLlizRbExCn+7k9ola9lKdPy9Z+fh9bG3v54WdR3nfnCqWTL8QOL7eo2ldXOAX/mvnURae+w5OmHzsS3e655GqdK93PhRjnQtZV1fiidmjamrK2L17yD0gY8wE1tLSwkknnZTVY8RO23Daaadl9VgTjXXvzDJVpbOzk0mTJlFSkvp8XiMREUpLSy3oGyeWLl3Ktm3b2LlzJwMDA6xevRogvm+DzYeZJ72hCH9o7mZaeQnnTEs/CJs3uRQfsOOodfE0+RGdeD3RmD6Idu+096cxxunv76e1tTXrLX21tbX4/f7BTKEmcyzoy7K+vj6CwWBGW/miSktL6e/vT9Y10BQRv9/PqlWruOSSSzjttNO48sorAfpsPsz8U1Ue29NFdyjC/zpxMj5Jv7tlWYmPOZP87LBxfSZPenpc0FdZmTzo6+8Pc9RuTBhjgF27dqGqWc3cCa4nW11dHa2t9pUk06x7Z5Z1drruMZMnT874vv1+P5FIhGAwSCCQeDC+KR7Lly9n+fLlg89vueUWmw+zAPzpQC9b2wd4z+xKZlZm7k/mgskBnt7fQ1cwwqRSu/9mcqu31yX1rahI/J6OztW3b18XixbZuBpjJrrt27cD2c3cGVVfX8+RI0eyfpyJxr5pZNnRo0eprKzE7898fB3dZ3+/DbY3Jhs2Henj2QM9nFFflvHJ1BdUuy/V1tpn8qGnxwV9lUluZETn6tu/vytndTLGFK5o0Jftlj6AKVOmWEtfFljQl0XhcJi+vr6stPKBBX3GZNOh3hCP7u5i3uRSLjthEpKBbp2xpleUUOUXdlrQZ/Jg5Ja+aNDXnbM6GWMK1/bt2ykvL2fSpElZP1Z9fT0dHR0EgzafbSZZ984sigZj2fqAlJSUUFJSYkGfMQmkkpEzmY6BMC8f6WNaRQl/OX8yJRkO+MAlY1pQHWBbxwAR1YyMFTQmVT09Ifx+H6WliROMWdBnjIm1fft2pk+fnvEboIlMmTIFgLa2tqwfayKxlr4s6u/vp6SkhPLyzKdcjyorK7Ogz5gM6glFWHeoj1KfcOVJNWOehD0VC6oD9IWV/V5XO5NfInKpiGwVke0i8pUE60VEfuSt3yQi53jL54rIUyLyuoi8JiI3xZSpF5HHRWSb97Mul+eUTG9vMGnXTnDdPv1+n3XvNMYALujLxXg+OBb02bi+zLKgL0tUlf7+fiZNyny3sFjRoM8yeBqTvoGwsu5QLxFVlkyryHqClXmTSxFsXF8h8KYw+TFwGXA6cJWInB632WW46U1OBq4DfuItDwFfUtXTgAuAG2LKfgV4QlVPBp7wnuddT08oaddOcC3RNTUBa+kzxhAMBtm1a1dOxvMBg5Oy27i+zLKgL0tCoRCRSCTrfZ/LysoIh8OEw+GsHseY8S4cUdYf7qU3pJyTg4APoMLvY3aVnx2dNm6hAJwHbFfVHao6AKwGrojb5grgXnXWALUiMktV96vqegBVPQq8DsyJKfNz7/efA3+R7RNJRW9vKOl0DVE1NWUW9JmcGmtru7euVkR+KSJbvFb3t+W29uPX7t27CYVCOWvpq6urQ0SspS/DbExflmR7PF9UWZkbd9HX15eTwbXGFIOIKhEFvy+1VnZVZVNrH+0DEc6aUk59WeJxTtkwb3Ipzx3opS8UydkxTUJzgD0xz5uB81PYZg6wP7pAROYBZwMveItmqOp+AFXdLyIJb5WLyHW41kNmzJhBU1PTmE6iq6srpbLBYA/V1T7q6/cl3WbatAhvvtky6rocPtw7+Ht5eXjM55INqV6fXCm0+kD+6hTT2n4x7rP1oog8rKqbYzaLbW0/H9faHv2c3gE8pqorRCQAVOas8uNcLjN3gktUWFNTYy19GZZS0Ccil+I+TCXAXap6e9x68dYvB3qAq6N3PUXkbuADQIuqnhFTph54AJgH7AKuVNU2b91XgWuBMPB5Vf3d2E8xPwYGBigpKeHo0aNZPU406It2JTVmIgtGlM1t/bT0hggrTCr1MX9yKbMr/Um7Wasqr7cPcLA3zKm1gYzOxZeKEycF+BO97O6y1r48S/QGie83P+w2IjIJN//lF1S1czQHV9VGoBFgyZIlumzZstEUH9TU1EQqZTs7N1NdPYnW1tlJt6mo6KSjoyWl/cVqbNw4+Pspp7SNunw2pXp9cqXQ6gN5rdNgazuAiERb22ODvsHWdmCN17o3C+gGLgSuBvBa663ffIbkOugDm6svG0bsv5TmOAeAe4BLE+w64TgHb98rgcVeuX/16lA0VJWBgYGcTJheWlqKz+ezZC5mwusPR1hzsIcDPSFmV/pZWB1AgFda+1nT0svR4NAu0KrKE3u72d0VZN6kUuZNzv5nNt7sKj9+gT9b0JdvzcDcmOcNQHwzWNJtRKQUF/Ddp6q/itnmoPelFO9nS4brPSaue+fwNzhqagK0t/fT22vvTZMTyVrSU9lmAXAI+DcR2SAid4lIVTYrO5Fs376dyspKqqurc3bM+vp6y96ZYanc0h7znRdvnMMzXneXeFcAy7zffw40AV/2lq9W1X5gp4hs9+rw/CjPLW+i4/mirXDZJCKWwdNMeKrKq6399IaVpdMqqC9394lOqi5lX0+ILe39PHegl/mTS1lYE2BSqY/DvSH+sLebXUeDnDiplEW1uQ/4wHVBbZhUyu6jQaaVW4/7PHoROFlE5gN7cTcfPxq3zcPAjd7/wfOBDq/LpgA/A15X1X9KUOZTwO3ez99k8RxS5hK5jDymD+DAgW7mz6/NRbXMxJZOa7sfOAf4a1V9QUTuwDUm/N/jCmeoG3WqCrH7brxU6vjCCy8wc+bMwayauTBr1iw2btzIU089RXd3d0Ffx2J4nSG1oC8j4xwSSDbOYQ6wJsG+isbAgOtRkIuWPnBdPLu6LK22mbj2doc41BfmtNrAYMAH7qbInKpSppX7eb29nx1Hg6x6tZWATxiIKKU+uHTuJFQ1YffPdOb6G078fgM+YVdfmP5wJKtTRJjkVDUkIjcCv8MNZbhbVV8Tkeu99T8FHsENY9iOG8rwaa/4O4BPAK+IyMvesptV9RFcsPegiFwL7AY+nKtzSqavL0QoFBk2eyccP1efBX0mB9JpbVegWVWjY2l/SYJMuZnqRp2qQuy+Gy+VOra1tXHWWWfldIxdZWUlwWCQxYsXs3nz5oK+jsXwOkNqQV/a4xxGKaV95fpuTSI9PT1s2LBhyPLq6moCgQCvvPJK1utQUlJCRUUFVVVVPP3008NO3VAsdyKguOoKxVffQjeaYCscUbZ3DlAT8HHCpMQtF4ES4cwp5ZxUHUHEzcVXX1bCqbVlVJX6kh4vV6KJY1r7w8yqtKAvX7wg7ZG4ZT+N+V2BGxKUe5bE/7tQ1SPARZmtaXo6OlzPkJG7d9oE7SanxtzaDiAie0RkkapuxX3mNmPSFg6H2bFjB1dcEZ/MOLvq6tyUpnv27BlhS5OqVIK+tMY5DONgtAto3DiHlPaV67s1iTzxxBOcffbZQ5YfPHiQ0tJS5s6dm6BUZtXX19PZ2cnu3btZunQplZXJk1UVy50IKK66QvHVdzx5tbWfvrCyuK5sxDkxJ5X60m6ly4bqgA+/wJG+MLNGSKNvTLra20cX9DU3ZzchmTGQdms7wF8D93mZO3fErTNj1NzczMDAAAsXLiQSyV2W6digr7bWehpkQipBX1p3XoaRbJzDw8AvROSfgNm45DBrU6hnQYjOmTdc8JVpsRk8c3lcY/JNVVnT0kN1qY+p5bnL9xRtGewNaUZaCX0i1JeV0Npv822a7IsGfSN175w8uZTKSj9//vOoEpEaM2ZjbW331r0MLMlqBSegbdu2AbBw4ULeeOONnB03OkF7c3OzBX0ZMmI/IlUNAdE7L68DD0bvvETvvuA+oDtwd17uBP4qWl5E7sclYVkkIs3euAZwwd7FIrINNyfL7d7xXgMexDXLPwbcoKpF800oGHRZznI1ni96LBGxZC5F7rHHHmPRokUsXLiQ22+/fcj6ESalvVtEWkTk1bgy9SLyuIhs837Wxaz7qrevrSJySVZPLkv2dIdo649w4uTSEVv5Cl19eQk9IaXX5uszWRbt3jlSIhcRYd68Gnbu7MhFtYwxBSg6XcPChQtzetxJkybh9/ute2cGpZQqLs07L1clWZ50nIOqfgv4Vip1KzTRJC6lpbnroiUiBAIBC/qKWDgc5oYbbuDxxx+noaGBpUuXAsT3QxxuUtp7gFXAvXFlolOj3C4iX/GefzluapTZwB9E5JRiusECsOlIHwGfMHOEFotiMMUb13ekL0zDJBvXZ7Knvd21To/UvRNg/nwL+oyZyLZv3055eTmzZyef0zMbfD4ftbW1FvRlkH2zyLBgMIjf78fny+2ltWkbitvatWtZuHAhCxYsIBAIsHLlSoD4/gyDU6Oo6hogOiktqvoMkCit1hW4KVHwfv5FzPLVqtqvqjtxrfTnZfassqs/HGFrez+n15VR4ivuVj5w4w1LfdA2UFRxtylCqXbvBJg3r5pdu6x7pzET1bZt21iwYEHOv9eCG9dnQV/mFP/t8QISnZS9oqIi58cuKyujs7OTSCSSlw+mSc/evXuPS/zT0NAAEN9HOC9To+QiU2589tPeUOIstO3+Y8Hd4dIagpVzGfjzZvb3p55dMHYfIx1vJMHeLva/nLkpRCsmn8jhYBn7dx/LCpyovukqxmyzxVjnQnUskcvIPVLmz6+ho6OftrY+6uoKLwmSMSa7tmzZwhlnnJGXY1vQl1kW9GVQOBxGVXPatTMqmsxlYGCA8nL7x1xskky1kfepUSA3mXLjs5+mMmXDr3d2UtUV5PJ3LmXjkdRbuRNl7xxrMpb9Lz/PrLPeNqayifR2DvBGxwBT3nIBgRL3EmUj22gxZpstxjoXqo6Ofnw+IRAY+QbhvHk1AOza1WFBnzETzMDAAG+++SYf/nB+phetr69n/fr1hMPWAyYTrEkog6JJXPIZ9FkXz+LU0NBw3N2s5uZmgGDcZmOeGgVgLFOjFKpQRNnRGeTkmpGnaSgmdd64PuviabKpvb2figp/Sp+d+fNd0Gfj+oyZeN58803C4TCnnnpqXo5fV1dHKBSira0tL8cfbyzoyyAL+sxYLV26lG3btrFz504GBgZYvXo1QHvcZg8Dn/SyeF7A6KZGgaFTo6wUkTJvOpaimhpl19EgAxHllNrcZcnNheqADwHabOoGk0Xt7f0pJXEBN6YPsHF9xkxAr7/+OkBegz6AlpaWEbY0qbDunRkUTeKSj5YHn89HaWmpBX1Fyu/3s2rVKi655BLC4TDXXHMNmzZt6kt1UlpvapRlwFQRaQa+rqo/w02F8qA3Vcpu4MPe/l4TkejUKCGKbGqU7R0DBHzCiZPG10TmJSLUBHy0W9Bnsqi9vS+lJC4AdXXlVFcHrKXPmAloy5YtADz33HOsX78+58ePztV36NChnB97PLKgL0NUlWAwONjilg+WwbO4LV++nOXLlw8+v+WWW2xqlARUlZ1HBzhxcum4yNoZryZQwp7uIBFVfOOo66opHB0dAyPO0RclIsyfX8P27da9ypiJ5vXXX6euri5vuSKiLX0W9GWGde/MkEgkQiQSyUvXzqho0JckKYgx40Jbf4SOgQjzJ4+vVr6omoCPiEJ30CZpN9nR3t6XcvdOgLe+dRobN9qXLmMmmi1btjBz5sy8Hb+qqoqKigrr3pkhFvRlSD7H80WVlZUNtjgaM17tODoAwILq8TWeL6o64JK5dAxY0GeyYzRj+gDOOms6+/d38/3vr6WxbzDRCwAAIABJREFUcSONjRuzWDtjTCGIRCJs2bKFGTNm5K0OIsLcuXMt6MsQC/oyJBQKAW5sVr5YMhczEezsHKCuzEetl+lyvKnyCyUCHZbB02RJNHtnqs4+203vuWfP0WxVyRhTYHbs2EFXV1d03uC8aWhosO6dGWJBX4aEQiF8Pl9eJ0a3oM+MdxFVmrtCnDhpfLbygbuzWRMoodO6d5osCIUidHcHRxX0nXWWBX3GTDQbNmwA4IQTTshrPaylL3MskUuG5DNzZ5Tf76ekpMSCPjNu/XF/D/0RJaI65gnVi0F1wMefj7pkLsZkUkeH+/9QWZn6UIS6unJOPLF6TEHf4cO9g91Br7vuzFGXN8bkx/r16/H7/cyePTuv9Zg7dy6tra2EQqG89qYbD6ylLwNUtWDejJbB04xnrd5UBvXl47NrZ1RNwIcCR621L6dE5FIR2Soi20XkKwnWi4j8yFu/SUTOiVl3t4i0iMircWVuFZG9IvKy91gev99cam93/x9G09IHrotnKkFfMBhmy5ZWBqx7sjFFbcOGDSxevDivuSrABX2RSIR9+/bltR7jgQV9GRCJRFDVvH8wwDJ4mvGtrS9MpV8oLxnff7pqLJlLzolICfBj4DLgdOAqETk9brPLgJO9x3XAT2LW3QNcmmT3P1TVs7zHIxmt+Ci1t7sW8tEkcgFYsmQmLS09gy2FiTzzzB7mzbuTH/5wHf/yLxss8DOmSKkq69ev55xzzhl54yybO3cuAHv27MlzTYrf+P7mlCPRbJmF0tIXDocJh+2frRlfVJXW/jD14zSBS6yKEqHUB532pTmXzgO2q+oOVR0AVgNXxG1zBXCvOmuAWhGZBaCqzwCtOa3xGHR0uOy3o23pu+KKk1CFDRsSj63ZubOdD33oYaqqSvnABxawbVsbq1dvSbu+xpjc27dvH4cOHSqooK+5uTnPNSl+KQV9aXZ5SVhWRB6I6e6yS0Re9pbPE5HemHU/jT9eoSmEzJ1RhZbMpbW1dcSHManoCkUIKdRNgKBPRKgOlFhLX27NAWJvJTd7y0a7TSI3ev8b7xaRuvSqmZ5oS1+qk7NHLV48lVmzqli37uCQdf39IVas+G/C4Qi//e2HuPzyk3jHO+bw4osH6O+397AxxebFF18EKKigz1r60jdilBLT5eVi3D+4F0XkYVXdHLNZbJeX83FdXs4frqyqfiTmGD8AOmL296aqnpXeqeVOIWTujCovLwegr6+PqqqqPNfGmMzp8L48Rrs+jnc1pT529gUJRpRSX/4SRE0giS5yfD/5VLaJ9xPgNm+724AfANcMObjIdbguo8yYMYOmpqYRdptYV1fXsGXXrDkMwOzZR6ivH3mMXlNT2+Dv73hHJQ89dAiRPx+3/Kc/3cP69Qe57baT2Lt3I/X1vbznPX6efTbCpk3tLF3qG7KvfBnp+uRaodUHCrNOJreeeOIJKisrWbJkCa+++urIBbKopqaGyspKC/oyIJWmqcEuLwAiEu3yEhv0DXZ5AdaISLTLy7yRyopLd3kl8N70Tyc/CiFzZ5Tf78fn89HXN34zG5qJqX0gjF/cPHYTQU2gBCVIS2+IOVX5Hy88ATQDc2OeNwDxmQNS2eY4qjrYNCYidwL/k2S7RqARYMmSJbps2bJU632cpqYmhiu7fv1LwC4GBubQ2jry+2rFimMZN5955jkeeugQd93VyZo1lyMi3H33KzzwwEtcf/2Z3HLLxQA0Nm5k1qwIVVX7eP75Hk466Ywh+8qXka5PrhVafaAw62Qyq7GxcfD3+vr6Iev/8Ic/cOGFFxIIFMb0SNOmTbOgLwNSaZpKp8tLKmXfBRxU1W0xy+aLyAYReVpE3pVCHfOmkDJ3gusWVl5eXjDdO43JlI6BCDVlJQVxcyUXqgPuz/OBnlCeazJhvAicLCLzRSQArAQejtvmYeCT3pCGC4AOVd0/3E6jY/48fwnk9bZ5R0c/IlBWNvr/WTNnVvGBDyxg7doD3HjjE3z+80/w2c/+nve/fx533HH8fduSEh9nnjmNl146SjhsXTyNKRZ7/3/23j0+rrrc938/M5NMJvdMrk2T3kha2lIoSEsritUil8IGdCN3UUELx6Kwj+coKLr9bWQftwKerbDBgP52EaUgCFSl3IrlIr1KSymU0jbpJZfmfp9LMjPf88fMpNM0l0kyM2tN8n2/XuuVzLp+1pr1nbWe7/N8n6eujo8++ojzzz/faCkDFBUVaaMvBkTzqz+RkJdotr0WeDLicwMwQynVKiKfAJ4XkYVKqa4TDhijUJiJ4HK52L17N4WFhRw7doyampqEa7BaTw51y8jIwG63n3RNjAjZiCahzFDnkGzhJcmmN9nwBRTd/QFOGeM4pGQmLZTMpcmtjb5EoJTyichtwMuAFfitUuoDEbk1tPwR4EVgFXAAcAFfC28vIk8CK4ACEakF/lUp9RvgZyKymOCz7xBwS8JOagg6OrxkZ9uxjDNkeNWqOTQ2uqiq2g3ATTedxi9+8VlShwi7njcvj3feqaex0UVpaeaEdGs0msSwceNGANMZfTt27DBaRtITjdE3kZCX1JG2FREb8EXgE+F5Sikv4A39/w8ROQjMBU74tmMVCjMRNm7cyLx582hra2P27NkDSVSMpre3l87OThYsWEBRUdHAfCNCNqJJ1DJUaEGyhZfEQu9LL73E7bffjt/v5+tf//pJy0Oh0P9J8KXTBXxVKfVuaNlFoWVW4DGl1E9D858C5oV2kQt0KKUWi8gsYC+wL7Rsi1Lq1gmdQBzpCiU0yZ0i4/kg6LXPSrHS5NYZPBNFqJzCi4PmPRLxvwLWDLPttcPM/3IsNU6Ujg4Pubnjf1ZZLMLNNy/ilVdOpa/PT2Fh+rDrlpVlAVBb262NPo0mSXj++ecpKipi0aJFRksZoLCwkMbGRrxer2netZORaIy+gZAXoI5gyMt1g9ZZTzA72TqCiVw6lVINItI8yrbnAx8ppQbysIpIIdCmlPKLyByCyWGqx3d68cdMmTvDhLWEtWnMj9/vZ82aNbz66quUlZWxZMkSgLRBq03ZhEkdodIFUyWJS5isFAt1vf0ElMIyRcJaNfGlo8M7IaMvTE7O6PsoKcnAZhOOHu1h6dIJH1Kj0cSZuro61q9fz3e+8x1TJCcMU1hYCAT1zZkzx2A1ycuo36hSygeEQ172Ak+HQ17CYS8Ee0arCYa8PAp8c6RtI3Z/DSeGdgKcB+wWkfeAZ4BblVKmzetvpsydYcJF4sP1AzXmZ9u2bVRUVDBnzhxSU1O55pprIOiZi2S4GmGj1heLSJg0uL0lBZ19waLsqdapZfhkp1rwKWjzam+fJjZ0dsbG6IsGm81CWZmd2trRs4RqNDCxEmGh5dZQToghEyZpRubRRx8lEAhwyy2GRqGfRDhqTY/rmxhRuacmGPJy0rYRy746xLxngWej0WUGzJS5M4zFYsFqtWpPXxJRV1c3UIsGoKysDILh0ZGMJWHSOYO2HTZhEtAF3K2Uemsi5xBPOvoCU6Io+2CyUoKdSU1uPwVp5okm0CQvHR1eZs7MTtjxZs1K4913tdGnGZ2JlAiLWH47QSdD4m7ySUJDQwMPPfQQF154oam8aVVVVQPODF2gfWLot4gJ4vP5cDgcRss4iZSUFNN5+pRSeL1e3G43/f39KKWwWq34/X5yc3MHGvVUJNhvcvLsQZ8TnjAJEpM0aXAiHLfvuPw+iw1v3nyk+QgNR1snfKyOIUo+RB5vLPS7e2jYtXmikoYlgCDOBez4qJom78lFscdLMiYeSkbNZqS93cPixUWjrxgjZs1KY9OmDjo7dUZpzaiMu0RYaEhRGXAJcC/wPxOsPalxuVxce+21uFwu7r//fqPlnER+fj6gPX0TRRt9E8BisaCUMqWxkpKSgsfjwe/3D5kdM9H4fD46Ojro6+vDYrGQmpqKiODz+WhsbKSpqQmn00lRUZEp9CaasrKyE37MQr1Zg632hCdMCi2Pe9KkwYlwdrUcrzN5zOWDVg8zZp9Crn3uhI+1uGDwUMkTjzcWGnZtZtri5ROVNCKNLW7SsktZUTE/ZvtMtkRJkJyazUhbmwen8+Q2EC9mzQoeS4d4aqIgmqiV4SJeGoD/C3wXyBruAInO/G7WzqpwAr36+npeeOEF1qxZQ1NTE3fddRdNTU00NTWdtK6RWK1WMjIy2LJliymvp1m/58Foo28ChBOmmCmJS5iwIerxeMjIyDBUS39/P62trSilyMnJIT09/YRwWJ/PR3d3N62trXR0dJCXl4ff7x/I/GmGH5x4s2TJEvbv309NTQ3Tp09n3bp1AB2DVpuSCZM6+/wIx+vWTTWKHDZquvqMlqGZBPT3++np6ScvL3FGX2lpMEq9qcmdsGNqkpZxlwgTkUuBplAn5orhDpDozO9m7ay67777WLt2LXv2BMuGLlu2jGeeeYZPf/rk0tiRhdyNwul0kpeXh8/nM+X1NOv3PBjzWStJRNgjpY2+4fH5fLS0tGCxWMjPzx/SK2qz2cjLyyM9PZ2Ojg5aWlpIS0vcS4kZsNlsPPjgg1x44YX4/X5uuukmdu/e7YmmRthw9cUidj9cwqR/ExEf4MfECZM6+wJkp1pilr1yvF49oyh22NjT5qW3P0BGytQ0fDWxob09eO8n0tOXm2sjJcVCS4srYcfUJC0TKRF2JXCZiKwimPk6W0SeUErdEEe9SUldXR0///nP6ejo4PLLL+fiiy82XeKWoSgsLOTgwYNGy0hqzGetJBE2m810mTvDWK1WLBYLbrdxvas+n4/W1lZEhPz8/FGNY7vdTkFBAe3t7eTk5NDV1UVW1rBRGpOOVatWsWrVqoHPd99995RPmKSUorvfT7Fj6v5UFTmCnUtNbh+zUwbn9tFooqet7bjR192dGO+xiFBY6KClRXv6NKMy7hJhwF2hiZCn739pg+84kd66hx9+mI6ODu644w5OOeWUpImmKiwsZNOmTaYZtpSMmM9aSSJsNpvpMndGkpKSYpjRp5Sirq4Ov9+P0+mM2htqtVrJz8/H7XbT09NDd3f3cElONFMAj1/RH4DsKVafL5KikMHb6NbZeDUTI2z0JTK8EyA/Xxt9mtGZSIkwTXR8+OGH7Nq1i0suuYRTTjnFaDljoqioiL6+Purq6oyWkrRM3e7zCaKUGjD6zEpqaird3d2G9Iq0tbXR3d1NTk4Oqalj806ICF1dXeTn59PT00NjYyPFxcWmNa418aO7PwAcL10wFXHYLGSnWGhy61p9mokRGd555MhJiXrjRmGhg/3721FK6d9xzYhMpERYxDqbgE1xkJf0rF+/noKCAlauXDnsOmYYwzcU4Vp9Bw4cYMaMGQarSU6m7pvUBAkXZTdj5s4wYWMr0d6+/v5+GhsbycjIID09fdz7CSd9aWlpobm5OYYKNclCV582+iDo7WvSnj7NBIkM70wkBQXpeDx+Wlu1t0+jMYr6+npqamr47Gc/a+p31+EoLCwEgkafZnxM7TepCeD1BmsOmdnTF27Uvb29CT1uQ0MDSilKS0sn1KsrIuTk5JCbm0tTUxMdHYOTWWomO939ftJtgs0ytb0DRelWWj1++gM61FkzfowK7ywoCNayra7uTOhxNRrNcd555x0sFgvnnDO4CkZykJubi91u10bfBDCvxWJyPJ7gw9PMRp/FYiEtLQ2XK3FZ07q6uujq6qK4uBi73T5hg1NEKC0tHYjjTklJMbwEhSZxdPUFyJnC4/nCFDlsKKDF42NaevL10GrMQTi8MzfXHvN9V1W9N+yySKNv6dJpMT+2RqMZGb/fz5YtWzjjjDOGTJBn1pDOSCwWC3PmzNFG3wQwr8VicrxeL4FAwJSZOyMJl0FIRDIUv99PfX39QBbOWGGxWJgxYwbV1dUcOXKEioqKpAxN0IyN/oDC7VeUTdH6fJGEs5c2ufza6NOMm7Y2D7m5dqzW6NvUcMbcSEbeYI4bfTpaQ6MxgpqaGrq7uzn77LONljIhKioqdNmGCaDfpsaJ1+vF5/OZflB6eno6gUBgwDMZT5qamvD5fEyfPj3m18VmszFz5kwCgQC1tbU6o+cUIJzEJXuKj+cDyE21kGoRncFTMyHa2jwJD+0EsNutZGenUlOjwzs1GiPYs2cPFouFBQsWGC1lQlRUVHDgwAECgYDRUpIS/TY1DpRSeDwefD7zv4BlZmYC0NPTE9fjuN1uWltbcTqdE0reMhJ2u51p06bR29tLa2trXI6hMQ/dfcFslVna04eIUOSw6mQumgnR3u5JeBKXMMGMod2GHFujmers2bOHOXPmxO39LFHMnz8fl8vF4cOHjZaSlOi3qXHg8/kIBAJJYfTZbDbS0tLo7o7Pw7atrY3W1lYOHz6MxWIhNTWVtra2gSnW5OXlkZ2dTWNjo6GF5zXxp7s/QKpFsE/xJC5hghk8/drLHUdE5CIR2SciB0TkziGWi4j8MrR8t4icFbHstyLSJCJ7Bm3jFJFXRWR/6G9eIs5lKNrajDP68vLSOHo0cWUiNBpNkPr6eo4ePcqiRYuMljJhwufw/vvvG6wkOdFG3zgIZ+5MBqMPgt4+l8sVt1DU3t5efD4fOTk5cRnjGGlEtre3k56ejohw+PBh7fGbxHT1BchKsZg+hDpRFDls9AUUnX06rCUeiIgVeAi4GFgAXCsig2OhLgYqQ9Nq4OGIZf8NXDTEru8ENiqlKoGNoc+GYFR4J4SNvm7daaHRJJiNGzcCJH1oJ8DChQuBoOdSM3aiekOfYO/nkNuKyI9FpE5EdoWmVRHL7gqtv09ELpzoScaa8Pg4vz85iiWHQzzjkfykv7+frq4u0tLSSEtLzMuExWIhJycHn89Hb2/vCUbhUJMm+QgoRU9/gGwd2jlAsSOYxVSP64sbS4EDSqlqpVQfsA64fNA6lwOPqyBbgFwRmQaglHoTGOoH53Jgbej/tcAVcVEfBcaGd9rp6emns9NryPE1mqnK5s2bSUtLo6yszGgpEyYrK4tZs2ZpT984GfWNaiK9n1Fs+wul1OLQ9GJomwXANcBCgr2m/xXaj2nwer1YrdakGUianp4+EHoZS5RSdHR0DBhhifTIpKWlYbfb6e7uThrjWxM9vf0BAuii7JEUOGwI6HF98WM6cDTic21o3ljXGUyxUqoBIPS3aII6x4VSytDwztzc4HFra/W4Po0mkWzevJlZs2aZPtt8tCxatEh7+sZJNCUbBno/AUQk3Pv5YcQ6A72fwBYRCfd+zopi28FcDqxTSnmBGhE5ENKweUxnFke8Xi92e+zrHMWLsFHm9/tjWmaiqamJ/v5+8vLysFoTa5eHC7c3NTXR3d1Nbm5uQo+viS/hzJ06ictxUiyCM81Ko1t3csSJoXqtBsciRrPO+A4usppgpynFxcVs2rRpXPvp6ekZctveXj9+v6KtrY5NmzbhdCZmTLTV2o/TWc+sWcF6sX/96zu0tOQk5NhDMdz1MQqz6QFzatKMj56eHnbv3s1FFw0VeZ6cnHbaaWzYsIG+vr6YOzMmO9EYfUP1bJ4TxTrTo9j2NhG5EdgBfEcp1R7aZssQ+zIF4cydyWZk5Obm0t7eTldXV0y09/b20tzcjMPhwOFwxEDh2LHZbGRkZNDb20tGRoau3TeJ6OoPYAEybNroi6TYYaO2t99oGZOVWqA84nMZUD+OdQbTKCLTlFINoc7QpqFWUkpVAVUAZ599tlqxYsUYpB9n06ZNDLXtoUOdwE7OPnshK1YsGlOdvYngdNbT1laK1eoBasjLm8WKFWck5NhDMdz1MQqz6QFzatKMj+3btxMIBJgzZ47RUmLGokWL8Pl8fPzxx5x22mlGy0kqojH6JtL7OdK2DwP3hD7fA9wP3BTl8WLWKzpWRIT8/HyOHDmCy+Vi586dCTnueIn0wOXm5lJdXU1X1/EMaqOFRg7lwbNYLOTm5hIIBDh8+DCHDh2Kmd4w0V5bEaGwsJAjR46ccF6RJMILGYue0W3btvHggw/i9/u55JJLTlouwfjZ/wRWAS7gq0qpd0PLLgotswKPKaV+Gpr/Y+AbQHNoN9+PCKW+C7gZ8APfVkq9PKETiCHdfQEyUyxYdBKXEyhyWPmw3YvHFyBNG8SxZjtQKSKzgTqCwwyuG7TOeoKdlesIdmB2hkM3R2A98BXgp6G/L8RUdZQ0Nwc9bUVFxqRsz8lJxWIRjh7V4Z1mp6XFPdApsHq1cQa6ZuJs3hwMkptMRt8ZZwTvyXfffVcbfWMkGqNvIr2fqcNtq5RqDM8UkUeBv4zheDHrFR0rPT09HDp0iNNOO42tW7dy5plnJuS448XpdA78v3XrVjIyMli6dOlArZbREp1Ebg/BxC01NTX4fD5OOeUUent7Yy8a2LlzZ9TXtrOzExFh5syZ2Gwn39KDzyEeTLRn1O/3c/PNN7Np0ybKyspYsmQJwODBN5FjZ88h2HFyTsTY2c8TbD/bRWS9UiocRv0LpdR9kTsaNHa2FHhNROYqpQyPHVRK0dXvp9gRzc/T1KIodE0a3T5mZumwlliilPKJyG3AywQ7T36rlPpARG4NLX8EeJFgp8sBgh0vXwtvLyJPAiuAAhGpBf5VKfUbgsbe0yJyM3AE+FLizuo4TU3GGn1Wq4XS0kxt9Gk0CWTLli3MmzePjIwMo6XEjFNPPZWcnBw2b97MjTfeaLScpCKaruKB3k8RSSX4orh+0DrrgRtDWTyXcbz3c9htwxnPQnwB2BOxr2tExB7qca0Eto3z/GJOOHNnojJVxhKPx4PVaqW5uXn0lYegr6+Pmpoa+vv7mTlzpmnGNSaqAH082bZtGxUVFcyZM4fU1FSuueYagMFxuMNlDowm6+BgBsbOKqVqCL7ELo3pSY2T7v4A/QGdxGUowoZwkx7XFxeUUi8qpeYqpU5RSt0bmvdIyOAj1PbWhJYvUkrtiNj2WqXUNKVUilKqLGTwoZRqVUqtVEpVhv4aklK4sTFo9BUXG1ecubw8Sxt9Gk2CUEqxefNmli1bZrSUmGKxWDjnnHMGvJia6Bm1K30ivZ/DbRva9c9EZDHB0M1DwC2hbT4QkacJJnvxAWvM4H0IE87cmejEJbFAKUV+fv5A8pOsrKyot+3q6qKurg6AWbNmmarXyGq1kp6ejsvlIisrKym/m7q6OsrLjzu4Q6mVB7tyDBk7m4hQ6sjw2HZbFmTMpP/wXhp8rpgfK5b0u3to2BXfB0+H7cQQ15SseeyuqaX3w7px79NMiRrcvqHzoDgGnbeZNCcjYU9fYaExY7AhaPTt3DnkkEaNRhNjDh48SEtLC8uXLzdaSsxZtmwZP/nJT8b8LjvViSp+KjQG6MVB8x6J+F8Ba6LdNjT/yyMc717g3mi0JRqPx0NaWlrSFowuKCigq6uL2tpaKioqRl3f6/XS2Ng4UIuvvLzcNB6+SMIF6Ht6esjJMS4z3HgZpmCx4WNnQ9riHkodGR7792Mu9jW4mLXgdGwWc7ezhl2bmbY4vg/UxQUnRhU0HujE5UtjxTmV496nmRI17GrxDDl/8HmbSXMy0tTkIiMjhYwM48KCy8uz+POfD6KUStpnqEaTLPz7v/87EOxUngw1+iJZvnw5gUCAbdu2sXLlSqPlJA06fmoMKKXwer1JGdoZxmKxUFZWhlKKgwcP0tfXd9I6gUAAl8tFa2sr+/fvp7u7m6KiIubMmWNKgw+CmTwdDgculytp6idGUlZWxtGjx511tbW1AIPTNA433nXYcbBKqUallF8pFQAe5XgI53iyECaEJrePdJuY3uAziiKHjRaPH38gJpUCNFOEpiaXoaGdAGVlWbjdPtrahjb0NRpN7KiuriYtLY3S0lKjpcScc84JBjPpEM+xoY2+MdDf308gEDCt4RMtaWlpzJ49G4CWlhaam5tpb2+nra2N5uZmjh07RkdHBz6fj8LCQubNm0dRUZHpC3tmZGSglMLlMndI4FAsWbKE/fv3U1NTQ19fH+vWrQPoGLTalBg72+T26fF8I1DssOFX0Oo1TdS7JglobHQZlsQlTHl5MAxLj+vTaOJPdXX1pCrKHkleXh6nn346GzduNFpKUqHT442BZE7iMhiHw0FlZSV1dXV4PJ4Bj5/NZiMzM5O0tDRSUlLIz883WGn0pKamkpKSMlC3L5nCh2w2Gw8++CAXXnghfr+fm266id27d3um2thZrz9AuzdAZY7OTDkcRenBMatNbt9ANk+NZjSamlzMmpVtqIZIo2/x4iJDtWg0k5ne3l7q6uomVVF2gKqqqoH/S0pKeP311+nq6iI729jftmRBvzGMgclk9EEwAUpmZuZA9svJQEZGBh0dHUkZhrtq1SpWrVo18Pnuu++ecmNnm0NZKbWn7ziDx7wFlMIi0OjycVr8q5FoJglNTS6WLi0xVMNxo2/omqoajSY2TMai7IM57bTTeOWVV9i4cSNf+MIXjJaTFGijbwx4PB5SU1Mnpat8OEar42c2HA4HXV1d9Pb2Jp3RpwnWnwPI1kbfsFhEyEqx6LINmqgJBBTNzS6Ki43NulxcnEFKikWHd2o0cSY81i08lGcyUlFRQVZWFhs2bNBGX5ToN6sxEM7cqTEvIkJ6ejperxefz2e0HM0YaXL7cFgFuzV5QnONICvFwjG3b7isrxrNCbS1ufH7leFj+iwWYfp0XaBdo4k3mzdvpri4eFJFcg3GarVy/vnn89e//jUpE/gZgTb6oiQQCNDX16eNviQgXEMwGRO6THUa3X6KHLakGo9pBLmpVrx+RZtO5qKJgnCNPqONvqqq97DZLGzffsxQHRrNZEYpxZYtWyZ1aGeYq666ivr6el3DNUq00Rclk20832TGarWSlpaGy+XSnpAkwh9QNLt9FKfrqPPRyLUHk7nU9WpvtmZ0wkaf0SUbAJzONF2yQTMsInKRiOwTkQMicucQy0VEfhlavltEzgrNLxeRv4nIXhH5QERuT7x6c1BdXU1zc/PHJKHnAAAgAElEQVSUMPouv/xysrKy+N3vfme0lKRAG31Roo2+5CI9PZ1AIDDwvWnMT7PHj1/BNG30jUqGLRgCW6+NPk0UNDaaw9MHkJubRkeHh4CuM6kZhIhYgYeAi4EFwLUismDQahcTLDFUCawGHg7N9wHfUUrNB5YBa4bYdkoQHs83FYw+h8PBl770JZ555hkd3RUF2uiLEo/Hg8ViISUlxWgpmiiw2+1YrVZ6e3uNlqKJkmOuoAFToo2+URERStNt1PX2Gy1FkwSYyehzOu34fMHEMhrzUFX13sBkIEuBA0qpaqVUH7AOuHzQOpcDj6sgW4BcEZmmlGpQSr0LoJTqBvYC0xMp3ixs3ryZzMzMSVmUfTBVVVU4nU56enp4/PHHjZZjevTbVZR4PB7sdrsea5QkhBO6dHd34/V6sdvtRkvSjMIxlw+7VchNtXDYaDFJQGmGjXeOufH6A9ituv9OMzxHj3Zht1spKHAYLYW8vGC0zNGj3YZnE9WYjunA0YjPtcA5UawzHWgIzxCRWcCZwNbBBxCR1QQ9hBQXF8d9LFhPT0/Cx5u9+uqrVFZWUlBQENX6VqsVp9Pc9X9G0rh06VIqKiq45557qKysxGq1JlidMd/zeNBGXxQopfB6vbr4Y5IRNvra29spKTG2PpVmdBpc/UxL10lcoqUsIwWFm/peH7OzdTF7zfAcOdLNjBnZpmhbTudxo+/ss/XvsuYEhrpBB8cBj7iOiGQCzwJ3KKVOKgiplKoCqgDOPvtstWLFinGLjYZNmzYR72NE0tvbS3V1NXfeeWfUJbecTqfpy3ONpnHlypX8+te/pqWlhauvvjqByoIk+nseL7p7OAp8Ph9+v1+P50sywgld2tvbdTpfkxNAaPb4KXHofqhomZ6RggU40pM8IZ67WjwnTZr4c+RIFzNmZBktAzjR06cxF4cOdfKb37xPfb3XKAm1QHnE5zKgPtp1RCSFoMH3e6XUn+Ko07Ts2LEDv9/P8uXLjZaSUBYvXszChQu56667cLvdRssxLdroi4LwDaSNvuQjPT0dv99PV9dJHX4aE+Gy2gkoPZ5vLKRahWkZtqQy+jy+AIe7+/io3cv+Ti+dfX6dYTcBHD7cxcyZ5ohUycxMCRVo17/JZqKlxc2DD+5k27ZjfPe71Rw82GGEjO1ApYjMFpFU4Bpg/aB11gM3hrJ4LgM6lVINEnRj/wbYq5R6ILGyzUM4icuyZcsMVpJYLBYLv/zlL6mpqeHnP/+50XJMizb6oiBs9Dkcxo+H0IwNu91OSkoK7e3tRkvRjECvNdi2tNE3NmZkptDQ66PPb5zhFI33rrc/wItHunmjwcXejj6O9vZT3dXP5kY3u9u8+EySyXG86eJH2lZEfiwidSKyKzStStT5AHi9PhoaepkxwxxGn4iQl2entrbHaCmaCJ588iP8fsUdd5yFw2Fhw4aahGtQSvmA24CXCSZieVop9YGI3Coit4ZWexGoBg4AjwLfDM0/F/gy8Dmj2prRVFVV8fvf/56SkhKeffZZo+UknM997nNcddVV/OQnP+Ef//iH0XJMSVRGX5wehD8XkY9C6z8nIrmh+bNExB3RaB+JxYlOBLfbTVpaGhaLtpGTDRHB6XTS29uL12tYyIpmFHqsDhxWISdVt7GxMCMzhQCYOovnxx1eHtvbzgdtXmZkpvCpknQ+X5bJ56ZnUJGdSoPLx/ZmN36DDb+JpIuPYttfKKUWh6YX43smJ1JXFzSuzBLeCcEQTx3eaR4aG3v54IMWPvOZcubPz2flyjz27GmhtTXxYXJKqReVUnOVUqcope4NzXtEKfVI6H+llFoTWr5IKbUjNP9tpZQopU43qq0ZTSAQ4MCBA8ydO9doKYbx8MMPU1JSwtVXX01ra6vRckzHqG9YcXwQvgqcppQ6HfgYuCtifwcjGu2tGIhSCrfbnbRevra2toHJ7/ef8NnsA3djRW5uLsCUOd9kpNfqoEQncRkz0zNSsAgc6jaf0aeU4u0GF3+q6SYn1crX5uUyP89OZkrwsZNiESpyUlmcn0ZnX4D327xGh3qOO118lNsawuHDwTBKs3j6IGz06fBOs/DUU/tQCpYuDSbWWbky+Mx86606I2VpxkhtbS0ej4fKykqjpRiG0+nkqaeeora2liVLlvCrX/2Kqqoqo2WZhmi61ePyIFRKvRJy5QNsITgY13T09fXh9/uT1ujTQEpKCtnZ2XR0dJg6octLL73EvHnzqKio4Kc//elJyyerR70/oHBb0nRo5zhItQrlGSkc7OozWsoJ+AKKFw518/YxF6c57dwwN4eCYZL0lKTbmJuTyjG3jwaXocXmh0sFH806o217W6gN/lZE8mIneXSOHAkaV2YZ0wdBo6+urge/37y/x1OJJ574kPLyLEpLMwEoLExl/vx83n230WBlmrGwf/9+ACoqKgxWYizLly9n3bp1HDp0iF//+tf4/X6jJZmGaN6yJlI3JZptAW4Cnor4PFtEdgJdwN1Kqbei0BkX9Hi+yYHT6aSrq4uurq4Bz5+Z8Pv9rFmzhldffZWysjKWLFkCMDhzUKRH/RyCHvVzIjzqnyfYxraLyHql1IcEPep3KaV8IvIfBD3q3wvt76BSanHcT24Umt0+lIg2+sbJKTmpvF7XS2efn5zUxNcnGoxfKf5U3UV1dz+fLU1naZFjVA/u7KwUGt0+PurooyDNRqrVEI/vRNLFj7Ttw8A9oc/3APcTfOaduOMY1Q8bXC/qjTeCyQ+rq3dSW3u8n9fpTEzontXaj9N5YgLGsjIPfr/iT396jcLCxJYbMVs9LaP1tLf3s337Ma67rmjge7Ja+1myxMbatS6eeupliot1ndtkYP/+/RQUFJi+5l68GOzRu+GGG/jd737Hf//3f3PLLbfoIVpEZ/TF60EY3FDkB4AP+H1oVgMwQynVKiKfAJ4XkYWD660kqsBmRkYGaWlpbN16Uo1PXC4XO3fujMtx40Ey6Y2l1nChzry8PGpqaujs7IzJfiOZ6IP7gw8+wOl0cuTIEY4cOcLSpUvZvXv3YOt0wKMObBGRsEd9FiGPOoCIhD3qHyqlXonYfgtw5bhFxomwd2eaNvrGxSnZKbxeBwc7+zir0NjOqYBS7Grx0Ozxc/GMTM7Ijy7jsYhwWp6dvze6qe7u49RcQ14yJ5IuPnW4bZVSA+4SEXkU+MtQB49V/bDB9aLuvvsPZGencuhQPqtXnzEwv6rqvXHtf6w4nfW0tZWeMM9uTwEaKC8/jWXLSofeME6YrZ6W0XqeeWYf8B7l5TNpaws+cpzOembOnAM00tNTytVXLzJMnyY6/H4/H3/8MaeffrrRUkzDpz71KXp6enjuuee47bbbeOihh6b8EJJo3rLi8iAEEJGvAJcCK0MvsiilvIA39P8/ROQgMBfYEXnARBXYPHDgAFarlUWLTv7R27hxI2eeeWZcjhsPdu7cmTR6Y6k13OvV1tZGfX09S5YsISMjIyb7DjPRB3dLSwtnnHHGwD6OHj3KY489NrgLfFJ61Ot6faQE+slK0b1w48Fpt5KbauFgl7FGn1KKXa1Bg++i8ugNvjBZqVamp9s40t3PrMyUOKkckYF08UAdwXTx1w1aZz3BUM11BNtYOF1883Dbisg0pVRDaPsvAHvifyrHaW31DBRENwuRtfqmWGZ50/HGG7VkZKScFP5bWppBTk4qr756iJtv1kaf2dm+fTu9vb0sXLjQaCmm4qKLLqK3t5eHH36YQ4cOccUVVwCwevVqg5UZQzRGX7wehBcRDDP7jFLKFd6RiBQCbUopv4jMIRjKVj2Rkxwvfr8fj8dDUVGREYfXxJjc3FwaGxtpaWmJudE3UYZJYGG4Rz20bdy86go4kDWPdE8nb7yxb2C+22eOFP6j0e/uoWHXZkOO3WE7/rU70kqo9jp5bdOb2Bh9nFQsQ8rC31V9egnNjkJKe+tp2NVKwyjbDUW2JYX63Lm8X3MUz0cn7iHeYXChEOhwungr8NtwuvjQ8kcIpotfRTBdvAv42kjbhnb9MxFZTPB2PwTcEreTGIJjx3o59VRzhXuFjb7aWp3B02jeeOMon/xkKVbriZ1uIsL8+fm89toRAgGFxTK1PSRm56WXXgp9Z/ONlmI6vvjFL+J2u9mwYQO5ubmm8vQnmlGNvjg+CB8E7MCrIXfrllCmzvOAfxMRH+AHblVKGZJ20eUK2qLp6elGHF4TYywWC/n5+TQ1NeHxeEhLM0/vd1lZGUePHnfW1dbWAgxOyZhwj3poedy86h1eP1s/bKdU+k74IR6q1psZadi1mWmLlxty7MUFx+/fBlc/a/d1UrhoaVQetliGlO1q8dDo8tHc6qE8w8bC8omlC+9p81Ar+Zx9duVApk9ITBhcKMX7i4PmPRLxvwLWRLttaP6XYywzajo6PHR0eJk2zVydXOnpNrKyUjl0SGfwNJLWVjfvv9/CNdecOuTyU091smVLA3v2tHD66YUJVqcZCy+//DKzZs0iMzPTaCmmQ0S47rrr6Ojo4KmnnmLatGlGSzKMqOKpxls3ZbhtQ/MrlFLlg0szKKWeVUotVEqdoZQ6Syn159iecvT09vYiItrom0Q4nU5EhJaWFqOlnMCSJUvYv38/NTU19PX1sW7dOoCOQautB24MZfFcRsijToQ3XkRSCXrU18MJHvXLBnvUQwlgMNKjfrQnaNdm+XoTfehJRYnDRm6qhb3tia9F6fEH2NPuITvFwvy8iY/Fm52digK2NSW+Rthk48MPg3Wqpk0z14ugiDB3bh779o2tP7eq6r2BSTNx7r77bQA6Oob+3aisDCaaffPNo0Mu15iD1tZWtm3bxoIFg6upacJYLBZuvvlmioqKqKqq4tChQ0ZLMgQ9iGYEent7cTgcOuPPJMJms+F0Ouns7KSvzzxp7m02Gw8++CAXXngh8+fP56qrrgLwiMitYa86QS9CNUGP+qPANyHoUQfCHvW9wNODPOpZBD3qkaUZzgN2i8h7wDMY5FGv7e3HbhXSA4k3ViYTIsKCPDuHu/vp7k9cemqlFHvavPgVnJ6fhiUGg+TTbRampdvY2eLG7dMp/SfCBx8Ejb7SUnN5+gDmzXPy0Ue6dqqRfPxxOykpFmbNGrqcR0GBgxkzsnjjjdoEK9OMheeff55AIKCTuIyCw+Hgm9/8Jn6/ny984Qt4vVPvvUOnyxsGn8+H2+2msFCHNEw28vPzaWtro7m5menTB5fhMo5Vq1axatWqgc933313LELLhizYo5R6Fnh2oponypGefsoybIh+95swi/LTeKfRze5WL+eWJCY64d0WDy0ePwty7SeEYk6UOVkpNLh8/KPZw6em6UiL8fLhh62kplrIzw8m+DGTh+zUU508+eReXK5+0tMNSdwz5dm/v505c3Kx2YZvu+edV84rrxxCKTXlMx+alaeffpo5c+Ywc+ZMo6WYnuLiYm666SYeeughvv/973P//fcbLSmhaBfWMPT09ACQlZVlsBJNrElNTcXpdNLe3j4le3rMQrvXT7s3wJzsxNbpmizsavGcMB3u7iffbmVXi4fA0ImBYkpXn59N9b0UpFkpz4xt/2FWqpWKnFR2NLvp8ydHUh8z8sEHLZSUZJgyCce8eXkoFTQ8NImnvd1DbW03c+eOXLf2M58po6nJNeZQXE1iaG1tZePGjVx11VXaKI+S008/nTVr1vDAAw/w6quvGi0noWijbxi6u7uxWq26KPskpbCwEIvFwrFjx4yWMmWp7gqG12qjL3bMyEyhuz/A/s74hy5vrOtFKViQZ4/Ly8byYgcef7AMhGZ8fPBBK6Wl5hrPF+bUU/MBxmxM7N/fzlNPfcSllz7Lf/7nP/QYv3Hy9tt1KAVz5+aNuN5555UB8OabOsTTjDzzzDP4/f7wkBBNlPz85z9n/vz5fOUrX6G1tdVoOQlDG31DoJSip6eHzMxM3XMySbHZbBQVFdHd3U13t04bbgTVXX3kplrIs1uNljJpKHQEa/ZtaXQPVwYkJlR39bGvo49PlqSTPkJo2ESYnpHCjMwUtjW68QW0t2+sNDe7qK/vMa3RV1mZiwhjGte3a1cTv/jFP3jrrTpefLGGBx7YQU+PecZmJxNvvHEUm83C7Nk5I65XWZlHSUmGHtdnQpRSPPLIIyxatIjFixcbLSepcDgc/OEPf6ClpYVvfOMbcX1emglt9A2By+XC7/fr0M5JjtPpxG63U19fj9+fuOQXGvAFFEd6+rWXL8ZYRFhWnE6Dy8eh7sEVP2KDL6B45WgPTruVpUXxjYT4ZLGDHl+APW06DHusvP12HQAVFSOH7xmFwxEsCL5vX3ThnUeOdPHYY+9TXp7Fz352HmvWLKa+vpcnntgbZ6WTk9dfP8KcOTmkpIzc6SYinHdeGW+8cXTKvBgnC1u3bmXXrl2sWbNGOyjGSFVVFdu2beOyyy7jueee4ze/+Y3RkhKCNvqGoKOjAxHRRt8kx2KxUFpaSn9/P42NjUbLmVJUd/XRH4CKHG30xZrTnHayUiy81eCKy0valkY3HX0BLijLwBbnsWIzs1KYlm5jS6ML/bo5Nt56q5a0NBszZw6dmdEMzJvnZO/e6EKrvve9NwG45ZbTSU9PYdGiQi69dA47dzaxZ4+5SvCYndZWN7t2NXHqqc6o1v/MZ8qoq+uhpqYzzso0Y+HBBx8kKyuL66+/3mgpScv555/PypUr+fa3v82ePXuMlhN3tNE3iEAgQFdXF9nZ2VitOuxsspORkTGQzbOrSxcKThR72704bMLMLJ21L9bsafMyMyuFepePDUd6Ylrovt3rZ3Oji/m5qcxKgJdWRFhe7KCjL0BryshhaJoTefPNWpYtmzZiZkajOeusYt5/vwWXa2Sv9LZtDaxb9xEXXDATp/O4d/nzn59JSUkGTz+9D79fl/eIlk2bjqIUURt9elyf+di3bx9PPvkkq1ev1gXZJ4DFYuGJJ54gJyeHK6+8ctIP9zHv08Agenp68Pv95OaaMyRGE3uKi4tJS0ujtrbWVLX7Jit9fsWBrj5OzbVj1SEpcWF6uo3sFAsfd/bFbDycUoqXj/ZgFeFzZYmr+1aZk0pBmpU6e6EOL4uS7u4+du5s4tOfNk9JmqE499xSfL4A27ePnFDr3/99K3l5aVxwwawT5ttsFi677BQaG1388Y8fx1Hp5GLjxsNkZqYMW59vMAsWFJCf7+D114/EWZkmWn784x/jcDj43ve+Z7SUpKekpIQnn3yS/fv3s3r16kn9nNFG3yBaW1ux2Wy652QK0dHRQXZ2NkopqquraWlpoa2tbWDSxJaPO730B2B+nt1oKZMWEWF+nh2PX7GvIzbj4Xa3ejnU3c+K0nSyRhkHFEtEhGXFDtzWNA506U6ZaHj77VoCAcV555UbLWVEli8vBeCdd+qHXeeDD1p44YUDfOtbZ5KWdnJpkDPPLGLatAzuuWczAZ3wZ1SUUrz22hHOO68MqzW6V0CLRbjoolm8+GINPp/2qBrNm2++ybp167jjjjt47rnnqKqqoqqqymhZSUtVVRUff/wxl112GevWreNnP/uZ0ZLihjb6InC73fT29pKfn68HxU4xbDYbTqcTn89HW1sbgYB+sMUDpRQ7mj047VbKM2Jb201zInl2K7OyUjja6+PgBEs4dPb52VjXy4zMFM4sSIuRwuiZn2fHHuhj87H4ZiWdLDz77H4yM1M499xSo6WMiNPpYP58J3//e92w63z96y+TmmohZ5jxvxaLsGrVbD78sJU//Ul7+0Zjz54W9u9v55/+6ZQxbXfFFRW0trp5553hvytN/HnwwQf50pe+REFBAcXFxUbLmVRceOGFLFmyhDvvvJP77rtvUj5rtNEXQUtLCxaLBaczujh3TXIQ6bUbagpjt9vJy8ujr69PG35xorbXxzGXj7ML03THSgKozE4lK8XCnw930+EdX4ZapRQvHelBoVg1w5gyNlYRSr3N1Lt8fNShvX0j0dcX4NlnP+aKKypxOMw/ZvaTn5zOO+/UD+mlO3Sok23bjvHpT5eRmTn8GNKzzy5h7tw87rlni/b2jcIf/7gPi0X44hcrx7TdhRfOJjXVyvPPH4iTsqlF2Ds3Fi9dIBBg7dq1NDU1ceONN2K362iZWGKxWPja177GlVdeyf/+3/+b66+/nrq6ydXJobvaQ7hcLjo7OykoKNAJXKYwDkcwSUB7ezvNzc26AyDGbGl0kWYVTnMm3ls0FbFahMX5aWxrdvNsdRfXV+aQNsbEHjuaPdR09/P5sgxyDaypWNTXjts5k9dqe5idlTLm85gqbN/eRUeHl2uvPdVoKVHx6U9P5ze/eZ8dO46xdOm0E5bdd992RIIJW0bCYhHuvnsZN964geef388Xvzg3npKTFqUUTz/9MZ/5TBlFRWMbl5uVlcrKlTN47rn93HffCixxztw7GaiqqsLpdPLLX/6SPXv2YLFYeP/992lubqarq4usrCzKy8spLy9nxYoVVFZWDtup5vP5uP3229mxYwdf/OIXmTdvXoLPZmpgtVpZuXIlfX19PP300zz77LOsXLmSs846i4KCArZu3YrVasVqtXLxxRfjdDrJz8+noaGBzs5OsrOzo+oYjTT0V69eHc9TOgFt9BH8IWxoaMBms1FYWGi0HI3BOBwOrFYrbW1ttLS04HA4yM42b9rzZOFAZx8Hu/r5bGk6qVb9wpAoMlIsfGF2Fk8f7OKP1V18aU7093JNVx+v1/UyNycVC8Q0E+hYEeCiGZms3dfBhqM9XDErS3uLh+DFF5vJz3eMaiiZhSuuqCQ9/TUefXT3CUbfxx+3UVW1m+XLS8nLG72TqKenj2nTMvgf/+NVLr30FFJTdeftYHbsOMa+fW3cfvtZ49r+y19ewHXX/ZUNG6q55JKxhYdORbq7u/nzn//Ma6+9hsfjweFwUF5eTm5uLjk5ObS3t/POO+/g9Xp5/PHHKSsrY+XKlQNlBKZNm4bP5+Ptt9/mhz/8IW+//Taf//znueCCC4w+tUmNxWLhkksu4ZxzzqG5uZkNGzbwyiuvnFTP+fHHHz9pW6vVSl5eHhaLhUAgQCAQQClFIBCgsLCQhQsXDhiVRpSF00YfcOzYMdxuN+Xl5drLpwEgNTWVgoIC2tvbOXLkCDk5OUybNg2bTTeZ8eDxB3ittod8u5WzC+Nb0FtzMrOyUrlsVhbrD3XzhwOdTJfRw/4OdffxbHUXBWlWLpmZyd5248MqS9JtrChN52/1LrY0ullekm60JFOxdWsD77zTyT33nDtq0W2zkJNj57rr5vOHP+zlvvtWkJNjRynF7be/jsNh47LLojMurFYLV145l1/9aie/+tW7fOc7S+KsPPm4996t5OYGr/d4uPLKuXz3u2/ywAP/0EbfCPT39/Nf//Vf/OhHP8Lj8XD22WfzqU99isrKSiyWEyMUAoEATU1NFBUVsXHjRv785z+zdu1aANLS0vD5fPh8PgoLC1m7di0ej3Edb1ONgoICCgoKmD9/PoFAALfbTSAQwO/34/f78Xq9uFwuent7AZgzZw5tbW20t7ejlGLv3r2IyEDnZFdXF++88w4vvPACVquVT33qU1x66aUJPacp/wbb0tJCa2srTqeTnBxdB0pzHJvNRkFBAX6/n+bmZnp6esjPzyc/P193DoyBgFKsr+mmqy/AtZU5WHVYkCGcmmsndY7wfE03rZmnUNrq4TSn/SRvWUAp/tHs4W/1veTbrVxTkYM9yix/iWBpkYNjLh9vNLiwWoSlRboTASAQUHzve2+Qm2vjjjs+YbScMXHrrWfw2GPvc9ddb/LQQ+fzwx/+nZdeOsQvfvFZ0tOjf01ZuDCfRYsK+MEP3ub882dyxhlFcVSdXLz3XhMvvHCAH//4k+TkjG8sWEqKlW9960y+9703+fvf6zj3XHOXBDGCV155hTvuuIO9e/cyf/58vvGNb5CRMXworcVioaSkhNWrV3PrrbcSCAT40Y9+xEcffUR7eztWq5WvfvWrXHHFFaSnp+ssnQZhsVhG/B6dTidtbW1kZWUxc2YwyuLMM88cct36+nr+9re/8dZbb7F161b8fj+33347aWnxH/YS1ZNcRC4SkX0ickBE7hxiuYjIL0PLd4vIWaNtKyJOEXlVRPaH/uZFLLsrtP4+Eblwoic5FH6/n/r6eo4dO0ZWVhYlJSXxOIwmyRERioqKqKioID09naamJvbt20dDQwNud2wzCb700kvMmzePiooKfvrTnw6lJenamccX4I8Hu6ju7ueC8kzKM82fWGIyMyc7la+dmktaoI+/HunhsY86eOeYi4OdfVR39bGl0cVv9nawsa6XOdmpXFeZQ0aKeQw+CLbJS2dlMTcnldfrevnL4W48MUojn+hnXSz5/vff4o03arn55ukjJj0xI5/4RAn/8i+f4OGH36Ok5GHuvXcLX//6Ir797bGFIYoIN964EKczjX/+5/UcPtwZJ8XJRXd3Hzfc8CJ5eWljvqaDufXWM5g1K5sbbvgrHR2x9zrFow0mgq1bt3L++edz4YUX0tfXxwsvvMDtt99OeXl0ZVPCCV0ee+wxZsyYwQUXXMDVV1/NlVdeyXXXXUd6uo5qmCyUlpZy/fXX86//+q9UVlZy5513Mn/+fP74xz/GPWPoqE9zEbECDwEXAwuAa0VkwaDVLgYqQ9Nq4OEotr0T2KiUqgQ2hj4TWn4NsBC4CPiv0H4mjFIKt9tNY2Mj+/fvp62tjfz8fGbMmHGSy12jicRutzNz5kxOOeUUMjMzaW1t5eDBg3z88cfU1dVht9vxeDzjzvjp9/tZs2YNGzZs4MMPP+TJJ58EGNztkzTtrMPrZ/MxF7/+sJ3DPf1cXJ7JYgNS/WtOJs9u5bTeav5pZiZpVuHNBhd/rO7i6YNdbKp3YbPAFbOy+OfZWThMmizFKsIVs7M4t8TBnjYvD3/Yzt/qejnm8hEY50Mz0c+6WIQmtpQAACAASURBVNHU1MvNN7/Ef/zHNm699QwuuaQglrtPGPffv4Jf/OKzfP7zM/nVrz7Hr399wbiShWRnp/Lss5fT0uJm+fI/8NRTH03pjJ4HDrSzatWz7N3bytNP/1NU4yNHIjvbzh/+cClHj3azcuUf2bu3NUZK49oG40JzczNPPPEEF1xwAcuWLWP37t1cddVV/Mu//AvHjh3TY441I1JSUsJtt93Ga6+9RnZ2NldddRWf+MQnePTRR2lqaorLMaOJm1gKHFBKVQOIyDrgcuDDiHUuBx5XQRN1i4jkisg0YNYI214OrAhtvxbYBHwvNH+dUsoL1IjIgZCGzeM5wb6+PhoaGvB6vfT39w9Y0ZmZmRQVFeneE82YcDgczJgxA5/PR3d3N52dnXR2dpKVlcWBA8FU1jabDZvNhsViGcjyNPjHXylFamoqRUXB8KNt27ZRUVHBnDlzALjmmmvYvXt37qDDm7adAbxytIdGt48Or59eX7CdzcpKYUVpBiVjCNHSxB8BFjrTWOhMw+0L0OoJDlDPtVvJNJlnbzgsInx6Wgbzcu283eBiW5ObrU1ubALZqcHzSLMKFoELyjNJH92ATfSzbly89tphnntuP11dfRw40M727cdQCu666xz+7d/O5e233xzvrg1FRGIWlrp8eSl///u1XH31n7nmmr+wZs1GPvGJYoqK0nG7W9iw4Y2BY4oc/xs5L0xkH0JkL/xo86Nd9+jRo/zlL5uG2cf4jg3Q1eVl7942duw4RlZWKo8/vorzz49Ncp/ly0t57rkr+OpXN7Bgwf/PWWcVM3++kxtuWMBFF82eyK7j1QbHTEdHB/fffz/9/f0D4+q8Xi+tra00NTVx6NAhDh8+DMCMGTO49957+da3vhXusI0pOqRzcrNy5Ureffdd1q5dywMPPMDq1atZvXo1paWlzJs3j/LyctLS0gamGTNmsGbNmnEdK5o3senA0YjPtcA5UawzfZRti5VSDQBKqQYRCQffTwe2DLGvExCR1QR7eQB6RGRfFOcSawqAFgOOO16SSW8yaYWJ680DskXkcOizE5g2aJ2EtzNIWFtLtu87jNadOOKpeSaJf9adwETb2f/5P8EJ8323Q+q55Zb4HXDwvltb4ZVXRtZjIHHX09UF118fnKJkQFM039O77wan3/9+1FVHszrj1QYHiMfz7MiRI/zgBz/gBz/4wVCLzXa/DYXWOHHGre+WYRpZfX099fX1Qy677bbbRtrlsO0sGqNvKP/04FiJ4daJZtvxHA+lVBVgaPeHiOxQSp1tpIaxkEx6k0krTFyviHwJuFAp9fXQ5y8T7PU8YbUhNo1rO4PEtLVk+77DaN2JI96aQ21wMPF81p24cozamdm+W61nZMymBwzVFPf3zUS/O5rx+x2M1jhxzK4vTDRGXy0QORK1DBhseg63TuoI2zaKyLRQz+c0IBzAGs3xNJrJhm5nGo2xJLoNajSaE4lXG9RoNESXvXM7UCkis0UklWDyh/WD1lkP3BjKqrQM6AyFs4y07XrgK6H/vwK8EDH/GhGxi8hsgoN1t43z/DSaZEG3M43GWBLdBjUazYnEqw1qNBqi8PQppXwichvwMmAFfquU+kBEbg0tfwR4EVgFHABcwNdG2ja0658CT4vIzcAR4EuhbT4QkacJDr71AWuUUv5YnXCMSbbRtcmkN5m0wgT16naWdN93GK07ccRVc6LbYBwx23er9YyM2fSAQZri2AaNxIzf72C0xoljdn0ASLxrQmg0Go1Go9FoNBqNxjiSIze3RqPRaDQajUaj0WjGhTb6NBqNRqPRaDQajWYSo42+cSIiF4nIPhE5ICJ3Gq1nMCJySETeF5FdIrIjNM8pIq+KyP7Q3zwD9f1WRJpEZE/EvGH1ichdoWu9T0QuNIHWH4tIXej67hKRVWbQmmyYvR1FYvY2FaEzadpWJLqdTRwztCej24nZ7n+z3dciUi4ifxORvSLygYjcHppv+t8IszPWe88AfWP+7g3QmCYi20TkvZDG/89sGkN6rCKyU0T+YkZ9w6KU0tMYJ4KDhA8CcwimCX4PWGC0rkEaDwEFg+b9DLgz9P+dwH8YqO884Cxgz2j6gAWha2wHZoeuvdVgrT8G/tcQ6xqqNZmmZGhHg/Sauk1FaEqathWFbt3Oor9+pmhPRrcTs93/ZruvgWnAWaH/s4CPQ8c1/W+E2aex3HsG6RvTd2+QRgEyQ/+nAFuBZWbSGNLwP4E/AH8x2/c80qQ9feNjKXBAKVWtlOoD1gGXG6wpGi4H1ob+XwtcYZQQpdSbQNug2cPpuxxYp5TyKqVqCGbtGly4PG4Mo3U4DNWaZCRrO4rENG0qTDK1rUh0O5swZm5PCWsnZrv/zXZfK6UalFLvhv7vBvYC00mC3wizM8Z7L+GM47tPOCpIT+hjSmhSmEijiJQBlwCPRcw2jb6R0Ebf+JgOHI34XBuaZyYU8IqI/ENEVofmFatgPRtCf4sMUzc0w+kz6/W+TUR2h0I6wq58s2o1I8l2rZKxTYVJtrYViW5n0WGWa2LGdmLG+9/w+1pEZgFnEvSmmPEaTQaMvveHJMrv3hBCoZO7gCbgVaWU2TT+X+C7QCBinpn0DYs2+saHDDHPbLUvzlVKnQVcDKwRkfOMFjQBzHi9HwZOARYDDcD9oflm1GpWku1aTaY2Fcbs34FuZ9FjlmuSTO3EqGtm+H0tIpnAs8AdSqmukVZNlCZNYhjDd28ISim/UmoxUAYsFZHTjNYURkQuBZqUUv8wWst40Ebf+KgFyiM+lwH1BmkZEqVUfehvE/AcwXCMRhGZBhD622ScwiEZTp/prrdSqjH0wxQAHuV4uIvptJqYpLpWSdqmwiRN24pEt7MxYYprYtJ2Yqr73+j7WkRSCL70/14p9afQbFNdo0mE0ff+CYzxuzcUpVQHsAm4CPNoPBe4TEQOEQyh/5yIPGEifSOijb7xsR2oFJHZIpIKXAOsN1jTACKSISJZ4f+BC4A9BDV+JbTaV4AXjFE4LMPpWw9cIyJ2EZkNVALbDNA3QLhxh/gCwesLJtRqYkzdjiJJ4jYVJmnaViS6nY0Jw9uTiduJqe5/I+9rERHgN8BepdQDEYtMdY0mEUbf+wOM47tPOCJSKCK5of8dwPnAR5hEo1LqLqVUmVJqFsHf2NeVUjeYRd+oGJ1JJlknYBXBzEcHgR8YrWeQtjkEs229B3wQ1gfkAxuB/aG/TgM1PkkwrKWfYE/izSPpA34Qutb7gItNoPV3wPvAboKNfZoZtCbbZOZ2NEin6dtUhNakaVtR6NbtbGzX0ND2ZIZ2Yrb732z3NfApguGZu4FdoWlVMvxGmH0a671ngL4xf/cGaDwd2BnSuAf4UWi+aTRGaF3B8eydptM31CQhsRqNRqPRaDQajUajmYTo8E6NRqPRaDQajUajmcRoo0+j0Wg0Go1Go9FoJjHa6NNoNBqNRqPRaDSaSYw2+jQajUaj0Wg0Go1mEqONPo1Go9FoNBqNRqOZxGijL0kRkUdE5Idx2O88EdkpIt0i8u1x7uMDEVkRY2kazYQxY7sRkf8WkZ/EWtMox9wkIl9P5DE1mjBmbIdGIyIbROQro6+p0Wg040MbfQlCRA6JiFtEekTkWOhFLzPKbb8qIm9HzlNK3aqUuicOUr8LbFJKZSmlfjmElk0i4hGR8oh554vIoQhtC5VSm0LLfiwiT8RBp2YKMAnbTY+ItIjInwYVaNZoTMtUb4eJ6CRRSl2slFobz2NoJj+hezs8BSLabY+IXG+0vvEQ+v0532gdkwFt9CWWf1JKZQKLgTOBuwzWMxQzCRbVHYleICa9tBJE34eakZgs7ea20HnMBXKBX8RdVZwREZvRGjQJQ7dDjcbkKKUywxNwhFC7DU2/N1rfYBLxDNHPqePol20DUEodA14m+PAEQETuFJGDobCUD0XkC6H584FHgOWhnpqO0PwTQsJE5BsickBE2kRkvYiUDnd8EbksFILZEerBnB+a/zrwWeDB0LHmDrOLXwLXikjFMPs/FPL+XQR8H7g6tL/3Qss3ici9IvJ3wAXMEZFPish2EekM/f1kxP5mi8iboWvzmog8FOk9FJFlIvJO6Hzek4jQ0tCx7hGRv4e2f0VECoa7NhrzMgnaTfg82oBngdMiZueJyF9D57FVRE6JOO5IbeOrIlId2q4m3JMbmv93EflVaLuPRGTlICkzh2sXw51raNkhEfmeiOwGekXENsr6KvK3IvI7kP/H3p3Hx3WXh/7/PDPa912W5UVe63hJQuzYEEhwElISIKT3VVoSaFlu+8svQLpd+mvp5bbAbSkUaEtYSmpC2kIoYSkNAQwhxBEmECeOHK/xJu+SJcvaJWudmef3xzmjjKWRNNJIc2ZGz/v10ssz53zPOc8Z6+jMc76bSIWI/MjdrlNEfin2ECippet1ONl1JiKfBG6O2O+X3OUPicgFEekVkQYRuTkixq0i8pK77pKI/JO7PEdEHhORDjf+vSJS7a4bq02UcS1kRKTOvY4yIsr+nTj3vX4R+aGIlIvIN91j7hWRuqnO3ywsIuKLuE47ROQ7IlLmrgv/fr3f/Z3uEpEHRORGETno/q5+KWJfU95fRKRYRL4mIi0i0uz+rvrHbfvPItIJfFxEVonILjeudvf3uMQt/w1gGfBD93f9L0Rku4g0jTu/sdpA9/r5nnut9QLvmyqmhcRurh4QkSXAXUBjxOJTODeWYuATwGMiUqOqR4EHgOfdJzUlUfZ3G/Ap4HeBGuAc8Pgkx14LfAv4U6AS2IlzMWWp6m3AL3GfhKrqiUlOoRn4KvDxqc5TVX8K/D3wbXd/10Ws/n3gfqAQ6AN+jJNMlgP/BPxYRMrdsv8JvOiu+7i7bfh8at1t/w4oA/4c+C8RqYw41ruA9wNVQJZbxqSYNLhuwvuqAH4beDli8X1u/KXu+X3SLVvGJNeGiOS7y+9S1ULgJmB/xD63AaeBCuBjwPfDN3lX1OtiqnMdF+9bcWpKVsZQfjIfBprc7apxHhJpDNsZj6TjdTjVdaaqHx233wfdXezFSXzLcO5R3xWRHHfdQ8BDqloErAK+4y5/r/sZLXWP8wAwOFWcU7gX515Y6x7jeeDf3HiO4lzzxoT9MfBbwBuBxUAX8OVxZbYBa4B3Ap8HPgq8CdgA/K6IvHFc2cnuL/8BBIDVOK0CfhP4wyjbVuHc6wTnb8Bi4Bqc6+PjAKr6+1xdY/mZGM/3HuB7OPeob8YQ04JgSV9iPSEifcAFoI2IP8qq+l1VvaiqIVX9NnAS2Brjft8NPKqq+1R1GKfZzesmedL3TuDHqvq0qo4CnwNycb4wzsSngLtFZMMMtwv7d1U9oqoBnIvvpKp+Q1UDqvot4Ji7/2XAjcDfqOqIqj4HPBmxn98DdqrqTvezexp4CXhLRJl/U9UTqjqIc/O9HpNK0uW6+YI4NR0HgBbgf0Ws+76qvuheD9/k1d/RtzLJteGuDwEbRSRXVVtUNbJpWxvweVUddT+b4+7+wia7LmI51y+o6gV323g+m1GcL/rL3Th/qaqW9CWndL4Op7vOJlDVx1S1wy3/j0A28Bvu6lFgtYhUqGq/qu6JWF4OrFbVoKo2qGrvDGKP9G+qekpVe4CfAKdU9efu35Dv4nyxNSbs/wU+qqpN7nX2ceAdcnXTx79V1SFV/RlON55vqWqbqjbjPPiI/J2Ken9xa67vAv5UVa+oahtOE+p7I7a9qKpfdK+dQVVtdK/pYVW9jPPQJTLBnI3nVfUJVQ0BRTHEtCBY0pdYv+U+kd8OrMN5QgKAiLxHRPa71ejdOE1OYm2GuBjn6SgAqtoPdOA8AZyubAjnJh6t7KTcC/NLwP+dyXYRLkwWk+ucG9NioFNVBybZdjnwO+HPzf3s3oDzRTKsNeL1ABDTAAQmaaTLdfPHqlqiqrWq+m73Ggqb7Hd00mtDVa/gfAl+AGgRp3nouohyzeMSqHPu/mZ0zEnOddLrd4afzWdxaox+Jk4z1Y/EsI3xRjpfh1Pdg6ISkQ+LyFG3eVs3Tg1e+Jz/AKfP4DG3qeXb3OXfwGka+7iIXBSRz4hI5gxij3Qp4vVglPd2nzORlgP/HXGNHgWCOC0swmbyOzXZ/WU5kIlzTwof619xavXCIu8fiEiViDzuNrvsBR4j9r8fkxn/PXG6mBYES/o8oKq/AP4d5yklIrIcp7nkg0C5Ok1gDuNUecP0zZ0u4vxS4+4vH+dpYnMMZQWnKj1a2el8FqcPxeYpykwWe+Tyq2JyLXNjagHKRCQvYt3SiNcXgG+4N/HwT76qfjqmMzApI42um5mY6tpAVZ9S1TtwHnIcw/k8wmrdOCO3uzjTY05yrpNev1HKDwCR1++isZ2o9qnqh1V1JU6tyv+SiX0PTRJJ0+twyuuMcecgTv+9v8RpklrqnnMP7jmr6klVvQ/nS+U/AN8TkXy3VuQTqroep3bybcB7osRzhUmuGWNm6QJOV4DI70o5bi3ebEx2f7kADAMVEccpUtXIVmHj/yZ8yl12rTpNon+PV/9+RCt/1fXh9s2rHFcmcptYYloQLOnzzueBO0TkeiAf5xf0MoCIvJ+rB3m4BCyZoo/MfwLvF5HrRSQbpx/dC6p6NkrZ7+BUwd/uPmH8MM7F8OuZnoCqdgP/iDNM9mQuAXUy9eAMO4G1IvIucQaFeCewHviRqp7Daa75cRHJEpHXcXWTm8dwmoG+WUT84nSU3+72OzHpJ+Wvmxma9NoQkWpxBrXId2Ppx3lyG1YF/LGIZIrI7+D0ldgZwzFneq7Tld8PvMu9Pu8kotmOiLxNRFa7Xx563fiDmGSXbtfhpNdZxDmsjChfiNM/6DKQISJ/g9OEDAAR+T0RqXRrIrvdxUERuVVENrlfUntxmntG+33fD9wiIstEpJjkHCnVpJaHgU+6D2kQkUoRuSeO/UW9v6hqC/Az4B9FpEicAWRWydX9AccrxLl/dYszTsP/N279+OvvBJAjIm91/w78H5zm1VHNMqa0ZEmfR9wmJV8H/lpVX8FJnp7H+eXeBPwqovgunGGoW0WkPcq+nsGZQuG/cGrGVjFJW2VVPY7zFOWLQDtOAnW3qo7M8lQeYuovad91/+0QkX2TxNSB88TzwzjNev4CeJuqhs/13cDr3HV/B3wb50aPql7A6bD7v3FuwBdw/mDY73YaSqPrJibTXBs+d/lFoBMnmfpgxOYv4HTKb8fpLP8Od3/THXNG5xpD+T9xl3XjXMtPRGy+Bvg5zg3/eeBf1J3j0ySvdLsOY7gHPYTT/6lLRL6A00TzJzhfPs8BQ1zdnOxO4IiI9Lvb3quqQzg1dt/DSfiOAr/AeXA5Pp6nce5zB4EGXk0+jZmth3DGQ/iZOH1z9+AMqDJbU91f3oMzONgrOAPGfI+ru9yM9wngBpza8h8D3x+3/lPA/3GbZv6524/1g8AjOLXxV3AGBJvKTGNKS6LWZ96kGBH5NnBMVW10MmOiEJH3AX+oqm/wOpZ059ZePgT4gUeiNS0XZxqZz+P0K2lX1QX3hNkYkx7s/pK6bMJCk/RE5EacmowzOCN93gNYnz1jjKfcZnpfBu7AedK8V0SedGu/wmVKgH8B7lTV8yKy4AYPMMYY4z1L+kwqWIRT3V+O88XqA6r68tSbGGPMvNsKNKrqaQAReRznodQrEWXehTMlx3kAd7hwY4wxJqHSonlnRUWF1tXVzWibK1eukJ+fPz8BJYidQ3KYr3NoaGhoV9XxI1J5KpZrLdn+T5MpnmSKBSyesNleayLyDpwavD903/8+sE1fncAbEQk369yAM2DBQ6r69an2O5t72lxItt+HWFjM82+u4k3Ge5oxC0la1PTV1dXx0ksvzWib+vp6tm/fPj8BJYidQ3KYr3MQkfHzRnkulmst2f5PkymeZIoFLJ6wOK41ibJs/JPUDJxpbW7HmUj8eRHZo6onxsVwP3A/QHV1NZ/73OdmGdLs9ff3U1CQWtO7Wczzb67ivfXWW5PunmbMQpIWSZ8xxhjjgSaunjd0CRPnQmzCGbzlCnBFRHYD1+GM/DhGVXcAOwC2bNmiXiS/yfYQIBYW8/xLtXiNMdHZsPbGGGPM7OwF1ojICnceuntxhkWP9APgZnf+tzycYdKPJjhOY4wxC5zV9BljjDGzoKoBEXkQZ942P/Coqh4RkQfc9Q+r6lER+SnOnGshnGkdDnsXtTHGmIXIkj5jUth0c4SJiLjr3wIMAO9T1X0R6/3AS0Czqr4tYYEbkyZUdSewc9yyh8e9/yzw2UTGZYwxxkSy5p3GpKiIOcLuAtYD94nI+nHF7gLWuD/3A18Zt/5PsKZmxhhjjDFpzZI+Y1LX2BxhqjoChOcIi3QP8HV17AFKRKQGQESWAG8FHklk0MYYY4wxJrGseadHGhoaJizbvHmzB5GYFFYLXIh434QzSMR0ZWqBFuDzwF/gzB02qfFDydfX108ZVH9//7RlEimZ4kmmWGB+4xnUwajLcyXXk3hM+tuxe8fY6/tvud/DSIwxJvnElfTF059IRB4F3ga0qerGiG0+C9wNjACngPeranc8cRqTpmKZIyxqGREJX3sNIrJ9qoPMdCj5ZBveO5niSaZYYH7jOTR8KOryTdmbPInHGGOMWchm3bxzDvoT/TtwZ5RdPw1sVNVrceYx+qvZxmhMmot1jrBoZV4PvF1EzuI0C71NRB6bv1CNMSZxnjn6DFs/uZV3f/XdHGqK/gDCGGMWknj69MXVn0hVdwOd43eqqj9T1YD7dg/Ol1RjzESxzBH2JPAecbwW6FHVFlX9K1Vdoqp17na7VPX3Ehq9McbMg0NNh/jNf/5NTl8+zRP7n+DuL91N72Cv12EZY4yn4kn6JusrNNMyU/mfwE9mFZ0xac59OBKeI+wo8J3wHGHhecJwhpI/DTQCXwU+6EmwxhiTAIFggMf3Ps6i4kX89dv+mgdve5ALnRf48Hc/7HVoxhjjqXj69M26P1FMOxf5KBAAvjnJ+hkNLjGe1wMGDAwMTFiWaucwF+wc4jPdHGGqqsCHptlHPVA/D+EZY0xC/erUr2jvb+fB2x4kNyuXVZWr+KPb/ogvPfslPva2j7GkzBoPGWMWpniSvnj6E01JRN6LM8jL7e6X1glmOrjEeF4PGDAXo3d6fQ5zwc7BGGPMXFBVnjr8FCsrV7Jx8dj4cFQWVhIMBfnANz/A3dfdbSN7GmMWpHiad866P9FUO3VHBP1L4O2qOrE6zBhjjDFmnFOXT9FxpYM3rn0jzuDhjsrCStbXrOdXjb8iGAp6GKExxnhn1jV9qhoQkXB/Ij/waLg/kbv+YZxmZ2/B6U80ALw/vL2IfAvYDlSISBPwMVX9GvAlIBt42v2jvUdVH8BENb7G0Ob6M8YYsxC9eOZFMv2ZXL/0+gnrbl5zM/+6+185cemEB5EZY4z34pqnL57+RKp63yTLV8cTkzHGGGMWltHAKA3nGrh2ybXkZOZMWL+xdiOZ/kz2X9jvQXTGGOO9eJp3GmOMMcZ47oUzL9A/3M+W5Vuirs/KyGLD4g0cuHCASYYKMMaYtGZJnzHGGGNS2q5juxCE31j0G5OWuX7p9XQNdLHv/L4ERmaMMcnBkj5jjDHGpLRdx3axtGwp+dn5k5bZtGQTIsKT+8ePOWeMMenPkj5jjDHGpKyB4QGeP/086xatm7JcQXYBdeV1/PzozxMUmTHGJA9L+owxxhiTsn516leMBEambNoZtm7ROl448wK9g70JiMwYY5KHJX3GGGOMSVm7ju0iw5/B6qrpB/++puYagqEgu0/sTkBkxhiTPCzpM8YYY0xKau9v5/EXH2d52fKoUzWMt7JyJblZudbE0xiz4FjSZ4wxxpiUdGX0Cuc6z03bny8s05/JzatvtqTPGLPgWNJnjDHGzJKI3Ckix0WkUUQ+EmX9dhHpEZH97s/feBFnujracRRVjak/X9ib1r+JIxeP0NLdMo+RGWNMcrGkzxhjjJkFEfEDXwbuAtYD94nI+ihFf6mq17s//zehQaa5w5cPk+nPZGXlypi3edM1bwLgmWPPzFdYxhiTdCzpM8YYY2ZnK9CoqqdVdQR4HLjH45gWlMPth1lVuYpMf2bM21y35DrKC8r5+SvWxNMYs3BY0meMMcbMTi1wIeJ9k7tsvNeJyAER+YmIbEhMaOnvct9lzvWem1HTToBHnnuEuvI6fnDgB6jqPEVnjDHJJcPrAIwxxpgUJVGWjc8i9gHLVbVfRN4CPAGsmbAjkfuB+wGqq6upr6+f41Cn19/f78lxZ6v+XD0A2wq3UdZbNqNtN5dspuFcA1/89hdZXLAYgIqCirkOMapU+5xTLV5jTHSW9BljjDGz0wQsjXi/BLgYWUBVeyNe7xSRfxGRClVtH1duB7ADYMuWLbp9+/Z5C3oy9fX1eHHc2Xr8G4+Tm5FLybISOn2dM9p28fLFcABeuvISb1j8BgDeccs75iPMCVLtc061eI0x0VnzTmOMMWZ29gJrRGSFiGQB9wJPRhYQkUUiIu7rrTj33Y6ER5qGdh3bxfry9fh9/hlvu6hoEQXZBTS2Nc5DZMYYk3ws6TPGGGNmQVUDwIPAU8BR4DuqekREHhCRB9xi7wAOi8gB4AvAvWodyeJ2ofMCJ9tOsrFy46y2FxFWV63mZNvJOY7MGGOSkzXvNMYYY2ZJVXcCO8ctezji9ZeALyU6rnT37LFnAdhYMbukD2B11Wr2X9hP90A3JXklcxWaMcYkJavpM8YYY0xK2XVsF+UF5SwrWjbrfaypcsbTsdo+Y8xCEFfSJyJ3ishxEWkUkY9EWS8i8gV3/UERuSFi3aMi0iYih8dtUyYiT4vISfff0nhiNMYYY0z6UFV2Hd/Frb9xKz6Z/deYpWVLyfRncubymTmMzhhjktOs/1qKiB/4MnAXsB64T0TWjyt2F87Q1GtwhqL+SsS6fwfujLLrjwDPqOoadkBiPwAAIABJREFU4Bn3vTHGGGMMpy6f4kLnBW5bd1tc+/H7/CwrW8aZDkv6jDHpL56avq1Ao6qeVtUR4HHgnnFl7gG+ro49QImI1ACo6m4g2hjL9wD/4b7+D+C34ojRGGOMMWlk17FdANy+7va497WyciXnO84TCAbi3pcxxiSzeAZyqQUuRLxvArbFUKYWaJliv9Wq2gKgqi0iUhWtULwT2Xo92ejAwMCEZbM5h/H7SbUJVL3+f5gL6XAOxhiTKnYd20VtSS1rqtdwkINx7WtFxQqeDj1NU1fTHEVnjDHJKZ6kT6IsGz8MdSxlZiXeiWy9nmy0oaFhwrLNmzfPaB/19fUUFhbGtQ+vef3/MBfS4RyMMSYVqCq7ju3izg134k5/GJcVFSsAON1+Ou59GWNMMouneWcTsDTi/RLg4izKjHcp3ATU/bctjhiNMcYYkyYONx/mct/luPvzhZXmlVKSW8KZduvXZ4xJb/EkfXuBNSKyQkSygHuBJ8eVeRJ4jzuK52uBnnDTzSk8CbzXff1e4AdxxGiMMcaYNBHuz3frulvnZH8iwvLy5ZzvOD8n+zPGmGQ16+adqhoQkQeBpwA/8KiqHhGRB9z1D+NMWPsWoBEYAN4f3l5EvgVsBypEpAn4mKp+Dfg08B0R+QPgPPA7s43RGGOMMaltx+4dY693HdvFqspVLC9fPmf7X1a+jINNB+kf6qcgp2DO9muMMckknj59qOpOnMQuctnDEa8V+NAk2943yfIOIP4huYwxxhiTNoKhIPUn6nnnlnfO6X6XlS1DUQ40HeD1q18/p/s2xphkEdfk7MYYY4wxiXCh8wK9g71z1p8vbFnZMgD2nds3p/s1xphkYkmfMSlMRO4UkeMi0igiH4myXkTkC+76gyJyg7s8R0ReFJEDInJERD6R+OiNMSZ2x1qPAXPXny+sOLeYopwi9p23pM8Yk74s6TMmRYmIH/gycBewHrhPRNaPK3YXsMb9uR/4irt8GLhNVa8DrgfudAdbMsaYpHS89TgbazdSXVQ9p/sVEZaVLbOkzxiT1uLq02eM8dRWoFFVTwOIyOPAPcArEWXuAb7u9q/dIyIlIlLjjqLb75bJdH/mZA5NY4yZa8FQkMa2Rm5afdNVA7uUUTYn+19atpSfvfIzhkaHyMnMmZN9GmNMMrGaPmNSVy1wIeJ9k7sspjIi4heR/ThzYT6tqi/MY6zGGDNrLT0tjARHWFW5al72X1taSzAU5Hjr8XnZvzHGeM1q+oxJXRJl2fjauknLqGoQuF5ESoD/FpGNqnp4wkFE7sdpGkp1dTX19fVTBtXf3z9tmURKpniSKRaY33gGdTDq8g7p8CQek9rOtp8FmNOpGiLVljjPy77wzBfYtnIbAPffcv+8HMsYY7xgSZ8xqasJWBrxfglwcaZlVLVbROqBO4EJSZ+q7gB2AGzZskW3b98+ZVD19fVMVyaRkimeZIoF5jeeQ8OHoi7flL3Jk3hMajvbcZa8rDyqCqvmZf9VhVX4fX4udo//E2qMMenBmnemsMHBQV5++WWee+45uru7vQ7HJN5eYI2IrBCRLOBe4MlxZZ4E3uOO4vlaoEdVW0Sk0q3hQ0RygTcBxxIZvDHGxOps+1mWly9HJFrjhfhl+DOoLqqmubt5XvZvjDFes6QvRakqx48fp7W1lf7+fg4fPowzVodZKFQ1ADwIPAUcBb6jqkdE5AERecAtthM4DTQCXwU+6C6vAZ4VkYM4yePTqvqjhJ6AMcbEYCQwQnN3M3XldfN6nNqSWqvpM8akLWvemaJOnDhBT08PGzduxO/3c+DAAZqbm9myZYvXoZkEUtWdOIld5LKHI14r8KEo2x0EXjPvAZq0MFlTTZi6uaYxsxU5QueFrguENERdRd28HnNxyWL2nt1rI3gaY9KS1fSlqN27d5Obm8uyZctYsmQJBQUFnD9/3uuwjDFmQRGRO0XkuIg0ishHpih3o4gEReQdiYwvHbR0twCvDrYyX8L7t9o+Y0w6sqQvBXV1dXHx4kVqamrw+XyICFVVVXR3dzM6Oup1eMYYsyCIiB/4MnAXsB64T0TWT1LuH3CaYpsZau1tJcOXQXl++bweZ3HJYsCSPmNMerKkzyPd3d0cP358VknakSNHAKisrBxbVlFRQSgUsto+Y4xJnK1Ao6qeVtUR4HHgnijl/gj4L5w5Mc0Mtfa0Ul1Ujc83v19ZygvKyc7ItsFcjDFpyfr0eeD555/nueeeA+DixYts27aNvLy8mLc/cuQItbW15OS82uegrKwMEeH06dOsWjU/k9caY8xkRnWUTMn0OoxEqwUuRLxvArZFFhCRWuB/ALcBN062o5nOhzkfkmmexLL+srHXl7svs6J4BWW9ZRPK+YP+qMtna2nBUi63X6ast2zePotk+pxjkWrxGmOis6QvwXp6enjmmWeoqqpi+fLl7Nu3j2PHjnHDDTfEtH13dzetra3ccccdjIyMjC3PyMigtLSUM2fOzFfoxhhzlZCGeHHwRfYP72dQB1masZSbc2+mMqNy+o3TQ7T5A8YPo/x54C9VNTjVdAMznQ9zPiTTPInhgVxGg6NcGrjE5pWb6SzqnFCurLcs6vLZqi6v5mDzQTqLOnnHLfPT/TKZPudYpFq8xpjorHlngoWflm3cuJHq6mrq6uq4ePEi/f39MW1/8uRJANauXTthXXl5Oa2trdavzxgz70Ia4sDwAZ4fep5FGYvYkrOF9mA7T/Q/wZXQFa/DS5QmYGnE+yXA+A5hW4DHReQs8A7gX0TktxITXuq73HcZVWVR8aKEHG9xyWL6hvroHexNyPGMMSZRLOlLoL6+Pg4cOMCWLVvGmnOuXLkSn8/HqVOnYtpHY2MjJSUllJdP7NBeWFiIqtLe3j6ncRtjTCRV5ZWRV+gKdXF73u28veDtvD739fx24W8zoiP89MpPF8q8oXuBNSKyQkSygHuBJyMLqOoKVa1T1Trge8AHVfWJxIeamlp6nJE7FxUlJumrLa296rjGGJMu4kr6phuqWhxfcNcfFJEbpttWRK4XkT0isl9EXhKRrfHEmEwOHTqEql41l152dja1tbW0tLQQCASm3D4QCHDmzBlWr15NtGZCRUVFALS12VgBxpj50xJsoS3YxsrMlWzM3ji2vNxfzhty30BToInuULeHESaGqgaAB3FG5TwKfEdVj4jIAyLygLfRpYfWnlYAqouqE3K88AieNpiLMSbdzDrpi3Go6ruANe7P/cBXYtj2M8AnVPV64G/c92nh4MGD1NbWUlFRcdXympoaAoHAtLV9P//5zxkdHUVEaGhoYGBg4Kr1eXl5+P1+Ll26NOexG2MMwHBomMaRRkp8JSzPWD5h/YbsDeRKLhcCF6JsnX5UdaeqrlXVVar6SXfZw6r6cJSy71PV7yU+ytTV3t9OcW4x2ZnZCTleUU4R+dn5NHdZ0meMSS/x1PTFMlT1PcDX1bEHKBGRmmm2VaDIfV3MxP4RKenSpUtcunSJa6+9dsK6iooKMjMzx6ZimExbWxs+n29C0hjm8/morKy0mj5jzLw5M3qGIEHWZa2L2uIgQzLYlL2J9mA7g6FBDyI06aTzSidl+XM3Oud0RITFxYuteacxJu3Ek/RFG6q6NsYyU237p8BnReQC8Dngr+KIMWkcPXoUEWHDhg0T1vl8PhYtWjTtvH2XL1+mvLwcv98/aZmqqipL+owx82IwNEhLsIXajFryfJNPM7Mh2/k7dzl4OVGhmTSV6KQPnCaeLT0tC6VfqjFmgYhnyoZYhqqerMxU234A+DNV/S8R+V3ga8CbJhw8zjmNEj3vzEsvvURhYSF79+4FmNA0Mzc3l5GREX7wgx9ErckbGhqiv7+f0tLSseabgUBgQlPO/v5++vr6ePrpp8nMTP45s9Jh/p90OAdjYnF29CyCsDxzYrPOSEW+IgqkgMvByyzLXJag6Ey6CWmIziudXLf0uoQet6a4hoGRAVp6Wsb6+BljTKqLJ+mLZajqycpkTbHte4E/cV9/F3gk2sHjndMokfPOdHd384tf/II77riDm266CYCGhoarylRWVnLhwgVEJGpc4WRx1apVFBQUAE6T0erqqzu3r127ltOnT7N27VqWL5/6i1kySIf5f9LhHIyZzqiOcil4iZqMGrJl+v5VFf4KzgbOLtRJ280c6B/qJxAKUJ4/cbTq+RRO9I5cPGJJnzEmbcTTvHPaoard9+9xR/F8LdCjqi3TbHsReKP7+jbgZBwxJoXjx48DsG7duknL+Hw+rrnmmkmbeJ44cYK8vDzy8/OnPFZ4KofOzrmbrNYYY1oCLYQIUZsxvhV/dBV+p8VCR7BjPsMyaazzinMfS3TzzpriGsBJ+owxJl3MOumLcajqncBpoBH4KvDBqbZ1t/l/gH8UkQPA3+M24UxlJ06coKKigrKyqW9cGzZsYHR0lBMnTly1/MqVK5w6dYqampqoAydEKi4uxufzWdJnjJkzqkpzoJliXzEFvoKYtin0FZJJJp1B+1tkZqfjivPAINFJX1FuEQXZBbxy8ZWEHtcYY+ZTPM07UdWdOIld5LKHI14r8KFYt3WXPwdsjieuZDIyMsK5c+e48cYbpy1bV1dHcXEx+/btu2rAlyNHjqCq1NZO/4T95ZdfJicnh9OnT1NSUgLA5s1p83EaYzzQG+plUAepy6yLeRsRodhfTE+oZ/4CM2nNq5o+cJp4Wk2fMSadxJX0memdPXuWYDDImjVrpi3r8/m44YYbePbZZ+ns7ByrGTx48CDV1dVjk69PJz8/nytXrsQVtzHGhLUF2xCESn/lhHWHhg9Nul2xr5j2YDsjOkKWZM1niCYNdV7pJDsjm7ysyUeKnS81xTXsv7AfVZ22hY0xxqSCePr0mRg0NjaSmZnJsmWxjWD3mte8BhHh17/+NeD0B2xubuY1r3lNzMfMy8vjypUrNty0MSZuqkpbsI1yfzkZMrPnhMW+YgB6g73zEZpJc+HpGrxIumpKaugZ7OFid1pMFWyMMVbTN98aGxtZsWIFGRmxfdSFhYXceOONvPjii5SVlbF3714qKyvZsmUL+/fvj2kf+fn5BAIBRkdHycqyp+vGmNhEq7XrDnYzrMOs8q+a8f4KfYUIQk+ohwomTkVjzFQ6r3QmfOTOsMXFr47gWVsa2+BFxhiTzKymbx40NDTQ0NDAL37xC7q6umaceN1xxx3U1tby9NNP09fXx1vf+tYpJ2QfLzzCpzXxNMbEqyPYgSBjo3HOhF/8FPoKrV+fmZXugW5K8ko8OXZ4qoZXWmwwF2NMerCavnl0+fJlwJmDbyYyMjJ497vfPTYPX25u7oy2z8tz+j8MDAxQWlo6o22NMSZSR7CDYl/xjJt2hhX5imgJtFjfKDMjoVCIvuE+inOLPTl+YU4hFQUVNpiLMSZtWE3fPGprayM/P3/aufWiyc3Npa6ubsYJH7ya9FlNnzEmHsOhYfq1n3L/7JvYFfgKCBJkUAfnMDKT7vqG+lBVz5I+gA2LN1jSZ4xJG5b0zZNgMEhHR8eMa/nmgt/vJzs7m8FB+5JljJm9jpAzT1pcSZ848/r1h/rnJCazMPQMOk2Ci3JjG7V6PoSTPhsUzRiTDizpmyednZ2EQiFPkj5wavsGBgY8ObYxJj10BjvJlmzyZeatFcLyfc62lvSZmegZcpI+r2v6egd7ae5q9iwGY4yZK5b0zZOOjg5EhPJyb0Yey83NtZo+Y8ysqSpdwS5KfaVx9cXzi588yaNfLekzsesZSI6aPsCaeBpj0oIN5DJP2tvbKSkpGZuqoaGhIaHHz83NpaWlxZqlGGNmZUAHGGWUEn/8oycW+AroDdlcfSZ2vUPO74uXNX3rF68HnBE837zxzZ7FYYwxc8Fq+ubB6OgoPT09ntXygdO8U1UZGhryLAZjTOrqCnYBUOqLfwTgAl8BQzrEqI7GvS+zMPQM9pCXlUemP9OzGCoLK6ksrLSaPmNMWrCkbx50dnaiqlRUeDcZcXjUT2viaYyZja5QF9mSTY7kxL2vAp8zmMtAyPoZm9j0DvZ6WssXZiN4GmPShTXvnAcdHR34fL6458iLp0moJX3GmNlSVbqD3ZT7y+dkbr3wQDBXNP2mkRGRO4GHAD/wiKp+etz6e4C/BUJAAPhTVX0u4YGmmJ7BHk/784VtWLyBb+z5hs0zaYxJeVbTNw/a29spLS3F7/d7FoMlfcaY2RrSIUYZpdg3NzUtOZKDD1/a1fSJiB/4MnAXsB64T0TWjyv2DHCdql4P/E/gkcRGmZp6BnsozvG+pm99zXp6B3tp6mryOhRjjImLJX1zbHBwkN7eXk/78wFkZGSQlZVl0zYYY2asJ+SOnOifm5oWESFXcrkSSruavq1Ao6qeVtUR4HHgnsgCqtqvr46olQ/Y6FrTUFUn6cvzPukLj+D5ysVXPI7EGGPiY80750BkM8yWlhYAT/vzhdm0DcaY2egN9eLDF9f8fOPl+/LpC/XN2f6SRC1wIeJ9E7BtfCER+R/Ap4Aq4K2JCS119Q31MRocpSgnOZp3gjNtg43gaYxJZZb0zbGOjg78fj8lJfEPcx6v3Nxc+vttbqx0FkN/InHXvwUYAN6nqvtEZCnwdWARTl+jHar6UEKDN0mrN9RLka8In8xdY5A8yaNN2whogAxJm1tPtE5eE2ryVPW/gf8WkVtw+ve9acKORO4H7georq6mvr5+biONQX9/vyfHHe9Cr5NHL2YxZb1lU5b1B/3Tlpmt8GdRllPGUy89xQ1ZN8zJfpPlc45VqsVrjIkube68yaKjo4PS0lJ8Pu9bzubl5dHW1mYd0NNURH+iO3BqGPaKyJOqGtkO6S5gjfuzDfiK+28A+LCbABYCDSLy9LhtzQIU0hB9oT6WZiyd0/3m+5xaw+5QNxV+71tCzJEmIPKDWgJcnKywqu4WkVUiUqGq7ePW7QB2AGzZskW3b98+D+FOrb6+Hi+OO95zJ5+DH4Kv1EdnUeeUZct6y6YtM1vvuOUdAGw7sI2mrqY5+2yS5XOOVarFa4yJLq7MRETuFJHjItIoIh+Jsl5E5Avu+oMickMs24rIH7nrjojIZ+KJMZECgQB9fX1xj9o5V3JzcwmFQtavL31N25/Iff91dewBSkSkRlVbVHUfgKr2AUdxmqqZBa4v1IeiFPnmtmldni8PgM7g/HxB98heYI2IrBCRLOBe4MnIAiKy2q1xx70HZgEdCY80hbT1tQFQmFPoaRw7du9gx+4dgDNB+9CozXtrjElds076Yhy1LLKW4X6cWoYptxWRW3G+qF6rqhuAz802xkTr7u4GSKqkD16Ny6SdaP2Jxidu05YRkTrgNcALcx6hSTm9oV6AORu5MyxPnKQvPOl7OlDVAPAg8BTOg5PvqOoREXlARB5wi/02cFhE9uPc994ZMbCLiaKt10n6kqFPH8DSsqUEQ0EONx/2OhRjjJm1eJp3jtUyAIhIuJYhsnnYWC0DsEdESkSkBqibYtsPAJ9W1WEAVW2LI8aECidXydCfD5zmnQA9PT3U1lolThqKpT/RlGVEpAD4L5y5w3qjHmSGfY2Srf9HMsWTTLHAxHgGdZDW5a1k5Gdwef/lOT9exvoMjnceZ/BC9AGmku3ziYWq7gR2jlv2cMTrfwD+IdFxpbJwTV9BToHHkTiWlS0D4OXzL7OlbovH0RhjzOzEk/TFMmrZZLUMU227FrhZRD4JDAF/rqp744gzYbq6usjPzycrK8vrUACr6VsAYulPNGkZEcnESfi+qarfn+wgM+1rlGz9P5IpnmSKBSbGc2j4EOcHz1PqK2XJTUvm/HhtQ23k5OawfdX2qOuT7fMx3mjrayM/Kx+/z7u5biOVF5RTlFvEyxde9joUY4yZtXiSvnhqGabaNgMoBV4L3Ah8R0RWjm8OE+9IZ3P5RHlgYABVpaOjg+LiYi5dujSr/YyPZ7q+eIFAYNpj+f1+jhw5wsjIyKximm+p+GR/PA/PYaw/EdCM05/oXePKPAk86NambwN6VLXF7WP0NeCoqv5TIoM2yWtERxjSIZb45j7hA8iV3LE5AI2ZTFtvm+f9+SL5xMf1S69n37l9XodijDGzFk/SF08tQ9YU2zYB33eTvBdFJARUAFe1NYp3pLO5fKLc0NDAwMAAgUCAmpoaqqurZ7WfzZs3T9jvVC5dujTtsfLy8igoKEjap+fp8GTfq3NQ1YCIhPsT+YFHw/2J3PUP4zQ7ewvQiDNlw/vdzV8P/D5wyO1rBPC/3aZqZoHqDTotfOd6EJewPF8eLcEWRnSELEmOFhEm+bT1JVfSB3Bj3Y18adeXGA2MkpmR6XU4xhgzY/EkffHUMlyeYtsngNuAehFZi5MgtpPkkq0/X1heXh49PfZkPV3F0J9IgQ9F2e45ote4mwWsJ9SDIBT65ucLd644Tc57gj1UZlTOyzFM6kvGpG9r3VaGA8McbD7I5uWbp9/AGGOSzKxH74xx1LKdwGmcWoavAh+calt3m0eBlSJyGGcI+vemwkhnXV1d+Hw+ioqSY7SxsNzcXOvTZ4yJSW+olwJfAX6Zn75UuT63n3HI/iaZySVl0rdiKwAvnnnR40iMMWZ24pqcfba1DJNt6y4fAX4vnri80N3dTXFxcVyTsk/XnHM2cnNzGR4eZmhoiJycnDnfvzEmPYQ0RG+ol0UZi+btGOGaPkv6zGQCwQAd/R1Jl/Q9deQpCrML+caeb/CB7R/wOhxjjJmxuCZnN45QKERPT0/SzM8XyUbwNMbEoivURZDgvPXnA8iQDPIkj56gNTk30bX3O705kmWOvjARoa6ijrPtZ70OxRhjZsWSvjnQ29tLKBRKuv58cPVcfcYYM5mWQAsw95Oyj1fiL7GaPjOp8Bx9yVbTB7CiYgWtPa30DNj91BiTeizpmwNdXV0AVtNnjElZrYFWMsgYa4I5X4p9xVbTZybV1pu8Sd/KypUoyvOnn/c6FGOMmTFL+uZAd3c32dnZSdlnLisri4yMDEv6jDFTag20UuQrwpnCcf6U+Ero135GdXRej2NSU7LX9PnExy9P/tLrUIwxZsYs6ZsDXV1dlJaWzvuXpdkQEUpKSqx5pzFmUsOhYTpCHRT757dpJzjNOwGbpN1ElcxJX05mDsvKllnSZ4xJSZb0xWlgYICBgYGk7M8XVlxcbDV9xphJtQZbgfnvzxd5DGviaaK53HeZDH8GuVnz28x4ttZUr+GFMy8wNDrkdSjGGDMjlvTFqampCUjO/nxhxcXFVtNnjJnUxcBFBJnXkTvDSnzOAzIbzMVE09bXRmVBJT5Jzq8nq6tWMxIYYe+ZvV6HYowxMxLXPH0GmpubASexSlYlJSUMDAwwMjJCVlaW1+EYY2bo0PChqMs3ZW+ak/23BFqo8FeQIfN/S8j2ZZMruXQHLekzE7X1tlFVWOV1GJNaXbUagF+e/CU3r73Z42iMMSZ2yfkoLYU0NTVRVFRERkby5s/hpqdW22eMGS+kIVoDrdRk1CTsmMW+YuvTZ6Jq62ujqih5k76C7AI2LN7A7pO7vQ7FGGNmJHkzlSTR0NAwYdnmzZsBUFWam5uprq5OdFgzEq6F7O7uprKy0uNojDHJpCPYwSij1GTUJGxEzRJ/Cc2B5oQcy6SWtr42VlWu8jqMKd2y9hYe2/MYwVAQv8/vdTjGGBMTq+mLQ3t7O8PDw0ndnw+sps8YM7mLgYsALPYvTtgxi33F9IX6CGggYcc0qSHZm3cC3Lz6ZvqG+jhw4YDXoRhjTMws6YtDuD9fMo/cCVBQUIDf7x+bRN4YY8Jagi3kSz6FvsQNkR8ezKU31JuwY5rkNzA8QP9wf1I37wQ433kegE//9NMeR2KMMbGzpC8OTU1NZGdnU1BQ4HUoU/L5fJSWllrSZ4yZoCXQQk1GTULnGQ3PB2iDuZhIl/svAyR9TV9pfikVBRWcvHTS61CMMSZmlvTFobm5mdra2qSclH28srIyOjs7vQ7DGJNERjNG6Q31JnQQF3i1ps8GczGR2nqdidmTPekDWFu9lhOXThAMBb0OxRhjYmJJ3yyNjIxw6dIlamtrvQ4lJqWlpXR2dqKqXodijEkSV/KvALA4I3H9+QByJIcsyUqLufpE5E4ROS4ijSLykSjr3y0iB92fX4vIdV7EmQra+tykL8mbdwKsW7SOgZEB9l/Y73UoxhgTE0v6ZqmlpQVVZcmSJV6HEpOysjJGR0fp7+/3OhRjTJLoL+gnk0wq/Ykd1VdEKPGV0BNM7Zo+EfEDXwbuAtYD94nI+nHFzgBvVNVrgb8FdiQ2ytQxlvSlQE3fukXrAHjm6DMeR2KMMbGxpG+WmpqaAFKmpq+srAzAmngaY8b0FfaxJHMJfkn8sPPFvuJ0qOnbCjSq6mlVHQEeB+6JLKCqv1bVcIfqPUBqPCn0QLh5Z2Vh8k8tVJxXTE1xjSV9xpiUYUnfLDU3N1NaWkp+fr7XocTEkj5jTKSeYA8j2SMsy1jmyfFL/CX0hfoIaciT48+RWuBCxPsmd9lk/gD4ybxGlMLa+trIz84nPzs17qvrFq3jl42/ZHh02OtQjDFmWnFNzi4idwIPAX7gEVX99Lj14q5/CzAAvE9V98W47Z8DnwUqVbU9njjnQ1NTE3V1dV6HEbOSkhJ8Pp8lfcYYAM4HnGHnl2cu9+T4xb5iQoToC/WNjeaZgqKN4hW147SI3IqT9L1hkvX3A/cDVFdXU19fP0chxq6/v9+T44YdOnmIwoxC6uvrKesvi2kbf9BPWW9sZefajcU38uzIs3zl+1/h+urrY97O6895plItXmNMdLNO+iL6MtyB83Rzr4g8qaqvRBS7C1jj/mwDvgJsm25bEVnqrjs/2/jmU29vL319fSnTtBOcaRtKSkps2gZj0sih4UOTrtuUvWnKbc+NniNzJHNsJM1ECx+3O9SdyklfE7A04v0S4OL4QiJyLfAIcJeqdkTbkaruwO2hgNb1AAAgAElEQVTvt2XLFt2+ffucBzud+vp6vDhu2Kde/hTLqpaxfft2duyOretjWW8ZnUXePMxcnLMY314fnTmdM/rcvP6cZyrV4jXGRBdP885p+zK477+ujj1AiYjUxLDtPwN/wSRPTL124YLTmieVkj5wmni2tyddpakxJsFGdISzo2cp7in2bMqZNJmrby+wRkRWiEgWcC/wZGQBEVkGfB/4fVU94UGMKeNS7yWqi6q9DiNmuVm53Fh3o/XrM8akhHiad0bry7AthjK1U20rIm8HmlX1wFRfRuJtChNrc4WBgYEJyy5evIjP5+PEiRM0NjZGLZMIgUCAS5cuTVsufJ5DQ0O0tbXx7LPPJs3cgunQbCQdzsEsLGdGzxAkSHGPdzVs+ZJPBhkpPVefqgZE5EHgKZyuCo+q6hERecBd/zDwN0A58C/u392Aqm7xKuZk1tLTwraV479GJLfbr7mdzzz1GfqG+ijMKfQ6HGOMmVQ8SV8sfRkmKxN1uYjkAR8FfnO6g8fbFCbW5goNDQ0TljU3N7N06VJuu+22ScskwqVLl6iunv6p6ObNmwGnX19TUxObNm2ioqJivsOLSTo0G0mHczCpoyvYxcXARfpCfQhCqb+UJRlLyPPlxbyPkyMnyZd88q94N2CGiFDsK07ppA9AVXcCO8ctezji9R8Cf5jouFJNIBjgcv9lFhUt8jqUGbl93e38/c6/Z/eJ3bz12rd6HY4xxkwqnuadsfRlmKzMZMtXASuAAyJy1l2+T0SS5i4QCARoaWlh2TJvRryLR1WVM/fR5cuXPY7EGDNTQQ1ydPgoLw+/TEewg3xfPtmSzcXARV4cepFzo+dQnb5F/GBokLOjZ1mTtQaJ+vwtcUr8JanevNPMkct9l1FVaoprvA5lRm5afRM5mTnWxNMYk/Tiqekb68sANOP0ZXjXuDJPAg+KyOM4zTd7VLVFRC5H21ZVjwBjs7K6id+WZBq9s7u7G1Vl6dKl0xdOMpWVztxHbW1tXHPNNR5HY4yJ1aiOcnD4ID2hHpZnLKcus25sbr1hHebEyAlOjZ7iSugK67LW4ZPJn+cdHD5IkCCbsjdxkIOJOoWoin3FnB09i6omTZNz442WnhYAFhUnzTPemORk5vD61a/nmWOW9Bljktusa/pUNQCE+zIcBb4T7ssQ7s+A0+TlNNAIfBX44FTbzvosEig85UEqJn2ZmZmUlpbS1tbmdSjGmBgFNMCB4QP0hnrZmLWRVVmrrppMPVuy2Zi1kRWZK2gNtnJ45PCkc9+F91WXWUeZ35th7iMV+4sJEqRf+70OxXistbcVIOVq+sBp4nmw6eDY5PLGGJOM4pqnL4a+DAp8KNZto5Spiye++dDV1UVVVRU5OTlehzIrVVVVlvQZkyJCGuKnV346lvBVZVRFLScirMhcQSaZnBg9wcHhg2zI3kCGXP0n/qWhlxjUQTZnb05E+NMKT9vQE+yh0GeDYCxkLd2pWdO3Y/cOeod6AfjoEx/lq+/5qscRGWNMdPH06VtwVJWurq6U7M8XVlVVRUdHB4FAwOtQjDHTeG7wOU6NnmJ15upJE75ISzKXsC5rHZ2hTr7f9336Q6/WoIX7/q3LWseSzCXzGXbMIufqMwtbuKYv1QZyAVhetpzczFyOthz1OhRjjJlUXDV9C01vby+BQCDlmnZGji565coVVJVLly6l3DyDxiwkB4cP8vLwy1yXfR1lvtibYi7OWEwGGRwfOc5jvY+xIWsDAAeGD1DgK2B77vZ5injmCnwF+PDZYC6G1p5WSvNKyc7M9jqUGfP5fKxfvJ5DzYcIhUL4fPY83RiTfCzpm4Fwf75UrukrLS0FnAnmvUj6vJrewphUcm70HPUD9dRl1nFL7i0cGZlZl+eqjCquz7me5wefZ9/wPgBWZa7itrzbyPYlz5dqn/go8hWl/LQNJn4tPS0p17Qz0rVLrqXhXAMN5xq4ccWNXodjjDETWNI3A11dXeTk5FBc7N2ExvHKzc2lqKiIpqYmr0Mxc0BE7gQewpkY+hFV/fS49eKufwswALxPVfe56x4F3ga0qerGhAZuJtUZ7GTnlZ2U+cu4K/+uKUfinEq5v5y3FbyNgAbw4Zv1fuZbqb+UrmCX12EYj7X2tqbkIC5hG2s3IiL88OAPLekzxiSl5PwWkKQ6OzspKytL+aHFly5daklfGhARP/Bl4C5gPXCfiKwfV+wuYI37cz/wlYh1/w7cOf+RmlgNhgZ5sv9J/Ph5e/7byZKsuPeZIRlJm/ABlPpK6Q51TzriqFkYWnpaUrI/X1hBdgGrK1fz5P4nvQ7FGGOiSt5vAklmcHCQoaGhseaRqWzJkiX09PTQ19fndSgmPluBRlU9raojwOPAPePK3AN8XR17gBIRqQFQ1d1AZ0IjNpMKaIAfX/kx/aF+7i64myJ/kdchJUSpv5QgQfpC9vdooVJVWntaqSlJ3Zo+cJp4Hmg6wPmO816HYowxE1jSF6Nwf76yMu/ntorXkiXOyH3nz9uNKcXVAhci3je5y2ZaxiTQoeFDE34ODB3gJ1d+QnOgmTflv4majNT+8jsTpT7nQVpXyJp4LlR9Q30MjAykdE0fOEkfwA8P/tDjSIwxZiLr0xejzs5O/H4/hYWpP5dUTU0NOTk5nDx5kg0bNngdjpm9aO2MdRZlpj6IyP04TUOprq6mvr5+yvL9/f3TlkmkZIqnv7+fwV8PXrVMUVqXtdJX1kdtUy2tHa200npVmUG9eptYdEhHTPFEfjazOc5MjY8r4A/ARth7fC+5/blJ839lEud8p/MAcllZ6g6SBs4cg2ur1/LDAz/kQ7dGnaLYGGM8Y0lfjLq6uigtLU2LoZj9fj9r167lxIkTNrx0amsCIucPWQJcnEWZKanqDmAHwJYtW3T79u1Tlq+vr2e6MomUTPHU19dT/rrysfeqyonRE/QF+nhdzuvYumlr1O0ODR+a8bE2ZW+KKZ7Iz2Y2x5mp8XGpKo09jZSvKMfX7kua/yuTOOc6zgGpn/QB3H3d3Xxx1xfpG+qjMCf1HxIbY9KHfduPwejoKL29vWnRtDNs3bp1DA4Ocu7cOa9DMbO3F1gjIitEJAu4Fxg/isCTwHvE8VqgR1VbEh1oPKI1hwz/pDJV5dToKZoDzSzLWMaNOQtzxD8RodRXas07F7Bw0re8fLnHkcTv7de9nZHACDsP7fQ6FGOMuYrV9MWgq8v5MpJOSd+qVavIyMjglVdeYcWKFXO+/+HhYY4ePcr58+fx+XwsW7aMjRttVoC5pKoBEXkQeApnyoZHVfWIiDzgrn8Y2IkzXUMjzpQN7w9vLyLfArYDFSLSBHxMVb+W2LNYuM4GznI+cJ7ajFpWZa5K+VGB41HqL+X86HnKKZ++sEk75zvPk5WRxQ8O/CCpR5qNxSsXX6Eot4jP/PQzvPPGd3odjjHGjLGkLwZdXV2ICCUlJV6HMmeysrJYv349Bw4c4NZbbyUvL29O9hsMBnnhhRd47rnnGBwcJDMzE3AmZf/5z3/O1q1byc3NnZNjGVDVnTiJXeSyhyNeKxC1c4mq3je/0ZnJnB89z5nRMyzyL2Jt5toFnfABlPnLODpylKAv6HUoxgPnOs6xtHRpyid8AD6fjxuW3cCvGn9F/1A/BTkFXodkjDGAJX0x6ezspKioiIwM5+NqaGjwOKK58YY3vIGDBw/y/PPPc/vtt8e9vwsXLvCjH/2ItrY2Vq9eTXV19Vii3NLSwoEDB9izZw833XQT2dnZcR/PmFR0MXCRxtFGKv2VrMtat+ATPnAmkgcYyhnyOBLjhXOd59KiaWfYluVbqD9ez599+8/GJmq//5b7PY7KGLPQpf5jtXkWCoXGBnFJN5WVlaxfv54XXniBtra2We+nt7eXJ554gkcffZTe3l62bNnCunXrKC0tRUQQERYvXsy2bdsYHBzk0KFDOBVQxiwsV/KucHzkOKW+UjZkbUiLmo25UO6zpG8hO99xPi0GcQlbVbWK4txiXjr3ktehGGPMGKvpm0Zvby+hUCit+vNFevOb38z58+f5z//8T971rndRVVUFTKzN3Lx584Rtu7u72bt3Ly+++CKqyqpVq1izZs1Yjeh4ZWVlrF27lmPHjtHS0sLixYvn/oSMSVJ9oT7O1p0lR3LYmL3Rs4QvcgCcQR1MigFxCn2FZJLJYO78TxlhkstIYISLPRfTqqbPJz42L9/M7hO7GRodIiczx+uQjDHGkr7ppNOk7NEUFRVx33338dhjj/Hwww+zatUqqquraW9vx+/3j9XUhUIhwBmgpauri+bmZi5duoSIsGHDBm6//XZOnTo17fFWrVpFS0sLr7zyCtXV1fN9eibNTZawxDJdQSIFNMCP+n9EyBfi2uxryZRMr0NKKiJCub+c/ux+r0OZMRG5E3gIZzClR1T10+PWrwP+DbgB+Kiqfi7xUSavpq4mVJXl5csZDY56Hc6c2bx8M7uO7eJg00G2rog+FYsxxiSSJX3T6OzsJC8vj5yc9H1St3jxYh588EGee+45Tp48yalTpyY0vzx8+PDY69zcXBYtWsTtt9/Opk2bKC4ujvlYIsI111zDnj17OHfuHJWVlXN2HsYkk8iEtHGkkbZgG4vPLSZ/Y/605ef6+Kmg3F9Oe06712HMiIj4gS8Dd+DMiblXRJ5U1VciinUCfwz8lgchJr3IidlPXZ7+wWGqWFm5kpLcEhrONVjSZ4xJCnElfTE84RR3/Vtwhot/n6rum2pbEfkscDcwApwC3q+q3fHEOVuqSldXFxUVFV4cPmHCTTnLy8spL3f61oRCIQKBAOB8Dtdeey0A2dnZZGZmjm3T2Ng44+NVVFRQXl5OY2Nj2tagGhPWFewam5ohvzd6wmecpC+QGWAgNECeb25GE06ArUCjqp4GEJHHgXuAsaRPVduANhF5qzchJrcz7WcAqCuvS6ukzyc+blh+A7tP7GZwxJotG2O8N+tOJRFPOO8C1gP3icj6ccXuAta4P/cDX4lh26eBjap6LXAC+KvZxhivrq4uhoeHF2Ri4vP5yMrKIisri+zsbI4fP/7/t3fv8VHVd8LHP9/MTO6BEBJCEgIBQuRmBYEgYlmeoqjYeukq9Vat2nW7ts+21bbbdp/enmdb3Yu729e21ap11bXiBdFSXigqAoIVuQuEa4AAIUDCndwzme/zx5zggElIMsmcSeb75jWvzJw5v3O+5zC/mfP7nd+FnTt3snnz5m4ZvbSoqIjGxkaOHj3aDdEaE5386md743aSJIlCX6Hb4US1lhE8jzX3qrt9ecDBkNflzjLTQSUVJST6EinILHA7lG43edhk/AE/m8s3ux2KMcaEdafvojWczusXnLnCVotIuojkAAVtpVXVd0LSrwZuDSPGsBw4EGx20hdH7nRbRkYG6enpHDx4kEAgQFycjWJo+p7SxlLqtZ5JCZPwiMftcKJalifY1LuquYqhvl4zkmNr8210aWhiEXmQYOUo2dnZLF++PIywuqa6ujri+125ZSX5qfms/GAlGdWdr2D1NHvIOBOdFbPpCekMTBzI5j2bzzuvbpzncPS2eI0xrQun0NdaDefUDqyT18G0APcDr4QRY1gOHjyIz+cjLS3NrRD6LBFh5MiRrF+/nu3btzNu3Di3QzKmW51qPkVFcwVDvUPp7+l4v9dYlRSXhK/RR6Wv69PHuKAcyA95PQSo6MqGVPUp4CmAyZMn68yZM8MOrrOWL19OpPdbsbiCmZfMZObMmTz1wVOdTp9xJoMT/U70QGTdY8LwCazYuYKJxRPpnxz8HnDjPIejt8VrjGldOIW+jtRwtrXORdOKyD8CfuCPre48zFrRjtRc7dixg+Tk5LDmsOtJfr+/S80jFy9e3APRdJ6IkJCQwJIlS6isrOy1k1RbLai5ULM2s7NxJwmSwHDfcLfD6TWSa5OpTIzO79s2rAVGichw4BBwO3CnuyH1HqdrT1N+spxxuX230m/ysMks3b6UhZ8s5KvTvup2OMaYGBZOoa8jNZxtrRPfXloRuRf4IjBL25jFO9xa0YvVXFVXV7NixQpGjx4dtVMLHD16NGpj66jTp0+ze/duhg8fTkFBgdvhdInVgkZWszZzwH+Aw/7DNGojaXFp5HnzyPZkR03FwaaGTdRoDZfGX2rNOjshqS6JI4EjNGgDCZLgdjgXpap+EfkWsITgoGTPqmqJiHzDef9JERkMrAP6AQER+Q4wVlXPuBZ4lCipKAFgfO54lyPpOcMzh5ORksGr6161Qp8xxlXhdKQ6V8MpIvEEazgXXrDOQuAeCboCOK2qh9tL64zq+Q/AjapaG0Z8Ydm/fz/AudEsTc8YPHgwKSkpfPjhh26HYnqB+kA9a+vXsq9pHymSQq43F7/62da4jZLGEgIacDtEzgTOsLpuNZmeTLK8NiVJZyTVJQFQ5a9yOZKOU9XFqlqkqiNV9ZfOsidV9Unn+RFVHaKq/VQ13Xke8wU++LTQ15fv9IkIk4ZNYknJEk7VujIQuTHGAGEU+lTVD7TUcG4HXm2p4Wyp5QQWA3uBUuBp4KH20jppfgOkAe+KyCYRebKrMYajrKyM+Pj4Ts1BZzovLi6OqVOnUlpaaiN5mnY1aiMbGjbQoA1MSJjAZYmXURRfRHFiMSN8I6hsrmRr41bXC34f1H4AwCjfKFfj6I1aCn2Vzb2qiafpopKKEpLjkxk2cJjbofSoScMm0dTcxJ82/cntUIwxMSysefpUdTHBgl3osidDnivwzY6mdZZHxbjmZWVlDB061EaVjIDJkyezcuVK/vKXv3DLLbe4HY6JQqpKSUMJjdrIxISJ5w2MIiIU+Arw4mVX0y72NO3hssTLXIlzb+Ne9jTtYXrS9F7RPDHa+Pw+0uLSOOI/4nYoJgI+2vsRE/In9Pnf2YKBBQwbOIxX173KvVfe63Y4xpgY1be/abuourqaY8eO9do+Zr1NUlISkydPZsuWLVRV9Z5mXSZyDvgPcDJwkqL4ojZHwhziG8IQ7xAO+g+yu3F3hCOEJm1ied1yMuIymJgwMeL77ytyvblU+Ctoozu36SNO1JxgXdk6rhl7jduh9DgR4bZJt/Hutnc5WXPS7XCMMTHKCn2taOnPN2xY325yEk2mT5+Oz+dj2bJlbodiokxtoJZ9TfvI8mSR681td91CXyH94vrxfu371AYi2yV4dd1qzgbOMitllg3eEoZcby41WsOZgHV768ve3/E+AQ0we+xst0OJiLmT51oTT2OMq6zQ14qW/nw5OTluhxIzUlJSmDZtGtu3b+fAgQNuh2OihKqyq3EXccRR5Cu66PpxEseY+DHBu261y3s+QEelv5KNDRsZHz/+ogVT076W81fh79J0d6aXeKfkHfol9aN4eLHboUTE5ILJFAws4OW1L7sdijEmRlmhrxX79+9n6NCheDxWWx9JV155Jf3792fRokU0Nze7HY6JAvv9+zkROMFw33AS4jrWRy4lLoXixGJ2N+2OSDPPgAZYWruUJElietL0Ht9fXzcwbiAJkmCFvj4sEAiwpGQJs0bPwusJa2iBXkNEuGvqXby77V2O1R5zOxxjTAyyQt8FampqqKqqsqadLoiPj2fOnDlUVVXZZOeGgAZYWbuSJEkiz5vXqbSTEicxyDOIZbXL8Hv8PRRh0PqG9VQ2VzIjeQaJcYk9uq++YEvDljYfELw4zvHmUO4vdzlS01Pmr5/PgRMH+MqUr7gdSkTdM+0eAhrgvbL33A7FGBODrNB3gbKyMgAbxMUlRUVFTJw4kVWrVrFjxw63wzEu2tq4lROBExT6ComTzn1VecTD1clX06ANHMo71EMRwhH/EVbXrWaUb1SHmp+ajhnqHcqpwClON592OxTTQU998NS5R3sCgQC/+PMvGJMzhlsn3Rqh6KJD0eAipo2cxpK9S2ygImNMxMVGu4pOKC0tJSEhwfrzRUhtbS3r168/93rSpEnMmTOHo0eP8vrrr3P77bdz6tT5E9pOmjQp0mGaCGvQBlbXrSbPm0emJ7NL28jyZjElcQofD/iYvY17GRE/oltjbNRG3q55m5S4FL6Q/AVEpFu3H8uG+4bzQd0HlDWVcZnHnek3TPdpKQgGNEDJoRK2Hd7Gyw++zB9W/cHlyCKn5RyMzBrJR3s+4sPSD7lq1FUuR2WMiSV2py+EqrJ7924KCwutP5+LvF4vd911FwMHDmTevHns37/fakVjzNq6tdRpHTOSZoRVmJqSOIXEukTer32f+kB9t8WnqiyrXcbpwGlG+Uaxu2l3q00VTdeke9JJj0tnX9M+t0MxYWjyN1FaWcrWQ1t5b9t7/PNb/8xvlv2Gh695mLmT57odniuKC4pJi0/j10t/7XYoxpgYY3f6QlRUVFBTU0NRkTXTcltycjL33nsvCxYsYMuWLRw5coRx48aRmprqdmimh51pPsOmhk2MiR/DIO8gjjYf7fK2POIh/2A+pUWlrKhbwbUp13ZLjBsbNrKjcQfDfcNJ96R3yzbN+Qp8BWxp2EKTNuETn9vhmIto9Dey7fA2dh7ZyZsb32R35W7Kjpfhb/60T21O/xyevudpHrjqgZi9M57gS2DOyDnM3zifA8cPMHTgULdDMsbECCv0hdi1axcAhYWFLkdiIDhp+5133sn8+fPZtWsXy5cvJycnh5ycHHJzbVj8vmpl3UoEYVrStG7ZXnJdMlMSp7Cmfg1DvUMZkzAmrO3tbNzJyrqVFPoKyffmd0uM5rNG+EawqWET+5r2URRvFXHR7I0Nb/DTP/2Uk7UniffEMzZ3LBPzJzJ38lwKBxWy/fB2BqUNIi0xja9//utuh+u6m4tu5vVdr/PoW4/yxN1PuB2OMSZGWKEvxI4dO8jPzyc5OdntUGJWaP++FiNGjCAvL499+/ZRVlbG008/zZAhQ7j88ssZN24c8fHxLkRqekJZUxmlTaVMS5xGWlxat213auJUDvkP8X7t+2R4Msj2ZndpO7sad7GkZgl53jxmp8xmR6MNNtRThniHkCqpbGvYZoW+KPbYW4/xowU/YsiAIdx9xd2MHjyah/7XQ+etc7HBXWLN4NTB/M3n/4anVz7NI7MfoXCQVTQbY3qeFfocR44cobKykuuvv97tUEwrEhISGD16NCNHjgSChcOFCxeyZMkSLr30UiZNmsTgwYNdjtKEw69+ltcuZ0DcAC5PvDysbYX2qavTOkoaSyjwFXCs+RgLqxcyN20u/T39O7w9VWVDwwZW1a0i15vLjak3WpPDHiYijEkYw7r6ddQEakiJS3E7JHOBu56+i5fWvMSUgincN/0+PHHBvvBWyLu4n9zwE577y3N8/7Xvs+ChBTHb3NUYEzk2kItj8+bNxMXFMX78eLdDMe3w+Xz4fD6mTp3KlVdeSWZmJhs2bOD3v/89zzzzDBs2bKCxsdHtME0XrKtfx+nAaWYmz8Qr3V8flSAJXJZwGc0089rZ1zjW3LEJkmsCNSyqWcSqulUU+gq5JfUW4sXuLkfCmPgxKMq2xm1uh2IusHbfWl5d9yrjc8dz//T7zxX4TMfkpOfw8y/9nDc3vcm8NfM6lKaj02IYY0xrrNBHcN6grVu3UlhYaE07ewkRISMjg4kTJ3L11Vdz3XXX0djYyJ///Gcef/xxFi1axMGDB23Uz17iRPMJ1tWvo8hXxFBfzw1skBqXyq1pt6Ior5x5hU31m2jW5lbXrQ/U83Hdx7xw+gX2N+3n80mfZ07KnB4pkJrWDfAMYKh3KBvrN+JX/8UTmIg4Xn2cW5+8lX5J/bjvqvuIi7NLia54ZPYjTBs5jb/749+xuXyz2+EYY/o4u3oBSkpKOHv2LHPmzHE7FNMFLX36iouLOXnyJAcOHOCTTz5h/fr19O/fn7Fjx1JQUMDQoUNJTEw8l+7C/oM2/587/OrnrZq3iJd4ZiTP6PH9ZXoyuaPfHbxb8y4r6lawvn49hfGFDPQMxIOHaq3msP8wB5oO0EwzI3wjuCrpKgZ4BvR4bOazihOLmV89n60NW5mQOMHtcGJeIBDgnmfv4fDpw3xv9vdITejciMp2l+pTnjgPL//Ny1z52JXM/o/ZvPXtt5g4dKLbYRlj+qiYL/SpKqtWrSIrK4tLLrnE7XBMGFru/mVkZDB+/Hh27NjB1q1b+fjjj/noo48AGDBgABkZGQwYMIDq6mqSk5NJSkoiKSkJVbV+FRHWMt/dseZj3Jh6Y8T6baXGpXJz6s3s9+9nU/0mtjRsoZlP7/glSRK53lxyvDmkxqVS7i+n3F8ekdjM+fJ8eeR581hTv4ai+CKS46w1hpsefetRFm9ZzG/v/C1eT8xfQnTJsepj5xV+3/nuO8z+j9lc8egVfG/297h/+v2MyBpBc6CZXUd3sbl8M5vLN7No8yIOnTpEQAPMWzOPOZfO4b4r7yMzLdPFozHG9BYx/429fft2Kisrufnmm+2Cvw/ZunUrAKNHj2bUqFEMGjSI/fv3c+zYMU6cOMGhQ4eorz9/su5ly5aRmZlJdnY2gwYNIjs7m9zcXGvy24PW1K9hW+M2ihOLGe4bHtF9iwgFvgIKfAU0azPr69ejKPESb004o8zM5JnMOzOPFbUruD7VBttyy+Iti/nJn37ClIIp1oevG60qXcXD1zzMS2te4tG3HuVXi3+Fz+Ojqbnp3Dpej5dBaYMYmTUSr8dLaWUpP5j/A37y5k/4+1l/zyOzHyG7X9dGJTbGxIaYvrKpq6tj8eLFDB482AZw6cM8Hg/Hjx8nNTWV1NRUCgoKAGhqaqKuru7co7a2lrNnz7Jr1y42b/60f8XAgQPJz88nPz+fESNGkJ5uk3GHS1VZXb+aNfVrGB0/misSr3A1Ho94SIpLcjUG07ZMTybFicWsrl9Ndn122KO7xpLQO0oPzniwy9tZu28tc38/99zUDFZJ2r1SE1N5cMaDVJ2tYvvh7VRVVzFtxDRGZo3kc0M+x+jBo3n+o+fPS5M7SeUAABLPSURBVFNxqoK3tr7F4+88zn+9/188OONBvj/7+wzJGOLSURhjollYhT4RuQ74NeABnlHVxy54X5z35wC1wNdUdUN7aUUkA3gFKADKgLmqejKcOFsTCAR44403qKur4+6778bjsVrLWNMyEmi/fv0+815jYyNnzpzh1KlTnDx5kpKSEjZt2gRAamoqgwYNIisri2uuuSbSYZ+nJ/JgT6sN1PJ+7fvsadrD2PixzEqeZReQ5qKmJE7hWPMxVtatBGBiwsSo+NyEkwcj4VTtKY6eOUp1QzWvr3+d/Ix8irKLSE/ueOXV21vf5rYnbyMzNZOHZj5Eoi/x4olMl2SlZZGVlgVcvJCem57LA1c9wIsPvMijbz3K75b/jieWP8FNE27ipgk3UVxQzJABQ0hOCLZW6a4KAGNM79TlQp+IeIDfAtcA5cBaEVmoqqFja18PjHIeU4EngKkXSftDYKmqPiYiP3Re/0NX42xNdXU1JSUlnDhxgjlz5tj8buYz4uPjyczMJDMz2FdCVamurqaqqorKykrKysrYu3cvGzdupF+/fvTv35/CwkLS0rpvQvGL6cE82CNqA7VsbdjK+ob1+NXPjKQZTEiYEBUX7ib6xUkc16Zci9YoK+tWcqDpANOTppPlzXItpnDyYE/F5G/282HphyzYuICFmxZSdrzs3HuhF/39k/oza8wspo+czvTC6a0OILKtYhv/uuRfee4vz3Fp3qW8/Z23WbR5UU+Fbi7Q0UFvlu1cxhUjrqBwUCFLty9lSckS5q+ff+79/kn9ye6XTXOgmbTENAamDKSpuYmRWSMZmTWSgoEF+Lw276gxfV04d/qKgVJV3QsgIi8DNwGhP3Y3AS9ocNz81SKSLiI5BO/itZX2JmCmk/55YDlhFPpUlcbGRqqrqzl27Bh79uxh8+bNNDY2csMNNzB58uSubtrEEBEhLS2NtLQ0RowYgd/v5/jx4wQCAbZs2cLChQsBSE9PJz8/n+zsbAYMGEB6ejqJiYnEx8cTHx+Pz+frzkJOT+XBLgloAD9+mrxNnGo+RXWgmupANScCJ6jwV1Dhr0BRCrwFXJV8FQM9A7u6q7CETtxueheveLkh5QY2NWzi4/qPeensS2R5ssj15pLlyWKAZwBJkkSiJOIRD3HOP0F6qnKhy3lQVQ+Hs+NAIEB9Uz1Hzhxh//H97Diyg6Xbl7J0x1JO1Z4iwZvA7HGzKR5eTF56HmmJaQQ0wPGa41SeqaT8ZDkf7PqABRsWAJDoS2Rk/5GM3DqS+qZ6dh7dyf7j+/F5fPzg2h/wsy/97NwdIxOdMlMz+cqUr3DbpNs4dOoQ5SfLKcou4vDpwxw9c5RPyj+h8kwlJRUlLN2x9Fy6OIlj2MBh5PTPISk+icozlfg8PnweH58b8jmOVx7nw5oPyemf8+kjPYe0hDTivfF44jxWeWdMLxBOoS8POBjyupzP1l62tk7eRdJmt/wYquphERkURoxUVlby5JNPnnvt9Xq55JJLSE5OtgKf6TKv10t2djaTJk0iJSWFMWPGsHfvXsrLyykrK2PLlosXLIYOHcp9990XThg9lQe7ZN7ZecEJz8fBtjOfXvMKQpYni8mJkxkdP5oMT0Y4uzExTkSYmDiRMfFj2N64ndKmUrY1bKOJpjbT3N//ftKkR+7Ch5MHu1To+/d3/p0fLvjheYN8tMjPyOevL/9rrht/HdeNu47UxNTP3C3Kz8g/7/Xp2tOUVpWSHJ/Mss3LKDtextn6s2SmZnLFiCuYUjCFtMQ0Xvz4xa6Ea1wQFxdHfkb+uf/rgakDGZ83nlljZgHByvAbL7uRPVV7KK0s5bX1r1F1torjNcfxn/HT1Nx07nHgxAHO1J5hwc4Fbe5PRIj3xPPolx/lu9d8NyLHaIzpvHAKfa1V61w4E3Zb63Qkbfs7F3kQaGmUXi0iOzuTHsgEjnUyTbSxY4gOXT6G+++/v723h10keUTyYBfyWrT9n0ZTPNEUC8RIPN/hOxdb5WJ5rS3h5MHzVwr/N42DHOQPzr8uOnf+97GP9aznFV7p6rYiJdo+wx3heszf4BudWb3deBWlgQYefvJhHubh9rbT1XxmjOkG4RT6yoHQKsMhQEUH14lvJ+3RlqYvTjO0ytZ2rqpPAV2e5VVE1qlqr77VZ8cQHVw8hp7Kg+fpbF6Ltv/TaIonmmIBi6cbhJMHzxPub1p36IXn32KOgN4WrzGmdXFhpF0LjBKR4SISD9wOLLxgnYXAPRJ0BXDaabrZXtqFwL3O83uBP4URozF9WU/lQWNMx4STB40xxpiI6fKdPlX1i8i3gCUEh6p+VlVLROQbzvtPAosJDlNdSnCo6vvaS+ts+jHgVRF5ADgA3NbVGI3py3owDxpjOiCcPGiMMcZEkgQHFIs9IvKg05ym17JjiA594Ri6U7Sdj2iKJ5piAYvHnK83nn+Luef1tniNMa2L2UKfMcYYY4wxxsSCcPr0GWOMMcYYY4yJcjFZ6BOR60Rkp4iUisgP3Y6nLSLyrIhUisjWkGUZIvKuiOx2/g4Iee9HzjHtFJFr3Yn6UyKSLyLLRGS7iJSIyLed5b3pGBJFZI2IfOIcwy+c5b3mGCLF7XzVzuft5yJySEQ2OY85EYypTES2OPtd5yxr87PTw7FcEnIONonIGRH5TiTPT2//TuvLRMQjIhtFZJHbsXSEBCe5ny8iO5w8P83tmC5GRL7rfDdtFZF5IpLodkwX6mweNcb0HjHXvFNEPMAu4BqCQ2mvBe5Q1W3tJnSBiMwAqoEXVHW8s+xfgBOq+phzYT1AVf9BRMYC84BiIBd4DyhS1WaXwkeCU27kqOoGEUkD1gM3A1+j9xyDACmqWi0iPmAV8G3gy/SSY4iEaMhX7Xze5gLVqvpvkYolJKYyYLKqHgtZ1moejnBcHuAQwYnE7yNC56e3f6f1ZSLyMDAZ6KeqX3Q7nosRkeeBlar6jARHTk1W1VNux9UWEckj+PsxVlXrRORVYLGqPuduZOfrTB51M05jTOfF4p2+YqBUVfeqaiPwMnCTyzG1SlU/AE5csPgm4Hnn+fMEL2pblr+sqg2quo/gSHHFEQm0Dap6WFU3OM/PAtuBPHrXMaiqVjsvfc5D6UXHECGu56t2Pm/Rpq3PTiTNAvao6v5I7rS3f6f1VSIyBLgBeMbtWDpCRPoBMyA4C72qNkZzgS+EF0gSES+QTBtzo7qpk3nUGNOLxGKhLw84GPK6nOi8MGxLdsscT87fQc7yqD4uESkAJgIf08uOwWn2tAmoBN5V1V53DBEQVcd9wecN4FsistlpuhTJpkkKvCMi60XkQWdZW5+dSLqd4F20Fm6dH7C8FA3+E/gBEHA7kA4aAVQB/+00SX1GRFLcDqo9qnoI+DeCU1EdJjhf4zvuRtVh0fCdZYwJUywW+qSVZX2hjWvUHpeIpAKvA99R1TPtrdrKMtePQVWbVXUCMAQoFpHx7awelccQAVFz3K183p4ARgITCF5sPR7BcKar6uXA9cA3naZTrnKawt0IvOYscvP8tCdqPlN9mYh8EahU1fVux9IJXuBy4AlVnQjUAFHbPx/AqUy5CRhOsLlyiojc7W5UxphYEouFvnIgP+T1EKKwiUU7jjp9l1r6MFU6y6PyuJx+cK8Df1TVBc7iXnUMLZzmQ8uB6+ilx9CDouK4W/u8qepRp+AeAJ4mgk0EVbXC+VsJvOHsu63PTqRcD2xQ1aNObK6dH4flJXdNB250+p++DHxBRF50N6SLKgfKnVYXAPMJFgKj2dXAPlWtUtUmYAFwpcsxdZTb31nGmG4Qi4W+tcAoERnu1HjfDix0OabOWAjc6zy/F/hTyPLbRSRBRIYDo4A1LsR3jjMIyh+A7ar67yFv9aZjyBKRdOd5EsEf7h30omOIENfzVVuft5aLFcctwNYL0/ZQPCnOgDI4Tc9mO/tu67MTKXcQ0rTTrfMTwvKSi1T1R6o6RFULCObb91U1qu9AqeoR4KCIXOIsmgVE3WBsFzgAXCEiyc531SyC/Y57A7e/s4wx3cDrdgCRpqp+EfkWsATwAM+qaonLYbVKROYBM4FMESkHfgY8BrwqIg8Q/BG5DUBVS5zRwLYBfuCbUTDK3XTgq8AWp08cwI/pXceQAzzvjHYYB7yqqotE5CN6zzH0uCjJV2193u4QkQkEmwaWAX8boXiygTeC13d4gZdU9W0RWUsrn51IEJFkgiOshp6Df4nU+ekD32kmevxv4I9OJdNegqPQRi1V/VhE5gMbCH6eNwJPuRvVZ3UmjxpjepeYm7LBGGOMMcYYY2JJLDbvNMYYY4wxxpiYYYU+Y4wxxhhjjOnDrNBnjDHGGGOMMX2YFfqMMcYYY4wxpg+zQp8xxhhjjDHG9GFW6DOfISJ3icg7nVj/n0TkmIgc6cC6z4nIPznPZzpDQhtjLiAiPxaRZ9p5v0xEru7IusaYyBORAhFREYm56bGMMdHHCn19mHNRWCci1SJyxClwpV4snar+UVVnd3Af+cAjwFhVHRxuzMZEs9CCVsiyr4nIqu7el6r+SlW/3tl17ULTxIK2Kg1FZLmItJpveiqvGmNMb2CFvr7vS6qaCkwAJgI/6ubtDwOOq2plN2/XGGOMMcYY0w2s0BcjVPUIsIRg4Q8R+aGI7BGRsyKyTURuaVn3wtpQ567BN0Rkt4icFJHfStDVwLtArnM38Tln/decO4unReQDERkX0YM1xiVOXikMef2Z5swi8gMRqRSRwyJys4jMEZFdInJCRH4ckvbnIvJiyOuvish+ETkuIv94wX5D1/3A+XvKyZd/5Wz70pD1BzmtALJ64jwY0x2cO+s/cn6jTorIf4tIYg/ta7SIvOvklZ0iMtdZfoXze+YJWfcWEdnsPI8L+T09LiKvikhGT8RojDHhsEJfjBCRIcD1QKmzaA/weaA/8AvgRRHJaWcTXwSmAJcBc4FrVfU9Z5sVqpqqql9z1n0LGAUMAjYAf+zeozGm1xoMJAJ5wE+Bp4G7gUkE8+NPRWTEhYlEZCzwBPBVIBcYCAxpYx8znL/pTr5cAbzs7KfFHcB7qloV9hEZ07PuAq4FRgJFwP/p7h2ISArBCsyXCP5u3QH8TkTGqepqoAb4QkiSO511Af4euBn4K4J58yTw2+6O0RhjwmWFvr7vTRE5CxwEKoGfAajqa6paoaoBVX0F2A0Ut7Odx1T1lKoeAJbh3DFsjao+q6pnVbUB+DlwmYj076bjMcZtb4rIqZYH8LtOpG0CfqmqTQQLYpnAr538UgKUAJ9rJd2twCJV/cDJVz8BAp3Y7/PAnSLS8p3/VeB/OpHeGLf8RlUPquoJ4JcEC2QtckPzopMfr+rCPr4IlKnqf6uqX1U3AK8TzHcA81r2KyJpwBxnGcDfAv+oquUhv3m3Wp9aY0y0sUJf33ezqqYBM4HRBC8yEZF7RGRTyA/l+Jb32hA6Mmct0OqAMCLiEZHHnKYuZ4Ay5632tm1Mb3Kzqqa3PICHOpH2uKo2O8/rnL9HQ96vo/W8lUuw4gYAVa0Bjnd0p6r6McG7FX8lIqOBQmBhJ+I2xi0HQ57vJ5gXWlSE5kUnP3ZloJZhwNQLCo93EbwzD8G7el8WkQTgy8AGVd0fkvaNkHTbgWYguwtxGGNMj7GaqBihqiucPnf/JiLfJtisbBbwkao2i8gmQLphV3cCNwFXEyzw9SfY3KU7tm1MtKsFkkNeDwa6Y1qSw8CYlhcikkywiWdrtI3lzxNs4nkEmK+q9d0QlzE9LT/k+VCgogf2cRBYoarXtPamqm4Tkf0EuzOENu1sSXu/qn54YToRKej+UI0xpmvsTl9s+U/gGoL9iRSoAhCR+wje6esOaUADwbsQycCvumm7xvQGmwg2o/SIyHUE+/l0h/nAF0XkKhGJB/4vbX9/VxFs+nlh38D/AW4hWPB7oZviMqanfVNEhjiDo/wYeCXM7YmIJIY+gEVAkTNYks95TBGRMSHpXiLYf28G8FrI8ieBX4rIMGfjWSJyU5gxGmNMt7NCXwxxBm14geC8eo8DHxFsWnYp8Jlayi56gWATnEPANmB1N23XmN7g28CXgJbmYW92x0ad/n7fJHjheZjg3fNW7yCqai3Bvk8fOk3OrnCWlxMcWEmBld0RlzER8BLwDrDXefxTmNu7kmAz6gsfs4HbCd5JPAL8M5AQkm4ewW4S76vqsZDlvybYVPodp//8amBqmDEaY0y3E9W2WgIZY4zpS0TkWYL9oLp9BERjupuIlAFfd0aKNsYYEwbr02eMMTHA6V/0ZWCiu5EYY4wxJtKseacxxvRxIvL/gK3Av6rqPrfjMcYYY0xkWfNOY4wxxhhjjOnD7E6fMcYYY4wxxvRhVugzxhhjjDHGmD7MCn3GGGOMMcYY04dZoc8YY4wxxhhj+jAr9BljjDHGGGNMH2aFPmOMMcYYY4zpw/4/4K3dFqmvG0MAAAAASUVORK5CYII=\n",
      "text/plain": [
       "<Figure size 1080x504 with 7 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "### Lets check the distribution of Agricultural Conditions\n",
    "\n",
    "plt.rcParams['figure.figsize'] = (15, 7)\n",
    "\n",
    "plt.subplot(2, 4, 1)\n",
    "sns.distplot(data['N'], color = 'lightgrey')\n",
    "plt.xlabel('Ratio of Nitrogen', fontsize = 12)\n",
    "plt.grid()\n",
    "\n",
    "plt.subplot(2, 4, 2)\n",
    "sns.distplot(data['P'], color = 'skyblue')\n",
    "plt.xlabel('Ratio of Phosphorous', fontsize = 12)\n",
    "plt.grid()\n",
    "\n",
    "plt.subplot(2, 4, 3)\n",
    "sns.distplot(data['K'], color ='darkblue')\n",
    "plt.xlabel('Ratio of Potassium', fontsize = 12)\n",
    "plt.grid()\n",
    "\n",
    "plt.subplot(2, 4, 4)\n",
    "sns.distplot(data['temperature'], color = 'black')\n",
    "plt.xlabel('Temperature', fontsize = 12)\n",
    "plt.grid()\n",
    "\n",
    "plt.subplot(2, 4, 5)\n",
    "sns.distplot(data['rainfall'], color = 'grey')\n",
    "plt.xlabel('Rainfall', fontsize = 12)\n",
    "plt.grid()\n",
    "\n",
    "plt.subplot(2, 4, 6)\n",
    "sns.distplot(data['humidity'], color = 'lightgreen')\n",
    "plt.xlabel('Humidity', fontsize = 12)\n",
    "plt.grid()\n",
    "\n",
    "plt.subplot(2, 4, 7)\n",
    "sns.distplot(data['ph'], color = 'darkgreen')\n",
    "plt.xlabel('pH Level', fontsize = 12)\n",
    "plt.grid()\n",
    "\n",
    "plt.suptitle('Distribution for Agricultural Conditions', fontsize = 20)\n",
    "plt.show()\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Some Interesting Patterns\n",
      "---------------------------------\n",
      "Crops which requires very High Ratio of Nitrogen Content in Soil: ['cotton']\n",
      "Crops which requires very High Ratio of Phosphorous Content in Soil: ['grapes' 'apple']\n",
      "Crops which requires very High Ratio of Potassium Content in Soil: ['grapes' 'apple']\n",
      "Crops which requires very High Rainfall: ['rice' 'papaya' 'coconut']\n",
      "Crops which requires very Low Temperature : ['grapes']\n",
      "Crops which requires very High Temperature : ['grapes' 'papaya']\n",
      "Crops which requires very Low Humidity: ['chickpea' 'kidneybeans']\n",
      "Crops which requires very Low pH: ['mothbeans']\n",
      "Crops which requires very High pH: ['mothbeans']\n"
     ]
    }
   ],
   "source": [
    "## Lets find out some Interesting Facts\n",
    "\n",
    "print(\"Some Interesting Patterns\")\n",
    "print(\"---------------------------------\")\n",
    "print(\"Crops which requires very High Ratio of Nitrogen Content in Soil:\", data[data['N'] > 120]['label'].unique())\n",
    "print(\"Crops which requires very High Ratio of Phosphorous Content in Soil:\", data[data['P'] > 100]['label'].unique())\n",
    "print(\"Crops which requires very High Ratio of Potassium Content in Soil:\", data[data['K'] > 200]['label'].unique())\n",
    "print(\"Crops which requires very High Rainfall:\", data[data['rainfall'] > 200]['label'].unique())\n",
    "print(\"Crops which requires very Low Temperature :\", data[data['temperature'] < 10]['label'].unique())\n",
    "print(\"Crops which requires very High Temperature :\", data[data['temperature'] > 40]['label'].unique())\n",
    "print(\"Crops which requires very Low Humidity:\", data[data['humidity'] < 20]['label'].unique())\n",
    "print(\"Crops which requires very Low pH:\", data[data['ph'] < 4]['label'].unique())\n",
    "print(\"Crops which requires very High pH:\", data[data['ph'] > 9]['label'].unique())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Summer Crops\n",
      "['pigeonpeas' 'mothbeans' 'blackgram' 'mango' 'grapes' 'orange' 'papaya']\n",
      "-----------------------------------\n",
      "Winter Crops\n",
      "['maize' 'pigeonpeas' 'lentil' 'pomegranate' 'grapes' 'orange']\n",
      "-----------------------------------\n",
      "Rainy Crops\n",
      "['rice' 'papaya' 'coconut']\n"
     ]
    }
   ],
   "source": [
    "### Lets understand which crops can only be Grown in Summer Season, Winter Season and Rainy Season\n",
    "\n",
    "print(\"Summer Crops\")\n",
    "print(data[(data['temperature'] > 30) & (data['humidity'] > 50)]['label'].unique())\n",
    "print(\"-----------------------------------\")\n",
    "print(\"Winter Crops\")\n",
    "print(data[(data['temperature'] < 20) & (data['humidity'] > 30)]['label'].unique())\n",
    "print(\"-----------------------------------\")\n",
    "print(\"Rainy Crops\")\n",
    "print(data[(data['rainfall'] > 200) & (data['humidity'] > 30)]['label'].unique())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Clustering Similar Crops"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(2200, 7)\n"
     ]
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>0</th>\n",
       "      <th>1</th>\n",
       "      <th>2</th>\n",
       "      <th>3</th>\n",
       "      <th>4</th>\n",
       "      <th>5</th>\n",
       "      <th>6</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>90.0</td>\n",
       "      <td>42.0</td>\n",
       "      <td>43.0</td>\n",
       "      <td>20.879744</td>\n",
       "      <td>6.502985</td>\n",
       "      <td>82.002744</td>\n",
       "      <td>202.935536</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>85.0</td>\n",
       "      <td>58.0</td>\n",
       "      <td>41.0</td>\n",
       "      <td>21.770462</td>\n",
       "      <td>7.038096</td>\n",
       "      <td>80.319644</td>\n",
       "      <td>226.655537</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>60.0</td>\n",
       "      <td>55.0</td>\n",
       "      <td>44.0</td>\n",
       "      <td>23.004459</td>\n",
       "      <td>7.840207</td>\n",
       "      <td>82.320763</td>\n",
       "      <td>263.964248</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>74.0</td>\n",
       "      <td>35.0</td>\n",
       "      <td>40.0</td>\n",
       "      <td>26.491096</td>\n",
       "      <td>6.980401</td>\n",
       "      <td>80.158363</td>\n",
       "      <td>242.864034</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>78.0</td>\n",
       "      <td>42.0</td>\n",
       "      <td>42.0</td>\n",
       "      <td>20.130175</td>\n",
       "      <td>7.628473</td>\n",
       "      <td>81.604873</td>\n",
       "      <td>262.717340</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "      0     1     2          3         4          5           6\n",
       "0  90.0  42.0  43.0  20.879744  6.502985  82.002744  202.935536\n",
       "1  85.0  58.0  41.0  21.770462  7.038096  80.319644  226.655537\n",
       "2  60.0  55.0  44.0  23.004459  7.840207  82.320763  263.964248\n",
       "3  74.0  35.0  40.0  26.491096  6.980401  80.158363  242.864034\n",
       "4  78.0  42.0  42.0  20.130175  7.628473  81.604873  262.717340"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "### Lets try to Cluster these Crops\n",
    "\n",
    "# lets import the warnings library so that we can avoid warnings\n",
    "import warnings\n",
    "warnings.filterwarnings('ignore')\n",
    "\n",
    "# Lets select the Spending score, and Annual Income Columns from the Data\n",
    "x = data.loc[:, ['N','P','K','temperature','ph','humidity','rainfall']].values\n",
    "\n",
    "# let's check the shape of x\n",
    "print(x.shape)\n",
    "\n",
    "# lets convert this data into a dataframe\n",
    "x_data  = pd.DataFrame(x)\n",
    "x_data.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 720x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "# lets determine the Optimum Number of Clusters within the Dataset\n",
    "\n",
    "from sklearn.cluster import KMeans\n",
    "plt.rcParams['figure.figsize'] = (10, 4)\n",
    "\n",
    "wcss = []\n",
    "for i in range(1, 11):\n",
    "    km = KMeans(n_clusters = i, init = 'k-means++', max_iter = 300, n_init = 10, random_state = 0)\n",
    "    km.fit(x)\n",
    "    wcss.append(km.inertia_)\n",
    "\n",
    "# lets plot the results\n",
    "plt.plot(range(1, 11), wcss)\n",
    "plt.title('The Elbow Method', fontsize = 20)\n",
    "plt.xlabel('No. of Clusters')\n",
    "plt.ylabel('wcss')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Lets check the Results After Applying the K Means Clustering Analysis \n",
      "\n",
      "Crops in First Cluster: ['maize' 'chickpea' 'kidneybeans' 'pigeonpeas' 'mothbeans' 'mungbean'\n",
      " 'blackgram' 'lentil' 'pomegranate' 'mango' 'orange' 'papaya' 'coconut']\n",
      "---------------------------------------------------------------\n",
      "Crops in Second Cluster: ['grapes' 'apple']\n",
      "---------------------------------------------------------------\n",
      "Crops in Third Cluster: ['maize' 'banana' 'watermelon' 'muskmelon' 'papaya' 'cotton' 'coffee']\n",
      "---------------------------------------------------------------\n",
      "Crops in Forth Cluster: ['rice' 'pigeonpeas' 'papaya' 'coconut' 'jute' 'coffee']\n"
     ]
    }
   ],
   "source": [
    "# lets implement the K Means algorithm to perform Clustering analysis\n",
    "km = KMeans(n_clusters = 4, init = 'k-means++', max_iter = 300, n_init = 10, random_state = 0)\n",
    "y_means = km.fit_predict(x)\n",
    "\n",
    "# lets find out the Results\n",
    "a = data['label']\n",
    "y_means = pd.DataFrame(y_means)\n",
    "z = pd.concat([y_means, a], axis = 1)\n",
    "z = z.rename(columns = {0: 'cluster'})\n",
    "\n",
    "# lets check the Clusters of each Crops\n",
    "print(\"Lets check the Results After Applying the K Means Clustering Analysis \\n\")\n",
    "print(\"Crops in First Cluster:\", z[z['cluster'] == 0]['label'].unique())\n",
    "print(\"---------------------------------------------------------------\")\n",
    "print(\"Crops in Second Cluster:\", z[z['cluster'] == 1]['label'].unique())\n",
    "print(\"---------------------------------------------------------------\")\n",
    "print(\"Crops in Third Cluster:\", z[z['cluster'] == 2]['label'].unique())\n",
    "print(\"---------------------------------------------------------------\")\n",
    "print(\"Crops in Forth Cluster:\", z[z['cluster'] == 3]['label'].unique())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Results for Hard Clustering\n",
      "\n",
      "Crops in Cluster 1: ['pomegranate', 'kidneybeans', 'lentil', 'chickpea', 'blackgram', 'orange', 'mango', 'mothbeans', 'mungbean']\n",
      "--------------------------------------------------\n",
      "Crops in Cluster 2: ['grapes', 'apple']\n",
      "--------------------------------------------------\n",
      "Crops in Cluster 3: ['cotton', 'banana', 'maize', 'watermelon', 'muskmelon']\n",
      "--------------------------------------------------\n",
      "Crops in Cluster 4: ['jute', 'coconut', 'papaya', 'rice', 'coffee', 'pigeonpeas']\n"
     ]
    }
   ],
   "source": [
    "# Hard Clustering\n",
    "\n",
    "print(\"Results for Hard Clustering\\n\")\n",
    "counts = z[z['cluster'] == 0]['label'].value_counts()\n",
    "d = z.loc[z['label'].isin(counts.index[counts >= 50])]\n",
    "d = d['label'].value_counts()\n",
    "print(\"Crops in Cluster 1:\", list(d.index))\n",
    "print(\"--------------------------------------------------\")\n",
    "counts = z[z['cluster'] == 1]['label'].value_counts()\n",
    "d = z.loc[z['label'].isin(counts.index[counts >= 50])]\n",
    "d = d['label'].value_counts()\n",
    "print(\"Crops in Cluster 2:\", list(d.index))\n",
    "print(\"--------------------------------------------------\")\n",
    "counts = z[z['cluster'] == 2]['label'].value_counts()\n",
    "d = z.loc[z['label'].isin(counts.index[counts >= 50])]\n",
    "d = d['label'].value_counts()\n",
    "print(\"Crops in Cluster 3:\", list(d.index))\n",
    "print(\"--------------------------------------------------\")\n",
    "counts = z[z['cluster'] == 3]['label'].value_counts()\n",
    "d = z.loc[z['label'].isin(counts.index[counts >= 50])]\n",
    "d = d['label'].value_counts()\n",
    "print(\"Crops in Cluster 4:\", list(d.index))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### visualizing the Hidden Patterns"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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JipXrkzDhg2ZMmUKCQkJxvDUMv/666/Z1hFpnTlzhm3btjFy5MgMe+jUqlWLhg0bmm2/ffv24ePjQ5UqVdi0adM9le/gwYP4+flRtWpVmjVrxtatW82Wt2/fPvz8/KhVqxa1atXC09PTWIaISEFSw4KISAHp1q0bJ06c4PDhw8Z78fHxbNu2jc6dO1OkSJEMpxs0aBB///03H374IevXr2fUqFHZnlRnJDo6mpEjR7Ju3TpmzJjB6dOnee6550hMTMzxPFavXk1oaKjx99ZbbwHw+OOPZzndiBEjePLJJ1m9ejWenp68/vrrHDhwwBi+detWxowZg5+fH6tXr6Zhw4a8+uqrWc6zUaNGTJkyBYBVq1YRGhrK6tWrzcYZOHCgMc9HH32Ul19+2ewL1saNG+nduzfNmjVj7dq1jB07luXLl/P222/neJukNWTIELp3787KlStJTk7mpZdeYtiwYVSrVo0VK1bw1FNPGfszrR9++IFFixYxbdo05s2bx6+//prucoWc7L9Zs2bx2muv4eHhQVBQELNnz6Zs2bLcuHGDMWPG4OXlhaurq7H/srqXx4gRI1izZg2jRo1i9erV1KhRg4CAAPbt2wfA7Nmz6d27N2XLljXmV7Vq1XveZl5eXhQtWpQffvghw+GrV6/G19eXJ554wqzc77//PgCLFy8mNDSUdu3aAfDSSy+xZs0aRo4cyeeff07Tpk15/vnn+emnn8zm++WXXxIZGcn777/P0qVLgZRLf95//3369u3LF198QYcOHXj11VfT9Ro4cOAAixcvZurUqXz44YccOXKE1157DUi5lGPx4sUAvP/++0aZMxMZGYm3t3eOLn/Kbp/kpHyQ8st5r169OHr0KPPmzWPatGksXLgw030AOctbWi+88AJhYWFMmTKFpUuXkpSURMeOHTl58qTZeJs2bWL37t18+OGHvP3224SEhBjLAZgzZw7r169n/PjxbNy4kRkzZlC2bNks662wsDD69euHq6sra9as4ZVXXmHevHmMHj063bjZ1RF327t3L8nJybRt2zbTcdKKi4tj8ODB9OnThy+//JJmzZrdU/n69etH+/btWbVqFQ0aNKBv3778/PPPQMolRT179qROnTqsXLmSFStW0LNnT65evZqjsomI5CVdCiEiUkCeeeYZypUrZ3Y9+X//+1+uXLliXAaRkYMHD/LZ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0fg8EBEpaHYxMTF5cytmERGRPDRr1ixmz57NqVOnjEYUW+Tv74+TkxMrV64s7KKIiIUICgpi6NChnD171mgYFBGxZOqxICIihe7ixYvMmTMHLy8vSpYsyd69e5k7dy69e/e26UYFEREREVughgURESl0xYoV4/jx43z++edcu3aNqlWrMmjQICZMmFDYRRMRERGRbOhSCBERERERERHJNT1uUkRERERERERyTQ0LIiIiIiIiIpJralgQERERERERkVxTw4KIiIiIiIiI5JoaFkREREREREQk19SwICIiIiIiIiK5poYFEREREREREck1NSyIiIiIiIiISK6pYUFEREREREREck0NCyIiIiIiIiKSa2pYEBEREREREZFcU8OCiIiIiIiIiOSaGhZEREREREREJNfUsCAiIiIiIiIiuaaGBRERERERERHJNTUsiIiIiIiIiEiuqWFBRERERERERHJNDQsiIiIiIiIikmtqWBARERERERGRXFPDgoiIiIiIiIjkmhoWRERERERERCTX1LAgIiIiIiIiIrmmhgURERERERERyTU1LIiIiIiIiIhIrqlhQURERERERERyTQ0LIiIiIiIiIpJralgQERERERERkVxTw4KIiIiIiIiI5JoaFkREREREREQk19SwICIiIiIiIiK5poYFEREREREREck1NSyIiIiIiIiISK6pYUFEREREREREck0NCyIiIiIiIiKSa2pYEBEREREREZFcU8OCiIiIiIiIiOSaGhZEREREREREJNfUsCAiIiIiIiIiuaaGBRERERERERHJNTUsiIiIiIiIiEiuqWFBRERERERERHJNDQsiIiIiIiIikmtqWBARERERERGRXFPDQh7q0aMHMTExhV0MEckjyrSIbVGmRWyLMi1iOexiYmKSC7sQtiA5OZnk5GTs7fOmrebOmq15Mh+xDut/PUTw0cMZDgsICCAwMLBAy3P8+HGcnZ0LdJmWRpnOW3cf4wEBATRr1uyBP86yohzmLVvP9PpfD1HE5YkC/7woDMpG4bOEfWDrmZaCldF5SmHXp5aQs3uhHgv3ISoqiubNmzNq1ChatWpFhQoVuHTpEgBr167FZDLh4eHBK6+8AsDFixfp3bs3Pj4++Pj4sH///sIsvojcRZkWsS3KtIhtUaZFLFfRwi6AtTt+/Djz589n9uzZuLi4AHD06FFm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smjRIts6atSowZYtW+jXrx+QP5xq69at+Pn58cwzz/DJJ5+wbds22wEsKiqKgwcPEhUVRVxcHHv27CE+Pr5IudevX8+PP/5IXFwcX375Jf/3f//HyZMnAfjxxx+JjIxk586d/PbbbyQlJVVGU4o4DOVaxFiUaRFjuZlM/+c//1GmRaQIh7x5o4+PD8nJyVy6dIlq1arRoUMHdu/eTWJiInPmzGHt2rV8/PHH5ObmcurUKfbv3899991XZBm7du1i//79dO/eHYCcnBw6d+5sm96nT58i8/fo0QOAdu3akZGRQd26dalbty41atQgNTWVqKgooqKiCAwMBCAjI4ODBw/i7+9vW0ZSUhL9+vXD3d2dO+64A7PZzH/+8x/q1q1Lp06daNq0KZDfY3vkyBH8/PzKv/FEHJRyLWIsyrSIsdxMpn///XdlWkSKcMiOhapVq9KiRQuWL19Oly5duO+++4iNjeXw4cPUqFGDt956i+3bt1O/fn3Gjh1LZmbmNcuwWq2EhISwZMmSYtdRu3btIq+rV68OgJubm+3/ACaTiby8PKxWKxMmTGDEiBHXLXdJw6sKL9Pd3Z3c3NzrzitiRMq1iLEo0yLGcjOZHjJkiDItIkU45E8hIH841sKFC/H398fPz4+PPvoIb29vLl26RK1atfDw8OD06dNs27bN9pm6dety6dIlADp37szOnTs5dOgQAJcvX+bXX38tc3lCQ0NZtmyZ7Xdiv//+O2fOnLmmzGvXriUvL4+zZ8+SkJDAAw88UOZ1ihiNci1iLMq0iLGUNtMFP5kAZVpE8jnkiAXIP0jMnz+fzp07U7t2bapXr46fnx/e3t506NABX19f7rrrLh566CHbZ4YNG0b//v1p1KgRGzduZNGiRYwaNYqsrCwApk2bRuvWrctUnq5du7J//37CwsKA/B7X9957j4YNG9rm6dWrF7t27SIgIACTycTMmTNp1KgRv/zyyy20hIhxKNcixqJMixhLaTPdoUMH22eUaREBMKWmpur2qGIozvbMV3uzWCzXPCbq8OHD3H333QAV+nxeKburt5szbidltWRqH6kIBccNZzteFKZs2J+2gfMw+rayV/0KrsMq8vrL2badw45YEJHKUdwB8dy5czRo0MBOJZLScMaOBBGxPx03RERuna7DruVyHQve3t7s2LEDT0/PcltmbGwsCxcu5N//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LPumI/vpp58ICwsjMzOT2rVrs2zZMtq3b2/rPHCmeqljwUGYTKYir61W6zXvSb5HHnmkyOsHH3wQHx8fVqxYQefOnQG1Z1mUpc3UrtenfTCf8npzlEPHpf3UvpSNyvPSSy+RlJTEli1bcHd3LzLNaNvBlXJthLqW977pCLy8vIiNjeXixYusX7+esWPHsnHjRtt0Z6qXfgphZ56enri7u1/TO3r27NlreqikeHXq1KFNmzYcOnSIRo0aAag9b0Jp2uyOO+4gLy+Pc+fOXXceyadMl0x5LZ5y6LiUaftSNirX1KlTWb16NevXr+euu+6yvW+07eBKuTbKufZW9k1HVq1aNf70pz/RsWNHZsyYgbe3N4sXL3bKeqljwc6qVauGj48P27dvL/L+9u3beeihh+xUKueSmZnJgQMHaNSoES1btqRRo0ZF2jMzM5PExES153WUps18fHyoWrVqkXmOHz/O/v371a5XUaZLprwWTzl0XMq0fSkblWfy5Ml88cUXrF+/vshjgcF428GVcm2Ec+2t7pvO5MqVK2RnZztlvdynTJnysr0L4erq1q1LZGQkjRs3pkaNGsydO5eEhAQWLlxIvXr17F08hzNt2jSqVavGlStX+PXXX5k0aRKHDh3i9ddfp379+uTl5fH666/TunVr8vLy+Pvf/86pU6d44403qF69ur2Lbxfp6en8/PPPnDp1ik8//ZR27drh4eFBdnY29erVu2Gb1ahRg5MnT/L+++9z3333cfHiRf7617/i4eHBP/7xD9zc1EdZmDL9B+X1D8qh81KmK5ayYX8vvvgin332GR9//DHNmjUjIyODjIwMIP9LuMlkMtx2MFKubzVDjqw89k1H9fLLL9uukY4fP87bb7/NqlWrePnll2nVqpXT1UuPm3QQH3zwAW+++SanTp2ibdu2zJo1C39/f3sXyyGNHDmShIQEzp07x+23386DDz7I3//+d9q0aQPk//Zo9uzZfPzxx6SmpvLAAw8wb9482rVrZ+eS209sbCy9evW65v1Bgwbx9ttvl6rNMjMzmT59Ol988QWZmZkEBQUxf/58mjVrVplVcRrKdD7l9Q/KoXNTpiuOsmF/BXfYv9rkyZOZOnUqULrjtbNtB6Pkujwy5KjKa990RGPHjiU2NpbTp0/j4eFB+/btef75522PZnW2eqljQURERERERETKzLHG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kWs4W9YL7pdw+fJlMjIy2LRpE35+ftStW7dI+dLS0rj99tupWrUqMTExtvs+nDhxgpo1azJw4EDGjRvHnj17SE9PJy0tjbCwMGbPns2PP/54zXoL57Twb7pFjKZ69eqcPXvWdh7Myclh3759N7WM4s6jXl5epV5uWlqa7eeJhTvVrz4OXe/8ebXQ0FDbzzEg/1ogIyPjpuok+dSxIA7l/vvvp3379qxevZoXXniBmTNn0r179yK9nEFBQezfv7/Yx0/NmTOH5cuXYzab+fe//83s2bOvWcfUqVPZvXs3ZrOZf/zjH7z99ts3Vca2bdty//33FzutpLKVVL7IyEgWLVpE165dOXnypO2CqmvXrvzlL38hLCwMs9nMsGHDbJ0MIvbkDFktjdGjR5OXl4fZbGbEiBEsXryY6tWrs2bNGvz8/AgICODAgQO2u7/fe++9rFy5ErPZzIULFxg1alSJyy9tHa7XHsOGDSM+Pp6uXbvy/fff2/7KEhcXR2BgIIGBgWzYsKHIvRtEypMzZd3Hx4fBgwcTGhpKt27dePLJJ7n//vtp0KABvr6++Pn5MX36dAYMGEBycjIPP/wwn3/+Offccw8A//3vf203bZ0/fz6TJk0iPT2dgQMHYjabCQ8Pt91EubDnnnuOJUuWEBYWZvsduYgRubm58cknnzBjxgz8/f0JDAy86ScxFHcerVatWqmXO2XKFIYNG0aPHj3w9PS0vd+jRw82btxou3nj9c6fVxs6dCht2rQhODgYPz8/xo8fr1HAZWRKTU0t3bgxEakwly9fpmbNmphMJlavXs0XX3yhoc0iDiYlJYWIiAgSExPtXRQRERGno/OosenmjSIOIDk5mUmTJmG1WqlXrx6LFi2yd5FERERERERKRSMWRERERERERKTMdI8FERERERERESkzdSyIiIiIiIiISJmpY0FEREREREREykwdCyIiIiIiIiJSZupYEBEREREREZEyU8eCiIiIiIiIiJSZOhZEREREREREpMzUsSAiIiIiIiIiZaaOBREREREREREpM3UsiIiIiIiIiEiZqWNBRERERERERMpMHQvlqH///qSmptq7GCJSTpRpEWNRpkVERCqGKTU11WrvQhiB1WrFarXi5lY+fTXZKzaUy3JEysvnP+3G3fseIiIi7F2USqFMi9F8/tNuVu9LLnbagAEDDJ9tZVrkxko6TlzNFY4bIlJ6GrFwC1JSUujSpQsTJ04kKCiIBg0acO7cOQBWrlyJ2WzG39+fp59+GoCzZ8/y5JNPEhISQkhICElJSfYsvohcRZkWMRZlWkREpHJUsXcBnN2BAwdYtGgR8+fPx9vbG/4/e3ceF1W9+H/8NaARLmhSaWna95d+3cLUJGHYRAxTNK9ZRlYuhV3UNL+a7V67lltmm1kuD3PL3K4LWl5NHoCyqeVWlilhQtimCQI6CAPz+8PrXMkNFZiZM+/n48HjMTNn+3zOzPvM4TOf8znAgQMHmDFjBps3b8bX15fc3FwAXnrpJYYPH05gYCA///wz/fr1Y+fOnY4svoj8hTItYizKtIiISNVTw8J1uuOOO/D39y/32rZt2+jTpw++vr4A3HTTTQAkJSXxww8/2OcrKCigoKCAunXrVl+BReSylGkRY1GmRUREqp4aFq5T7dq1L3jNZrNhMpkueL2srIwtW7bg7e1dHUUTkWugTIsYizItIiJS9TTGQhUICwtj7dq1nDhxAsDexbJr167MmzfPPt8333zjkPKJyNVRpkWMRZkWERGpXGpYqAKtW7dm7NixREVFERQUxCuvvALAtGnT2LNnD2azmc6dO7NgwQIHl1REKkKZFjEWZVpERKRy6XaTIhWQkZFBixYtHF0Mh7JYLADqIuzmjJYFd6qPxWKx5/ivvL29lW2pFkbLXHWr6v13uePEX+m4ISLn0xgLIlIhOnkQcW36J0BErkTHCRG5VuqxUEn27NnDsmXLeOuttyplfUWffVwp6xHXZCmxUmS1AuDVdxCgL/vqpkyLEZw7lug4clZl5trdM32tny31WLg+2n8i4qzUsOCk3P2Exd3967tM1hw4XO61/v37Ex0d7aASyfVSpsUR/nos0XGk8rh7pq/1s6V/jK+P9p+IOCsN3nierKws/P39GTlyJIGBgQwdOpSkpCS6d+9Ox44d2bVrF7t27SIyMpKQkBAiIyPJyMgAIDk5mUcffRSARx55hODgYIKDg2natCmfffYZpaWljB8/nvDwcMxmswaEEqkGyrSI8SjXIiIizkdjLPzF4cOHWbhwIa1btyY8PJxVq1axadMmNm7cyIwZM5g9ezYbN26kRo0aJCUlMXHiRJYsWVJuHatWrQJg7969DB8+nKioKJYsWYKPjw+JiYmcOXOG7t27Ex4ezp133umAWoq4D2VaxHiUaxEREeeihoW/aNasGW3btgWgVatWhIWFYTKZaNu2LdnZ2eTn5zNs2DAOHz6MyWSipKTkouv5888/+fvf/86CBQuoV68eCQkJfPfdd8TFxQGQn5/P4cOHdbIiUsWUaRHjUa5FREScixoW/sLLy8v+2MPDw/7cZDJRWlrKpEmTCAkJYenSpWRlZdGrV68L1lFaWspTTz3FCy+8QJs2bQCw2Wy89dZbREREVE9FRARQpkWMSLkWERFxLhpj4Srl5+dz2223AfDZZ59ddJ7XX3+dtm3b0q9fP/trERERzJ8/3/6ryY8//sipU6eqvsAiclnKtIjxKNciIiLVSz0WrtJzzz3HsGHD+OijjwgJCbnoPDNnzqR169YEBwcD8MorrzBw4ECys7MJCwvDZrPh6+vL0qVLq7PoInIRyrSI8SjXIiIi1Uu3mxSpgOq+vZPFYsFisZR7zd3vPy/OwWi3OjN6ff56LNFxRCrLtX62jJa56qb9JyLOyi16LAwbNowHHniAPn36lHv9119/5cUXX2Tx4sWXXNbPz4+kpCR8fX2rupjlHF88sFq391e1H5kDoBNQB9HJ/+Up045zE3A83dGlqDzuVp9T//mrbEUlNoqs//2dosEjH5SbXpFjmqvl2iiZvhpFJTZq9X3f/n7qe0pERM5xi4aFS7ntttsue6Lizs6NiB0dHe3gkohUnDIt4hhf/ljM5wfPu/PC5phy0/v373/N3yfKtfP48sdiPo+Jua73U0REjMmQgzcuW7YMs9lMUFAQzzzzDACpqalERkZyzz332P9pzsrKIjAwEDg7OvRrr72G2WzGbDYzZ86ccuu0WCz069ePRYsWkZWVhb+/P7GxsZjNZgYOHMjp06eBs/fD7tmzJ2FhYTz00EP89ttvACxatIjw8HCCgoJ48skn7fOLyJUp0yLGo1yLiIgYh+EaFg4cOMCMGTPYsGEDqampTJs2DYDff/+dTZs2sWLFCl5//fULllu4cCFZWVls27aNtLQ0+vfvb59WWFhIdHQ0Dz/8MIMGDQLOXuM2ePBg0tLSqFu3rn0U6RdeeIHFixezdetWnnjiCd544w0AevfuTWJiIqmpqbRs2ZIlS5ZU/c4QMQBlWsR4lGsRERFjMdylENu2baNPnz726yxvuukmAKKiovDw8KBVq1YcO3bsguWSkpJ46qmnqFGjRrnlAAYMGMBzzz1X7gSmSZMmBAQEAGe7eM6ZM4eIiAgOHDjA3/72NwDKyspo2LAhAN9//z2TJk3i5MmTFBYW6h7ZIhWkTIsYj3ItIiJiLIZrWLDZbJhMpgte9/LyKjdPRZcDCAgIID4+nkceeeSS85hMJmw2G61atWLLli0XTB8+fDhLly7Fz8+PpUuXkpKSUtEqibg1ZVrEeJRrERERYzHcpRBhYWGsXbuWEydOAJCbm1uh5bp27conn3yC1Wq9YLlXXnmFm266ibFjx9pfy8nJYefOnQCsXr2agIAAWrRowfHjx+2vl5SUcODAAeBsF81GjRpRUlLCqlWrrr+iIm5CmRYxHuVaRETEWAzXsNC6dWvGjh1LVFQUQUFBvPLKKxVabuDAgTRp0oSgoCCCgoIuOKGYOnUqRUVF/OMf/wCgZcuW9oGncnNzefrpp7nhhhtYtGgREyZMICgoiJCQEPuJy6uvvkpERAR/+9vfdP9hkaugTIsYj3ItIiJiLKa8vLwL+xrKZWVlZREdHU16uoFufv4XFosFQPeo/o+MjAydZBqYO2S6shgtC6pP5bBYLPbvjYvx9vau9u8T5brynXufr+f9NFrmqpv2n4g4K8ONsSCVQw0KIiJSUY5oOJDqp/dZREQuxWl6LFzsl4U9e/awbNky3nrrrQvm9/PzIykpyT6idGWIiorizTffpEOHDpW2zmv13YrHHV2EKzpTYuPOnh+6xYmGfiG4esp0ea6QaXF+VzruVuWxSpkuT5m+dmdKbJyx/vd5ywc/LDfdHc4rrpXOR0TEWTl1j4UOHTo4xcmDXFzqQStvromhf//+REdHO7o44gKUaZHr42zHXWVarkXqQSuJ353XsrA+ptx0Z/l8i4hIxTnl4I1HjhwhJCSEDz74gEcffRSAEydO0LdvX0JCQhg9erT9NlRZWVncd999jBo1ioCAAPr27Wu/zvOnn36iX79+hIWF0aNHDw4dOkRBQQHt2rWjpKQEgPz8fPz8/OzPV6xYQWRkJIGBgezatQuAU6dOMWLECMLDwwkJCeGLL76wb7tHjx6EhoYSGhrKjh07AEhOTiYqKoqBAwfi7+/P0KFD7eV9/fXX6dy5M2azmddee62a9qiIYynTIsaiTIuIiMj5nK5hISMjgyeffJJZs2aV+xVk6tSpBAQEkJycTI8ePcjJybFPy8zMJCYmhu3bt1OvXj3Wr18PwHPPPcdbb73F1q1beeONNxg7dix169YlODiYzZs3A7BmzRoefPBBatasCcDp06f58ssvefvtt3n22WcBmDFjBqGhoSQmJrJhwwb+8Y9/cOrUKW655RbWrl3Ltm3bWLBgAS+++KK9TN9++y1Tpkxhx44dHDlyhO3bt5Obm8vnn3/O9u3bSUtL4/nnn6/y/SniaMq0iLEo0yIiIvJXTnUpxPHjxxkwYACLFy+mdevWJCcn26elpaXx6aefAtC9e3fq169vn9asWTPatWsHQPv27cnOzqawsJCdO3cyaNAg+3zFxcXA2dtVvf/++/Tq1YulS5fy/vvv2+fp168fAEFBQRQUFJCXl0dCQgL//ve/mTlzJgBnzpwhJyeHRo0aMW7cOPbv34+HhweZmZn29XTs2JHGjRsDZ68zzc7Oxt/fHy8vL0aOHElkZCQPPPBApe4/EWejTIsYizItIiIiF+NUDQs+Pj40btyYHTt20Lp16wov5+XlZX/s6emJxWKhrKyMevXqkZKScsH8AQEBjB07lpSUFEpLS2nTpo19mslkKjevyWTCZrOxePHiCwbLmTJlCrfeeispKSmUlZXRsGHDS5bJarVSo0YNEhIS2Lp1K6tXr2bevHls2LChwvUUcTXKtIixKNMiIiJyMU51KcQNN9zA0qVLWb58OatWrSo3zWw221/bsmULeXl5l12Xj48PzZo1Y926dQDYbDa+/fZb+/To6GhiYmJ4/PHyozqvXbsWgPT0dHx8fKhXrx4RERHMnTvXfv3lvn37gLPXfTZs2BAPDw+WL19OaWnpZctUWFhIfn4+kZGRTJ06tVx5RIxImRYxFmVaRERELsapGhYAateuzfLly/noo4/Iz8+3v/7SSy+RlpZGaGgoCQkJNGnS5Irrmjt3LkuWLCEoKIiAgAA2btxon9a/f3/y8vLsXSrPqV+/PpGRkYwZM8bepXLcuHGUlJQQFBREYGAgkydPBiAmJoZly5bRrVs3MjMzqV279mXLU1hYyKOPPorZbCYqKsq+HhEjU6ZFjEWZFhERkb8y5eXl2RxdCEeIi4vjiy++YO7cuY4uisuyWCxYLBa3uN+07hvt/JTp6mG0LLhafa503HW1+lyOMm1MGRkZNGnSxH5nkItxh/OKa2WkjIuIsTjVGAvVZdy4ccTHx1/QjVOujr74xVko0+Iu3OW4q0wbm7t8jkVE3IlL9lgYOXIkI0aMoFWrVo4uSpVJXh3t6CK4jeISG8Ul5V/rHPVxuee//PILd999dzWWyv0YPdfKtFyv849Vfz1GnftHzZl+zVSmxdlV5PsfnKshxJkyLiJyPpdsWHAHOmGpPunfWNn+bdll5+nWrRvDhw+vphKJESnTcr0ud6zq378/0dHR+qejGinTrq8i3//w33w5A2VcRJyV0w3eeL6srCz8/f2JjY3FbDYzcOBATp8+TVRUFHv27AFg8eLF3HvvvURFRTFq1CjGjRsHnL3X9pNPPkl4eDjh4eFs374dgNzcXAYMGIDZbKZbt27s378fOHtLqhEjRhAVFcU999zD7NmzL1sGgL1799KzZ0/CwsJ46KGH+O233wBYtGgR4eHhBAUF8eSTT9rnX7duHYGBgQQFBdGjR4/q25EiTkS5FjEWZVpEREScumEBzrbMDh48mLS0NOrWrcv8+fPt03799VemT59OfHw869atIyMjwz7tpZdeYvjw4SQmJrJ48WJGjRoFwOTJk2nXrh1paWmMHz+e2NjYcttas2YNCQkJTJs2jZKSkkuWoaSkhBdeeIHFixezdetWnnjiCd544w0AevfuTWJiIqmpqbRs2ZIlS5YA8NZbb7F69WpSU1NZtmxZle87EWelXIsYizItIiLi3px+8MYmTZoQEBAAnO2KNmfOHPu0Xbt2ERQUxE033QRAnz59yMzMBCApKYkffvjBPm9BQQEFBQVs377dfvIQFhZGbm4uJ0+eBCAyMhIvLy+8vLy45ZZb+OOPPy5ZhoiICA4cOMDf/vY3AMrKymjYsCEA33//PZMmTeLkyZMUFhYSEREBQOfOnRk+fDh9+/ald+/eVbPDRFyAci1iLMq0iIiIe3P6hoW/MplM9sc226WHhygrK2PLli0XDLZzsWXOrdPLy8v+mqenJ1ar9ZJlsNlstGrVii1btlwwffjw4SxduhQ/Pz+WLl1KSkoKAO+++y5ff/01mzdvJiQkhOTkZBo0aHCZ2oq4B+VaxFiUaREREffi9JdC5OTksHPnTgBWr15t/zUC4N577yU1NZW8vDysVivr16+3T+vatSvz5s2zP//mm28AMJvNrFy5EsB+suDj43PVZWjRogXHjx+3v15SUsKBAwcAKCwspFGjRpSUlJS7VdZPP/1Ep06dePXVV2nQoAE5OTnXvF9EXJlyLWIsyrSIiIh7c/oeCy1btmTZsmWMHj2au+66i6effppNmzYBcPvttzN27FgiIiJo1KgRrVq1sp94TJs2jeeffx6z2UxpaSlms5l3332Xl19+meHDh2M2m6lVqxYff3zhbYUqUoYbbriBRYsW8eKLL5Kfn09paSnDhg2jdevWvPrqq0RERHDHHXfQpk0bCgsLARg/fjyHDx/GZrMRGhqKn5/fJbcZ0m95Jew9qYijJcvZ/u1KRxfDrbhjro2SaaONSO5K9XHmY5UyLRXlTJlz5kyJiLgap77dZFZWFtHR0aSnp19ynsLCQurUqYPVauXxxx/niSeeqNRrIitSBnFtFosFi8Vy2Xl++eUX7r777moqkbEp167Nmf4pqAyuVJ/LHau8vb3x9vZ2SH2UabkazpS5inz/w3/z5Qycaf+JiJzP6XssXMnUqVNJSkrizJkzhIeH06tXr3LTk5OTueGGG+jcuTMAw4YN44EHHqBPnz4XzPfhhx+yYsWKaiv75axb+4ijiyB/8eN/BjIvKbHxn0HIeaDHXPt0ZzrxcHWXy7Uy7Xjf7Xd0CSqXO9fn/OPZpZx/nLuYkydPUlpaisViueQxUJmuHufez3Pvmb6XLk/7R0Sk8jh1w0KzZs2u+OvDm2++ednpKSkp1KlTx37CUhVlEPfy3fc2vvnmbEeff62Osb/ev39/oqOjHVUsl3G9ub7eTFe0DCLu4Pzj2aWcf5y7lCZNmhAXF3fJY6AyXT3OvZ/n3jN9L4mISHVx+sEb4WwXR39/f0aOHElgYCBDhw4lKSmJ7t2707FjR3bt2kVubi4DBgzAbDbTrVs39u/fT1ZWFgsWLOCjjz4iODiYtLQ0AFJTU4mMjOSee+4hLi7Ovp38/Hwef/xxOnfuzP/93/9RVlYGQEJCAvfffz+hoaEMGjTIfh3mtGnTCA8PJzAwkOeee84+inVUVBQTJkyga9eu3HvvvfbtHjhwgK5duxIcHIzZbLbfbkvE3SjTIsZisViYMGGCMi0iIuKmXKJhAeDw4cPExsaSmprKoUOHWLVqFZs2beKNN95gxowZTJ48mXbt2pGWlsb48eOJjY2lWbNmDBkyhOHDh5OSkoLZbAbg999/Z9OmTaxYsYLXX3/dvo3du3czadIk0tLS+Omnn9iwYQN//vkn06dPZ926dWzbto0OHTowa9YsAJ555hkSExNJT0/HYrHYB6oCsFqtJCQkMGXKFKZNmwbAJ598QmxsLCkpKSQlJXH77bdX3w4UcTLKtIixHDt2TJkWERFxU059KcT5mjVrRtu2bQFo1aoVYWFhmEwm2rZtS3Z2Nj///DNLliwBICwsjNzcXE6ePHnRdUVFReHh4UGrVq04duyY/fWOHTty5513AtCvXz/S09Px8vLi4MGDdO/eHTh7qyp/f38Atm3bxgcffIDFYiE3N5fWrVvTo0cPAPugVO3btyc7OxuA++67jxkzZvDLL7/Qu3dv7rrrrkreSyKuQ5kWMRZfX19lWkRExE25TMOCl5eX/bGHh4f9uclkorS0FE9PzwuWMZlMV1zXuW6RF5vfZDJhs9kIDw9n/vz55aYVFRXx/PPPk5iYSJMmTZgyZQpFRUUXbMPT0xOr1QrAI488QqdOndi8eTMPPfQQH3zwAWFhYRWqv4jRKNMixlKzZk37Y2VaRETEvbjMpRBXYjabWbny7L2Ik5OTadCgAT4+PtSpU4eCgoIKrWP37t0cOXKEsrIy1q5dS0BAAP7+/uzYsYPDhw8DcPr0aX788Uf7yYmvry+FhYWsX7/+ius/cuQId955J7GxsfTo0YPvvvvuGmsrYnzKtIixKNMiIiLG5TI9Fq7k5ZdfZvjw4ZjNZmrVqsXHH38MQI8ePRg4cCAbN27krbfeuuw6/P39+ec//8n333+P2Wymd+/eeHh4MGvWLJ5++mnOnDkDwGuvvUbz5s0ZNGgQZrOZpk2b0qFDhyuWcc2aNaxcuZIaNWrQsGFDXnzxxeuvuIhBKdMixqJMi4iIGJcpLy/v8veZEhEyMjJo0aIFcHb0c4vFcsE8uh+2uIPzHLvSRQAAIABJREFUs2AE7l6fSx3ProWOgY731/fTGd8To2Wuumn/iYizMkyPBZHq4ownaiIi10LHM2PR+ykiIo6ihoUrWLp0KXv37mX69OnVut1PvuhfrduTK0s+5OgSOCdrsY2Hw+a4zAmtMn39jJYF1ae8c5kG1/hHVZk+y9WOxSIiYiyGGbxRRBwjc38ZMTExxMXFObooIlIJzmVauXYtOhaLiIgjuVzDQlZWFv7+/owcOZLAwECGDh1KUlIS3bt3p2PHjuzatYspU6Ywc+ZM+zKBgYFkZWWRlZXFfffdx6hRowgICKBv3772axF3796N2Wzm/vvvZ/z48QQGBtqXz8nJoV+/fnTq1ImpU6faX1+xYgVdu3YlODiY0aNHU1paCsCYMWPo0qULAQEBTJ482T6/n58fkydPJjQ0FLPZzKFDBvuZTOQaKNMixqJMi4iIuB+Xa1gAOHz4MLGxsaSmpnLo0CFWrVrFpk2beOONN5gxY8Zll83MzCQmJobt27dTr149++2nRowYwbvvvsuWLVsuuNf27t27mTdvHsnJycTFxbFnzx4OHjzImjVr2Lx5MykpKXh6etpvozV+/HiSkpJITU0lNTWV/fv329fl6+vLtm3beOqpp8qdVIm4M2VaxFiUaREREffikmMsNGvWjLZt2wLQqlUrwsLCMJlMtG3bluzsbPz8/C67bLt27QBo37492dnZ5OXlUVBQQOfOnQF4+OGH2bx5s32ZLl260KBBAwB69epFeno6NWrUYN++fYSHhwNQVFTEzTffDMDatWtZuHAhVquV33//nYMHD3L33XcD0Lt3b/u2N2zYUJm7RcRlKdMixqJMi4iIuBeXbFjw8vKyP/bw8LA/N5lMlJaWUqNGDcrKyuzzFBUVXXRZT0/PCt1my2QyXfDcZrPx2GOPMWHChHLTjhw5wsyZM0lMTKR+/foMGzbsotv39PTEarVWpLoihqdMixiLMi0iIuJeXPJSiCtp2rQp+/btA2Dv3r1kZWVddv769etTt25dvvrqKwDWrFlTbnpSUhK5ublYLBa++OILAgICCAsLIy4ujmPHjgGQm5tLdnY2BQUF1KpVCx8fH/744w/i4+OroIYi7kWZFjEWZVpERMRYXLLHwpU8+OCDLF++nODgYDp27Ejz5s2vuMzMmTMZNWoUtWvXJjg4GB8fH/u0gIAA/v73v3P48GEefvhhOnToAMBrr71G3759KSsro2bNmrz99tv4+/vTrl07AgICuPPOO+3dNkXk2inTIsaiTIuIiBiLKS8vz+boQjiDwsJC6tSpA8C7777Lb7/9xrRp0xxcKnEWGRkZtGjRwtHFcEoWiwWLxeJ0905XpquG0bKg+lzoXKYBp8q1Mn15znos/iujZa66af+JiLMyZI+Fa/Hll1/yzjvvUFpayh133MFHH33k6CKJuARnPYlVpkWujTLtmpz1fRMREffgcg0LWVlZREdHk56eXu71qKgo3nzzTXv3x4paunQpe/fuZfr06Tz00EOVWdTr8s/4Rx1dBPmry18CXOlKi22UlcCYkNmGP2GsqlynpKRUZjGvi6EyXc1ZqHKqzzU5/xgF5f+xVaZdzwtBCwEM/V0jIiJVx5CDN1aX0tJSRxdBDOzY3jK+X2QlJiaGuLg4RxfHbSjXIhVz/jHKmY9TynTFxMXFOe17KCIizs8lGxasViuxsbGYzWYGDhzI6dOny00fM2YMXbp0ISAggMmTJ9tf3717N5GRkQQFBdG1a1cKCgrKLbd582buv/9+/vzzT3766Se6detGeHg4kyZNonHjxgAkJyfTq1cvYmJiMJvNAAwYMICwsDACAgJYuHChfX2NGzdmwoQJhIWF0adPH3bt2kVUVBT33HMPGzdurKK9I+KalGsRY1GmRURE3IdLNixkZGQwePBg0tLSqFu3LvPnzy83ffz48SQlJZGamkpqair79++nuLiYIUOGMHXqVFJTU1m3bl257n4bNmzgvffeY9WqVfj6+vLSSy8RGxtLYmIit912W7n17969m9dee40dO3YAMGvWLLZu3UpiYiJz5szhxIkTAJw6dYrg4GC2bt1KnTp1ePPNN1m3bh2ffvppuZMoEVGuRYxGmRYREXEfLjfGAkCTJk0ICAgAoH///syZM6fc9LVr17Jw4UKsViu///47Bw8exGQy0ahRIzp27AhQ7jZVycnJ7NmzhzVr1thf37lzJ0uXLgXg4YcfZvz48fb5O3bsyJ133ml/Pnv2bD7//HMAjh49SmZmJg0aNOCGG26gW7duALRp0wYvLy9q1qxJ27Ztyc7OruS9IuLalGsRY1GmRURE3IdL9lj4K5PJZH985MgRZs6cyfr160lLSyMyMpKioiJsNlu5+c7XrFkzCgsLyczMrND2ateubX+cnJzM1q1b2bJlC6mpqfj5+VFUVARAzZo17dv08PDAy8vL/ljXfIpcnnItYizKtIiIiHG5ZMNCTk4OO3fuBGD16tX2X0QACgoKqFWrFj4+Pvzxxx/Ex8cD8L//+7/8+uuv7N692z6f1WoFoGnTpixZsoTY2FgOHDgAgL+/P+vXrwdgzZo1lyxLfn4+9erVo1atWhw6dIivv/668iss4gaUaxFjUaZFRETch0teCtGyZUuWLVvG6NGjueuuu3j66afZtGkTAH5+frRr146AgADuvPNOOnfuDMANN9zAggULeOGFF7BYLHh7e7Nu3Tr7Olu0aMHcuXMZPHgwy5cvZ8qUKTzzzDN8+OGHREZGluuOeb5u3bqxYMECzGYzLVq0oFOnTpVSxwndVlTKeqRyZGRk0KJFi2rd5vLjy1n59cpq3aYjGT3XRsm0I7JQlVSfa3elY5Qy7VqWL1/u6CKIiIgLM+Xl5dkcXQhndPr0aby9vTGZTKxevZp//etfLFu2zNHFEgdxxD8fFosFi8UClL8/vFw75fr66R9x51ad9Tn/GAWOOU4p05Xn/O+bqmS0zFU37T8RcVYu2WOhOuzdu5dx48bx/fffExAQwKxZs656HUuXLqVr1672kapHjhzJiBEjaNWqFX5+fiQlJeHr63vRZaOTXryu8ksVOFq9m7MVl/Fxp5fVqFCJ9u7dS69evWjVqhX16tW76lwr0/9RzVmocqrPZdmKy6C4DICPza/aX/f29qZBgwaVu7Gr5MjvamfJtK24jIUhE6/7e0LfMyIicj3UsHAJZrOZ1NRUGjduzL///e9rWsdnn31GmzZt7CcrM2fOrMwiisGV7s4lZnYM/fv3Jzo62tHFMQSz2Yy3tzdpaWnXtLwyLe6odHcuZTvP3pox5pMY++vOcGzSd/XZ9yfuRJzD3wsREXFvLjl4o6N88MEHhIeHYzab7fe2zsrK4r777mPUqFEEBATQt29fLBYLcXFx7N27l6FDhxIcHIzFYiEqKoo9e/Y4uBYico4yLWI8yrWIiEj1U8NCBSUkJJCZmUlCQgIpKSns27eP1NRUADIzM4mJiWH79u3Uq1eP9evX06dPH9q3b8+8efNISUlRF0MRJ6NMixiPci0iIuIYuhSighISEkhISCAkJASAU6dOkZmZSZMmTWjWrBnt2rUDoH379mRnZzuyqCJSAcq0iPEo1yIiIo6hhoUKstlsjBkzhiFDhpR7PSsrCy8vL/tzT0/PcqNki4hzUqZFjEe5FhERcQxdClFBERERfPrppxQWFgLwyy+/cOzYscsuU6dOHQoKCqqjeCJylZRpEeNRrkVERBxDPRYqqGvXrhw8eJDIyEgAateuzdy5c/HwuHTbzIABAxgzZgw33ngjW7Zsqa6iikgFKNMixqNci4iIOIYpLy/P5uhCiDi7jIwMWrRoUa3btFgsWCwWvL29NaCYOA1HZKEqqT5Xdu5Y9Fc6NjmHc++Nq7wXRstcddP+ExFnpR4LIk5KJ+0i4gx0LHJuem9ERMQZqMdCFZoxYwZjx469pmUfS/i4kksj4li2Yisf3fe4S/+TokyLq7MVW6HYCsBHwYMA9244UKadxyeBgwE1lFyJeiyIiLPS4I3nsVqtlbq+d955p1LXJ+LKSvdkERMTQ1xcXLVtU5kWKa90TxbFC5MpXphMTExMtWfyeinTxhUXF+dSn0URESnPYZdCZGVl8fDDD3PvvffyzTff0Lx5c2bPns1XX33Fa6+9RmlpKR06dOCdd97By8sLPz8/HnnkEZKTkykpKeG9995j4sSJHD58mFGjRvHUU08B8MEHH7B27VrOnDlDr169eOWVVwB46623WLVqFY0bN8bX15f27dszcuRIoqKi6Ny5M9u3b6dHjx40b96ct99+m+LiYho0aMC8efO49dZbmTJlCjk5ORw5coScnByGDRtGbGwscHbgp6NHj3LmzBliY2MZPHgwr7/+OhaLheDgYFq3bs28efNYsWIFc+bMobi4mE6dOjFjxgw8PT0d9RaIVKrTf+Ty9RsLqN+iCfk//Uqt227mnuceIfdgNj8s2oitoIi6NW+kpKQEQJkWcXL6nlamRUREKsqhPRYyMjIYPHgwaWlp1K1bl1mzZjF8+HAWLFhAWloaVquV+fPn2+dv3LgxW7ZsITAwkOHDh7No0SLi4+OZPHkyAAkJCWRmZpKQkEBKSgr79u0jNTWVPXv2sH79erZt28aSJUvYs2dPuXKcPHmSjRs3MnLkSAIDA4mPjyc5OZl+/frx/vvvlyvvmjVrSEhIYNq0afZ/kGbNmsXWrVtJTExkzpw5nDhxgtdffx1vb29SUlKYN28eBw8eZM2aNWzevJmUlBQ8PT1ZuXJlNexlkepz6ugx7rj/PoLffY4atbz4aX0K3878Fx3GPkbg0/2w2Wxs27bNPr8yLeLc9D2tTIuIiFSEQwdvbNKkCQEBAQD079+f6dOn07RpU5o3bw6c/YVh3rx5DB8+HIAePXoA0KZNG06dOkXdunWpW7cuN954I3l5eSQkJJCQkEBISAgAp06dIjMzk8LCQnr27Gm/bu+BBx4oV46+ffvaHx89epQhQ4bw+++/U1xcTLNmzezTIiMj8fLywsvLi1tuuYU//viDxo0bM3v2bD7//HP78pmZmTRo0KDcNrZu3cq+ffsIDw8HoKioiJtvvrlydqSIk7jx5nrc1PpOABqHdeDHVQl4N7yJ2rffgvXnfBo1akRGRoZ9fmVaxLnpe1qZFhERqQiXuiuEl5cXAB4eHvbHACaTidLSUmw2G2PGjGHIkCHllps1a9Zl11u7dm374xdeeIERI0bQs2dPkpOTmTp16gXbB/D09MRqtZKcnMzWrVvZsmULtWrVIioqiqKiogu2YbPZeOyxx5gwYcLVVVrEpZiuam5lWsRYlGkRERH35NBLIXJycti5cycAq1evpkuXLvz8888cPnwYgOXLlxMUFFTh9UVERPDpp59SWFgIwC+//MKxY8cIDAxk06ZNFBUVUVhYyJdffnnJdeTn53P77bcDsGzZsituMz8/n3r16lGrVi0OHTrE119/bZ9Wo0YNezfMsLAw4uLiOHbsGAC5ublkZ2dXuG4irqDoeB65B7MA+CV5H77tmmP5I5dTvx4H4Lfffruq0ayVaRHH0ve0Mi0iIlIRDu2x0LJlS5YtW8bo0aO56667mDp1Kv7+/gwaNMg+KNS5wZ4qomvXrhw8eJDIyEjg7C8cc+fOpWPHjvTo0YPg4GDuuOMOOnTogI+Pz0XX8dJLLzFo0CBuv/12OnXqRFZW1mW32a1bNxYsWIDZbKZFixZ06tTJPm3w4MEEBQVxzz33MG/ePF577TX69u1LWVkZNWvW5O2336Zp06YVrp+Is6vd5FaOJu7mu9nrqHWbL22e7kX9/72DPW9/Zh+8MTQ0tMLrU6ZFHEvf08q0iIhIRZjy8vJsjthwVlYW0dHRpKenV8v2CgsLqVOnDqdPn6Znz5689957tG/fvlq2La5P942+sitl2mKxYLFY8Pb2rpT7lCvTjmG0LLhbfc7l8HyXyqS+p6UqXOozeu5zWRnfD0ZmtGOWiBiHS42xcD1Gjx7NDz/8wJkzZ3jsscd0siJyBZXdEFBZ6zlHmRb5r3N5zc/Pt+f2Yio7h5VJmXYv5zdyOfPnUkREKsZhPRYcpbp/gblWA+KXO7oI4uZKdu6n9Ovv6d+/P9HR0Y4uziUp0yL/zSvg9Jm9EmXaPRjpM1ud1GNBRJyVQwdvFBERERERERHX5pYNC1arldjYWMxmMwMHDuT06dNMmzaN8PBwAgMDee6557DZznbkiIqKYsKECXTt2pV7772XtLQ04OwvKj169CA0NJTQ0FB27NgBQHJyMlFRUQwcOBB/f3+GDh1qX9eltiEi10eZFjEWZVpERMS1uGXDQkZGBoMHDyYtLY26desyf/58nnnmGRITE0lPT8disbBp0yb7/FarlYSEBKZMmcK0adMAuOWWW1i7di3btm1jwYIFvPjii/b5v/32W6ZMmcKOHTs4cuQI27dvB7jsNkTk2inTIsaiTIuIiLgWt2xYaNKkCQEBAcDZ6/rS09PZtm0bERERmM1mkpOT+eGHH+zz9+7dG4D27dvb72ldUlLCqFGjMJvNDBo0iIMHD9rn79ixI40bN8bDwwM/Pz/7MpfbhohcO2VaxFiUaREREdfiNneFuByTycTzzz9PYmIiTZo0YcqUKRQVFdmne3l5AeDp6YnVagXgo48+4tZbbyUlJYWysjIaNmx4wfznL1NUVHTZbYhI5VGmRYxFmRYREXFubtljIScnh507dwKwevVq+68ivr6+FBYWsn79+iuuIz8/n4YNG+Lh4cHy5cspLS297PznTk6uZhsiUjHKtIixKNMiIiKuxS17LLRs2ZJly5YxevRo7rrrLp5++mlOnjyJ2WymadOmdOjQ4YrriImJ4cknnyQuLo6QkBBq16592fnr16/PoEGDKryNz7rptkvOxB1v72QJOnuPcVe4t7gyXX2MlgWj1OdcXn/66SfatGnj6OJcN2XauM5l7txnFnCJ7xkREbk8U15enoY8FrkCo/zzIXK9jJYF1Uekeukzen20/0TEWblljwVX8PiWzx1dBPkPW3EJYxs1dZlf78U5GSrTRw5eeR5X4gL1sRWXQEmJ/fmHod3sj729vXVscgBDZbqK2IpLmN/1AX0+RUTcgBoWRK7Auu9bJs1fQv/+/YmOVtdXEal+1n3fYv16r/15zOLl9sc6Nomzsu77lriCM/p8ioi4AbduWMjKyuLhhx8mICCAr7/+mrvvvpvHH3+cKVOmcOzYMebNmwfAyy+/bP+1etasWbRo0YKlS5fy73//235Na69evZg4cSIAixcv5v3336dRo0bcddddeHl5MX36dLKzs3n22Wc5fvw4N998M7NmzeKOO+5w5C4QMRRlWsRYlGkRERHX4JZ3hTjf4cOHiY2NJTU1lUOHDrFq1So2bdrEG2+8wYwZM2jRogUbN24kOTmZV155xX5SAvDtt9/yySefkJaWxpo1a8jJyeHXX39l+vTpxMfHs27dOjIyMuzzjxs3jujoaNLS0njkkUd48cUXHVFlEUNTpkWMRZkWERFxfm7dYwGgWbNmtG3bFoBWrVoRFhaGyWSibdu2ZGdnk5+fz7Bhwzh8+DAmk4mS865xDQsLo169evZlf/75Z/7880+CgoK46aabAOjTpw+ZmZkAfPXVV3z66acAREdHM2HChOqsqohbUKZFjEWZFhERcX5u32PBy8vL/tjDw8P+3GQyUVpayqRJkwgJCSE9PZ1ly5bZ73P912U9PT2xWq3YbBW/yYbJZKqEGojI+ZRpEWNRpkVERJyf2zcsXEl+fj633XYbAJ999tkV57/33ntJTU0lLy8Pq9XK+vXr7dPuu+8+Vq9eDcDKlSsJCAiomkKLyCUp0yLGokyLiIg4nhoWruC5555j4sSJdO/endLS0ivOf/vttzN27FgiIiLo06cPrVq1wsfHB4Bp06axdOlSzGYzK1asYOrUqVVdfBH5C2VaxFiUaREREccz5eXlVbxPoFRIYWEhderUwWq18vjjj/PEE0/Qu3dvRxdLrpHFYuH777+nTZs2uhe3m1Km/ysjI4MWLVo4uhiVxlXqY7FYsFgsF53m7e1tPza5Sn0cTZmuHuc+s+d/d+ozen20/0TEWbn94I1VYerUqSQlJXHmzBnCw8Pp1auXo4sk18Hb2xsfHx81KrgxZVoc7fzGA7l+ynT10GdWRMR9qMeCk3pyS5KjiyBiZysuZl7XEJ0kXgdlWqqarbiY/2vky//8z/+oIaIaGDHT1XGs1y/u10f7T0SclcZYuAKr1eroIog4XMm+3cTFxTm6GJVCmRajKtm3m0mTJhETE2OYvFaEMl15jHSsFxGR6uX2l0K89dZbrFq1isaNG+Pr60v79u3ZtGkTnTt3Zvv27fTo0YPmzZvz9ttvU1xcTIMGDZg3bx633norU6ZM4aeffuLXX3/l6NGjPPfccwwaNAiADz74gLVr13LmzBl69erFK6+8wqlTpxgyZAhHjx6lrKyMcePG8dBDDzl4D4gYizItYizKtIiIiPNz64aFPXv2sH79erZt24bVaiUsLIz27dsDcPLkSTZu3AhAXl4e8fHxmEwmFi9ezPvvv8+kSZMA+O6774iPj+f06dOEhoYSGRnJgQMHyMzMJCEhAZvNxmOPPUZqairHjx+nUaNGrFy50r4NEak8yrSIsSjTIiIirsGtGxbS09Pp2bOn/VrCBx54wD6tb9++9sdHjx5lyJAh/P777xQXF9OsWTP7tHPLe3t7ExwczK5du9i+fTsJCQmEhIQAcOrUKTIzMzGbzYwfP54JEybQvXt3zGZzNdVUxD0o0yLGokyLiIi4BrduWLDZLj1uZe3ate2PX3jhBUaMGEHPnj1JTk4ud19rk8lUbjmTyYTNZmPMmDEMGTLkgvVu3bqVL7/8kokTJxIeHs6LL75YCTUREVCmRYxGmRYREXENbj14Y2BgIJs2baKoqIjCwkK+/PLLi86Xn5/P7bffDsCyZcvKTdu4cSNFRUWcOHGC1NRUOnbsSEREBJ9++imFhYUA/PLLLxw7doxff/0Vb29vHn30UZ599ln27dtXtRUUcTPKtIixKNMiIiKuwa17LHTs2JEePXoQHBzMHXfcQYcOHfDx8blgvpdeeolBgwZx++2306lTJ7KysuzT7r33Xvr3709OTg7jxo3jtttu47bbbuPgwYNERkYCZ39VmTt3LocPH2b8+PF4eHhQs2ZN3nnnnWqrq4g7UKZFjEWZFhERcQ2mvLy8S/czdAOFhYXUqVOH06dP07NnT9577z37wFBXMmXKFOrUqcPIkSOruJTiaO5+32iLxQJQpfc2ryzKdNUyWhaMVB+LxcL333/P//zP/9jHFDACZbr6VMex3kiZcwTtPxFxVoa6FOKjjz7i9OnTV7XM6NGjCQ4OJiwsjAcffLDCJyvna9y48VUvI+JKLvVPisVi4cSJE5w4ccJ+QlrZrjbXlZFpUK7F9Xh7e+Pj40ODBg0q9I9hdeT3YpTpqnXufb2W99RIDVIiIlK9DNVjwc/Pj6SkJHx9fSu8TGlpKZ6ente13caNG3P06NHrWsdfDY7fU6nrE6kKRTu3Uvz1NgD69+9PdHR0pW/DKLlWpsXZVEd+L0aZrlrn3tfqfE+vhn5xvz7afyLirJyyx8L777/P7NmzAXj55Zfp3bs3cHak5meeeYYxY8bQpUsXAgICmDx5MgCzZ8/mt99+o3fv3vTq1QuAhIQE7r//fkJDQxk0aJB9kCY/Pz+mTZvGAw88wLp16/Dz82PixIncf//9dOnShb179/LQQw/Rvn17PvnkE3u5PvjgA8LDwzGbzfbtns9mszF+/HgCAwMxm82sWbMGgOTkZKKiohg4cCD+/v4MHTr0siNdixiRci1iLMq0iIiInOOUDQtms5n09HQA9u7dy6lTpygpKSE9PZ3AwEDGjx9PUlISqamppKamsn//fmJjY2nUqBEbNmzg888/588//2T69OmsW7eObdu20aFDB2bNmmXfxo033simTZvo168fcPaXjC1bthAYGMjw4cNZtGgR8fHx9pOShIQEMjMzSUhIICUlhX379pGamlqu3OvXr+fbb78lJSWFdevW8Y9//IPffvsNgG+//ZYpU6awY8cOjhw5wvbt26tjV4o4DeVaxFiUaRERETnHKe8K0b59e/bu3UtBQQE33HAD7dq1Y8+ePaSnpzNt2jTWrl3LwoULsVqt/P777xw8eJC777673Dq++uorDh48SPfu3QEoKSnB39/fPr1v377l5u/RowcAbdq04dSpU9StW5e6dety4403kpeXR0JCAgkJCYSEhABw6tQpMjMzCQoKsq9j+/bt9OvXD09PT2699VbMZjO7d++mbt26dOzY0X59p5+fH9nZ2QQGBlb+zhNxUsq1iLEo0yIiInKOUzYs1KxZk6ZNm7J06VLuu+8+7r77bpKTk/npp5+48cYbmTlzJomJidSvX59hw4ZRVFR0wTpsNhvh4eHMnz//otuoXbt2uedeXl4AeHh42B8DmEwmSktLsdlsjBkzhiFDhlyy3JfrMnn+Oj09PbFarZecV8SIlGsRY1GmRURE5BynvBQCznax/PDDDwkKCiIwMJAFCxbg5+dHQUEBtWrVwsfHhz/++IP4+Hj7MnXr1qWgoAAAf39/duzYweHDhwE4ffo0P/744zWXJyIigk8//dR+7ecvv/zCsWPHLijz2rVrKS0t5fjx46SlpXHvvfde8zZFjEa5FjEWZVpERETASXsswNkv/hkzZuDv70/t2rXx8vIiMDAQPz8/2rVrR0BAAHfeeSedO3e2LzNo0CAeeeQRGjZsyOeff86sWbN4+umnOXPmDACvvfYazZs3v6bydO3alYMHDxIZGQmc/RVl7ty53HLLLfZ5evfuzVdffUVwcDAmk4mJEyfSsGFDDh06dB17QsQ4lGsRY1GmRUREBAx2u0mRqqLbO11PFO1CAAAgAElEQVScxWKx3ytd9z93D0bLgjvXR/k1pnPvq7O+p0bLXHXT/hMRZ+W0PRZExPk564mriFyZ8mtMel9FRMQR3K5hwc/Pj6SkJHx9fSttncnJyXz44YesWLGi0tYZE/9zpa1LKsONkKX3pLLNDLoZ4LpOgpXp6ma0LLhXfWzFRUwP8HXqfz7dKdO24iI+DG/itO+FiIhIRTnt4I0iYnxxcXHExcU5uhgibsOyN5GYmBjlzklY9ibqvRAREUNw6oaFrKws/P39GTlyJIGBgQwdOpSkpCS6d+9Ox44d2bVrF1OmTGHmzJn2ZQIDA8nKyuLUqVP079/fPlL1mjVryq3bYrHQr18/Fi1aVKHtwNn7YY8YMYLw8HBCQkL44osvLihzbm4uAwYMwGw2061bN/bv3w/AlClTGDFiBFFRUdxzzz3Mnj27CveciHM69cdRNo/qzdcfT+DL/+vL/PnzOXDggDIt4qL0PS0iIiLgApdCHD58mIULF9K6dWvCw8NZtWoVmzZtYuPGjcyYMQM/P7+LLhcfH0+jRo1YuXIlACdPnrRPKyws5KmnniI6OprHHnuMrKysK27ns88+Y8aMGYSGhjJr1izy8vKIiIigS5cu5bY7efJk2rVrx2effcbWrVuJjY0lJSUFODvgzoYNGygsLKRTp048/fTT1KxZs2p2nIiTKvztZwLGzsDn7xP46vmzo7Mr0yKuS9/TIiIi4tQ9FgCaNWtG27Zt8fDwoFWrVoSFhWEymWjbti3Z2dmXXK5t27YkJSUxYcIE0tLSqFevnn3agAEDePzxx3nssceuajsJCQm89957BAcH06tXL86cOUNOTk657W7fvp3o6GgAwsLCyM3NtZ8sRUZG4uXlha+vL7fccgt//PFHpe0nEVdR+9bG1Gv2v5g8PLjtttto2bKlMi3iwvQ9LSIiIk7fsODl5WV/7OHhYX9uMpkoLS2lRo0alJWV2ecpKioCoHnz5mzdupU2bdowceJEpk2bZp8nICCA+Ph4bLb/3mnzStsBsNlsLF68mJSUFFJSUti/fz8tW7YsV97z13mOyWS6YBuenp5Yrdar3Bsirs+j5g32xyaTiRo1atgfK9Mirkff0yIiIuL0DQtX0rRpU/bt2wfA3r17ycrKAuDXX3/F29ubRx99lGeffdY+D8Arr7zCTTfdxNixY69qWxEREcydO9d+UnL+Os8xm832bp3Jyck0aNAAHx+fa6qbiDtSpkWMRZkWERExPpdvWHjwwQfJzc0lODiYTz75hObNmwPw/fff07VrV4KDg5kxYwbjxo0rt9zUqVMpKiriH//4R4W3NW7cOEpKSuwDTU2ePPmCeV5++WX27NmD2Wzmn//8Jx9//PH1VVDEzSjTIsaiTIuIiBifKS8v78I+gSJSTkZGBi1atHB0MQzHYrEA6B7uLsRoWXC3+lgsFiwWC97e3sqdE3DHY6DRMlfdtP9ExFk5/V0hRMS43OlkWsQZqEHBuei9EBERo1CPhSq0dOlS9u7dy/Tp06962Q8STlVBiUScy9DAs1djucrJtTItzsZabKG0uIing2vh7e1NTk6Ofs28Steaa2W66liLLQy+z0MNYRehHgsi4qxcfowFEXFdcXFxxMXFOboYIi7r5z3/Jn3hKGJiYpQlMYyf9/xbn2kRERejhoVLGDBgAGFhYQQEBLBw4UIAGjduzKuvvkpoaCgPPvggx48fByAqKoqXXnqJyMhIAgMD2bVr1wXrO378OE8++STh4eGEh4ezffv26qyOiNtTpkWMR7kWERFxDmpYuIRZs2axdetWEhMTmTNnDidOnODUqVPcc889bNu2jaCgoHL33D59+jRffvklb7/9Ns8+++wF63vppZcYPnw4iYmJLF68mFGjRlVndUTcnjItYjzKtYiIiHPQ4I2XMHv2bD7//HMAjh49SmZmJh4eHjz00EMAPProozzxxBP2+fv16wdAUFAQBQUF5OXllVtfUlISP/zwg/15QUEBBQUF1K1bt6qrIiIo0yJGpFyLiIg4BzUsXERycjJbt25ly5Yt1KpVi6ioKIqKii6Yz2QyXfTxxZ6XlZWxZcsWDUIk4gDKtIjxKNciIiLOQ5dCXER+fj716tWjVq1aHDp0iK+//ho4e8JxbiChVatWERAQYF9m7dq1AKSnp+Pj40O9evXKrbNr167MmzfP/vybb76p6mqIyH8o0yLGo1yLiIg4D/VYuIhu3bqxYMECzGYzLVq0oFOnTgDUrl2bAwcOEBYWho+PDwsWLLAvU79+fSIjIykoKODDDz+8YJ3Tpk3j+eefx2w2U1paitls5t13371kGUZ1rV35FZNrpts7VY3ly6tnO8p05TFaFly9Psv/qEnWV44uhWM4OtdGyXR1q0jm3PlzLSLiqkx5eXk2RxfCVTRu3JijR49e8HpUVBRvvvkmHTp0cECppDq4+j8fzspisQA4rNuxMn31jJYFV6+PxWIpl6OcnByXrk9lUK6dW0Uyd+5z7e3trctS/sLVj1kiYlzqseCkNseXOLoIUs6dHM7Se1KZiostdAuv6TYnjcbJtNGy4Or1qQGcP7Bg1dWnuNhCScnZRowuITXtr7vrP3/GyXR1u/xn9Nx3Q4MGDaqxTCIicr3UY+ESbDYbNpsNDw/HDEOhExYxuq93ruKu/+dBdHR0tWxPmRa5Pl/vXMWur/91wev9+/evthyfT5k2pur+bnA16rEgIs7KrQdv/PDDDwkMDCQwMJCPPvqIrKws7rvvPsaOHUtoaCg5OTmMGTOGLl26EBAQwOTJk+3L+vn5MXnyZEJDQzGbzRw6dAiA48eP87e//Y3Q0FBGjx7N3XffzZ9//gnAihUr6Nq1K8HBwYwePZrS0lKH1FvEqJRpEWNRpkVERFyD2zYs7N27l88++4z4+Hi2bNnC4sWLycvLIyMjg+joaJKTk2natCnjx48nKSmJ1NRUUlNT2b9/v30dvr6+bNu2jaeeeoqZM2cCZwd+Cg0NZdu2bfTq1YucnBwADh48yJo1a9i8eTMpKSl4enqycuVKh9RdxIiUaRFjUaZFRERch9uOsZCenk5UVBS1a58d1blXr16kp6dzxx134O/vb59v7dq1LFy4EKvVyu+//87Bgwe5++67AejduzcA7du3Z8OGDfb1fvrpp8DZEavr168PwNatW9m3bx/h4eEAFBUVcfPNN1dPZUXcgDItYizKtIiIiOtw24YFm+3iQ0ucO4EBOHLkCDNnziQxMZH69eszbNgwioqK7NO9vLwA8PT0xGq1Xna9NpuNxx57jAkTJlRWFUTkPMq0iLEo0yIiIq7DbS+FMJvNfPHFF5w+fZpTp07xxRdfEBgYWG6egoICatWqhY+PD3/88Qfx8fFXXG9gYCDr1q0DICEhgby8PADCwsKIi4vj2LFjAOTm5pKdnV3JtRJxX8q0iLEo0yIiIq7DbXsstG/fngEDBhAREQHA/2/v3qNjvvM/jj+TuLZx29BQaWoXRTRxj2QSFEccG5eqE6Yc97K0ulpkg3Laqrrrxa2KbLVFujSsa6sXilxcjm3a2lWVhqDrskgkYRIR+f2h5idIxOQyM995Pc7JceY7n/nM+zvfvD4zPvnM9zt48GDrcsjb/P39CQgIICgoiAYNGtC+ffsH9hsVFcXIkSPZuHEjISEh1K1bF09PT7y8vJg2bRp9+/bl5s2bVKxYkQULFuDr61sm+yfiapRpEWNRpkVERJyHLjdZynJycvDw8KBChQocPHiQCRMmEBcXZ++ypIR0eafSZ7FYAKhataqdKymaMl2Q0bKg/Sk+i8Vize2dqlat6vA5vpMybV8P+h11lvcGezHamCUixuGyKxbKypkzZxg2bBg3b96kUqVKLFq0yN4liTgkZ/nQqEyL3OJsEwiFUaYdmxF+x0REXJFWLDioH7Zct3cJImXuqW63rhHvCh8klWlxJjm5Fhp1zjPMZEJZUKYdV06uheu5966uuZNfWKVi9+dIOdCKBRFxVFqxICJ2s3nzZgDMZrOdKxGROyUe2cactbH0799f+RSnk3hkG3t+iC260fri96cciIg8mMtdFSI1NZV27doxZswYTCYTQ4YM4dq1a8ydO5fOnTsTHBzM+PHjrZejCg8PZ/LkyYSFhREcHMzhw4cBOHz4MGFhYXTo0IGwsDCOHz8OQI8ePfjxxx+tz9e9e3eOHDlSaHsRKTnlWsRYlGkRERHn4nITC3BrGdmwYcNISEigWrVqREdHM3r0aHbv3k1iYiIWi4Uvv/zS2v7atWt89dVXLFiwgHHjxgHQuHFjduzYwb59+5g6dSozZswAbp21et26dQAkJyeTk5PD008/XWh7ESkdyrWIsSjTIiIizsMlvwrh4+NDUFAQcGt524cffoivry+LFi3CYrGQlpZGs2bN6NGjBwD9+vUDICQkhMzMTNLT08nKymLs2LGkpKTg5uZGbm4uAM8++yzz58/nrbfeYs2aNQwcOBCAjIyM+7YXkdKhXIsYizItIiLiPFxyxcLd3NzcmDRpEh9//DEJCQkMGTKE7OzsAvff3f7tt9+mQ4cOJCYmEhMTY23/yCOP0LlzZ3bs2MGmTZuIiIgAKLS9iJQN5VrEWJRpERERx+WSEwtnzpzh4MGDAMTGxlr/IuLl5UVWVhZbtmwp0H7Tpk0AJCYmUr16dWrUqEFGRgb16tUDsC6nvG3IkCFERUXRunVratWqBVBkexEpOeVaxFiUaREREefhkl+FaNKkCTExMbzyyis0bNiQkSNHcuXKFUwmE76+vrRq1apA+5o1axIWFkZmZiZLliwBYPz48YwdO5Zly5bRoUOHAu1btmxJtWrVGDRokHVbUe1FpOSUaxFjUaZFRESch1t6enq+vYsoT6mpqZjNZhITE4vVPjw8nJkzZ97zAaYoZ8+epWfPnhw6dAh3d5dcFGI4um502bBYbl1nvKTXB1euy4/RsqD9uT+LxYLFYqFq1aolzqctlGnjKo/M3f79LS32ysH9GG3MEhHjcMkVC2UpJiaGmTNn8vbbb+uDisgDOMoHtQdRrsXVONJ/pMqCMm1sRv/9FRFxRC63YsHefvzxR86dO0dYWFiR7c6u1QmjxPVk51qo0TvfqT4UKtPiCG5n58SJE/j5+TlNfhyRMi0lkZ1rIftGwd8N7+cqF7hdkvc4rVgQEUelFQvl7KeffiIpKemBH1hEXNG3x7ez44WN9O/fH7PZbO9yikWZFkdwOzuAU+XHESnTUhLfHt/OjqMbC27cUfCmMioiRuQy6/9iYmIwmUyEhIQwevRoTp06Re/evTGZTPTu3ZvTp08DcOHCBQYNGkRISAghISEcOHAAgCVLlhAcHExwcDDLli0Dbn0HNDAwkL/+9a8EBQXRt29f63f6wsPD+f777wG4dOkS/v7+XL9+ndmzZ7Nx40ZCQ0PZuHHjfSoVkeJQpkWMRZkWERFxXi4xsXD06FEWLlzI1q1biY+PZ+7cuURGRmI2m0lISCAiIoKoqCgAoqKiCAkJIT4+nr1799K0aVOSkpJYt24d33zzDV9//TWffPIJP/zwAwC//vorL7zwAvv376dGjRr3XP7qTpUqVWLKlCk899xzxMXF8dxzz5XL/osYjTItYizKtIiIiHNziYmFvXv30qdPH7y8vACoVasWhw4dIiIiAgCz2cz+/futbUeOHAmAh4cHNWrUIDExkfDwcB599FE8PT3p2bOn9UzVTz75JAEBAcCtS1edOnWqvHdPxOUo0yLGokyLiIg4N5eYWMjPz8fNza3INkXdn59f+PktK1f+/xPyeHh4cOPGDQAqVKjAzZs3AcjO1gmeREqTMi1iLMq0iIiIc3OJiYVOnTqxadMmLl++DEBaWhqBgYHExsYCsH79eoKCgqxto6OjAcjLyyMjIwOTycT27du5du0aV69eZfv27QQHBxf5nL6+viQlJQGwefNm63ZPT08yMzNLfR9FXIkyLWIsyrSIiIhzc4mrQjRr1oyJEycSHh6Ou7s7AQEBzJ07l3HjxrFo0SJq167N0qVLAZgzZw7jx49nzZo1uLu788477xAYGMjAgQPp2rUrAIMHD6ZFixakpqYW+pwvv/wyw4YN4x//+AcdO3a0bu/YsSPvvfceoaGhTJgwodDvb9YbVKUUXwEpKV3eqXx4flYBjj64nTJtP0bLglH2p7jZcVTKtOswSuYK4+xZFBGxlVt6enrh6wdFBDD+ByFHYbFYsFgsJbrGt5Qto2XBKPtzOzsnTpzAz89P+RGHZZTMFeZ2FotSkvc4o79+IuK8NLHwkLZt20ajRo1o2rQpAGvXrqVLly7Uq1evVJ/HsvJSqfYnUlyWG9lk38imyqBaLvEffGVapHC3xwOAKoNqWbc78tigTJcdy41sGHDruNv6O6D/GJeMXj8RcVQucY6F0rR9+3aOHTtmvb1u3TrOnTtnx4pESteOE18xbnckL7zwQoHvHRuVMi1SuNvjwe0x4faPI48NynTZ2XHiK6f4HRARkfKniYXfxcTEYDKZCAkJYfTo0Zw6dYrevXtjMpno3bs3p0+f5sCBA3zxxRdMnz6d0NBQ3nvvPZKSkhg1ahShoaFYLBb27NlDhw4dMJlMvPTSS+Tk5ADg7+/PrFmz6NixIyaTiV9++cXOeyxibMq0iLEo0yIiIo5LEwvA0aNHWbhwIVu3biU+Pp65c+cSGRmJ2WwmISGBiIgIoqKiaN++PT169OCtt94iLi6OV155hZYtW7Jy5Uri4uJwc3PjxRdf5KOPPiIhIYEbN25Yz1wN4OXlxd69exkxYgSLFy+24x6LGJsyLWIsyrSIiIhj08QCsHfvXvr06YOXlxcAtWrV4tChQ0RERABgNpvZv3//A/s5fvw4vr6+NGrUCICBAweSkJBgvb9Xr14AtGzZklOnTpX2bojI75RpEWNRpkVERBybJhaA/Px83NzcimzzoPtv91OUypUrA+Dh4cGNGzeKX6CIPBRlWsRYlGkRERHHpokFoFOnTmzatInLly8DkJaWRmBgILGxsQCsX7+eoKAgADw9PcnMzLQ+9s7bTz31FKdPnyYlJQWAzz77jJCQkPLcFRFBmRYxGmVaRETEsVWwdwGOoFmzZkycOJHw8HDc3d0JCAhg7ty5jBs3jkWLFlG7dm2WLl0KQL9+/Rg/fjwffvghn3zyCQMHDmTChAlUqVKFr7/+mqVLlzJ06FDy8vJo1aoVI0aMsPPeibgeZVrEWJRpERERx+aWnp5e9LpAEXGp60ZbLBYsFgvg2NeqF/swWha0P0W7czy4k8YG11Qa7w9Gy1x50+snIo5KKxZEpAD9h0FEbtN4IHfS74OIiBRGKxZKICwsjK+++qrQ+5ctW8awYcN45JFHHrrv7I+OlaQ0EZtYbuSQfSOHygMaWre50gdJZVpc3e0xALCOA848BijTYi93v5+WVo60YkFEHJUmFsqQv78/3333nfXyWA9DH1jEHj4/9g0bf/m2wLb+/ftjNpvtVJFjUabF6FxtDFCmpazcnaXSypEmFkTEUemqECVQv3599u3bx4ABA6zbIiMjWbt2LcuXL+fcuXP06tWLnj17ArBr1y66detGx44dGTp0KFlZWfYqXUTuQ5kWMRZlWkREpHxoYqGMjBkzhrp167J161a2bdvGpUuXmD9/Pv/85z/Zu3cvrVq1sp7BWkQcnzItYizKtIiISOnRyRvLyaFDhzh27Bjdu3cHIDc3l3bt2tm5KhGxlTItYizKtIiIiO00sVBCFSpU4ObNm9bb2dnZ922Xn59P586diY6OLq/SRMQGyrSIsSjTIiIiZU9fhSihJ554gp9//pmcnByuXLnCnj17rPdVq1aNzMxMANq1a8eBAwdISUkB4Nq1ayQnJ9ulZhEpnDItYizKtIiISNnTioUScHNzw8fHh759+xISEkLDhg0JCAiw3j906FAiIiLw9vZm27ZtLF26lJEjR5KTc+tSXtOmTaNRo0b2Kl9E7qJMixiLMi0iIlI+dLlJG12+fJmOHTty5MgRe5ci5cBVLu9ksViwWCwFtjnzNewfhjJdPEbLgvanICONAcq0YzJa5gpzd5ZKK0eu8vqJiPPRigUbnD17lp49e/Lyyy/buxRxAs70Qd1R6ypryrTILbfHgDvHrdv/OtPYoExLSd3vvftuRb1nuur7qYi4Lk0s2KBixYrUrFmTNWvW4O/vz4ULF5g1axaPPfYY27ZtK5XnyPk0sVT6kdLhC+Tsv2jTYzcejSf254QC2/r374/ZbC6FyqQ0KNPFV5IsOCLtz/3dPW4525ilTDsuZ8nc/d677+ZsuRARKUuaWLDBnj17aNy4McuXLwegX79+LFiwgI4dO9q5MhGxhTItYizKtIiISPnSxMIdYmJiWLx4MW5ubjRv3pxp06Yxbtw4Ll68SO3atVm6dClpaWm8/vrrWCwWQkND6dmzJ/v37yc1NZUePXrwxhtv8MYbbxAXF0dOTg6jRo1i+PDhACxatIhNmzaRk5NDz549mTp1qp33WMTYlGkRY1GmRUREHJMmFn539OhRFi5cyM6dO/Hy8iItLY0xY8ZgNpsZOHAgn376KVFRUaxbt44pU6aQlJTE/PnzAdi3bx8zZ86kVatWrF69murVq7N7925ycnLo3r07nTt3JiUlhV9//ZVdu3aRn5/P888/T3x8PCEhIXbecxFjUqZFjEWZFhERcVyaWPjd3r176dOnD15eXgDUqlWLQ4cOsWbNGgDMZjOvv/76A/vZtWsX//73v9m8eTMAGRkZpKSksGvXLnbt2kWHDh0AuHr1Kr/++qs+sIiUEWVaxFiUaREREceliYXf5efn4+bmVmSbB91/u5958+bRtWvXAtu//fZbJkyYYF1uKSJlS5kWMRZlWkRExHG527sAR9GpUyc2bdrE5cuXAUhLSyMwMJDY2FgA1q9fT1BQ0AP76dq1K9HR0eTm5gKQnJzM1atX6dq1K2vWrCErKwuA//73v/zvf/8ro70REWVaxFiUaREREcelFQu/a9asGRMnTiQ8PBx3d3cCAgKYO3cu48aNY9GiRdaTQj3IkCFDOHXqFJ06dSI/Px8vLy/Wrl1Lly5dOHbsGGFhYQA8+uijrFixgjp16ty3n8qDg0t1/8R+BhHMIHsX4YKUaRHbOeK4pUxLeXLEDAA0btzY3iWIiNyXW3p6er69ixARERERERER56SvQoiIiIiIiIiIzTSxICIiIiIiIiI208SCiIiIiIiIiNhMEwsiIiIiIiIiYjNNLDiIVatWERAQgLe3N506dSIhIcHeJbmU2bNnU7NmzQI/Tz31lPX+/Px8Zs+eTdOmTalbty7h4eEcPXrUjhUbU3x8PGazmWbNmlGzZk3Wrl1b4P7iHIecnBwiIyP505/+xOOPP47ZbOa3334rz90AjJPpd955h86dO/PEE0/QsGFDBgwYwH/+8x97l1VqFi5cSM2aNYmMjLR3KTY7d+4cY8aMoWHDhnh7e9O+fXvi4uLsXZbhGCXTpc1I47Y9FGeM1WsoIs5AEwsOYOPGjUyePJmJEyeyd+9eAgMDiYiI4PTp0/YuzaU0btyYY8eOWX/u/ND4/vvvs3TpUubOncuuXbuoU6cOffv2JTMz044VG8/Vq1fx8/Njzpw5VK1a9Z77i3McpkyZwtatW4mOjmbHjh1kZmYyYMAA8vLyym0/jJTpuLg4Ro4cyc6dO9myZQsVKlTg2WefJS0tzd6lldihQ4f4+OOPad68ub1LsVl6ejrdu3cnPz+f9evXc+DAAebNm1foJRLFNkbKdGkzyrhtL8UZY/Uaiogz0OUmHUDXrl1p3rw5ixYtsm5r3bo1ffr04fXXX7djZa5j9uzZbNmyhcTExHvuy8/Pp2nTpowaNYpJkyYBYLFYaNy4MW+99RbDhw8v73JdQv369Zk3bx6DBt26knhxjsOVK1do1KgRS5cupX///gCcOXMGf39/Pv/8c7p27VoutRs501lZWfj6+rJ27Vp69Ohh73JsduXKFTp16sT777/PvHnz8PPzY/78+fYu66HNmDGD+Ph4du7cae9SDM3ImS5NzjxuO4q7x1i9hiLiLLRiwc6uX79OUlISXbp0KbC9S5cuHDhwwE5VuaaTJ0/SrFkzAgICGDFiBCdPngQgNTWV8+fPFzhGVatWxWQy6RiVo+Ich6SkJHJzcwu08fHxoUmTJuV2rIye6aysLG7evEnNmjXtXUqJvPLKK/Tp04dOnTrZu5QS2b59O23atGH48OE0atSI0NBQVqxYQX6+/mZQWoye6bLkLOO2I7l7jNVrKCLOQhMLdnbp0iXy8vLuWbZap04dLly4YKeqXE/btm1ZtmwZGzZsYNGiRZw/f56wsDAuX77M+fPnAXSM7Kw4x+HChQt4eHjg5eVVaJuyZvRMT548GX9/fwIDA+1dis0+/vhjUlJSeO211+xdSomdPHmS6OhoGjRoQGxsLGPGjOHNN99k5cqV9i7NMIye6bLkLOO2I7l7jNVrKCLOooK9C5Bb3NzcCtzOz8+/Z5uUnW7duhW43bZtW1q2bMm6deto164doGPkKGw5DvY4Vkb8fZk6dSr79+/nyy+/xMPDw97l2OT48ePMmDGDL774gkqVKtm7nBK7efMmrVq1si7Hb9GiBSkpKaxatYrRo0fbuTpjMWKmy4uzjNv2VtQYq9dQRBydVizYmZeXFx4eHvfMKF+8eFEn37IjT09PmjZtSkpKCt7e3gA6RnZWnOPw2GOPkZeXx6VLlwptU9aMmukpU6YQGxvLli1baNCggb3LsdnBgwe5dOkSwcHBeHl54eXlRXx8PKtWrcLLy4ucnBx7l/hQvL29adKkSYFtTz31FGfOnLFTRcZj1EyXB2cZtx1BYWOsXkMRcRaaWLCzSpUq0bJlS3bv3l1g++7du2nfvr2dqpLs7GyOHz+Ot7c3Tz75JN7e3gWOUXZ2NomJiTpG5WAXYmcAAAkhSURBVKg4x6Fly5ZUrFixQJvffvuNY8eOlduxMmKmo6Ki+Pzzz9myZUuBy7A6o/DwcBISEti3b5/1p1WrVvTr1499+/Y53SqGoKAgkpOTC2xLTk7miSeesFNFxmPETJcXZxm37a2oMVavoYg4C4/Jkye/Ye8iXF21atWYPXs2devWpUqVKsyfP5+EhASWLFlCjRo17F2eS5g2bRqVKlXi5s2bJCcnExkZSUpKCu+++y41a9YkLy+Pd999l0aNGpGXl8drr73G+fPnee+996hcubK9yzeMrKwsfv75Z86fP8+nn36Kn58f1atX5/r169SoUeOBx6FKlSqcO3eOlStX8vTTT3PlyhVeffVVqlevzptvvom7e/nMpRop05MmTeKzzz5j9erV+Pj4cPXqVa5evQrgdP8JB6hSpQp16tQp8LNhwwZ8fX0ZNGiQ0y0b9vHxYe7cubi7u1O3bl327NnDzJkzefXVV2nTpo29yzMMI2W6tBll3LaXB42xbm5ueg1FxCnocpMOYtWqVbz//vucP3+eZs2aMWvWLEJCQuxdlssYMWIECQkJXLp0idq1a9O2bVtee+01mjZtCtz6nuKcOXNYvXo16enptGnThgULFuDn52fnyo1l37599OrV657tzz//PB988EGxjkN2djbTp0/n888/Jzs7m44dO7Jw4UJ8fHzKc1cMk+nCrv4QFRXFlClTyrmashEeHu60l5sE2LlzJzNmzCA5ORkfHx9GjRrFX/7yF6ebJHF0Rsl0aTPSuG0PxRlj9RqKiDPQxIKIiIiIiIiI2Exro0RERERERETEZppYEBERERERERGbaWJBRERERERERGymiQURERERERERsZkmFkRERERERETEZppYEBERERERERGbaWJBDKV+/foFbq9du5bIyMhS6fvvf/87MTEx92xPTU0lODgYgO+//56//e1vwK1rex84cKBUnltE/l94eDjff/+99fadGSyu6dOnExQUxPTp0wtsv3DhAgMGDCAkJIT27dsTERFRZD9nz55lyJAhwK3MDxgw4KHqEHF1f/jDHwgNDSU4OJgBAwaQnp5eZPs732eLsnz5cgIDAxk1alShbe7MbGl+XhARcUUV7F2AiLMYMWLEA9u0atWKVq1aARAXF4enpyft27cv69JE5CGtXr2a5ORkKleuXGD7rFmzeOaZZxg7diwAR44cKbKfevXq8cknn5RZnSJGV7VqVeLi4gAYM2YMq1atYtKkSYW2v/N9tijR0dFs2LCBBg0alFapIiJSBK1YEJcxduxYNm/ebL19e3XDvn37+POf/8ywYcNo06YNb7zxBuvXr6dLly6YTCZOnDgBwOzZs1m8eDEASUlJhISE0K1bN1atWmXt8/ZfP1JTU/noo49YtmwZoaGhJCQkEBAQQG5uLgAZGRn4+/tbb4tIQampqbRr144xY8ZgMpkYMmQI165de6g+8vPzmT59OsHBwZhMJjZu3AiA2Wzm6tWrdO3a1brttnPnzvH4449bbz/99NNF9mXLagkRub/AwEDOnj0LwOHDhwkLC6NDhw6EhYVx/PhxoOAqg9mzZ/PSSy8RHh5OixYtWL58OQCvvvoqJ0+eZODAgSxdurTQvkREpPRoxYIYisViITQ01Ho7PT2dHj16PPBxR44c4eDBg9SqVYuWLVsyePBgdu3axQcffMCHH37InDlzCrR/8cUXmTdvHqGhofcspQZ48sknGT58OJ6enrz88ssAhIaGsnPnTnr27MnGjRvp3bs3FStWLOEeixjX8ePHWbx4MUFBQbz00ktER0db8zRq1CiqVKkCQG5uLu7u986Tb9myhZ9++om4uDguXbpknSz87LPPqF+/vvWvpHcaNWoUw4cPZ+XKlTzzzDMMGjSIevXqFdqXiJSOvLw89uzZw+DBgwFo3LgxO3bsoEKFCnz33XfMmDGDTz/99J7HHT9+nK1bt5KVlUXbtm0ZOXIk7777Lt988w1bt27Fy8uLjIyMYvUlIiK204oFMZTbSypv/0yZMqVYj2vdujV169alcuXKNGjQgC5dugDg5+fHqVOnCrS9cuUKGRkZ1gmM4n6nesiQIaxduxa49V3OQYMGFXe3RFySj48PQUFBAPTv35/ExETrfStXrrTmfP369fd9/P79++nXrx8eHh489thjmEwm/vWvfxX5nF27duWHH35g6NCh/PLLL3Ts2JGLFy/a1JeIPNjtPwj88Y9/JC0tjc6dOwO3VvYNHTqU4OBgpk6dys8//3zfx4eFhVG5cmW8vLyoU6cOFy5cuKdNcfsSERHbaWJBXEaFChW4efMmcGtZ8/Xr16333fk9a3d3d+ttd3d38vLyCvSTn59v0/MHBQVx6tQp4uLiyMvLw8/Pz6Z+RFyVm5vbQ7W3Nau1atUiIiKCFStW0Lp1a+Lj423uS0SKdvsPAj/99BO5ubmsXLkSgLfffpsOHTqQmJhITEwM2dnZ9338ne/fHh4e3Lhx4542xe1LRERsp4kFcRm+vr4kJSUBsH37dpvPb1CzZk2qV69u/evphg0b7tvO09OTzMzMAtvMZjMvvPCCViuIFMOZM2c4ePAgALGxsdbVC8VlMpnYtGkTeXl5XLx4kYSEBNq0aVPkY/bs2WM9l0NmZiYnTpzgiSeesKkvESm+GjVqMGfOHJYsWUJubi4ZGRnUq1cPgHXr1pWo79LsS0RE7k8TC+Iyhg4dSnx8PF26dOHw4cM8+uijNve1bNkyJk2aRLdu3azf875bjx492LZtm/XkjXBrOXd6ejr9+vWz+blFXEWTJk2IiYnBZDKRlpbGyJEjH+rxvXr1onnz5oSGhtK7d29mzJiBt7d3kY/54Ycf6Ny5MyaTibCwMAYPHkzr1q1t6ktEHk6LFi1o3rw5sbGxjB8/nhkzZtC9e/d7Vg4+rNLsS0RE7s8tPT1d6ztFysnmzZvZvn07K1assHcpIg4tNTUVs9lc4LwKIiIiIuKYdFUIkXISGRnJN998U+hXJ0RERERERJyRViyIiIiIiIiIiM10jgURERERERERsZkmFkRERERERETEZppYEBERERERERGbaWJBRERERERERGymiQURERERERERsZkmFkRERERERETEZv8He/iblwIn7PIAAAAASUVORK5CYII=\n",
      "text/plain": [
       "<Figure size 1080x576 with 7 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "### Data Visualizations\n",
    "\n",
    "plt.rcParams['figure.figsize'] = (15, 8)\n",
    "\n",
    "plt.subplot(2, 4, 1)\n",
    "sns.barplot(data['N'], data['label'])\n",
    "plt.ylabel(' ')\n",
    "plt.xlabel('Ratio of Nitrogen', fontsize = 10)\n",
    "plt.yticks(fontsize = 10)\n",
    "\n",
    "plt.subplot(2, 4, 2)\n",
    "sns.barplot(data['P'], data['label'])\n",
    "plt.ylabel(' ')\n",
    "plt.xlabel('Ratio of Phosphorous', fontsize = 10)\n",
    "plt.yticks(fontsize = 10)\n",
    "\n",
    "plt.subplot(2, 4, 3)\n",
    "sns.barplot(data['K'], data['label'])\n",
    "plt.ylabel(' ')\n",
    "plt.xlabel('Ratio of Potassium', fontsize = 10)\n",
    "plt.yticks(fontsize = 10)\n",
    "\n",
    "plt.subplot(2, 4, 4)\n",
    "sns.barplot(data['temperature'], data['label'])\n",
    "plt.ylabel(' ')\n",
    "plt.xlabel('Temperature', fontsize = 10)\n",
    "plt.yticks(fontsize = 10)\n",
    "\n",
    "plt.subplot(2, 4, 5)\n",
    "sns.barplot(data['humidity'], data['label'])\n",
    "plt.ylabel(' ')\n",
    "plt.xlabel('Humidity', fontsize = 10)\n",
    "plt.yticks(fontsize = 10)\n",
    "\n",
    "plt.subplot(2, 4, 6)\n",
    "sns.barplot(data['ph'], data['label'])\n",
    "plt.ylabel(' ')\n",
    "plt.xlabel('pH of Soil', fontsize = 10)\n",
    "plt.yticks(fontsize = 10)\n",
    "\n",
    "plt.subplot(2, 4, 7)\n",
    "sns.barplot(data['rainfall'], data['label'])\n",
    "plt.ylabel(' ')\n",
    "plt.xlabel('Rainfall', fontsize = 10)\n",
    "plt.yticks(fontsize = 10)\n",
    "\n",
    "plt.suptitle('Visualizing the Impact of Different Conditions on Crops', fontsize = 15)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Predictive Modelling"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Shape of x: (2200, 7)\n",
      "Shape of y: (2200,)\n"
     ]
    }
   ],
   "source": [
    "# lets split the Dataset for Predictive Modelling\n",
    "\n",
    "y = data['label']\n",
    "x = data.drop(['label'], axis = 1)\n",
    "\n",
    "print(\"Shape of x:\", x.shape)\n",
    "print(\"Shape of y:\", y.shape)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "The Shape of x train: (1760, 7)\n",
      "The Shape of x test: (440, 7)\n",
      "The Shape of y train: (1760,)\n",
      "The Shape of y test: (440,)\n"
     ]
    }
   ],
   "source": [
    "# lets create Training and Testing Sets for Validation of Results\n",
    "from sklearn.model_selection import train_test_split\n",
    "\n",
    "x_train, x_test, y_train, y_test = train_test_split(x, y, test_size = 0.2, random_state = 0)\n",
    "\n",
    "print(\"The Shape of x train:\", x_train.shape)\n",
    "print(\"The Shape of x test:\", x_test.shape)\n",
    "print(\"The Shape of y train:\", y_train.shape)\n",
    "print(\"The Shape of y test:\", y_test.shape)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [],
   "source": [
    "# lets create a Predictive Model\n",
    "\n",
    "from sklearn.linear_model import LogisticRegression\n",
    "\n",
    "model = LogisticRegression()\n",
    "model.fit(x_train, y_train)\n",
    "y_pred = model.predict(x_test)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 720x720 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "              precision    recall  f1-score   support\n",
      "\n",
      "       apple       1.00      1.00      1.00        18\n",
      "      banana       1.00      1.00      1.00        18\n",
      "   blackgram       0.86      0.82      0.84        22\n",
      "    chickpea       1.00      1.00      1.00        23\n",
      "     coconut       1.00      1.00      1.00        15\n",
      "      coffee       1.00      1.00      1.00        17\n",
      "      cotton       0.89      1.00      0.94        16\n",
      "      grapes       1.00      1.00      1.00        18\n",
      "        jute       0.84      1.00      0.91        21\n",
      " kidneybeans       1.00      1.00      1.00        20\n",
      "      lentil       0.94      0.94      0.94        17\n",
      "       maize       0.94      0.89      0.91        18\n",
      "       mango       1.00      1.00      1.00        21\n",
      "   mothbeans       0.88      0.92      0.90        25\n",
      "    mungbean       1.00      1.00      1.00        17\n",
      "   muskmelon       1.00      1.00      1.00        23\n",
      "      orange       1.00      1.00      1.00        23\n",
      "      papaya       1.00      0.95      0.98        21\n",
      "  pigeonpeas       1.00      1.00      1.00        22\n",
      " pomegranate       1.00      1.00      1.00        23\n",
      "        rice       1.00      0.84      0.91        25\n",
      "  watermelon       1.00      1.00      1.00        17\n",
      "\n",
      "    accuracy                           0.97       440\n",
      "   macro avg       0.97      0.97      0.97       440\n",
      "weighted avg       0.97      0.97      0.97       440\n",
      "\n"
     ]
    }
   ],
   "source": [
    "# lets evaluate the Model Performance\n",
    "from sklearn.metrics import classification_report, confusion_matrix\n",
    "\n",
    "# lets print the Confusion matrix first\n",
    "plt.rcParams['figure.figsize'] = (10, 10)\n",
    "cm = confusion_matrix(y_test, y_pred)\n",
    "sns.heatmap(cm, annot = True, cmap = 'Wistia')\n",
    "plt.title('Confusion Matrix for Logistic Regression', fontsize = 15)\n",
    "plt.show()\n",
    "\n",
    "# lets print the Classification Report also\n",
    "cr = classification_report(y_test, y_pred)\n",
    "print(cr)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Real time Predictions"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>N</th>\n",
       "      <th>P</th>\n",
       "      <th>K</th>\n",
       "      <th>temperature</th>\n",
       "      <th>humidity</th>\n",
       "      <th>ph</th>\n",
       "      <th>rainfall</th>\n",
       "      <th>label</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>90</td>\n",
       "      <td>42</td>\n",
       "      <td>43</td>\n",
       "      <td>20.879744</td>\n",
       "      <td>82.002744</td>\n",
       "      <td>6.502985</td>\n",
       "      <td>202.935536</td>\n",
       "      <td>rice</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>85</td>\n",
       "      <td>58</td>\n",
       "      <td>41</td>\n",
       "      <td>21.770462</td>\n",
       "      <td>80.319644</td>\n",
       "      <td>7.038096</td>\n",
       "      <td>226.655537</td>\n",
       "      <td>rice</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>60</td>\n",
       "      <td>55</td>\n",
       "      <td>44</td>\n",
       "      <td>23.004459</td>\n",
       "      <td>82.320763</td>\n",
       "      <td>7.840207</td>\n",
       "      <td>263.964248</td>\n",
       "      <td>rice</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>74</td>\n",
       "      <td>35</td>\n",
       "      <td>40</td>\n",
       "      <td>26.491096</td>\n",
       "      <td>80.158363</td>\n",
       "      <td>6.980401</td>\n",
       "      <td>242.864034</td>\n",
       "      <td>rice</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>78</td>\n",
       "      <td>42</td>\n",
       "      <td>42</td>\n",
       "      <td>20.130175</td>\n",
       "      <td>81.604873</td>\n",
       "      <td>7.628473</td>\n",
       "      <td>262.717340</td>\n",
       "      <td>rice</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    N   P   K  temperature   humidity        ph    rainfall label\n",
       "0  90  42  43    20.879744  82.002744  6.502985  202.935536  rice\n",
       "1  85  58  41    21.770462  80.319644  7.038096  226.655537  rice\n",
       "2  60  55  44    23.004459  82.320763  7.840207  263.964248  rice\n",
       "3  74  35  40    26.491096  80.158363  6.980401  242.864034  rice\n",
       "4  78  42  42    20.130175  81.604873  7.628473  262.717340  rice"
      ]
     },
     "execution_count": 23,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# lets chech the Head of the Dataset\n",
    "data.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "The Suggested Crop for Given Climatic Condition is : ['rice']\n"
     ]
    }
   ],
   "source": [
    "prediction = model.predict((np.array([[90,\n",
    "                                       40,\n",
    "                                       40,\n",
    "                                       20,\n",
    "                                       80,\n",
    "                                       7,\n",
    "                                       200]])))\n",
    "print(\"The Suggested Crop for Given Climatic Condition is :\", prediction)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>N</th>\n",
       "      <th>P</th>\n",
       "      <th>K</th>\n",
       "      <th>temperature</th>\n",
       "      <th>humidity</th>\n",
       "      <th>ph</th>\n",
       "      <th>rainfall</th>\n",
       "      <th>label</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>1600</th>\n",
       "      <td>22</td>\n",
       "      <td>30</td>\n",
       "      <td>12</td>\n",
       "      <td>15.781442</td>\n",
       "      <td>92.510777</td>\n",
       "      <td>6.354007</td>\n",
       "      <td>119.035002</td>\n",
       "      <td>orange</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1601</th>\n",
       "      <td>37</td>\n",
       "      <td>6</td>\n",
       "      <td>13</td>\n",
       "      <td>26.030973</td>\n",
       "      <td>91.508193</td>\n",
       "      <td>7.511755</td>\n",
       "      <td>101.284774</td>\n",
       "      <td>orange</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1602</th>\n",
       "      <td>27</td>\n",
       "      <td>13</td>\n",
       "      <td>6</td>\n",
       "      <td>13.360506</td>\n",
       "      <td>91.356082</td>\n",
       "      <td>7.335158</td>\n",
       "      <td>111.226688</td>\n",
       "      <td>orange</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1603</th>\n",
       "      <td>7</td>\n",
       "      <td>16</td>\n",
       "      <td>9</td>\n",
       "      <td>18.879577</td>\n",
       "      <td>92.043045</td>\n",
       "      <td>7.813917</td>\n",
       "      <td>114.665951</td>\n",
       "      <td>orange</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1604</th>\n",
       "      <td>20</td>\n",
       "      <td>7</td>\n",
       "      <td>9</td>\n",
       "      <td>29.477417</td>\n",
       "      <td>91.578029</td>\n",
       "      <td>7.129137</td>\n",
       "      <td>111.172750</td>\n",
       "      <td>orange</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "       N   P   K  temperature   humidity        ph    rainfall   label\n",
       "1600  22  30  12    15.781442  92.510777  6.354007  119.035002  orange\n",
       "1601  37   6  13    26.030973  91.508193  7.511755  101.284774  orange\n",
       "1602  27  13   6    13.360506  91.356082  7.335158  111.226688  orange\n",
       "1603   7  16   9    18.879577  92.043045  7.813917  114.665951  orange\n",
       "1604  20   7   9    29.477417  91.578029  7.129137  111.172750  orange"
      ]
     },
     "execution_count": 25,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# lets check the Model for Oranges also\n",
    "data[data['label'] == 'orange'].head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "The Suggested Crop for Given Climatic Condition is : ['orange']\n"
     ]
    }
   ],
   "source": [
    "# lets do some Real time Predictions\n",
    "prediction = model.predict((np.array([[20,\n",
    "                                       30,\n",
    "                                       10,\n",
    "                                       15,\n",
    "                                       90,\n",
    "                                       7.5,\n",
    "                                       100]])))\n",
    "print(\"The Suggested Crop for Given Climatic Condition is :\", prediction)"
   ]
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
   "version": "3.6.8"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 4
}
