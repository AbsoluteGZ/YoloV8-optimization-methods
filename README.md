# Исследование оптимизации модели YOLOv8 для мобильных устройств

Этот репозиторий содержит результаты исследования по оптимизации модели YOLOv8 для повышения производительности на мобильных устройствах.  Основная метрика оценки – время инференса на тестовом наборе изображений.  Были исследованы и сравнены следующие методы оптимизации:

* **Baseline:** Базовая модель YOLOv8 без оптимизации, служащая контрольной группой.
* **FP16:**  Использование полуточной арифметики (FP16) для сокращения вычислительной нагрузки и ускорения инференса.
* **Pruning:**  Применение L1 unstructured pruning к свёрточным слоям для удаления менее важных весов и уменьшения размера модели.
* **Quantization:** Динамическое квантование линейных слоев для снижения точности вычислений с плавающей запятой до целочисленной, что способствует ускорению.
* **Pruning+Quantization:** Комбинированное применение pruning и quantization для достижения синергетического эффекта.


## Результаты

Для каждого метода были измерены времена инференса на нескольких тестовых изображениях.  Результаты представлены в виде:

* Таблиц CSV и HTML (файл `inference_results_table.csv` и `styled_table.html`).
* Графиков (файлы `barplot_mean_times.png` и `barplot_methods_images.png`), визуализирующих среднее время инференса и время для каждого изображения.
* Визуализации результатов детекции объектов для каждого изображения и метода (сохранены как изображения).
* Детальной таблицы в формате Rich, содержащей обнаруженные классы и их достоверность (выводится в консоль при запуске скрипта).

**Ключевые наблюдения:**

* **FP16:** Продемонстрировал значительное ускорение инференса без потери точности детекции.
* **Quantization:**  Также показал улучшение производительности, но менее значительное, чем FP16.
* **Pruning и Pruning+Quantization:** Привели к полной потере способности модели к детекции объектов из-за слишком агрессивного уровня обрезки (PRUNING_AMOUNT = 0.3).  Дальнейшая настройка этого параметра необходима.

Подробный анализ результатов и выводы представлены в файле `README.md`.

## Установка и запуск

1. **Клонируйте репозиторий:**
   ```bash
   git clone 'https://github.com/AbsoluteGZ/YoloV8-optimization-methods'
   ```

2. **Установите необходимые библиотеки:**
   ```bash
   pip install ultralytics onnxruntime tensorrt gdown matplotlib seaborn pandas rich
   ```

3. **Запустите скрипт:**
   ```bash
   python YoloV8_optimization.ipynb
   ```

Скрипт загрузит тестовые изображения и предобученную модель YOLOv8n, выполнит оптимизацию и инференс для каждого метода, а затем сохранит результаты и визуализации.

## Дальнейшие шаги

* Настройка параметра `PRUNING_AMOUNT` для поиска оптимального баланса между размером модели и точностью.
* Исследование комбинации FP16 и Quantization.
* Количественная оценка точности детекции (например, mAP) для каждого метода.
* Изучение других методов оптимизации, таких как Quantization Aware Training.
* Экспорт модели в ONNX и TFLite для развертывания на мобильных устройствах.


## Сравнение с YOLOv7 от Qualcomm

В данном исследовании мы сосредоточились на YOLOv8.  Для сравнения, стоит отметить работу Qualcomm с YOLOv7, где они использовали ONNX, QNN и TFLite для оптимизации и квантования модели, достигнув высоких показателей производительности на различных мобильных устройствах.  Подробное сравнение с их результатами может стать темой дальнейшего исследования.
