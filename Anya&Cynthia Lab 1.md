## Lab 1

Cynthia Chu, Anya Sherman


```python
  from aguaclara.core.units import unit_registry as u
  import numpy as np
  import matplotlib.pyplot as plt
  import pandas as pd
  from scipy import stats
  #The data file path is the raw data url on github. Happily python can read directly from a web page.
  data_file_path = "https://raw.githubusercontent.com/cc2394/CEE4530/master/lab1.txt"



  #Now we create a pandas dataframe with the data in the file
  df = pd.read_csv(data_file_path,delimiter='\t')
  #if you want to see what is in the dataframe you can print it!
  print(df)
  # The column headers can be access by using the list command
  list(df)
  columns = df.columns
  print(columns)
  #Below are three equally fine methods of extracting a column of data from the pandas dataframe.

  # 1) We can select a column by using the column header. Here we use the column header by selecting one array element from the list command.
  x = df[list(df)[0]].values * u.mg/u.L
  x
  # 2) We can use the loc command to select all of the rows (: command) and the column with the label given by list(df)[0].
  x = df.loc[:, list(df)[0]].values * u.mg/u.L
  x
  # 3) We can use the iloc command and select all of the rows in column 0.
  x = df.iloc[:,0].values * u.mg/u.L
  x
  #The iloc method is simple and efficient, so I'll use that to get the y values.
  y = df.iloc[:,1].values * u.mg/u.L

  # We will use the stats package to do the linear regression.
  # It is important to note that the units are stripped from the x and y arrays when processed by the stats package.
  slope, intercept, r_value, p_value, std_err = stats.linregress(x,y)

  #We can add the units to intercept by giving it the same units as the y values.
  intercept = intercept * y.units

  # Note that slope is dimensionless for this case, but not in general!
  # For the general case we can attach the correct units to slope.
  slope = slope * y.units/x.units


  # Now create a figure and plot the data and the line from the linear regression.
  fig, ax = plt.subplots()
  # plot the data as red circles
  ax.plot(x, y, 'ro', )

  #plot the linear regression as a black line
  ax.plot(x, slope * x + intercept, 'k-', )

  # Add axis labels using the column labels from the dataframe
  ax.set(xlabel=list(df)[0])
  ax.set(ylabel=list(df)[1])
  ax.legend(['Measured', 'Linear regression'])
  ax.grid(True)
  # Here I save the file to my local harddrive. You will need to change this to work on your computer.
  # We don't need the file type (png) here.
  plt.savefig('lab1graph')
  plt.show()
  print(intercept)
  print(slope)
```

![linear](https://github.com/cc2394/CEE4530/blob/master/lab1graph.png)

Figure 1: Concentration vs Absorbance of a Red Dye Sample

$$ y=1.045 \times x + 0.6818 $$
Questions:
3. What is the value of the extinction coefficient, $\epsilon$?
    $\qquad \epsilon=144 \\$
4. Did you use interpolation or extrapolation to get the concentration of the unknown?$\\ \qquad $ We didn't have time to take an unknown reading, but in theory, we would have used interpolation to find the concentration. $\\$
5. What measurement controls the accuracy of the density measurement for the NaCl solution?
$\qquad$ The volumetric flask measurement controls the accuracy of the density for the NaCl solution. The only tools used were a volumetric flask and a mass balance, and the mass balance is more accurate than the volumetric flask. The 100mL measurement controls the accuracy of the density of NaCl. $\\$
6. What density did you expect (see prelab 2)?
$\qquad 1095.2 \ g/L$
$\\$
7. Approximately what should the accuracy be for the density measurement?$\\$
$\qquad $ The volumetric flask has an accuracy of +/- .08mL, so the accuracy for the density measurement would be .08%.$\\$
8. Conclusions and suggestions
$\qquad $ The goal of Lab 1 was to learn about different laboratory measurements and procedures. We prepared a solution of NaCl using a mass balance and a volumetric flask. Additionally, we prepared dilutions of a red dye standard using pipettes and volumetric flasks. We then used a photometer to measure the absorbance of the red dye dilutions. We analyzed our photometry data using ProCoDa, and learned how to calibrate and use different sensors in ProCoDa. Using the data we collected, we created a linear regression relating absorbance and concentration. Two points, the 100mg/L and 200mg/L concentrations, were outside the detection limits of our sensor, so we excluded them from our linear regression. We had problems using the photometer, and we were getting a lot of air bubbles while using the syringe. We changed the angle of the tubing we used, and got better photometer reading. Overall, our data was relatively linear and we did not have many outliers.
