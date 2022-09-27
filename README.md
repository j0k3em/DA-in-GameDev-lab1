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
- Написал в Google Colab программу "Hello world" на питоне и запустил её. ![Скриншот 27-09-2022 171828](https://user-images.githubusercontent.com/103302913/192529245-dc1cbf18-e791-4a11-beef-2c0b0ae470e8.jpg)
- Сохранил программу на свой Google Диск. ![Скриншот 27-09-2022 172105](https://user-images.githubusercontent.com/103302913/192529511-7120b1e3-c649-4246-80c8-492697a4c6f9.jpg)
- Настроил VS Code под работу с Unity, создал C# скрипт и написал программу "Hello World" ![Скриншот 27-09-2022 173236](https://user-images.githubusercontent.com/103302913/192530106-edff7a4d-448e-48ed-8692-2e855f41f9e4.jpg)
- Запустил проект на Unity с выводом на экран программы "Hello World" ![Скриншот 27-09-2022 173639](https://user-images.githubusercontent.com/103302913/192533460-1ad9b91a-08aa-45a6-892f-c5a7dc1e4482.jpg)





## Задание 2
### Пошагово выполнить каждый пункт раздела "ход работы" с описанием и примерами реализации задач
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

Выполнил 1 задание, написав две простые программы "Hello World" на Python и Unity. Также познакомился с Google Colab, выполнив 2 задание.

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
