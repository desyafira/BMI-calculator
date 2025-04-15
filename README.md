from PyQt5.QtCore import Qt
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QLabel, QPushButton, QLineEdit, QRadioButton, QButtonGroup
class MainWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()
    def initUI(self):
        self.setWindowTitle('CALCULATE YOUR BMI')
        self.setFixedSize(600, 550)

        self.layout = QVBoxLayout(self)
        #halamanpertama
        self.firstpage = QWidget()
        self.firstpage_layout = QVBoxLayout(self.firstpage)
        self.title = QLabel('make sure to measure your weight!')
        self.title.setStyleSheet('font-size: 18px; font-weight: bold')
        self.firstpage_layout.addWidget(self.title)
        self.button = QPushButton('start')
        self.button.clicked.connect(self.show_secondpage)
        self.firstpage_layout.addWidget(self.button)
        
        #halamankedua
        self.secondpage = QWidget()
        self.secondpage_layout = QVBoxLayout(self.secondpage)
        self.text1 = QLabel('The Body Mass Index (BMI) Calculator can be used to calculate BMI value\n and corresponding weight status while taking age into consideration.\n Use the "Metric Units" tab for the International System of Units or\n the "Other Units" tab to convert units into either US or metric units.\n Note that the calculator also computes the Ponderal Index in addition to BMI,\n both of which are discussed below in detail.')
        self.text2 = QLabel('For adults: < 16 = underweight.\n18.5 - 25 = normal.\n > 40 = overweight')
        self.secondpage_layout.addWidget(self.text1)
        self.secondpage_layout.addWidget(self.text2)
        self.secondpage.setLayout(self.secondpage_layout)
        self.weight_input = QLineEdit()
        self.weight_input.setPlaceholderText('Your weight(kg)')
        self.height_input = QLineEdit()
        self.height_input.setPlaceholderText('Your height(cm)')
        self.age_input= QLineEdit()
        self.age_input.setPlaceholderText('Your age')
        self.secondpage_layout.addWidget(self.weight_input)
        self.secondpage_layout.addWidget(self.height_input)
        self.secondpage_layout.addWidget(self.age_input)
        self.gender1 = QRadioButton('Male')
        self.gender2 = QRadioButton('Female')
        self.group = QButtonGroup(self)
        self.group.addButton(self.gender1)
        self.group.addButton(self.gender2)
        self.secondpage_layout.addWidget(self.gender1)
        self.secondpage_layout.addWidget(self.gender2)
        self.submit = QPushButton('calculate')
        self.submit.clicked.connect(self.calculate)
        self.secondpage_layout.addWidget(self.submit)

        #halamantiga
        self.thirdpage = QWidget()
        self.thirdpage_layout = QVBoxLayout(self.thirdpage)
        self.results = QLabel('Here is the results!')
        self.thirdpage_layout.addWidget(self.results)
        self.result = QLabel('')#angka
        self.result2 = QLabel('')#category
        self.thirdpage_layout.addWidget(self.result)
        self.thirdpage_layout.addWidget(self.result2)
        
    




        self.layout.addWidget(self.firstpage)
        self.layout.addWidget(self.secondpage)
        self.layout.addWidget(self.thirdpage)
        self.show_firstpage()
    def show_secondpage(self):
        self.firstpage.hide()
        self.secondpage.show()
        self.thirdpage.hide()
    def show_firstpage(self):
        self.firstpage.show()
        self.secondpage.hide()
        self.thirdpage.hide()
    def show_thirdpage(self):
        self.firstpage.hide()
        self.thirdpage.show()
        self.secondpage.hide()

    def calculate(self): 
        weight = float(self.weight_input.text())   
        height = float(self.height_input.text()) / 100
        if height > 0:
            bmi = weight / (height**2)
            category = self.get_bmi_category(bmi)
            self.result.setText(f"BMI: {bmi:.2f}")
            self.result2.setText(f"Category: {category}")
            self.show_thirdpage()
            return
    def get_bmi_category(self, bmi):
        if bmi < 18.5:
            return"Underweight"
        elif 18.5 <= bmi < 24.9:
            return"Normal"
        elif 25 <= bmi < 29.9:
            return"Overweight"
        else: 
            return "Obese"
        


app = QApplication([])
my_win = MainWindow()
my_win.show()
app.exec()

