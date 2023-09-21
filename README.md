# Библиотека глубокого обучения для мобильных роботов и роботизированных систем на ARM-процессорах

Библиотека состоит из следующих модулей:

## RoboAICore
[RoboAICore](https://github.com/Cognitive-systems-and-technologies/RoboAICore) - это кроссплатформенная библиотека глубокого обучения, для мобильных роботов и ПК. Библиотека может применяться в направлениях, где требуется разработка систем, использующих нейронные сети, модели глубокого машинного обучения, обучение с подкреплением интеллектуальных агентов.
Функции библиотеки:
- Создание основных нейросетевых структур (разные типы слоев, сетей и алгоритмов их обучения) на аппаратной части и ПК,
- Реализация алгоритмов самообучения интеллектуальных агентов,
- Реализация алгоритмов глубокого обучения с подкреплением,
- Формирование сообщений для передачи между серверной частью (модулем мониторинга NeuralInterface) и агентами.

## NeuralInterface
[NeuralInterface](https://github.com/Cognitive-systems-and-technologies/NeuralInterface) - это модуль мониторинга и визуализации, используемый для отображения работы агентов (мобильных роботов). Данный модуль представляет собой веб-приложение с несколькими страницами, отображающими текущий статус агентов, их принадлежность к определенным группам. Также реализован функционал прямого управления агентами. Можно задать направление их перемещения, загрузить и выгрузить данные их состояния, начать или прекратить их обучение.
Функции модуля:
- Визуализации и удаленный мониторинг процессов, происходящих на аппаратной части,
- Веб интерфейс для управления и мониторинга процессами работы нейросетевых алгоритмов на аппаратной части (взаимодействие с RoboAICore),
- Реализация обмена сообщениями между интеллектуальными агентами при работе в группах.

## SwarmAILib
[SwarmAILib](https://github.com/Cognitive-systems-and-technologies/SwarmAILib) - это библиотека, написанная на языке C#, для реализации и тестирования роевых алгоритмов.
Функции библиотеки:
- Реализация алгоритмов мультиагентного взаимодействия на основе роевых алгоритмов,
- Тестирование алгоритмов взаимодействия агентов внутри мультиагентных систем.

## Принцип работы
При реализации интеллектуальных агентов используются: библиотека RoboAICore и модуль мониторинга и визуализации NeuralInterface. На аппаратной части используются функции библиотеки RoboAICore, модуль NeuralInterface запускается в виде веб-сервера на ПК. Данные о процессе работы алгоритмов и сигналы управления передаются виде http-сообщений между серверной частью и агентами.

Связь между агентами и сервером осуществляется по TCP/IP протоколам. На каждом из агентов работают: 
-	http клиент, для отправки запросов на другие сервера,
-	http сервер, для обработки входящих запросов от других клиентов.

При работе с stm32 клиент-серверная часть  реализована при помощи внешнего модуля ESP8266. На raspberry pi работа web сервера и клиента реализована при помощи сокетов.

Обмен сообщениями осуществляется посредством http запросов. Сообщения представляют собой POST запросы с наборами данных. Данные в теле POST запроса представляют собой строку в формате json. Обмен сообщениями может осуществляться как между сервером и агентом, так и между самими агентами. Так как на каждом из агентов запущен свой web сервер, агент может обрабатывать входящие запросы и отправлять ответ отправителю. Чтобы агент мог отправить запрос другому агенту, ему нужно знать его адрес. Адреса других агентов хранятся у каждого агента и синхронизируются при помощи сервера при включении агента или по запросу.

Пример post запроса при авторизации агента на сервере:
```
POST /api/syncAgentData HTTP/1.1
Content-Type: application/json
Content-Length: 105

{"r":"agent","t":"info","m":"authorization","b":{"name":"RASPBOT","mac":"b8:27:eb:2e:10:8d","port":8081}}
```

Пример взаимодействия модулей RoboAICore и NeuralInterface:

https://github.com/Cognitive-systems-and-technologies/materials/assets/100981393/958dcf7e-fd5f-44c1-9138-4b7db7ad76fe


