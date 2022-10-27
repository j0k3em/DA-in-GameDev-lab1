# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Лямпасов Дмитрий Сергеевич
- РИ210914
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | # | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Познакомиться с программными средствами для создания системы машинного обучения и ее интеграции в Unity.

## Задание 1
- Создал 3d проект в Unity ![image](https://user-images.githubusercontent.com/103302913/198290497-ad55232b-87cd-4795-aa99-3578ba2c0ec1.png)
- Добавил .json файлы ![image](https://user-images.githubusercontent.com/103302913/198291444-47158570-9e7a-43af-b80b-c195b1c83b26.png)
- Запустил Anaconda Prompt от имени администратора и скачал все необходимые библиотеки для работы ![image](https://user-images.githubusercontent.com/103302913/198292087-7c17d6a9-3e97-4e4e-b7bf-adb2d235e4b4.png)
- Создал на сцене куб, сферу и плоскость. Также создал скрипт и подключил его к сфере. ![image](https://user-images.githubusercontent.com/103302913/198292917-653ef615-3433-4d51-8b2a-b5583031811d.png)
- В скрипте RollerAgent.cs написал код:
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{

    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target.localPosition = new Vector3(Random.value * 8-4, 0.5f,Random.value * 8-4);
    }
    
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);

        if(distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
	
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}
```
- Сфере добавил компоненты Rigidbody, Decision Requester и Behaviour Parameters и настроил их ![image](https://user-images.githubusercontent.com/103302913/198295653-a282fd95-852d-4fb8-b38a-99159e7c4c0e.png)
- В корень проекта добавил файл конфигурации нейронной сети, доступный в папке с файлами проекта
```
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 10
      buffer_size: 100
      learning_rate: 3.0e-4
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
```
- Запустил работу MLAgent ![image](https://user-images.githubusercontent.com/103302913/198297616-d952810a-9045-44fa-a1a4-190457ef73be.png)
- Создал 3, 9, 27 копий ![image](https://user-images.githubusercontent.com/103302913/198297833-3cb39867-4ee1-4557-ab3b-ebf1e632e41d.png)
![image](https://user-images.githubusercontent.com/103302913/198297937-98006200-bdeb-4022-ac01-0949e59dbe00.png)
![image](https://user-images.githubusercontent.com/103302913/198298011-b51b2d50-1dcb-46db-8bc8-8d84cb4452d6.png)
- Выводы: после того, как я обучил модель, шар начал непосредственно двигаться к кубу более плавно, перестал падать за пределы плоскости







## Задание 2
### Подробно опишите каждую строку файла конфигурации нейронной сети, доступного в папке с файлами проекта по ссылке. Самостоятельно найдите информацию о компонентах Decision Requester, Behavior Parameters, добавленных на сфере.
```c#
behaviors:
  RollerBall:                        // Имя агента
    trainer_type: ppo                // Установка режима обучения Proximal Policy Optimization
    hyperparameters:                 // Гиперпараметры             
      batch_size: 10                 // Количество опытов на каждой итерации для обновления экстремумов функции
      buffer_size: 100               // Количество опыта, которое нужно набрать перед обновлением модели
      learning_rate: 3.0e-4          // Установка шага обучения
      beta: 5.0e-4                   // Отвечает за случайность действия, повышая разнообразие и иследованность пространства обучения
      epsilon: 0.2                   // Порог расхождений между старой и новой политиками при обновлении
      lambd: 0.99                    // Определяет авторитетность оценок значений во времени
      num_epoch: 3                   // Количество проходов через буфер опыта, при выполнении оптимизации
      learning_rate_schedule: linear // Определяет, как скорость обучения изменяется с течением времени, линейно уменьшает скорость
    network_settings:                // Cетевые настройки
      normalize: false               // Отключает нормализация входных данных
      hidden_units: 128              // Количество нейронов в скрытых слоях сети
      num_layers: 2                  // Количество скрытых слоев для размещения нейронов
    reward_signals:                  // Задает сигналы о вознаграждении
      extrinsic:                     // Внешние факторы
        gamma: 0.99                  // Коэффициент скидки для будущих вознаграждений
        strength: 1.0                // Шаг для learning_rate
    max_steps: 500000                // Общее количество шагов, которые должны быть выполнены в среде до завершения обучения
    time_horizon: 64                 // Количество циклов ML агента, хранящихся в буфере до ввода в модель
    summary_freq: 10000              // Количество опыта, который необходимо собрать перед созданием и отображением статистики
```
- Decision Requester - запрашивает решение через регулярные промежутки времени и обрабатывает чередование между ними во время обучения.
- Behavior Parameters - определяет принятие объектом решений, в него указывается какой тип поведения будет использоваться: уже обученная модель или удалённый процесс обучения.

## Выводы

- Игровой баланс —  субъективное «равновесие» между персонажами, командами, тактиками игры и другими игровыми объектами. Игровой баланс определяет, будет ли игра равная или же будет перевешивать в сторону более сильной стороны.
- Cистемы машинного обучения можно использовать для того, чтобы тренировать и обучать игровые объекты для выявления не сбалансированных моментов в игре, а также достижение этого баланса.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
