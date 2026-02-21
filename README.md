Tested on Linux (Ubuntu 20.04) with [Rye](https://github.com/refaktor/rye) version 0.1.02

Costing now implemented! I still need to sort out column widths on the table.

You can choose between 3 different analyses:
* Grouped by Year (Annual Totals)
* Grouped by Month (multi-year totals for each month)
* Year-Month (monthly totals for a single year).

At present, everything happens at the command-line with nothing resembling anything recognizable as an actual user interface. To run the program, you need the appropriate version of Rye installed. Follow Rye documentation for how to run this (these) programs from the command line.

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
* More error avoidance and maybe even some actual error handling
* Try out some of the UI stuff in Rye:
    * There is a way to use (Fyne)[https://fyne.io/] for a true Graphical User Interface and the developer is working other GUI libraries.
    * Some Terminal User Interface features have been added to more recent versions of Rye
* I think it is still highly experimental, but there are ways to compile to standalone executables and, I think, APK (Android) files.


Stay tuned!
