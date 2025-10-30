# EcoOs_Lab2

## ОТЧЕТ
### Лабораторная работа: Реализация компонента EcoLab1 с использованием механизмов включения и агрегирования

---

### 1. ЦЕЛЬ РАБОТЫ
Разработать компонент EcoLab1, демонстрирующий механизмы включения (containment) и агрегирования (aggregation) компонентов калькулятора (A, B, C, D, E), выполняющих арифметические операции (сложение, вычитание, умножение, деление).

---

### 2. ОПИСАНИЕ АРХИТЕКТУРЫ СИСТЕМЫ

#### 2.1 Структура компонентов
Система состоит из следующих компонентов:

**Основной компонент:**
- **EcoLab1** - главный компонент, предоставляющий функциональность сортировки и включающий/агрегирующий компоненты калькулятора

**Компоненты калькулятора:**
- **EcoCalculatorB** - реализует интерфейс IEcoCalculatorX (сложение, вычитание) - механизм включения
- **EcoCalculatorC** - реализует интерфейс IEcoCalculatorX (сложение, вычитание) - механизм включения  
- **EcoCalculatorD** - реализует интерфейс IEcoCalculatorY (умножение, деление) - механизм агрегирования
- **EcoCalculatorE** - реализует интерфейс IEcoCalculatorY (умножение, деление) - механизм агрегирования

#### 2.2 Реализованные интерфейсы
- **IEcoLab1**: Основной интерфейс компонента с методами сортировки
- **IEcoCalculatorX**: Addition(), Subtraction()
- **IEcoCalculatorY**: Multiplication(), Division()

---

### 3. МЕХАНИЗМЫ ВКЛЮЧЕНИЯ И АГРЕГИРОВАНИЯ

#### 3.1 Механизм включения (Containment)
**Определение:** Внешний компонент создает и полностью контролирует внутренний компонент.

**Применение в проекте:**
- Компоненты B и C включены в EcoLab1 через интерфейс IEcoCalculatorX
- EcoLab1 управляет временем жизни этих компонентов

**Преимущества:**
- Полный контроль над внутренним компонентом
- Инкапсуляция реализации

#### 3.2 Механизм агрегирования (Aggregation)  
**Определение:** Внешний компонент делегирует часть функциональности внутреннему компоненту.

**Применение в проекте:**
- Компоненты D и E агрегированы в EcoLab1 через интерфейс IEcoCalculatorY
- Поддерживается обратная навигация к EcoLab1

**Преимущества:**
- Переиспользование существующего кода
- Расширение функциональности без наследования

---

### 4. РЕАЛИЗАЦИЯ

#### 4.1 Регистрация компонентов
```c
#ifdef ECO_LIB
    pIBus->pVTbl->RegisterComponent(pIBus, &CID_EcoLab1, ...);
    pIBus->pVTbl->RegisterComponent(pIBus, &CID_EcoCalculatorB, ...);
    pIBus->pVTbl->RegisterComponent(pIBus, &CID_EcoCalculatorC, ...);
    pIBus->pVTbl->RegisterComponent(pIBus, &CID_EcoCalculatorD, ...);
    pIBus->pVTbl->RegisterComponent(pIBus, &CID_EcoCalculatorE, ...);
#endi
```

#### 4.2 Получение интерфейсов

```c
// Получение основного интерфейса EcoLab1
IEcoLab1* pIEcoLab1 = 0;
result = pIBus->pVTbl->QueryComponent(pIBus, &CID_EcoLab1, 0, 
                                      &IID_IEcoLab1, (void**)&pIEcoLab1);

// Получение интерфейсов калькулятора через механизмы включения и агрегирования
IEcoCalculatorX* pIX = 0;
IEcoCalculatorY* pIY = 0;

if (pIEcoLab1 != 0) {
    // Механизм включения - получение IEcoCalculatorX
    result = pIEcoLab1->pVTbl->QueryInterface(pIEcoLab1, 
                                             &IID_IEcoCalculatorX, 
                                             (void**)&pIX);
    
    // Механизм агрегирования - получение IEcoCalculatorY  
    result = pIEcoLab1->pVTbl->QueryInterface(pIEcoLab1, 
                                             &IID_IEcoCalculatorY, 
                                             (void**)&pIY);
}

// Использование полученных интерфейсов
if (pIX != 0) {
    int32_t sum = pIX->pVTbl->Addition(pIX, 10, 5);
    int32_t diff = pIX->pVTbl->Subtraction(pIX, 10, 5);
}

if (pIY != 0) {
    int32_t product = pIY->pVTbl->Multiplication(pIY, 10, 5);
    int32_t quotient = pIY->pVTbl->Division(pIY, 10, 5);
}
```


### 5. РЕЗУЛЬТАТЫ ТЕСТИРОВАНИЯ

#### 5.1 Тестирование отдельных компонентов калькулятора

![Тестирование компонентов калькулятора](https://github.com/user-attachments/assets/9a8a6811-3509-4c18-b2ea-704aafa25229)


#### 5.2 Тестирование механизмов включения и агрегирования

![Тестирование механизмов включения и агрегирования](https://github.com/user-attachments/assets/93068ae3-1ae6-45e7-ba0c-5cb79fc574f8)



#### 5.3 Тестирование функциональности сортировки

![Тестирование сортировки](https://github.com/user-attachments/assets/59f54b3b-95a2-4361-9b82-2d794db96f13)

