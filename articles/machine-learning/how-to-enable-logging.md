---
title: Logboek registratie inschakelen in Azure Machine Learning
description: Meer informatie over het inschakelen van logboek registratie in Azure Machine Learning met behulp van zowel het standaard python-logboek registratie pakket als het gebruik van specifieke SDK-functies.
ms.author: trbye
author: trevorbye
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: trbye
ms.date: 03/05/2020
ms.openlocfilehash: 73b9ae6bc3c15526bfdafd74330c7b86286631b1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "78396152"
---
# <a name="enable-logging-in-azure-machine-learning"></a>Logboek registratie inschakelen in Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Met de Azure Machine Learning python-SDK kunt u logboek registratie inschakelen met zowel het standaard python-logboek registratie pakket als met de SDK-specifieke functionaliteit voor lokale logboek registratie en logboek registratie in uw werk ruimte in de portal. Logboeken bieden ontwikkel aars met realtime informatie over de toepassings status en kunnen helpen bij het vaststellen van fouten of waarschuwingen. In dit artikel leert u verschillende manieren om logboek registratie in te scha kelen op de volgende gebieden:

> [!div class="checklist"]
> * Trainings modellen en reken doelen
> * Installatie kopie maken
> * Geïmplementeerde modellen
> * Python `logging` -instellingen

[Maak een Azure machine learning-werk ruimte](how-to-manage-workspace.md). Gebruik de [gids](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) voor meer informatie over de SDK.

## <a name="training-models-and-compute-target-logging"></a>Trainings modellen en logboek registratie van Compute-doelen

Er zijn meerdere manieren om logboek registratie in te scha kelen tijdens het model trainings proces en de voor beelden die worden weer gegeven, illustreren veelvoorkomende ontwerp patronen. U kunt aan de hand van de `start_logging` functie van de- `Experiment` klasse eenvoudig uitvoer gegevens vastleggen in uw werk ruimte in de Cloud.

```python
from azureml.core import Experiment

exp = Experiment(workspace=ws, name='test_experiment')
run = exp.start_logging()
run.log("test-val", 10)
```

Zie de referentie documentatie voor de klasse [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) voor extra logboek registratie functies.

Als u de lokale logboek registratie van de toepassings status tijdens de trainings `show_output` voortgang wilt inschakelen, gebruikt u de para meter. Als u uitgebreide logboek registratie inschakelt, kunt u de details van het trainings proces en informatie over externe bronnen of COMPUTE-doelen bekijken. Gebruik de volgende code om logboek registratie in te scha kelen bij het verzenden van experimenten.

```python
from azureml.core import Experiment

experiment = Experiment(ws, experiment_name)
run = experiment.submit(config=run_config_object, show_output=True)
```

U kunt ook dezelfde para meter in de `wait_for_completion` functie gebruiken bij de resulterende uitvoering.

```python
run.wait_for_completion(show_output=True)
```

De SDK biedt ook ondersteuning voor het gebruik van het standaard pakket voor python-logboek registratie in bepaalde scenario's voor training. In het volgende voor beeld wordt een logboek `INFO` registratie niveau `AutoMLConfig` van een object ingeschakeld.

```python
from azureml.train.automl import AutoMLConfig
import logging

automated_ml_config = AutoMLConfig(task='regression',
                                   verbosity=logging.INFO,
                                   X=your_training_features,
                                   y=your_training_labels,
                                   iterations=30,
                                   iteration_timeout_minutes=5,
                                   primary_metric="spearman_correlation")
```

U kunt ook de `show_output` para meter gebruiken bij het maken van een persistent Compute-doel. Geef de para meter op `wait_for_completion` in de functie om logboek registratie in te scha kelen tijdens het maken van het reken doel.

```python
from azureml.core.compute import ComputeTarget

compute_target = ComputeTarget.attach(
    workspace=ws, name="example", attach_configuration=config)
compute.wait_for_completion(show_output=True)
```

## <a name="logging-for-deployed-models"></a>Logboek registratie voor geïmplementeerde modellen

Als u logboeken van een eerder geïmplementeerde webservice wilt ophalen, laadt u `get_logs()` de service en gebruikt u de functie. De logboeken bevatten mogelijk gedetailleerde informatie over eventuele fouten die zijn opgetreden tijdens de implementatie.

```python
from azureml.core.webservice import Webservice

# load existing web service
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```

U kunt ook aangepaste stack traceringen voor uw webservice registreren door Application Insights in te scha kelen, waarmee u aanvraag-en reactie tijden, fout tarieven en uitzonde ringen kunt bewaken. Roep de `update()` functie aan op een bestaande webservice om Application Insights in te scha kelen.

```python
service.update(enable_app_insights=True)
```

Zie [gegevens bewaken en verzamelen van ml-webservice-eind punten](how-to-enable-app-insights.md)voor meer informatie.

## <a name="python-native-logging-settings"></a>Systeem eigen logboek registratie-instellingen van python

Bepaalde Logboeken in de SDK bevatten mogelijk een fout die aangeeft dat u het logboek registratie niveau wilt instellen op fout opsporing. Als u het logboek registratie niveau wilt instellen, voegt u de volgende code toe aan het script.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## <a name="next-steps"></a>Volgende stappen

* [Gegevens van ML-webservice-eind punten bewaken en verzamelen](how-to-enable-app-insights.md)