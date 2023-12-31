# DB_sub_system
Подсистема хранения данных с двумя реализациями: LSM дерево и Redis

---

Переключение между реализациями LSM и Redis реализовано при помощи опции конфига

## LSM дерево

В оперативной памяти поддерживается сбалансированная структура данных MemTable, а также разреженный индекс.
Когда размер превышает заданный (конфигурируемый параметр), записываем MemTable на диск в виде файла SS-таблицы. 
Запросы на чтение обслуживает MemTable. Если в ней не нашлось нужного ключа, то ищем ключ в обратном порядке в файлах сегментов.
Периодически запускаем в фоне процесс слияния и уплотнения

- Реализован механизм WAL
-	Реализовано сжатие блоков в файлах сегментов
  
### конфигурируемые параметры, задается в application.properties
-	Время запуска процесса мерджа SS-таблиц
-	Размер SS-таблицы
-	Размер MemTable

---

## Redis

При помощи docker-compose реализована схема ниже секционирования в Redis
[![image.png](https://i.postimg.cc/QCVHkPmh/image.png)](https://postimg.cc/Fk5rhTwB)

Узлы аутентифицируются при помощи пароля
Конфигурация узлов предусматривает:
- Указание ip, порта
- Параметры аутентификации
- Настройки секционирования
- Отдельные volumes для данных и конфигурации




