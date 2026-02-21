Tested on Linux (Ubuntu 20.04) with [Rye](https://github.com/refaktor/rye) version 0.1.02

This project duplicates the work I did manually and with the help of spreadsheets to help me figure out whether it was a good idea to install a heat pump to do the easy work, leaving our pellet stove to do the hard work. I determined that it was a good idea and performance actually exceeded my expectations.

All data comes from publicly available [Environment Canada weather records](https://collaboration.cmc.ec.gc.ca/cmc/climate/Get_More_Data_Plus_de_donnees/). I used the English instructions in the file `Command_Lines_EN.txt` as the foundation of the data retrieval code. There is a French version of that file in the same location.

There are 3 principle program files:
* `get-station-list.rye`: retrieves the list of weather stations from Environment Canada. This can be run on it's own if all you want is the station list.
* `get-station-data.rye`: retrieves the daily weather data for the selected station. This can be run on it's own if all you want is the weather data. If there is no station list when you run it, it will automatically call `get-station-list.rye`.
* `heat-cost.rye`: the main program. Running this will, if necessary, call `get-station-data.rye` to get you started. You also have the option to create a new station-data file if you don't want to use an existing one.

There are a few support files:
* `heat-cost.cfg`: set up your various heat source parameters. The one included is the one that reflects my situation. If you end up breaking this file, just delete it and `heat-cost.rye` with generate the sample one from scratch. This file must contain exactly 4 lines. I have an idea for how to handle more, but that's a constraint for now.
    * The first line is the header row and must be left untouched
    * There needs to be one line to represent the "no heat required" condition. This will have a 0.00 cost and the threshold temperature would be the average daily temperature where you switch from needing heat to not needing heat. All temperatures are in Celsius
    * There needs to be one line to represent the primary heat source, which can be used regardless of outdoor temperature. It needs a non-zero cost and should have the lowest threshold temperature in the file so that the program can identify it as the default.
    * There needs to be one line to represent the alternate heat source, which will be used between it's threshold temperature and the "no heat" temperature. It should also have a non-zero cost, presumably a lower cost than that of the primary heat source.
    * Other than the header row, the lines can be in any order. I sort them by threshold temperature to determine which is primary, alternate, or "no heat."
* `chart-base.txt`: contains text representing an HTML foundation for later display of a chart.
* `chart-spec.txt`: contains text representing the chart section of the final HTML file
* `chart.rye`: contains some helper code that `heat-cost.rye` will use to put the previous two files together with the data in order to generate an HTML file containing the chart and a table of the relevant data.
* `echarts.bar.min.js`: the is a subset of [Apache e-charts](https://echarts.apache.org/en/index.html). I chose to use this instead of Rye's built-in charting, because I wanted to be able to stack bars and run them horizontally. As I was learning how to use this, I found that there was a way to download just the chart types needed, which dramatically reduces the size on disc.
* `utilities.rye`: this started as an experiment in creating and using my own library code. It only has one utility in it so far (a `gap` function to manage the vertical spacing between successive requests for user input), but figuring it out led to my ability to break out `get-station-list.rye` and `get-station-data.rye` as sub-programs that can be used independently or from within `heat-cost.rye`. It might sound silly, but figuring this out gave me great satisfaction, not just for the mental challenge, but for the ability to do more modular programming.
* `html.rye`: not currently in use. Maybe I'll never use it for this project, but I'm leaving it here in case I (or you) find a use for it or its concepts.

You can choose between 3 different analyses:
* Grouped by Year (Annual Totals)
* Grouped by Month (multi-year totals for each month)
* Year-Month (monthly totals for a single year).

At present, everything happens at the command-line with nothing that would normally be considered an actual user interface. To run the program, you need the appropriate version of Rye installed. Once installed and with the `rye` executable in the same directory, navigate to that directory and run the command:
`./rye heat-cost.rye` (or `./rye get-station-list.rye` or `./rye get-station-data.rye` if you like).

The basic flow is:
* Choose a file to analyze, creating new files as you see fit.
    * If creating a new file, `get-station-data.rye` will be executed to do that
    * You will create/refresh the Station List
    * Create/refresh the Station Data
    * CSV files will be created for use by the main program (`heat-cost.rye`), but you have the option of also generating XLSX files for use with Excel or other spreadsheet programs. Note that most spreadsheet and database programs can use CSV files directly.
* Having chosen a file you will be asked what kind of grouping you want.
    * If you choose Year, the chart and associated table will show annual totals
    * If you choose Month, you will also be asked to choose whether to show the totals for each month across all years or each month for a given year.
* That's it! It will generate an HTML file based on the file name being processed and attempt to open it in your browser. If it fails to open, just browse to the displayed file storage location and open it manually. You can copy the HTML file to a web server if you want, but that's out of scope for this project.

Yet to come:
* Migrate and test with whatever version of Rye is available whenever I work on it.
* There are some things that I think could be done better, so I'll probably pick away at them over time.
* More error avoidance and maybe even some actual error handling
* Try out some of the UI stuff in Rye:
    * There is a way to use (Fyne)[https://fyne.io/] for a true Graphical User Interface and the developer is working other GUI libraries.
    * Some Terminal User Interface features have been added to more recent versions of Rye
* I think it is still highly experimental, but there are ways to compile to standalone executable and, I think, APK (Android) files.


Stay tuned!
