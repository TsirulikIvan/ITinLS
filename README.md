# Task1:
  ### Создание облака ключевых слов англоязычного текста по профессиональной тематике
## Solution:
1. [Aticle link]  (https://devblogs.nvidia.com/accelerated-ray-tracing-cuda/)
2. Subtheme Preliminaries
3.
  3.1 Google translate
```no-highlight
Механизм трассировки лучей C ++ в книге One Weekend отнюдь не самый быстрый трассировщик лучей, но перевод вашего кода C ++ в CUDA может привести к увеличению скорости в 10 раз или более! Давайте пройдемся по процессу преобразования кода C ++ из Ray Tracing за один выходной в CUDA.
Обратите внимание, что при прохождении процесса кодирования на C ++ рассмотрите возможность использования тегов или веток git, чтобы вы могли легко вернуться к коду каждой главы. Вы можете сравнить свой код C ++ с кодом Питера Ширли по адресу https://github.com/petershirley/raytracinginoneweekend.
Попробовав свои силы в использовании CUDA, вы также можете сравнить его с моим кодом CUDA по адресу https://github.com/rogerallen/raytracinginoneweekendincuda. Обязательно используйте ветки, которые я создал для каждой главы. (Например, git checkout ch12_where_next_cuda.)

В этом посте предполагается, что вы понимаете некоторые основы CUDA. Если нет, вы можете начать с Еще более простого введения в CUDA здесь, в блоге разработчиков NVIDIA. 
Вам необходимо настроить среду разработки для компиляции и запуска кода CUDA. Мы также будем выполнять профилирование с помощью nvprof, поэтому вы можете ознакомиться с тем, как профилировать код с помощью nvprof. Код в моем репозитории был написан с использованием Ubuntu Linux и CUDA 9.x, но вы должны быть в состоянии адаптировать эти инструкции для последних выпусков CUDA либо на Windows, либо на MacOS.

Если вы начнете с моего Makefile, обратите внимание, что я строю для карты GTX 1070, используя специальные флаги -gencode для этой карты (-gencode arch = compute_60, code = sm_60). Вам нужно будет настроить архитектуру и параметры функций для графического процессора или графических процессоров, на которых вы будете работать. В моем Makefile основные цели, которые вы будете использовать, предназначены для сборки исполняемого файла make cudart, а также для запуска и создания выходного образа make out.ppm.

Обратите внимание, что когда код CUDA выполняется дольше, чем несколько секунд, вы можете заметить ошибку 6 (cudaErrorLaunchTimeout), и в Windows ваш экран также может отключиться на несколько секунд. Это связано с тем, что ваш оконный менеджер считает, что графический процессор работает со сбоями, когда он не отвечает через определенное время. Пользователи Linux могут предпринять шаги для решения этой проблемы с помощью этого поста.

В этом посте вы узнаете о следующем:
*кадровый буфер Unified Memory, который записывается графическим процессором и читается процессором
*запуск рендеринга на графическом процессоре
*Написание кода на C ++, который может работать на CPU или GPU
*Проверка медленного кода с плавающей запятой двойной точности.
*C ++ Управление памятью. Выделение памяти на GPU и создание ее экземпляра во время выполнения на GPU.
*Генерация случайных чисел для каждого потока с помощью cuRAND.
```
  3.2 Yandex translate
```no-highlight
Механизм трассировки лучей C++ в книге One Weekend никоим образом не является самым быстрым трассировщиком лучей, но перевод вашего кода C++ на CUDA может привести к улучшению скорости в 10 раз или более! Давайте пройдемся по процессу преобразования кода C++ из трассировки лучей за один уик-энд в CUDA. Обратите внимание, что по мере прохождения процесса кодирования на C++ рекомендуется использовать теги git или ветви, чтобы легко вернуться к коду каждой главы. Вы можете сравнить свой код C++ с кодом Питера Ширли по адресу https://github.com/petershirley/raytracinginoneweekend. Попробовав свои силы в использовании CUDA, вы также можете сравнить с моим кодом CUDA по адресу https://github.com/rogerallen/raytracinginoneweekendincuda обязательно используйте ветви, которые я создал для каждой главы. (Например, git checkout ch12_where_next_cuda.)

Этот пост предполагает, что вы понимаете некоторые основы CUDA. Если нет,вы можете начать с еще более простого введения в CUDA здесь, в блоге разработчика NVIDIA. Вам потребуется настроить среду разработки для компиляции и запуска кода CUDA. Мы также будем профилировать с помощью nvprof, поэтому вы можете ознакомиться с тем, как профилировать свой код с помощью nvprof. Код в моем репозитории был написан с использованием Ubuntu Linux и CUDA 9.x, но вы также должны иметь возможность адаптировать эти инструкции к последним выпускам CUDA на Windows или MacOS.

Если вы начинаете с моего файла Makefile, обратите внимание,что я создаю для карты GTX 1070, используя конкретные флаги-gencode для этой карты (-gencode arch=compute_60, code=sm_60). Вы захотите настроить архитектуру и параметры функций для графического процессора или графических процессоров, на которых вы будете работать. В моем файле Makefile основные цели, которые вы будете использовать, - это создание исполняемого файла make cudart и запуск и создание выходного изображения make out.ЦБК.

Обратите внимание, что когда код CUDA работает дольше нескольких секунд, вы можете заметить ошибку 6 (cudaErrorLaunchTimeout), и на Windows ваш экран также может отключиться на несколько секунд. Это связано с тем, что ваш оконный менеджер считает, что графический процессор неисправен, когда он не отвечает через определенное время.  Пользователи Linux могут предпринять шаги для решения этой проблемы с помощью этого сообщения.

В этом посте вы узнаете о следующем:

*Единый буфер кадров памяти, который записывается графическим процессором и считывается процессором
*запуск работы рендеринга на GPU
*Написание кода на C++, который может работать как на CPU, так и на GPU
*Проверка более медленного кода с плавающей запятой двойной точности.
*Управление памятью C++. Выделение памяти на GPU и создание ее экземпляра во время выполнения на GPU.
*Генерация случайных чисел в потоке с помощью cuRAND.
```
4 / 5.

Word    |Translator | Lingvolive
--------|-----------|-------------
familiarize| ознакомлять |ознакомлять
malfunctioning | неисправен | -
thread |  поток | нитка 

6.
![Word Cloud](/wordcloud.png)

# Task 2:
 ### Создание сценария изучения онлайн-курса по иностранному языку
## Solution :

  1.Course link: https://stepik.org/course/53562/promo
  2.
  ![Course scheme](/scheme.png)
  3.
  ![Done tasks](/done_tasks.png)


# Task 3:
 ### Создание интерактивного HTML-элемента для озвучивания произношения англоязычных слов
## Solution : 
https://kodaktor.ru/535561f_e3b71

# Task 4:
 ### Разработка сценария использования мобильного приложения Sounds of Speech
## Solution :

