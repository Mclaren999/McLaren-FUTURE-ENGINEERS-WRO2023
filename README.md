# McLaren-FUTURE-ENGINEERS-WRO2023
Этот репозиторий содержит код, который использовался для создания автономного транспортного средства для соревнований WRO.
# Используемые модули:
В коде используется основной модуль OpenCV (и numpy для Opencv). Это хорошо известный модуль, используемый в известных программах компьютерного зрения. Он используется в Raspberry Pi для обработки информации с камеры для обнаружения сигналов светофора, а также цвета сигналов. Он также используется для программы обнаружения стен, а также для программ обнаружения кругов.

Другие используемые модули включают в себя:

 1. Модуль Picamera и picamera.array - Для получения кадра с пикамеры.
 2. Время - чтобы получить время.
# Работа программы:
Программа состоит из основного «Игрового кода», который запускает программу, а также содержит еще один файл с именем «utlis», который содержит все функции, используемые в программе. Эти оба файла должны находиться в одном каталоге для правильной работы программы.

Цикл программы выглядит следующим образом:

1. Программа определяет сигналы и определяет, находится ли он справа от сигнала. В следующем цикле
2. Программа определяет контуры стены и приближает к ним линию
3.Затем программа определяет наклон линии и использует некоторые заранее определенные измерения диапазона камеры, чтобы определить расстояние автомобиля от линии.
4. В соответствии с определенным расстоянием он будет маневрировать, чтобы удалиться от линии в той же петле,
5. Затем программа обнаруживает синие и оранжевые линии и в соответствии с порядком их появления определяет направление круга и использует эту информацию для определения направления поворота, когда автомобиль сталкивается со стеной.
# Электромеханические части автомобиля:
Автомобиль содержит следующие части:

1. Raspberry Pi 4 модель B — мозг автомобиля, который получает кадры с камеры, обрабатывает их, а затем отдает команды контроллеру двигателей.
2. Picamera v2
3. два DC motors
4. L289N драивер
5. готовый купленный каркас от машины и дополнительные компоненты распечатанные на 3д принтере для устоичивости
6. (дполнительный)устоновлена круглая установка из 30 диодов
7. питание Powerbank на 10000w и две батареики для драивера 18650
# Подробная программа работы:
Программа состоит из трех частей:

1. Обнаружение сигнала
2. Обнаружение стены
3. Обнаружение круга
# Обнаружение сигнала:

1. Программа сначала преобразует входной сигнал RGB с камеры в формат HSV, чтобы упростить поиск сигналов на изображении.
2. Затем он фильтрует кадр HSV, используя функцию opencv cv.inRange() с помощью предопределенных значений для зеленого и красного цветов HSV.
3. Затем он использует функцию приближения контура в модуле opencv, чтобы найти контуры и приблизить к ним круг и прямоугольник.
4. Центр круга используется для аппроксимации положения сигнала в реальной жизни.
5.Затем программа проверяет правильность направления сигнала и, соответственно, отправляет сигналы контроллеру мотора для выполнения маневров.
![image](https://github.com/Mclaren999/McLaren-FUTURE-ENGINEERS-WRO2023/assets/135827054/0ea1f9eb-a7e9-4d4e-bb83-d1b6bc7b8834)
и формула
![image](https://github.com/Mclaren999/McLaren-FUTURE-ENGINEERS-WRO2023/assets/135827054/2659fc76-7757-431a-b4bc-5ba3d049f771)


# Обнаружение стены:

1. Кадр, полученный с камеры, сначала обрезается (ROI), так как нам нужна только часть кадра, где есть стена.
2. Затем он преобразуется в формат HSV, и черный цвет стены фильтруется с предопределенными ограничениями.
3. Затем передается в функцию аппроксимации контура, после чего линия аппроксимируется к стене
4. Наклон линии используется для определения расстояния от стены до автомобиля.
5. В зависимости от расстояния автомобиль определяет, должен ли он двигаться влево или вправо и на сколько, и соответственно посылает сигналы контроллеру двигателя для маневров.
# Обнаружение круга:

1. Кадр, полученный с камеры, сначала обрезается (ROI), так как нам нужна только часть кадра, где есть линии
2. Затем он преобразуется в формат HSV, а синие и оранжевые строки фильтруются с заранее заданными ограничениями.
3.Затем приближаем окружность к каждой из синей и оранжевой линий и находим их центры
4. Если центр синей линии находится выше оранжевой линии, то направление круга определяется как левое, в противном случае оно будет определяться как правое.
5. После того, как он обнаружит строки, он увеличивает счетчик, а затем проверяет только в следующий раз, по крайней мере, через 3 секунды, чтобы убедиться, что он не увеличивает счетчик, видя ту же строку.
6. Как только счетчик достигает 12, программа останавливается еще через три секунды.
