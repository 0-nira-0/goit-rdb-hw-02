# goit-rdb-hw-02

Домашнє завдання 2 з курсу Relational Databases for Data Science: ER-модель, DDL у PostgreSQL 16, нормалізація, offline- та online-рівні feature store, point-in-time correctness.

## Сценарій

Сервіс доставки їжі. У момент призначення кур'єра на замовлення модель прогнозує, чи буде доставка пізнішою за обіцяний час. Entity feature store: `courier_id`, момент прогнозу: `prediction_ts`, цільова змінна: `is_late`.

## Дані

Зовнішнього датасету й посилання на джерело немає: завдання вимагає тестових даних через `INSERT`, тому вибірку складено вручну. Дані синтетичні, усі часові значення мають явний зсув `+00`. Нічого завантажувати не потрібно.

| Таблиця | Рівень | Рядків |
| --- | --- | --- |
| `hw2_zones` | бізнес | 5 |
| `hw2_restaurants` | бізнес | 4 |
| `hw2_couriers` | бізнес | 5 |
| `hw2_courier_zones` | бізнес, bridge M:N | 8 |
| `hw2_orders` | бізнес | 14 |
| `hw2_courier_features_offline` | offline, історія | 12 версій для 4 кур'єрів |
| `hw2_training_events` | offline | 13 |
| `hw2_courier_features_online` | online | 4 |

## Запуск

Google Colab: відкрити `hw_t2.ipynb` і виконати Runtime → Run all. Перші кодові клітинки встановлять залежності й запустять PostgreSQL через `pgserver` у `/tmp/hw2_pg`. Діаграми запишуться в робочий каталог runtime. У Colab потрібна версія середовища 2026.07 (Python 3.12): Runtime → Change runtime type → Runtime version, бо `pgserver` не має збірок для Python 3.13.

Локально потрібен Python 3.12, бо для новіших версій Python може не бути wheel-пакета `pgserver`:

```bash
python3.12 -m venv .venv
source .venv/bin/activate
pip install pgserver psycopg2-binary "psycopg[binary]" sqlalchemy pandas matplotlib jupyter
jupyter nbconvert --to notebook --execute --inplace hw_t2.ipynb
```

Повторний запуск безпечний: notebook на початку виконує `DROP TABLE IF EXISTS ... CASCADE`.

## Структура

```text
hw_t2.ipynb       notebook з усіма завданнями й результатами виконання
er_diagram.png    ER-діаграма, зображення
er_diagram.svg    ER-діаграма, вектор
er_diagram.dbml   ER-діаграма, текстовий DSL для dbdiagram.io
README.md
.gitignore
```

Файли `er_diagram.*` генерує сам notebook з одного опису схеми, який звіряється з каталогом PostgreSQL.
