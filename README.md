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
- Создал новый 3d проект. В нём создал пустой объект. ![image](https://user-images.githubusercontent.com/103302913/204284487-b4d58097-76ee-4f8d-89d1-b1af33820f74.png)
- Прикрепил C#-скрипт в пустой объект:
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour {

	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f);
		}
		bias = Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; 
		}
		bias += error;
	}

	double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train(int epochs)
	{
		InitialiseWeights();
		
		for(int e = 0; e < epochs; e++)
		{
			totalError = 0;
			for(int t = 0; t < ts.Length; t++)
			{
				UpdateWeights(t);
				Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
			}
			Debug.Log("TOTAL ERROR: " + totalError);
		}
	}

	void Start () {
		Train(8);
		Debug.Log("Test 0 0: " + CalcOutput(0,0));
		Debug.Log("Test 0 1: " + CalcOutput(0,1));
		Debug.Log("Test 1 0: " + CalcOutput(1,0));
		Debug.Log("Test 1 1: " + CalcOutput(1,1));		
	}
	
	void Update () {
		
	}
}
```
- Настроил массив ts на логический оператор OR ![image](https://user-images.githubusercontent.com/103302913/204286233-942e83c7-573e-4213-bd42-9c0814184009.png)
- Запустил программу и получил следующие значения: ![image](https://user-images.githubusercontent.com/103302913/204286772-f22d4d75-488f-4db8-8db5-f4487f806681.png)
![image](https://user-images.githubusercontent.com/103302913/204286894-9447e215-1851-4ff4-8909-64b248126fe5.png)
![image](https://user-images.githubusercontent.com/103302913/204287748-da03d77d-9d3d-4d36-94f4-2e9e647b95e7.png)
За 8 итераций перцептрон успел научиться т.к. TotalError снижается до 0. Тесты метода CalcOutput выдают результат работы перцептрона.
- Настроил массив ts на логический оператор AND ![image](https://user-images.githubusercontent.com/103302913/204290207-b76804aa-98af-4c99-813f-2d9f96aa5847.png)
![image](https://user-images.githubusercontent.com/103302913/204290499-d1775c93-66f3-4f74-8e5f-450f2bc480c6.png)
За 8 итераций перцептрон успел научиться, но не так быстро как с OR.
- Настроил массив ts на логический оператор NAND ![image](https://user-images.githubusercontent.com/103302913/204291070-b59e25da-88ba-46ad-95e9-321506e7d8f4.png)
За 8 итераций перцептрон успевает научиться.
- Настроил массив ts на логический оператор XOR ![image](https://user-images.githubusercontent.com/103302913/204291395-a21c37e9-b406-474a-a5f6-456023ce3f6f.png)
С увеличением итераций увеличивается TotalError. Перцептрон не обучается и работает некорректно. 


## Задание 2
### Пошагово выполнить каждый пункт раздела "ход работы" с описанием и примерами реализации задач
Ход работы:
- Для составления графиков работы перцептрона буду проводить 5 попыток по 8 итераций для каждого оператора.
- Для оператора OR ![image](https://user-images.githubusercontent.com/103302913/204303078-70e61898-d73c-4b55-b880-98704e042a46.png)
- Для оператора AND ![image](https://user-images.githubusercontent.com/103302913/204302084-250302fa-46b3-45e1-8346-953c6bf565ad.png)
- Для оператора NAND ![image](https://user-images.githubusercontent.com/103302913/204302372-892ff014-2e68-47bd-b3b7-5e3367a6c4d9.png)
- Для оператора XOR ![image](https://user-images.githubusercontent.com/103302913/204302999-ae16e103-c15c-4257-9614-72891529a299.png)
 Количество эпох обучения зависит от операции. При операции OR ошибок нет уже на 6 итерации, у AND и NAND - на 7.

## Задание 3

## Выводы

В ходе лабораторной работы познакомился с перцептроном. Смог реализовать логические операции: OR, AND, NAND. Также составил графики из пяти попыток для оценки обучения перцептрона в каждой операции.

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
