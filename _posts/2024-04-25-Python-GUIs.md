# Building GUI's in Python

Over the last few years I have been tasked with creating GUI's for various projects. When working on university projects, the sad reality is that the GUI you design is a *one-time use GUI*. The courses never really ever seem to place an emphasis on the user experience and rather focus on the achieving a set of functionality within the GUI.

This means that it is not always feasible to spend hours on UI/UX design for a GUI which it is not relevant for.

## One Solution - PyQT

A lot of people seem to be conflicted with this challenge and as such new technologies and applications are constantly being created to streamline UI/UX design for prototyping. Personally, I am really fond of the *PyQt Designer* tool. This tool is a drag-drop interactive UI design tool, which is compatible with the well known QT framework. It allows you to swiftly mock up and develop functional and nice looking GUIs.

![QT Designer Example](/images/QT designer example.PNG "Using QT Designer to Develop a GUI for ELEC4630")

In this example, I have used the QT designer to develop a GUI for an ELEC4630 assignment. After you settle on a design for your user interface, you can import your UI file into a Python file and map the relevant functionality to all of your components. In the code below, you can see I first instantiate a `MainWindow` class and load the UI file for the window. After this, I link all of the UI components that I added to the designer to class variables so I can access them in all of the class methods. Note, that this linking is done by using the component type and name. Lastly, after linking the components, I linked some functions to the button click triggers so that the performs certain tasks when different buttons are clicked.

```python
class MainWindow(QMainWindow):
    def __init__(self):
        super(MainWindow, self).__init__()

        # Load in the UI File
        loadUi(os.path.abspath("assignment2/fingerprint_gui.ui"), self)

        # Initialise UI Elements
        self.enrollNameT = self.findChild(QTextEdit, "addName_T")
        self.enrollPathT = self.findChild(QTextEdit, "enrolPath_T")
        self.selectFingerprintB = self.findChild(QPushButton, "enrollSelectPath_B")
        self.enrollFingerprintB = self.findChild(QPushButton, "enroll_B")
        self.enrollPreviewL = self.findChild(QLabel, "previewLabel_L")
        self.enrollTemplatePreviewL = self.findChild(QLabel, "templatePreview_L")
        self.comparePathT = self.findChild(QTextEdit, "comparePath_T")
        self.compareFingerprintB = self.findChild(QPushButton, "compare_B")
        self.compareTemplatePreviewL = self.findChild(QLabel, "compareTemplatePreview_L")
        self.matchPreviewL = self.findChild(QLabel, "matchPreview_L")
        self.batchEnrollFingerprintB = self.findChild(QPushButton, "batchEnroll_B")
        self.matchScoreT = self.findChild(QTextEdit, "matchScore_T")
        self.falsePositiveRateT = self.findChild(QTextEdit, "fpr_T")

        # Connect Buttons
        self.compareFingerprintB.clicked.connect(self.select_compare_fingerprint)
        self.enrollFingerprintB.clicked.connect(self.enrollFingerprint)
        self.selectFingerprintB.clicked.connect(self.select_enroll_fingerprint)
        self.batchEnrollFingerprintB.clicked.connect(self.batch_enroll)
```

{% include info.html text="Make sure you give all of your components names, this can be done by double clicking on a component in the Object Inspector in the top left of the screen. You use the names of the components to link the elements within the UI file to your application code." %}

The beauty of this approach is that it that the UI file is dynamically loaded into the Python script, this means you can modify your GUI layout and add additional components without worrying about breaking existing functionality.

## Other Solutions

The other solution for building elegant GUIs in Python, is to use a tool called [Streamlit](https://streamlit.io/). I personally have not used this tool but have heard a lot about it and from my peers & colleagues. This tool is designed for building web applications, this means that it is particularly useful for building dashboards.

I hope that this blog gave you some insight on some of the techniques you can use to construct GUIs in python quickly & efficiently.
