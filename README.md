package com.example.yourapp

import android.content.Intent
import android.os.Bundle
import android.view.View
import android.widget.Button
import android.widget.EditText
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity

class PinInputActivity : AppCompatActivity() {

    private lateinit var editTextPin: EditText
    private var enteredPin: String? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_pin_input)

        editTextPin = findViewById(R.id.editTextPin)

        // Восстанавливаем введенные цифры, если они есть
        if (savedInstanceState != null) {
            enteredPin = savedInstanceState.getString("enteredPin")
            editTextPin.setText(enteredPin)
        }

        val buttons = arrayOf(
            findViewById<Button>(R.id.button0),
            findViewById<Button>(R.id.button1),
            findViewById<Button>(R.id.button2),
            findViewById<Button>(R.id.button3),
            findViewById<Button>(R.id.button4),
            findViewById<Button>(R.id.button5),
            findViewById<Button>(R.id.button6),
            findViewById<Button>(R.id.button7),
            findViewById<Button>(R.id.button8),
            findViewById<Button>(R.id.button9)
        )

        buttons.forEach { button ->
            button.setOnClickListener { onNumberClick(button.text.toString()) }
        }

        findViewById<Button>(R.id.buttonClear).setOnClickListener { clearLastCharacter() }
        findViewById<Button>(R.id.buttonOk).setOnClickListener { validatePin() }
    }

    private fun onNumberClick(number: String) {
        val currentText = editTextPin.text.toString()
        if (currentText.length < 4) {
            editTextPin.setText(currentText + number)
            editTextPin.setSelection(editTextPin.text.length)
            enteredPin = editTextPin.text.toString()  // Сохраняем введенные цифры
        }
    }

    private fun clearLastCharacter() {
        val currentText = editTextPin.text.toString()
        if (currentText.isNotEmpty()) {
            editTextPin.setText(currentText.substring(0, currentText.length - 1))
            editTextPin.setSelection(editTextPin.text.length)
            enteredPin = editTextPin.text.toString()  // Сохраняем введенные цифры
        }
    }

    private fun validatePin() {
        val pin = editTextPin.text.toString()
        if (pin == "1567") {
            // Открываем другую активность при правильном вводе кода
            val intent = Intent(this, NextActivity::class.java)
            startActivity(intent)
        } else {
            Toast.makeText(this, "Неверный код!", Toast.LENGTH_SHORT).show()
        }
    }

    override fun onSaveInstanceState(outState: Bundle) {
        super.onSaveInstanceState(outState)
        // Сохраняем введенные цифры при повороте экрана
        outState.putString("enteredPin", editTextPin.text.toString())
    }

    override fun onRestoreInstanceState(savedInstanceState: Bundle) {
        super.onRestoreInstanceState(savedInstanceState)
        // Восстанавливаем введенные цифры
        enteredPin = savedInstanceState.getString("enteredPin")
        editTextPin.setText(enteredPin)
    }
}
package com.example.yourapp

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity

class NextActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_next) // Создайте соответствующий layout
    }
}
<activity android:name=".NextActivity" />
