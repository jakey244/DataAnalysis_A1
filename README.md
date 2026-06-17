# Assigment 1 write up

Jakob Domenig, Data Analysis in Physical Oceanography, Summer 2026.

The figures can be found in the directory /figures/

## The MOCHA dataset 

The time series that is the focus of this assignment ist the Meridional Heat Transport in PW from the MOCHA dataset. 
The Mocha dataset contains data of the meridional heat transport at 26.5 °N (positive is northward) from April 2004 to December 2020.
I chose the whole time series as this should produce the most accurate spectrum.

In my first uploaded figure I used the Meridional Overturning Circulation data from MOCHA, which I was not sure was the same as the forbidden one (from RAPID as well 26.5 °N)
and so I chose a different variable. 

## Series characterization

The record length is 12-hourly data from April 2004 to December 2020 which results in a time series of 12202 data points.
The data appears to be already gap-filled, the documentation states "missing: 0.0%". The dataset was furthermore convolved with a 10-day lowpass filter.

## PART A

Mean: 1.19 PW 
Standard deviation: 0.38 PW (ddof = 0, used because I used the whole dataset to calculate the standard deviation)
Range: 3.16 PW

In the figures directory is a file MHT_histogram.pdf which shows a histogram of the distribution.

The figure lowpassed_timeseries.pdf shows the timeseries as well as a filtered timeseries with a 90-day lowpass filter.
The 90-day window was chosen to preserve the seasonal cycle but remove all high frequency components.
It can be observed that the minimal heat transport (or even negative) mostly occurs in northern hemispheric winter, but not for every year. This points to the fact that there is more variability involved.

## PART B

The figure mocha_PSD_MHT.pdf shows the power spectral density of the time series calculated for the original MOCHA data and the lowpass-filtered data using Welch's method.
The figure shows the Nyquist frequency, the smallest resolved frequency, 1 cpd. 
However, the original MOCHA data has already been fitered using a 10-day-lowpass. This leads to the spectral densisty dropping sharply around the 0.1 cpd frequency. 
The figure furthermore includes a Chi^2 confidence interval between the 0.025 and 0.975 percentiles for both the filtered and original spectra.
The degrees of freedom for the chosen segments (see below) are 20.

Both spectra show a peak at the 1 year period, corresponding to the yearly cycle. Furthermore there is a peak at 1/(6 month). This matches the observations from the timeseries plot. 
All higher frequency signals are attenuated in the filtered spectrum. Even though it seems to me that a lot of higher frequency variability is still visible, for example there is a peak at around 50 cpd which (even though attenuated by an order of magnitudecompared to the original dataset) is easily identifiable as a peak.
However in that sense, the filter still passes what I am interested in (yearly cycle as well as seasonal variability) and suppresses all higher frequency signals.

The spectra are overall red, excluding the very low frequency signals which appear blue and the frequencies 1/6month to 1/month which appear to be white. Especially the very low frequency signals (periods of 1 year and longer) should be disregarded because the frequency sampling for those periods is very coarse. 

Figure mocha_PSD_MHT_compared_to_boxcar.pdf shows the filtered spectrum as well as the same spectrum but calculated with a simple boxcar window instead of the Hann window used before. It is clearly visible that the spectra look similar, preserving the 1/year and 1/(6 month) peaks but the higher frequency variability attenuated by the filter is not suppressed as much when using the boxcar.

For Welch's method, the function was built from the Periodogram and then averaging segments. The Periodogram handles the detrending for each segment using a linear function.The chosen segment length was 3 years with a Hann filter applied and 50% overlap between the segments. In the case that the segment choice did not match up with the length of the dataset, two methods were built in the Welch function. 
Either padding the last incomplete segments with zeroes or truncating the dataset such that it matches the segment choice. For the figures, the zero-padding was used.

For the Periodogram function, the integral over the spectrum is equal to the variance up to an error of 0.43%.




## ABOUT THE TESTS

Not all the tests that are included in the template-assignment pass but the required ones (Parseval satisfied and correct mean calculation) pass.


## FURTHER INFORMATION

The plots were created in the two notebooks spectrum.ipynb and timeseries.ipynb, roughly corresponding to parts B and A of the assignment respectively.
