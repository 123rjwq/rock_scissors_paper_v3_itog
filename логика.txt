package com.example.rom_kir_rock_scissors_paper_v2

import android.os.Bundle
import android.widget.Button
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import kotlin.random.Random

class MainActivity : AppCompatActivity() {

    // Список возможных выборов игрока
    private val elements = listOf("Камень", "Ножницы", "Бумага", "Ящерица", "Спок")
    // Переменная для хранения последней выбранной кнопки
    private var lastSelectedButton: Button? = null
    // Флаг, указывающий, активен ли текущий раунд
    private var isRoundActive = true
    // Переменные для хранения ссылок на кнопки игры и кнопку "Раунд"
    private lateinit var buttons: List<Button>
    private lateinit var buttonRound: Button

    private lateinit var buttonStartOver: Button

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Получаем ссылки на кнопки игры
        buttons = listOf(
            findViewById<Button>(R.id.button_stone),
            findViewById<Button>(R.id.button_scissors),
            findViewById<Button>(R.id.button_paper),
            findViewById<Button>(R.id.button_lizard),
            findViewById<Button>(R.id.button_spock)
        )

        // Получаем ссылку на кнопку "Раунд"
        buttonRound = findViewById<Button>(R.id.button_round)
        // Получаем ссылку на кнопку "Сначала"
        buttonStartOver = findViewById<Button>(R.id.button_start_over)

        // Устанавливаем обработчики нажатий для кнопок игры
        buttons.forEach { button ->
            button.setOnClickListener {
                if (isRoundActive) {
                    // Проверяем, была ли кнопка уже выбрана
                    if (lastSelectedButton == it) {
                        // Если кнопка уже была выбрана, возвращаем все в начальное состояние
                        resetGameState()
                    } else {
                        // Отключаем все кнопки, кроме выбранной
                        buttons.forEach { it.isEnabled = false }
                        it.isEnabled = true // Включаем выбранную кнопку
                        lastSelectedButton = it as Button // Сохраняем выбранную кнопку
                        buttonRound.isEnabled = true // Активируем кнопку "Раунд"
                    }
                } else {
                    // Включаем все кнопки, если раунд не активен
                    buttons.forEach { it.isEnabled = true }
                    isRoundActive = true
                }
            }
        }


        // Обработчик нажатия на кнопку "Раунд"
        buttonRound.setOnClickListener {
            if (isRoundActive) {
                //Toast.makeText(this, "Кнопка Раунд нажата", Toast.LENGTH_SHORT).show()
                // Выполняем логику игры
                val userChoice = lastSelectedButton?.text.toString()
                val pcChoice = elements.random()
                Toast.makeText(this, "Компьютер выбрал: $pcChoice", Toast.LENGTH_SHORT).show()
                val result = checkWinner(userChoice, pcChoice)
                Toast.makeText(this, result, Toast.LENGTH_LONG).show()
                // Деактивируем все кнопки после игры
                buttons.forEach { it.isEnabled = false }
                // Изменяем название кнопки "Раунд" на "Ещё раз"
                buttonRound.text = "Ещё раз"
                buttonStartOver.isEnabled = true
            }
        }

        // Обработчик нажатия на кнопку "Сначала"
        val buttonStartOver = findViewById<Button>(R.id.button_start_over)
        buttonStartOver.setOnClickListener {
            // Включаем все кнопки и сбрасываем состояние игры
            buttons.forEach { it.isEnabled = true }
            buttonRound.isEnabled = false // Деактивируем кнопку "Раунд"
            buttonStartOver.isEnabled = false // Деактивируем кнопку "Сначала"
            buttonRound.text = "Раунд" // Возвращаем кнопку "Раунд" к исходному названию
            lastSelectedButton = null // Сбрасываем выбор пользователя
            isRoundActive = true // Восстанавливаем активность раунда
        }

        // Деактивируем кнопки "Раунд" и "Сначала" в начале
        buttonRound.isEnabled = false
        buttonStartOver.isEnabled = false
    }

    // Функция для определения победителя
    private fun checkWinner(userChoice: String, pcChoice: String): String {
        return when {
            userChoice == pcChoice -> "Ничья!"
            (userChoice == "Камень" && pcChoice == "Ножницы") ||
                    (userChoice == "Ножницы" && pcChoice == "Бумага") ||
                    (userChoice == "Бумага" && pcChoice == "Камень") ||
                    (userChoice == "Камень" && pcChoice == "Ящерица") ||
                    (userChoice == "Ящерица" && pcChoice == "Спок") ||
                    (userChoice == "Спок" && pcChoice == "Ножницы") ||
                    (userChoice == "Ножницы" && pcChoice == "Ящерица") ||
                    (userChoice == "Ящерица" && pcChoice == "Бумага") ||
                    (userChoice == "Бумага" && pcChoice == "Спок") ||
                    (userChoice == "Спок" && pcChoice == "Камень") -> "Победа пользователя!"
            else -> "Победа компьютера!"
        }
    }

    // Функция для сброса состояния игры
    private fun resetGameState() {
        // Включаем все кнопки
        buttons.forEach { it.isEnabled = true }
        // Сбрасываем выбор пользователя
        lastSelectedButton = null
        // Деактивируем кнопку "Раунд"
        buttonRound.isEnabled = false
        // Возвращаем кнопку "Раунд" к исходному названию
        buttonRound.text = "Раунд"
        // Восстанавливаем активность раунда
        isRoundActive = true
    }
}
