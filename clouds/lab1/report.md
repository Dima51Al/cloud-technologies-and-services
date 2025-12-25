# Отчёт по лабораторной работе №1  

## 1 Цель работы

- Знакомство с облачными сервисами
- Понимание уровней абстракции над инфраструктурой в облаке
- Формирование понимания типов потребления сервисов в сервисной-модели

## 2 Основные модели предоставления облачных услуг

- **IaaS (Infrastructure as a Service)** - аренда виртуальной инфраструктуры: серверов, хранилищ данных и 
других вычислительных ресурсов: пользователь (это часто крупные компании) разворачивает свою систему на чужих серверах
- **PaaS (Platform as a Service)** - готовая платформа с предустановленными настройками под конкретные задачи:
пользователь использует её без необходимости администрирования и установки
- **SaaS (Software as a Service)** - полноценные программы в облаке, которые уже готовы к работе без настроек разработки и т.д.:
для доступа к ним пользователю нужен только интернет

## 3 Ход работы
### 3.1 Необходимо сделать
В соответствии с заданием построить иерархию потребления сервисов:
```
IT Tower → Service Family → Service Type → Service Sub Type → Service Usage Type
```
Где:
- **IT Tower:** самый высокий уровень категоризации, определяющий основную область 
- **Service Family:** группа родственных сервисов внутри IT Tower
- **Service Type:** конкретный сервис AWS с официальным названием
- **Service Sub Type:** тип использования внутри сервиса
- **Service Usage Type:** конкретная метрика


### 3.2 Алгоритм выполнения
- По ```Product Code``` определить ```Service Type``` (даёт однозначное совпадение)
- По ```Service Type``` с помощью дополнительных источников информации найти ```Service Family```
- По семейству определить область, в которую он относится
- По ```Usage Type``` определить тип услуги ```Service Usage Type```
- По типу услуги заполнить более обширный подтип ```Service Sub Type``` 


### 3.3 Типы сервисов (пример заполнения таблицы по ним)

#### **Amazon Macie**

| IT Tower | Service Family | Service Type | Service Sub Type | Service Usage Type | Product Code | Usage Type |
|----------|----------------|--------------|------------------|-------------------|--------------|------------|
| Cloud Services | Security and Identity | **Amazon Macie** | Data Discovery & Classification | | AmazonMacie | |

---

#### **Amazon MSK**

| IT Tower | Service Family | Service Type | Service Sub Type | Service Usage Type | Product Code | Usage Type |
|----------|----------------|--------------|------------------|-------------------|--------------|------------|
| Cloud Services | Analytics | **Amazon Managed Streaming for Apache Kafka** | Broker | Broker Runtime | AmazonMSK | |
| Cloud Services | Analytics | **Amazon Managed Streaming for Apache Kafka** | Storage-GP2 | Storage-GP2 | AmazonMSK | %Storage.GP2 |
| Cloud Services | Analytics | **Amazon Managed Streaming for Apache Kafka** | Data Transfer | Data Transfer | AmazonMSK | %Bytes |

---

#### **Amazon Polly**

| IT Tower | Service Family | Service Type | Service Sub Type | Service Usage Type | Product Code | Usage Type |
|----------|----------------|--------------|------------------|-------------------|--------------|------------|
| Cloud Services | Artificial Intelligence | **Amazon Polly** | Additional Costs | Tax | AmazonPolly | |

---

#### **Amazon Personalize**

| IT Tower | Service Family | Service Type | Service Sub Type | Service Usage Type | Product Code | Usage Type |
|----------|----------------|--------------|------------------|-------------------|--------------|------------|
| Cloud Services | Artificial Intelligence | **Amazon Personalize** | Processing | TPS Hours | AmazonPersonalize | %TPS-hours |
| Cloud Services | Artificial Intelligence | **Amazon Personalize** | Model Training | Training Hours | AmazonPersonalize | %TrainingHour |
| Cloud Services | Artificial Intelligence | **Amazon Personalize** | Data Ingestion | Data Ingestion | AmazonPersonalize | %DataIngestion |

---

#### **Amazon S3**

| IT Tower | Service Family | Service Type | Service Sub Type | Service Usage Type | Product Code | Usage Type |
|----------|----------------|--------------|------------------|-------------------|--------------|------------|
| Storage | Storage&Content Delivery | **Amazon S3** | Data Transfer | Data Transfer | AmazonS3 | %DataTransfer% |
| Storage | Storage&Content Delivery | **Amazon S3** | Glacier | GDA Byte Hours | AmazonS3 | %TimedStorage-GDA-ByteHrs |
| Storage | Storage&Content Delivery | **Amazon S3** | Requests | Tier6 Requests | AmazonS3 | %Requests%Tier6 |


---

#### **Amazon ES**

| IT Tower | Service Family | Service Type | Service Sub Type | Service Usage Type | Product Code | Usage Type |
|----------|----------------|--------------|------------------|-------------------|--------------|------------|
| Cloud Services | Analytics | **Amazon Elastic Search** | Storage | GP2 Storage | AmazonES | %ES:GP2-Storage% |
| Cloud Services | Analytics | **Amazon Elastic Search** | Instance | Heavy Usage | AmazonES | %HeavyUsage% |
| Cloud Services | Analytics | **Amazon Elastic Search** | Cloud Front | CloudFront Transfer | AmazonES | %CloudFront% |


## 4 Выводы

Во время этой работы я:
1. Разобралась в IaaS/PaaS/SaaS на примере AWS
2. Создала 5-уровневую иерархию:
   - IT Tower → Service Family → Service Type → Service Sub Type → Service Usage Type
3. Обработала настоящий AWS Billing
4. Научилась выделять из кодов категории


## 5 Источники информации

1. [AWS Documentation](https://docs.aws.amazon.com/)
2. [AWS Billing - User Guide](https://docs.aws.amazon.com/pdfs/awsaccountbilling/latest/aboutv2/awsaccountbilling-aboutv2.pdf#custom-tags)
3. [Exploring the Basics of AWS: AWS Cheat Sheet](https://www.whizlabs.com/blog/aws-cheat-sheet/)

