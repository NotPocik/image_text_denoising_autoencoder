В ноутбуке autoencoder_models обучал все модели (в google colab), он довольно кривой и непоследовательный местами, тк много чего в нем изменял и удалял. Сохранил всего 4 модели:
   - autoencoder_simple.keras - самая простая из моделей, по 2 сверточных слоя в энкодере и декодере, струкрура создавалась функцией build_autoencoder_simple() (в ноутбуке можно посмотреть), loss функция - binary_crossentropy;
   - autoencoder_bce.keras - модель на основе U-Net, но без соединения сверточных слоев энкодера и декодера, loss функция - binary_crossentropy;
   - autoencoder_bce_2.keras - модель на основе U-Net, в отличие от предыдущей сверточные слои энкодера и декодера соединены, из-за чего изображение выходит чуть более четким и детальным, loss функция - binary_crossentropy;
   - autoencoder_ssim.keras - модель на основе U-Net, сверточные слои энкодера и декодера соединены, loss функция - ssim, визульно работает лучше всех на тестовой и обучающей выборках, и с точки зрения очистки от дефектов, и с точки зрения четкости и резкости букв текста.

Во все модели подаются изображения размером 540x540, и возвращают они изображения в этом же размере, приведение всех изображений к этому размеру делал через cv2.resize(), но возможно потом будет лучше это сделать через добавление отступов.
Шумы дополнительно не добавлял на изображения пока что.

В ноутбуке autoencoder_test есть визуальное сравнение всех четырех моделей на случайных картинках из тестовой выборки, а также можно отдельно посмотреть на работу каждой модели на обучающей выборке.
Пытался еще сделать объективную оценку всех моделей по метрике ssim, но что-то не вышло.  

### UPD:
- Обновил ноутбуки, сделал их более структурированными и понятными.
- Для каждой модели вывел знечнеие loss и построил графики loss по эпохам обучения в ноутбуке autoencoder_models.
- В ноутбуке autoencoder_test добавил вывод метрик MSE и SSIM для каждой модели.

Также пробовал играться с learning rate, но особо безрезультатно. При поднятии его до 0.01 модели вели себя неадекватно в большинстве случаев, на первой эпохе loss улетает в космос, а дальше как повезет. loss = 0.001 оказался стабильным и при этом отностильно быстро модели обучались, поэтому в большинстве оставил такое значение. Batch size тоже попробовал пару раз изменить, отличия на уровне погрешности. Оптимизатор менять не пробовал.

В итоге на первой эпохе почти на всех моделях loss на уровне 0.5 примерно, что-то ниже выходит только если очень повезет.
