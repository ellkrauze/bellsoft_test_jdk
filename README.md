# ТЕСТОВОЕ ЗАДАНИЕ ОТ BELLSOFT | Бенчмарк кодов Рида-Соломона
## Входные параметры
Для того чтобы замерить производительность декодирования при различных входных параметрах, были использованы [функции тестирования](https://github.com/zxing/zxing/blob/master/core/src/test/java/com/google/zxing/common/reedsolomon/ReedSolomonTestCase.java), включающие в себя запуск декодирования при следующих входных параметрах.

1. Реальные данные
``` java
testEncodeDecode(GenericGF.DATA_MATRIX_FIELD_256, new int[] {
   0x69, 0x75, 0x75, 0x71, 0x3B, 0x30, 0x30, 0x64,
   0x70, 0x65, 0x66, 0x2F, 0x68, 0x70, 0x70, 0x68,
   0x6D, 0x66, 0x2F, 0x64, 0x70, 0x6E, 0x30, 0x71,
   0x30, 0x7B, 0x79, 0x6A, 0x6F, 0x68, 0x30, 0x81,
   0xF0, 0x88, 0x1F, 0xB5 },
   new int[] {
   0x1C, 0x64, 0xEE, 0xEB, 0xD0, 0x1D, 0x00, 0x03,
   0xF0, 0x1C, 0xF1, 0xD0, 0x6D, 0x00, 0x98, 0xDA,
   0x80, 0x88, 0xBE, 0xFF, 0xB7, 0xFA, 0xA9, 0x95 });
   ```
   
2. Синтетические данные
``` java
testEncodeDecodeRandom(GenericGF.DATA_MATRIX_FIELD_256, 10, 240);
   ```
   
## Бенчмарки
Был использован код JMH со следующими атрибутами.
``` java
@Benchmark
@BenchmarkMode(Mode.AverageTime)
@Warmup(iterations = 10)
@Measurement(iterations = 5)
   ```
где **Mode.AverageTime** позволяет увидеть среднее время выполнения, Warmup задает количество итераций прогрева функций, Measurement задает количество итераций вызова для замера.

**JDK 8**
```
Benchmark                                       Mode  Cnt  Score    Error  Units
TestBM.testDecoder                              avgt    5  0,005 ±  0,001   s/op
TestBM.testEncodeDecodeRandom                   avgt    5  0,275 ±  0,158   s/op
```

**JDK 11**
```
Benchmark                                       Mode  Cnt  Score    Error  Units
TestBM.testDecoder                              avgt    5  0,004 ±  0,001   s/op
TestBM.testEncodeDecodeRandom                   avgt    5  0,213 ±  0,004   s/op
```

## Профилирование на JDK 8

Реальные данные

![alt text](https://github.com/ellkrauze/bellsoft_test_jdk/blob/main/1.jpg?raw=true)

Синтетические данные

![alt text](https://github.com/ellkrauze/bellsoft_test_jdk/blob/main/2.jpg?raw=true)

## Профилирование на JDK 11

Реальные данные

![alt text](https://github.com/ellkrauze/bellsoft_test_jdk/blob/main/3.jpg?raw=true)

Синтетические данные

![alt text](https://github.com/ellkrauze/bellsoft_test_jdk/blob/main/4.jpg?raw=true)


Как видно из отчета, наибольшее время выполнения занимает метод evaluateAt класса GenericGFPoly, который возвращает значение полинома в заданной точке.
