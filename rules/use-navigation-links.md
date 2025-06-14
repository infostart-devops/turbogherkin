# Используйте переход по навигационной ссылке вместо вместо перехода по разделам

**Основной принцип:** Для открытия форм системы (списков, форм элементов справочников и документов и т.п.) **предпочитайте использовать прямые навигационные ссылки**, если сам процесс навигации по разделам интерфейса не является целью вашего тестирования.

**Почему это важно и какие преимущества это дает?**

*   **Повышение стабильности тестов:**
    *   Навигационные ссылки (например, `e1cib/list/Справочник.Номенклатура`) гораздо менее чувствительны к изменениям в пользовательском интерфейсе (UI), таким как переименование разделов, изменение структуры меню или прав доступа к командам интерфейса.
    *   Тесты, использующие ссылки, продолжат работать корректно даже после редизайна навигационной части приложения.
*   **Ускорение выполнения тестов:**
    *   Прямой переход по ссылке — это один шаг, который выполняется значительно быстрее, чем последовательность кликов по нескольким элементам меню для достижения той же формы.
*   **Улучшение читаемости и фокусировки сценария:**
    *   Сценарий становится короче и концентрируется на проверке целевой функциональности формы, а не на рутинных шагах ее открытия.
*   **Независимость от контекста пользователя:**
    *   Навигационная ссылка откроет форму независимо от того, какие разделы доступны текущему пользователю в командном интерфейсе (при условии, что у пользователя есть права на доступ к самой форме).

**Когда навигация по разделам (через UI) оправдана?**

Несмотря на преимущества навигационных ссылок, существуют ситуации, когда необходимо тестировать именно путь через интерфейс:
1.  **Тестирование доступности команд:** Если сценарий проверяет, что определенная команда (например, "Товары" в разделе "Продажи") видна и доступна пользователю с определенными правами.
2.  **Проверка специфического пользовательского пути:** Если тестируется конкретный бизнес-процесс, который включает в себя навигацию пользователя по определенным разделам и командам интерфейса.
3.  **Тестирование логики открытия форм из контекста:** Например, открытие списка подчиненного справочника из формы владельца.

**Во всех остальных случаях, когда вам просто нужно открыть известную форму для дальнейших действий, используйте навигационную ссылку.**

## Примеры

**Сценарий:** Тестирование создания нового элемента в справочнике "Товары". Сам путь открытия формы списка товаров не является целью данного теста.

### ❌ Неправильно (потенциально менее стабильно, медленнее, избыточно для данной цели)

```gherkin
Сценарий: Создание нового товара через UI навигацию
    Когда в командном интерфейсе я выбираю 'Товарные запасы' 'Товары'
    Тогда открылось окно 'Товары'
    Когда я нажимаю на кнопку с именем 'ФормаСоздать'
    # ... дальнейшие шаги по созданию товара
```

Что не так: этот сценарий проверяет создание товара, а не навигацию по меню. Лишние шаги перехода по меню раздела делают тест хрупким и зависимым от изменений в панели разделов.


### ✔️ Правильно (более стабильно, быстро и сфокусировано)

```gherkin
Сценарий: Создание нового товара с использованием навигационной ссылки
    Когда Я открываю навигационную ссылку "e1cib/list/Справочник.Товары"
    Тогда открылось окно 'Товары'
    Когда я нажимаю на кнопку с именем 'ФормаСоздать'
    # ... дальнейшие шаги по созданию товара
```

В данном примере прямое открытие формы списка экономит время и делает тест устойчивее.