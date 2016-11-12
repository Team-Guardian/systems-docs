Qt4 is used for the vision system ground station GUI.

To update the GUI:

* Use Qt Designer to visually update UI elements
* Use pyuic4 to regenerate the Python file from the updated .ui file:
 * sudo apt-get install pyqt4-dev-tools
 * pyuic4 -o edytor.py edytor.ui