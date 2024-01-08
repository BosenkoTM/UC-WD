# Integrating-and-Deploying-Software-with-Containers. Using Containers for Working with Data
## Задания на зачет(экзамен)

<details>
<summary> Теоретические вопросы </summary>


 1. Микросервисы и контейнеры.
 2. Платформы Docker.
 3. Основные компоненты Docker. Привести пример.
 4. Основные компоненты Docker Composer. YAML и docker-compose.
 5. Архитектура Docker. Часто используемые команды Docker при создании проекта для анализа данных.
 6. Dockerfile. Основные компоненты.
 7. Распространение образов. Dockerhub. Github.
 8. Cредств оркестрации и кластеризации в экосистеме Docker: Swarm и fleet.
 9. Cредств оркестрации и кластеризации в экосистеме Docker:  Kubernetes и Mesos.
 10. Обеспечение безопасности контейнеров и связанные с этим ограничения.
 11. Kubernetes.
    
</details>

<details>
<summary> Практические задания </summary>
 
- Проект [ticket1](https://github.com/BosenkoTM/exam_prepare_icud_1_2_sem/tree/main#%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82-ticket1);
 
- Проект [ticket2](https://github.com/BosenkoTM/exam_prepare_icud_1_2_sem/tree/main#%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82-ticket2);
  
- Проект [ticket2.2](https://github.com/BosenkoTM/exam_prepare_icud_1_2_sem/tree/main#%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82-ticket22).
 
</details>

## Текущая успеваемость
 [АДЭУ-201](https://docs.google.com/spreadsheets/d/1rldI1A9V3_WH9H8McTe3qNr6o0migXObak8nKQSQg-E/edit?usp=sharing)
 
## Лекция 1. Микросервисы и контейнеры
[МГПУ 02-09-23](/lectures/1%20-%20intro%20Применение%20контейнеров.pptx)

Перед второй лекцией нужно установить [Docker](https://docs.docker.com/get-docker/) на свой компьютер или виртуальную машину с Linux.

## Лекция 2. Docker

<details>
<summary> Docker </summary>
 
🔹 [Сеть контейнеров — это не сложно](https://habr.com/ru/company/timeweb/blog/558612/)

🔹 [Overview of Docker CLI](https://docs.docker.com/engine/reference/run/)

🔹 [10 команд для Docker, без которых вам не обойтись](https://tproger.ru/translations/top-10-docker-commands/)

🔹 [Как начать использовать Docker в своих проектах](https://tproger.ru/translations/how-to-start-using-docker/)

🔹 [50 вопросов по Docker, которые задают на собеседованиях, и ответы на них](https://habr.com/ru/company/southbridge/blog/528206/)

</details>

<details>
<summary> Dockerfile </summary>

🔹 [20 лучших практик по работе с Dockerfile](https://habr.com/ru/company/domclick/blog/546922/)

🔹 [ENTRYPOINT vs CMD: назад к основам](https://habr.com/ru/company/southbridge/blog/329138/)

🔹 [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)

🔹 [Use multi-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/)

🔹 [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#add-or-copy%23add-or-copy)

</details>

<details>
<summary> Docker Compose </summary>

🔹 [Overview of docker-compose CLI](https://docs.docker.com/compose/reference/)

🔹 [Quickstart: Compose and Django](https://docs.docker.com/samples/django/)

🔹 [Compose file version 3 reference](https://docs.docker.com/compose/compose-file/compose-file-v3/)

🔹 [Compose file version 2 reference](https://docs.docker.com/compose/compose-file/compose-file-v2/)

</details>

## Лекция 3. Введение в Kubernetes

>Уважаемые студенты, просьба по возможности до начала занятия поставить себе утилиту для работы с Kubernetes – kubectl.
>Это можно сделать по инструкциям из официальной документации для вашей ОС.
>https://kubernetes.io/docs/tasks/tools/install-kubectl/

<details>
<summary> Делаем работу с kubectl удобнее: </summary>
 
🔹 [kubectl auto-complition](https://kubernetes.io/docs/tasks/tools/included/optional-kubectl-configs-bash-linux/)

🔹 [kubectl aliases](https://github.com/adterskov/kubectl-aliases)

🔹 [kubens - быстрый способ переключения между namespaces в kubectl](https://github.com/ahmetb/kubectx/)

🔹 [kubecolor - раскрашивает вывод kubectl](https://github.com/dty1er/kubecolor/)

</details>

<details>
<summary> Запустить локальный кластер Kubernetes </summary>

🔹 [Minikube](https://kubernetes.io/ru/docs/tasks/tools/install-minikube/)

🔹 [Minishift (OpenShift)](https://www.okd.io/minishift/)

🔹 [KiND](https://kind.sigs.k8s.io/docs/user/quick-start/)

🔹 [Docker Desktop](https://docs.docker.com/desktop/kubernetes/)

</details>

## Лекция 4. Хранение данных и ресурсы
🔹 [Рациональное использование ресурсов в Kubernetes](https://habr.com/ru/company/timeweb/blog/560670/)
## Лекция 5. Сетевые абстракции Kubernetes
🔹 [Configure Liveness, Readiness and Startup Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes)

🔹 [iptables: How Kubernetes Services Direct Traffic to Pods](https://dustinspecker.com/posts/iptables-how-kubernetes-services-direct-traffic-to-pods/)

## Лекция 6. Устройство кластера
🔹 [Kubernetes is so Simple You Can Explore it with Curl](https://blog.tilt.dev/2021/03/18/kubernetes-is-so-simple.html)

🔹 [Как увеличить скорость реакции Kubernetes на отказ узлов кластера?](https://habr.com/ru/company/timeweb/blog/561084/)

## Практические работы
- `ПР 1`. [First Alpine Linux Containers](https://training.play-with-docker.com/ops-s1-hello/).
- `ПР 2`. [Docker for Beginners - Linux](https://training.play-with-docker.com/beginner-linux/).
- `ПР 3`. [Swarm Mode Introduction for IT Pros](https://training.play-with-docker.com/ops-s1-swarm-intro/).
- `ПР 4`. [Swarm mode introduction](https://training.play-with-docker.com/swarm-mode-intro/).
- `ПР 5`. [Application Containerization and Microservice Orchestration](https://training.play-with-docker.com/microservice-orchestration/).
- `ПР 6`. [Docker images deeper dive](https://training.play-with-docker.com/docker-images/).
- `ПР 7`. [Docker Orchestration Hands-on Lab](https://training.play-with-docker.com/orchestration-hol/).
- `ПР 8`. [Security Lab: Capabilities](https://training.play-with-docker.com/security-capabilities/).
- `ПР 9`. [Анализ данных в Docker. Jupyter](https://github.com/BosenkoTM/using-docker-containers-for-data/tree/main).
- `ПР 10`. [Анализ данных Docker. Jupyter+Postgres](https://github.com/BosenkoTM/icdc_10/blob/main/README.md).
## ТЕСТ 1.   
- `14.10.2023`. Начало в `9.15`. [Контейнеры](https://forms.gle/2k3z94t1wUXqfj776)
## ТЕСТ 2.  
- `25.11.2023`. Начало в `9.15`. [Оркестрация данных](https://forms.gle/xp6uS8rVQ2v7hgdR8)
## Семинары
- Семинар `1`. `16/09/2023` [Установка Docker](https://github.com/BosenkoTM/IC-UD_Practice/tree/main/1%20%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0%20DOCKER#1-%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0-docker---ubuntu).
- Семинар `2`. `16/09/2023` [Первый контейнер](https://github.com/BosenkoTM/IC-UD_Practice/tree/main/2%20%D0%9F%D0%B5%D1%80%D0%B2%D1%8B%D0%B9%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80#%D0%BF%D0%B5%D1%80%D0%B2%D1%8B%D0%B9-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80).
- Семинар `3`. `16/09/2023` [Создание образа](https://github.com/BosenkoTM/IC-UD_Practice/tree/main/3%20%D0%A1%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20%D0%BE%D0%B1%D1%80%D0%B0%D0%B7%D0%B0#%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BE%D0%B1%D1%80%D0%B0%D0%B7%D0%B0).
- Семинар `4`. `23/09/2023` [Работа с файлами](https://github.com/BosenkoTM/IC-UD_Practice/tree/main/4%20%D0%A0%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%20%D1%84%D0%B0%D0%B9%D0%BB%D0%B0%D0%BC%D0%B8#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D1%84%D0%B0%D0%B9%D0%BB%D0%B0%D0%BC%D0%B8---%D1%82%D0%B5%D0%BE%D1%80%D0%B8%D1%8F). Создание [TG-BOT 2022](https://github.com/BosenkoTM/tg_bot_2022.git).
- Семинар `5`. `23/09/2023` [Переменные окружения, логи и порты](https://github.com/BosenkoTM/IC-UD_Practice/tree/main/5%20%D0%9F%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D0%B5%20%D0%BE%D0%BA%D1%80%D1%83%D0%B6%D0%B5%D0%BD%D0%B8%D1%8F%2C%20%D0%BB%D0%BE%D0%B3%D0%B8%20%D0%B8%20%D0%BF%D0%BE%D1%80%D1%82%D1%8B#%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D0%B5-%D0%BE%D0%BA%D1%80%D1%83%D0%B6%D0%B5%D0%BD%D0%B8%D1%8F).
- Семинар `6`. [Введение в сети](https://github.com/BosenkoTM/IC-UD_Practice/tree/main/6%20%D0%92%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5%20%D0%B2%20%D1%81%D0%B5%D1%82%D0%B8#%D1%81%D0%B5%D1%82%D0%B8-%D0%B2-docker).
- Семинар `7`. [Веб-приложение в контейнерах](https://github.com/BosenkoTM/IC-UD_Practice/tree/main/7%20%D0%92%D0%B5%D0%B1-%D0%BF%D1%80%D0%B8%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5%20%D0%B2%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D0%B0%D1%85#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D0%B2%D0%B5%D0%B1-%D0%BF%D1%80%D0%B8%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%B8%D1%8F-%D1%81-%D0%BE%D0%B4%D0%BD%D0%B8%D0%BC-%D0%B8-%D0%B4%D0%B2%D1%83%D0%BC%D1%8F-%D0%B2%D0%B5%D0%B1-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0%D0%BC%D0%B8).
- Семинар `8`. [YAML и docker-compose](https://github.com/BosenkoTM/IC-UD_Practice/tree/main/8%20YAML%20%D0%B8%20docker-compose#yaml).
- Семинар `9`. [Веб-приложение в docker-compose](https://github.com/BosenkoTM/IC-UD_Practice/tree/main/9%20%D0%92%D0%B5%D0%B1-%D0%BF%D1%80%D0%B8%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5%20%D0%B2%20docker-compose#%D0%B8%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%86%D0%B8%D0%B8-docker-compose).
- Семинар `10`. `07/10/2023` [Kubernetes for Beginners](https://training.play-with-kubernetes.com/kubernetes-workshop/).
- Семинар `11`. `14/10/2023` [Ход работы](https://github.com/BosenkoTM/IC-UD/blob/main/practice/seminar_11.md#%D1%81%D0%B5%D0%BC%D0%B8%D0%BD%D0%B0%D1%80-11).
- Семинар `12`. [`k8s_1 Общие команды kubectl](https://github.com/BosenkoTM/IC-UD/blob/main/practice/seminar_12.md).

## Основная литература
1. Иан Милл, Эйдан Хобсон Сейерс Docker на практике / пер. с англ. Д. А. Беликов. – М.: ДМК Пресс, 2020. – 516 с. [скачать](https://disk.yandex.ru/d/mrcntkbTLAfHPQ).
2. Моуэт Э. Использование Docker / пер. с англ. А. В. Снастина; науч. ред. А. А. Маркелов. – М.: ДМК Пресс, 2017. – 354 с. [скачать](https://disk.yandex.ru/d/mrcntkbTLAfHPQ).
3. Liz Rice Container Security: Fundamental Technology Concepts that Protect Containerized Applications. O'Reilly Media, 2020, 175 с.[скачать](https://disk.yandex.ru/d/mrcntkbTLAfHPQ).
4. Jordan Lioy Software Containers: The Complete Guide to Virtualization Technology. Create, Use and Deploy Scalable Software with Docker and Kubernetes. 2023, 673с.[скачать](https://disk.yandex.ru/d/mrcntkbTLAfHPQ).
5. Play with Docker Classroom [link](https://training.play-with-docker.com/).
6. Container Training [link](https://container.training/)

<details>
<summary> Дополнительная литература </summary>

7. Certified Kubernetes Application Developer (CKAD) Exam Success Guide [скачать](https://disk.yandex.ru/i/BNsogUl6SAa5Kg).
8. [Linux-контейнеры: изоляция как технологический прорыв](https://habr.com/ru/company/redhatrussia/blog/352052/).
9. [Namespaces](https://habr.com/ru/company/selectel/blog/279281/).
10. [Cgroups](https://habr.com/ru/company/selectel/blog/303190/).
11. [Capabilities](https://habr.com/ru/company/otus/blog/471802/).
12. [Могут ли контейнеры быть безопасными?](https://habr.com/ru/company/oleg-bunin/blog/480630/).
13. [Митап "Stateful-приложения в 2020 году"](https://www.youtube.com/watch?v=ykIh4-616Ic&list=PL8D2P0ruohODzihD0D0FZXkVHXtXbb6w3&index=4&ab_channel=HighLoadChannel).
14. [Jobs & Cronjobs in Kubernetes Cluster](https://medium.com/avmconsulting-blog/jobs-cronjobs-in-kubernetes-cluster-d0e872e3c8c8).
15. [Tоп-10 PromQL запросов для мониторинга Kubernetes](https://habr.com/ru/company/timeweb/blog/562374/).

</details>

## Онлайн сервисы:

1. [Play with Docker](https://labs.play-with-docker.com/).
2. [Katacoda](https://www.katacoda.com/).
3. [Play with Kubernetes](https://labs.play-with-k8s.com/).

