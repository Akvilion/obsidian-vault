
Метапрограмування в Python — це **написання коду, який змінює або створює інший код під час виконання програми**. Грубо кажучи, код, який розуміє і змінює себе.

Це дуже потужний інструмент, коли тобі потрібно автоматизувати шаблонні завдання, розширювати класи, змінювати поведінку функцій або створювати DSL (Domain Specific Language).

---

## 🔧 Основні інструменти метапрограмування в Python:

|Інструмент|Приклад використання|
|---|---|
|`getattr`, `setattr`, `hasattr`|Маніпуляція атрибутами об’єкта|
|`type`|Динамічне створення класів|
|`decorators`|Зміна поведінки функцій/методів|
|`metaclasses`|Зміна поведінки класів|
|`__getattr__`, `__setattr__`|Перехоплення доступу до атрибутів|
|`inspect`|Інспекція структури коду|

---

## 🔍 Простий приклад: `getattr`

```python
class User:
    name = "Akvilion"
    role = "Engineer"

u = User()

# Отримаємо атрибут за іменем
print(getattr(u, "name"))  # -> Akvilion

# Перевіримо чи є атрибут
print(hasattr(u, "role"))  # -> True

# Встановимо новий атрибут
setattr(u, "level", 99)
print(u.level)  # -> 99
```

---

## 🧠 Декоратор — найчастіший випадок метапрограмування

```python
def logger(func):
    def wrapper(*args, **kwargs):
        print(f"[LOG] Calling {func.__name__} with args={args}")
        return func(*args, **kwargs)
    return wrapper

@logger
def say_hi(name):
    print(f"Hello, {name}!")

say_hi("Grim")
```

📌 **Результат:**

```
[LOG] Calling say_hi with args=('Grim',)
Hello, Grim!
```

---

## 🧬 Динамічне створення класу через `type`

```python
# Створюємо клас RuntimeClass з атрибутом x
RuntimeClass = type("RuntimeClass", (), {"x": 42, "hello": lambda self: print("Hi!")})

obj = RuntimeClass()
print(obj.x)       # -> 42
obj.hello()        # -> Hi!
```

🔍 `type(name, bases, dict)` — створює клас під час виконання:

- `name`: ім'я класу
    
- `bases`: кортеж батьківських класів
    
- `dict`: атрибути та методи класу
    

---

## 🔮 Метаклас (`metaclass`)

Метаклас — це клас для класів. Якщо клас створює об'єкти, то метаклас створює класи.

```python
class Meta(type):
    def __new__(cls, name, bases, dct):
        print(f"[Meta] Creating class {name}")
        dct["created_by_meta"] = True
        return super().__new__(cls, name, bases, dct)

class MyClass(metaclass=Meta):
    pass

print(MyClass.created_by_meta)  # -> True
```

🧨 Метакласи в дії:

- Ти можеш змінювати, додавати, валідовувати атрибути класу до того, як він буде створений.
    

---

## 📚 Інспекція функції через `inspect`

```python
import inspect

def test_func(x, y=10):
    return x + y

print(inspect.signature(test_func))
# -> (x, y=10)
```

---

## 💡 Де це реально корисно?

- 🚀 Фреймворки (типу FastAPI, Django ORM)
    
- 🔄 Генерація API на основі моделей
    
- 🔍 Логування та профілювання
    
- 🧪 Автоматичне тестування або обгортання викликів
    
- 🧱 Створення декларативних DSL
    

---

## 🔥 Резюме

Метапрограмування — це як чорна магія Python:

- Декоратори — легкий вхід
    
- `getattr`/`setattr` — робота з динамікою атрибутів
    
- `type()` — runtime генерація класів
    
- Метакласи — глибинний контроль над класами
    

---

**Українською суть:**  
Метапрограмування — це коли ти як Гендальф у світі Python, і можеш **переписати структуру коду під час виконання**. Це відкриває двері для дуже гнучких систем, але й вимагає обережності.