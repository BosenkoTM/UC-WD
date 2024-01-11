# Using Containers for Working with Data
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
 
## Лекция 1. Микросервисы и контейнеры
[Лекция 1](/lectures/1%20-%20intro%20Применение%20контейнеров.pptx)

Перед второй лекцией нужно установить [Docker](https://docs.docker.com/get-docker/) на свой компьютер или виртуальную машину с Linux.

## Лекция 2. Docker

🔹 [Overview of Docker CLI](https://docs.docker.com/engine/reference/run/)

🔹 [Dockerfile](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#add-or-copy%23add-or-copy)

🔹 [Docker Compose CLI](https://docs.docker.com/compose/reference/)


## Лекция 3. Введение в Kubernetes

>Уважаемые студенты, просьба по возможности до начала занятия поставить себе утилиту для работы с Kubernetes – kubectl.
>Это можно сделать по инструкциям из официальной документации для вашей ОС.
>https://kubernetes.io/docs/tasks/tools/install-kubectl/
 
🔹 [kubectl auto-complition](https://kubernetes.io/docs/tasks/tools/included/optional-kubectl-configs-bash-linux/)

🔹 [Minikube](https://kubernetes.io/ru/docs/tasks/tools/install-minikube/)

🔹 [Minishift (OpenShift)](https://www.okd.io/minishift/)

🔹 [KiND](https://kind.sigs.k8s.io/docs/user/quick-start/)

## Лекция 4. Хранение данных и ресурсы

## Лекция 5. Сетевые абстракции Kubernetes

## Лекция 6. Устройство кластера

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
- `2024`. Начало в `9.15`. [Контейнеры](https://forms.gle/2k3z94t1wUXqfj776)
## ТЕСТ 2.  
- `2024`. Начало в `9.15`. [Оркестрация данных](https://forms.gle/xp6uS8rVQ2v7hgdR8)

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

