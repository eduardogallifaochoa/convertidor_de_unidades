import sys
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QLineEdit, QLabel, QPushButton, QComboBox, QCheckBox, QMessageBox
from PyQt5.QtGui import QFont, QColor
from PyQt5.QtCore import Qt

class UnitConverter(QWidget):
    def __init__(self):
        super().__init__()

        self.initUI()

    def initUI(self):
        self.setWindowTitle('Convertidor de Unidades')
        self.setGeometry(100, 100, 400, 300)

        # Layout principal
        layout = QVBoxLayout()

        # Input de cantidad
        self.input_field = QLineEdit(self)
        self.input_field.setFont(QFont("Arial", 14))
        self.input_field.setStyleSheet("color: black;")
        layout.addWidget(self.input_field)

        # ComboBox de unidad de entrada
        self.input_unit = QComboBox(self)
        self.input_unit.setFont(QFont("Arial", 14))
        self.add_input_units()
        self.input_unit.currentIndexChanged.connect(self.update_output_units)
        layout.addWidget(self.input_unit)

        # ComboBox de unidad de salida
        self.output_unit = QComboBox(self)
        self.output_unit.setFont(QFont("Arial", 14))
        layout.addWidget(self.output_unit)

        # Botón para realizar la conversión
        convert_button = QPushButton('Convertir', self)
        convert_button.setFont(QFont("Arial", 14))
        convert_button.clicked.connect(self.convert_units)
        layout.addWidget(convert_button)

        # Label para mostrar el resultado
        self.result_label = QLabel(self)
        self.result_label.setFont(QFont("Arial", 14))
        layout.addWidget(self.result_label)

        # Opción para activar/desactivar el modo oscuro
        self.dark_mode_checkbox = QCheckBox("Modo Oscuro", self)
        self.dark_mode_checkbox.setFont(QFont("Arial", 14))
        self.dark_mode_checkbox.setChecked(True)  # Modo oscuro activado por defecto
        self.dark_mode_checkbox.stateChanged.connect(self.toggle_dark_mode)
        layout.addWidget(self.dark_mode_checkbox)

        self.setLayout(layout)
        self.toggle_dark_mode(Qt.Checked)  # Aplicar modo oscuro al iniciar
        self.update_output_units()  # Actualizar unidades de salida al iniciar

    def add_input_units(self):
        # Añadir unidades de entrada con divisiones
        self.input_unit.addItem("Kilogramos")
        self.input_unit.addItem("Gramos")
        self.input_unit.addItem("Libras")
        self.input_unit.insertSeparator(self.input_unit.count())
        
        self.input_unit.addItem("Kilómetros")
        self.input_unit.addItem("Millas")
        self.input_unit.addItem("Metros")
        self.input_unit.addItem("Centímetros")
        self.input_unit.insertSeparator(self.input_unit.count())

        self.input_unit.addItem("Litros")
        self.input_unit.addItem("Mililitros")
        self.input_unit.insertSeparator(self.input_unit.count())

        self.input_unit.addItem("Celsius")
        self.input_unit.addItem("Fahrenheit")

    def update_output_units(self):
        input_unit = self.input_unit.currentText()
        self.output_unit.clear()

        conversions = {
            "Kilogramos": ["Gramos", "Libras"],
            "Gramos": ["Kilogramos", "Libras"],
            "Libras": ["Kilogramos", "Gramos"],
            "Kilómetros": ["Millas", "Metros"],
            "Millas": ["Kilómetros", "Metros"],
            "Metros": ["Kilómetros", "Centímetros"],
            "Centímetros": ["Metros"],
            "Litros": ["Mililitros"],
            "Mililitros": ["Litros"],
            "Celsius": ["Fahrenheit"],
            "Fahrenheit": ["Celsius"],
        }

        if input_unit in conversions:
            self.output_unit.addItems(conversions[input_unit])

    def convert_units(self):
        try:
            input_value = float(self.input_field.text())
        except ValueError:
            QMessageBox.warning(self, "Error", "Por favor, ingresa un valor numérico válido.")
            return

        input_unit = self.input_unit.currentText()
        output_unit = self.output_unit.currentText()
        result = self.perform_conversion(input_value, input_unit, output_unit)
        self.result_label.setText(f'{input_value} {input_unit} = {result} {output_unit}')

    def perform_conversion(self, value, input_unit, output_unit):
        conversions = {
            "Kilogramos": {"Gramos": 1000, "Libras": 2.20462},
            "Gramos": {"Kilogramos": 0.001, "Libras": 0.00220462},
            "Libras": {"Kilogramos": 0.453592, "Gramos": 453.592},
            "Kilómetros": {"Millas": 0.621371, "Metros": 1000},
            "Millas": {"Kilómetros": 1.60934, "Metros": 1609.34},
            "Metros": {"Kilómetros": 0.001, "Centímetros": 100},
            "Centímetros": {"Metros": 0.01},
            "Litros": {"Mililitros": 1000},
            "Mililitros": {"Litros": 0.001},
            "Celsius": {"Fahrenheit": lambda c: (c * 9/5) + 32},
            "Fahrenheit": {"Celsius": lambda f: (f - 32) * 5/9},
        }

        if input_unit == output_unit:
            return value

        conversion = conversions.get(input_unit, {}).get(output_unit, None)
        if conversion:
            return conversion(value) if callable(conversion) else value * conversion

        return "Conversión no disponible"

    def toggle_dark_mode(self, state):
        if state == Qt.Checked:
            self.setStyleSheet("background-color: #2e2e2e; color: white;")
            self.input_field.setStyleSheet("background-color: white; color: black;")
        else:
            self.setStyleSheet("background-color: white; color: black;")
            self.input_field.setStyleSheet("color: black;")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    converter = UnitConverter()
    converter.show()
    sys.exit(app.exec_())
