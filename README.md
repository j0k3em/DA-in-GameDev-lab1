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
Ознакомиться с основными операторами зыка Python на примере реализации линейной регрессии.

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
behaviors:
Ход работы:
- Произвести подготовку данных для работы с алгоритмом линейной регрессии. 10 видов данных были установлены случайным образом, и данные находились в линейной зависимости. Данные преобразуются в формат массива, чтобы их можно было вычислить напрямую при использовании умножения и сложения. ![1](https://user-images.githubusercontent.com/103302913/192551188-5f28ad6e-36e2-4271-ae50-979b2a58d1ef.jpg)

- Определите связанные функции. Функция модели: определяет модель линейной регрессии wx+b. Функция потерь: функция потерь среднеквадратичной ошибки. Функция оптимизации: метод градиентного спуска для нахождения частных производных w и b. ![2](https://user-images.githubusercontent.com/103302913/192551631-df39e492-1bc3-4fde-b0e2-b91a1ec5bb16.jpg)
- Начать итерацию

- Инициализация и модель итеративной оптимизации ![3](https://user-images.githubusercontent.com/103302913/192551802-0accdbdb-1472-4b67-b3a8-8ded0e4273fb.jpg)
- На 2 итерации отображаются значения параметров, значения потерь и эффекты визуализации после итерации ![4](https://user-images.githubusercontent.com/103302913/192551922-7bdc521c-e69d-48cb-8acf-6dad9a1cb634.jpg)
- На 3 итерации отображаются значения параметров, значения потерь и эффекты визуализации после итерации ![5](https://user-images.githubusercontent.com/103302913/192551990-7037b74d-3b1a-4ce9-b5ad-43ec20d12562.jpg) 
- На 4 итерации отображаются значения параметров, значения потерь и эффекты визуализации после итерации ![6](https://user-images.githubusercontent.com/103302913/192552086-1c32b6d2-cfd2-42b4-b540-2aea129af28e.jpg)
- На 5 итерации отображаются значения параметров, значения потерь и эффекты визуализации после итерации ![7](https://user-images.githubusercontent.com/103302913/192552194-e53b91a6-fc5b-4624-9d60-217a51792ff8.jpg)
- На 10000 итерации отображаются значения параметров, значения потерь и эффекты визуализации после итерации ![8](https://user-images.githubusercontent.com/103302913/192552232-9e7447bb-583b-4470-9814-cf39fefef514.jpg)

## Задание 3
### Какова роль параметра Lr? Ответьте на вопрос, приведите пример выполнения кода, который подтверждает ваш ответ. В качестве эксперимента можете изменить значение параметра. 
### Должна ли величина loss стремиться к нулю при изменении исходных данных? Ответьте на вопрос, приведите пример выполнения кода, который подтверждает ваш ответ.

- Перечисленные в этом туториале действия могут быть выполнены запуском на исполнение скрипт-файла, доступного [в репозитории](https://github.com/Den1sovDm1triy/hfss-scripting/blob/main/ScreatingSphereInAEDT.py).
- Для запуска скрипт-файла откройте Ansys Electronics Desktop. Перейдите во вкладку [Automation] - [Run Script] - [Выберите файл с именем ScreatingSphereInAEDT.py из репозитория].

```py

import ScriptEnv
ScriptEnv.Initialize("Ansoft.ElectronicsDesktop")
oDesktop.RestoreWindow()
oProject = oDesktop.NewProject()
oProject.Rename("C:/Users/denisov.dv/Documents/Ansoft/SphereDIffraction.aedt", True)
oProject.InsertDesign("HFSS", "HFSSDesign1", "HFSS Terminal Network", "")
oDesign = oProject.SetActiveDesign("HFSSDesign1")
oEditor = oDesign.SetActiveEditor("3D Modeler")
oEditor.CreateSphere(
	[
		"NAME:SphereParameters",
		"XCenter:="		, "0mm",
		"YCenter:="		, "0mm",
		"ZCenter:="		, "0mm",
		"Radius:="		, "1.0770329614269mm"
	], 
)

```

## Выводы

Выполнил 1 и 2 задания, написав две простые программы "Hello World" на Python и Unity. Также познакомился с Google Colab, выполнив работу с алгоритмом линейной регрессии. 

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
