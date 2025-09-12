# sssss
задание 1 (лаба1) - 
1. Загрузите датасет uber_rides_bookings.csv (предполагаем, что файл находится в той же директории, что и ваш
скрипт) в DataFrame df. Подсказка: pd.read_csv()
2. Выведите первые 5 строк датасета, чтобы ознакомиться с его содержимым. Подсказка: . head()
3. Выведите общую информацию о датасете (количество записей, количество столбцов, типы данных, количество
непустых значений). Подсказка:. info()
4. Выведите статистическое описание числовых столбцов датасета (среднее, стандартное отклонение, мин/макс
значения, квартили). Подсказка: .describe()
5. Определите количество строк и столбцов в датасете. Подсказка: .shape
6. Выберите только столбцы Booking ID , booking_datetime, Booking Status , Vehicle Type, Payment Method и
выведите первые 5 строк полученного подмножества.
7. Отфильтруйте и выведите все бронирования, которые имеют статус "Cancelled by Driver" (отменено
водителем).
8. Отфильтруйте и выведите бронирования, выполненные с использованием VehicleТуpe "Auto" И имеющие
Booking Value более 500.
9. Отфильтруйте и выведите бронирования, которые были сделаны в течение марта 2024 года (с '2024-03-01' по
'2024-03-31' включительно). Подсказка: Используйте столбец booking_datetime .

``` python

import pandas as pd

# Загрузка датасета
try:
    df = pd.read_csv('C:/Users/student/Documents/Downloads/ncr_ride_bookings.csv')
except FileNotFoundError:
    print("Ошибка: Файл 'ncr_ride_bookings.csv' не найден. Проверьте путь к файлу.")
    exit()

# Вывод первых 5 строк
print("Первые 5 строк датасета:")
print(df.head())

# Общая информация о датасете
print("\nОбщая информация о датасете:")
df.info()

# Печать названий столбцов для проверки
print("\nНазвания столбцов в датасете:")
print(df.columns.tolist())

# Статистическое описание числовых столбцов
print("\nСтатистическое описание числовых столбцов:")
print(df.describe())

# Количество строк и столбцов
num_rows, num_columns = df.shape
print(f'\nКоличество строк: {num_rows}, Количество столбцов: {num_columns}')

# 1. СОЗДАНИЕ СТОЛБЦА booking_datetime (объединение Date и Time)
try:
    # Проверяем наличие столбцов Date и Time
    if 'Date' in df.columns and 'Time' in df.columns:
        df['booking_datetime'] = pd.to_datetime(df['Date'].astype(str) + ' ' + df['Time'].astype(str))
        print("\nСоздан столбец booking_datetime (объединение Date и Time)")
        print("Первые 5 значений нового столбца:")
        print(df['booking_datetime'].head())
    else:
        print("\nОшибка: Столбцы 'Date' и/или 'Time' не найдены в датасете")
        
except Exception as e:
    print(f"Ошибка при создании столбца booking_datetime: {e}")

# 2. ФИЛЬТРАЦИЯ с правильными названиями столбцов и ВЫВОД ТОЛЬКО НУЖНЫХ СТОЛБЦОВ
print("\n" + "="*80)
print("ФИЛЬТРАЦИЯ ДАННЫХ С ПРАВИЛЬНЫМИ НАЗВАНИЯМИ СТОЛБЦОВ")
print("="*80)

# Список нужных столбцов для вывода
output_columns = ['Booking ID', 'booking_datetime', 'Booking Status', 'Vehicle Type', 'Payment Method', 'Booking Value']

# 2.1 Фильтрация бронирований со статусом "Cancelled by Driver"
try:
    cancelled_by_driver = df[df['Booking Status'] == 'Cancelled by Driver']
    print("\nБронирования со статусом 'Cancelled by Driver' (первые 5):")
    print(cancelled_by_driver[output_columns].head() if not cancelled_by_driver.empty else "Нет данных")
    
    # ПРОВЕРКА УНИКАЛЬНЫХ ЗНАЧЕНИЙ
    print(f"\nУникальные статусы в отфильтрованных данных: {cancelled_by_driver['Booking Status'].unique()}")
    print(f"Количество отмененных водителем бронирований: {len(cancelled_by_driver)}")
    
except KeyError as e:
    print(f"Ошибка: Столбец {str(e)} не найден в датасете.")

# 2.2 Фильтрация бронирований с Vehicle Type "Auto" и Booking Value больше 500
try:
    auto_bookings_over_500 = df[(df['Vehicle Type'] == 'Auto') & (df['Booking Value'] > 500)]
    print("\nБронирования с Vehicle Type 'Auto' и Booking Value больше 500 (первые 5):")
    print(auto_bookings_over_500[output_columns].head() if not auto_bookings_over_500.empty else "Нет данных")
    
    # ПРОВЕРКА УНИКАЛЬНЫХ ЗНАЧЕНИЙ
    print(f"\nУникальные типы транспортных средств: {auto_bookings_over_500['Vehicle Type'].unique()}")
    print(f"Минимальное значение бронирования: {auto_bookings_over_500['Booking Value'].min() if not auto_bookings_over_500.empty else 'N/A'}")
    print(f"Количество бронирований: {len(auto_bookings_over_500)}")
    
except KeyError as e:
    print(f"Ошибка: Столбец {str(e)} не найден в датасете.")

# 2.3 Фильтрация бронирований за март 2024 года
try:
    # Используем созданный столбец booking_datetime
    march_2024_bookings = df[(df['booking_datetime'].dt.year == 2024) & (df['booking_datetime'].dt.month == 3)]
    print("\nБронирования за март 2024 года (первые 5):")
    print(march_2024_bookings[output_columns].head() if not march_2024_bookings.empty else "Нет данных")
    
    # ПРОВЕРКА УНИКАЛЬНЫХ ЗНАЧЕНИЙ
    print(f"\nДиапазон дат: {march_2024_bookings['booking_datetime'].min()} - {march_2024_bookings['booking_datetime'].max()}" if not march_2024_bookings.empty else "Нет данных")
    print(f"Количество бронирований за март 2024: {len(march_2024_bookings)}")
    
except KeyError as e:
    print(f"Ошибка: Столбец {str(e)} не найден в датасете.")
except Exception as e:
    print(f"Ошибка при фильтрации по дате: {e}")
# ДОПОЛНИТЕЛЬНО: Проверка уникальных значений во всем датасете
print("\n" + "="*80)
print("ПРОВЕРКА УНИКАЛЬНЫХ ЗНАЧЕНИЙ ВО ВСЕМ ДАТАСЕТЕ")
print("="*80)

# Проверяем уникальные значения в ключевых столбцах
for column in ['Booking Status', 'Vehicle Type', 'Payment Method']:
    if column in df.columns:
        print(f"\nУникальные значения в столбце '{column}':")
        print(df[column].unique())
        print(f"Количество уникальных значений: {df[column].nunique()}")
```

Задание 2 (лаба 2) - Написать скрипт, который выводит на экран статистический обзор данных, подсчёт количества пропущенных значений в каждом столбце и уникальные значения в категориальных столбцах (Booking Status, Vehicle Type)

```python
import pandas as pd

file_path = r'C:\Users\student\Documents\Downloads\ncr_ride_bookings.csv'
try:
    # Читаем данные из файла
    data = pd.read_csv(file_path)

    # Общая статистика
    print('\\n\\nОбщая статистика:')
    print(data.describe(include='all'))

    # Пропуски в данных
    missing_data = data.isnull().sum()
    print('\\n\\nКоличество пропусков в каждом столбце:')
    print(missing_data)

    # Категориальные переменные
    categorical_columns = ['Booking Status', 'Vehicle Type']
    for col in categorical_columns:
        unique_values = data[col].unique()
        print(f'\\n\\nУникальные значения в столбце "{col}" ({len(unique_values)} шт.):')
        print(unique_values)

except FileNotFoundError:
    print(f'Файл {file_path} не найден.')
```
