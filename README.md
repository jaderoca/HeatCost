(I moved development out of my "Rye" repository because the whole system looked like it might have actual uses beyond tinkering.)

Another almost complete rewrite! (Tested on Linux (Ubuntu 20.04) with Rye version 0.0.99)

First, I renamed my base project to `heat-cost.rye`. No costing yet, but it's coming.

Then I did a bunch of stuff that started as just tinkering with possibilities and I ended up liking the code management.
* Split the original `weather.rye` in two: `get-station-list.rye` and `get-station-data.rye`. `get-station-data.rye` loads and runs `get-station-list.rye` as a module. Then `heat-cost.rye` loads and runs `get-station-data.rye` as a module, but only on demand. Note that the 'get-station` files can be run completely independently, without including them in other projects.
* Split out a utility function for putting gaps between user-input requests into `utilities.rye`. Loaded as a context just about everywhere. Other utility functions will be added as I figure them out.
* Created `chart.rye` and `html.rye` as loadable contexts to help with my charting and HTML stuff. As companion pieces, I've got a few "template" files to hold the main HTML structures I need. (`chart-base.txt` and `chart-spec.txt`. I created the charting stuff because I had charting needs not provided for in Rye's `echart` context. To that end, I use (Apache's e-chart)[https://echarts.apache.org/handbook/]. Specifically, I used their tools to extract and download just the charting components I require (`echarts.bar.min.js`, included here.

I used all of that to generate a web page containing a chart and scrolling table to show the results of analysis. You can choose between 3 different analyses:
* Grouped by Year (Annual Totals)
* Grouped by Month (multi-year totals for each month)
* Year-Month (monthly totals for a single year).

At present, everything happens at the command-line with nothing resembling anything recognizable as an actual user interface. To run the program, you need the appropriate version of Rye install (0.0.99 at the time of writing). Follow Rye documentation for how to run this (these) programs from the command line.

The basic flow is:
* Choose a file to analyze, creating new files as you see fit.
    * If creating a new file, `get-station-data.rye` will be executed to do that
    * You will create/refresh the Station List
    * Create/refresh the Station Data
    * CSV files will be created for use by the main program (`heat-cost.rye`), but you have the option of also generating XLSX files for use with Excel or other spreadsheet programs. Note that most spreadsheet and database programs can use CSV files directly.
* Having chosen a file you will be asked what kind of grouping you want.
    * If you choose Year-Month, you will also be asked to pick a year
* That's it! It will generate an HTML file based on the file name being processed and attempt to open it in your browser. If it fails to open, just browse to the displayed file storage location and open it manually. You can copy the HTML file to a web server if you want, but that's out of scope for this project.

Yet to come:
* Migrate and test with whatever version of Rye is available the next time I have time to work on it.
* Add the costing. Just total days is of little value for decision making.
* More error avoidance and maybe even some actual error handling
* Try out some of the UI stuff in Rye:
    * There is a way to use (Fyne)[https://fyne.io/] for a true Graphical User Interface and the developer is working other GUI libraries.
    * Some Terminal User Interface features have been added to more recent versions of Rye
* I think it is still highly experimental, but there are ways to compile to standalone executables and, I think, APK (Android) files.


Stay tuned!
