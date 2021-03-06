---
title: 'Zelf studie: een machine learning model maken met R'
titleSuffix: Azure Machine Learning
description: In deze zelf studie gebruikt u de Azure Machine Learning R SDK voor het maken van een logistiek regressie model dat de kans op een onherstelbare fout in een auto ongeval voorspelt.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.reviewer: sgilley
author: revodavid
ms.author: davidsmi
ms.date: 02/07/2020
ms.openlocfilehash: 5b1c6561519bc25c2b7ac77f0a25eff89413a07a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "81256481"
---
# <a name="tutorial-use-r-to-create-a-machine-learning-model"></a>Zelf studie: een machine learning model maken met R
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In deze zelf studie gebruikt u de Azure Machine Learning R SDK voor het maken van een logistiek regressie model dat de kans op een onherstelbare fout in een auto ongeval voorspelt. U ziet hoe de Azure Machine Learning cloud resources met R werken om een schaal bare omgeving te bieden voor het trainen en implementeren van een model.  

In deze zelfstudie voert u de volgende taken uit:
> [!div class="checklist"]
> * Een Azure Machine Learning-werk ruimte maken
> * Een notitieblokmap-map klonen met de bestanden die nodig zijn om deze zelf studie uit te voeren in uw werk ruimte
> * RStudio openen vanuit uw werk ruimte
> * Gegevens laden en trainingen voorbereiden
> * Gegevens uploaden naar een gegevens opslag, zodat deze beschikbaar is voor externe training
> * Een reken resource maken om het model op afstand te trainen
> * Een model `caret` trainen om de kans op onherstelbaarheid te voors pellen
> * Een Voorspellings eindpunt implementeren
> * Het model testen vanuit R

Als u nog geen abonnement op Azure hebt, maak dan een gratis account aan voordat u begint. Probeer vandaag nog de [gratis of betaalde versie van Azure machine learning](https://aka.ms/AMLFree) .


## <a name="create-a-workspace"></a>Een werkruimte maken

Een Azure Machine Learning-werk ruimte is een Foundation-resource in de cloud die u gebruikt om machine learning modellen te experimenteren, te trainen en te implementeren. Uw Azure-abonnement en resource groep worden gebonden aan een eenvoudig verbruikte object in de service. 

U maakt een werk ruimte via de Azure Portal, een webconsole voor het beheren van uw Azure-resources. 

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal.md)]

>[!IMPORTANT] 
> Noteer uw **werk ruimte** en **abonnement**. U hebt deze nodig om ervoor te zorgen dat u op de juiste plaats uw experiment kunt maken. 


## <a name="clone-a-notebook-folder"></a><a name="azure"></a>Een notitieblokmap-map klonen

In dit voor beeld wordt de Cloud notebook server in uw werk ruimte gebruikt voor een installatie zonder een vooraf geconfigureerde ervaring. Gebruik [uw eigen omgeving](https://azure.github.io/azureml-sdk-for-r/articles/installation.html) als u de controle wilt over uw omgeving, pakketten en afhankelijkheden.

U voltooit de volgende proef installatie en voert stappen uit in Azure Machine Learning Studio, een geconsolideerde interface met machine learning-hulpprogram ma's voor het uitvoeren van data Science-scenario's voor data Wetenschappen van alle vaardigheids niveaus.

1. Meld u aan bij [Azure machine learning Studio](https://ml.azure.com/).

1. Selecteer uw abonnement en de werk ruimte die u hebt gemaakt.

1. Selecteer **notebooks** aan de linkerkant.

1. Open de map met voor **beelden** .

1. Open de map **R** .

1. Open de map met een versie nummer.  Dit nummer vertegenwoordigt de huidige release voor de R SDK.

1. Selecteer de **'... '** aan de rechter kant van de map **vignettes** en selecteer vervolgens **klonen**.

    ![Map klonen](media/tutorial-1st-r-experiment/clone-folder.png)

1. Er wordt een lijst met mappen weer gegeven met alle gebruikers die toegang hebben tot de werk ruimte.  Selecteer de map waarin u de map **vignettes** wilt klonen.

## <a name="a-nameopenopen-rstudio"></a><a name="open">RStudio openen

Gebruik RStudio op een reken instantie of laptop-VM om deze zelf studie uit te voeren.  

1. Selecteer **Compute** aan de linkerkant.

1. Voeg een compute-resource toe als deze nog niet bestaat.

1. Zodra de compute wordt uitgevoerd, gebruikt u de **RStudio** -koppeling om RStudio te openen.

1. In RStudio is uw *vignettes* -map een aantal niveaus lager dan *gebruikers* in de sectie **files** op de rechter benedenhoek.  Selecteer onder *vignettes*de map *Train-and-Deploy-to-ACI* om de bestanden te vinden die nodig zijn in deze zelf studie.

> [!Important]
> De rest van dit artikel bevat dezelfde inhoud als u ziet in de *training-en-Deploy-to-ACI. RMD* -bestand. Als u ervaring hebt met RMarkdown, kunt u de code uit dat bestand gebruiken.  Of u kunt de code fragmenten uit deze of van dit artikel kopiëren/plakken in een R-script of de opdracht regel.  


## <a name="set-up-your-development-environment"></a>De ontwikkelomgeving instellen
De configuratie voor uw ontwikkel werkzaamheden in deze zelf studie bevat de volgende acties:

* De vereiste pakketten installeren
* Verbinding maken met een werk ruimte, zodat uw reken instantie kan communiceren met externe resources
* Een experiment maken om uw uitvoeringen bij te houden
* Een extern Compute-doel maken om te gebruiken voor training

### <a name="install-required-packages"></a>De vereiste pakketten installeren

 * Installeer de nieuwste versie van de KRANs.

    ```R
    # install the latest version from CRAN
    install.packages("azuremlsdk")
    azuremlsdk::install_azureml(envname = 'r-reticulate')
    ```
    
* Of installeer de ontwikkelings versie van GitHub.

    ```R
    # or install the development version from GitHub
    remotes::install_github('https://github.com/Azure/azureml-sdk-for-r')
    azuremlsdk::install_azureml(envname = 'r-reticulate')
    ```

Ga nu verder met het importeren van het **azuremlsdk** -pakket.

```R
library(azuremlsdk)
```

De trainingen en scores scripts`accidents.R` ( `accident_predict.R`en) hebben een aantal aanvullende afhankelijkheden. Als u van plan bent om deze scripts lokaal uit te voeren, zorg er dan voor dat u die vereiste pakketten ook hebt.

### <a name="load-your-workspace"></a>Uw werk ruimte laden
Een werk ruimte-object instantiëren vanuit uw bestaande werk ruimte. Met de volgende code worden de werkruimte details uit het bestand **config. json** geladen. U kunt ook een werk ruimte ophalen [`get_workspace()`](https://azure.github.io/azureml-sdk-for-r/reference/get_workspace.html)met.

```R
ws <- load_workspace_from_config()
```

### <a name="create-an-experiment"></a>Een experiment maken
Een Azure ML-experiment houdt een groep van uitvoeringen bij, meestal uit hetzelfde trainings script. Maak een experiment om de uitvoeringen bij te houden voor het trainen van het caret-model op de ongel ukken-gegevens.

```R
experiment_name <- "accident-logreg"
exp <- experiment(ws, experiment_name)
```

### <a name="create-a-compute-target"></a>Een reken doel maken
Met Azure Machine Learning Compute (AmlCompute), een beheerde service, kunnen gegevenswetenschappers Machine Learning-modellen trainen op clusters met virtuele Azure-machines. Voorbeelden hiervan zijn virtuele machines met GPU-ondersteuning. In deze zelf studie maakt u een AmlCompute-cluster met één knoop punt als uw trainings omgeving. Met de code hieronder wordt het berekenings cluster voor u gemaakt als dit nog niet in uw werk ruimte bestaat.

Mogelijk moet u enkele minuten wachten totdat het berekenings cluster al is ingericht.

```R
cluster_name <- "rcluster"
compute_target <- get_compute(ws, cluster_name = cluster_name)
if (is.null(compute_target)) {
  vm_size <- "STANDARD_D2_V2" 
  compute_target <- create_aml_compute(workspace = ws,
                                       cluster_name = cluster_name,
                                       vm_size = vm_size,
                                       max_nodes = 1)
}

wait_for_provisioning_completion(compute_target)
```

## <a name="prepare-data-for-training"></a>Gegevens voorbereiden voor training
In deze zelf studie wordt gebruikgemaakt van gegevens van het Amerikaanse [National snelweg-verkeer voor beveiliging](https://cdan.nhtsa.gov/tsftables/tsfar.htm) (met dank aan [Mary C. Meyer en Tremika Finney](https://www.stat.colostate.edu/~meyer/airbags.htm)).
Deze gegevensset bevat gegevens van meer dan 25.000 car-crashes in de VS, met variabelen die u kunt gebruiken om de kans op een onherstelbaarheid te voors pellen. Importeer eerst de gegevens in R en transformeer deze in een nieuwe `accidents` data frame voor analyse en exporteer deze naar een `Rdata` bestand.

```R
nassCDS <- read.csv("nassCDS.csv", 
                     colClasses=c("factor","numeric","factor",
                                  "factor","factor","numeric",
                                  "factor","numeric","numeric",
                                  "numeric","character","character",
                                  "numeric","numeric","character"))
accidents <- na.omit(nassCDS[,c("dead","dvcat","seatbelt","frontal","sex","ageOFocc","yearVeh","airbag","occRole")])
accidents$frontal <- factor(accidents$frontal, labels=c("notfrontal","frontal"))
accidents$occRole <- factor(accidents$occRole)
accidents$dvcat <- ordered(accidents$dvcat, 
                          levels=c("1-9km/h","10-24","25-39","40-54","55+"))

saveRDS(accidents, file="accidents.Rd")
```

### <a name="upload-data-to-the-datastore"></a>Gegevens uploaden naar het gegevens archief
Gegevens uploaden naar de Cloud, zodat deze toegankelijk is voor uw externe trainings omgeving. Elke Azure Machine Learning-werk ruimte wordt geleverd met een standaard gegevens opslag waarin de verbindings gegevens worden opgeslagen in de Azure Blob-container die is ingericht in het opslag account dat is gekoppeld aan de werk ruimte. Met de volgende code worden de ongel ukken die u hierboven hebt gemaakt, geüpload naar het gegevens archief.

```R
ds <- get_default_datastore(ws)

target_path <- "accidentdata"
upload_files_to_datastore(ds,
                          list("./accidents.Rd"),
                          target_path = target_path,
                          overwrite = TRUE)
```


## <a name="train-a-model"></a>Een model trainen

Voor deze zelf studie past u een logistiek regressie model toe op uw geüploade gegevens met behulp van uw externe Compute-Cluster. Als u een taak wilt verzenden, moet u het volgende doen:

* Het trainings script voorbereiden
* Een estimator maken
* De taak verzenden

### <a name="prepare-the-training-script"></a>Het trainings script voorbereiden
Er is een trainings `accidents.R` script met de naam voor u in dezelfde map als deze zelf studie. Let op de volgende Details **in het trainings script** dat is gedaan om Azure machine learning te gebruiken voor training:

* Het trainings script gebruikt een argument `-d` om de map te vinden die de trainings gegevens bevat. Wanneer u later uw taak definieert en verzendt, wijst u de gegevens opslag voor dit argument aan. Azure ML koppelt de opslagmap aan het externe cluster voor de trainings taak.
* Met `log_metric_to_run()`het trainings script wordt de uiteindelijke nauw keurigheid vastgelegd als een metriek voor het uitvoeren van een record in azure ml. De Azure ML SDK biedt een set logboek registratie-Api's voor het vastleggen van verschillende metrische gegevens tijdens trainings uitvoeringen. Deze metrische gegevens worden vastgelegd en bewaard in de record voor het experiment run. De metrieken kunnen op elk gewenst moment worden geopend of worden weer gegeven op de detail pagina Details in [Studio](https://ml.azure.com). Zie de [Naslag informatie](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-training-experimentation) voor de volledige set logboek registratie `log_*()`methoden.
* Het trainings script slaat uw model op in een map met de naam **uitvoer**. De `./outputs` map ontvangt een speciale behandeling door Azure ml. Tijdens de training worden bestanden die `./outputs` zijn geschreven naar, automatisch geüpload naar uw door Azure ml uitgevoerde record en worden ze persistent gemaakt als artefacten. Door het getrainde model `./outputs`op te slaan in, kunt u uw model bestand openen en ophalen, zelfs nadat de uitvoering is voltooid en u geen toegang meer hebt tot uw externe trainings omgeving.

### <a name="create-an-estimator"></a>Een estimator maken

Een Azure ML-Estimator kapselt de configuratie gegevens uit die nodig zijn voor het uitvoeren van een trainings script op het reken doel. Azure ML-uitvoeringen worden uitgevoerd als taken met containers op het opgegeven Compute-doel. De docker-installatie kopie die is gemaakt voor uw trainings taak bevat standaard de R, de Azure ML SDK en een set veelgebruikte R-pakketten. Bekijk de volledige lijst met standaard pakketten die hier zijn opgenomen.

Als u de Estimator wilt maken, definieert u het volgende:

* De map die uw scripts bevat die nodig zijn voor`source_directory`training (). Alle bestanden in deze map worden geüpload naar de cluster knooppunt (s) voor uitvoering. De Directory moet uw trainings script en eventuele extra scripts bevatten.
* Het trainings script dat wordt uitgevoerd (`entry_script`).
* Het reken doel (`compute_target`), in dit geval het AmlCompute-cluster dat u eerder hebt gemaakt.
* De para meters die zijn vereist uit`script_params`het trainings script (). Azure ML voert uw trainings script uit als een opdracht regel script met `Rscript`. In deze zelf studie geeft u één argument op voor het script, het Active Directory-koppel punt, waarmee u `ds$path(target_path)`toegang hebt.
* Eventuele omgevings afhankelijkheden die nodig zijn voor de training. De standaard-docker-installatie kopie voor de training bevat al de`caret`drie `e1071`pakketten ( `optparse`, en) die nodig zijn in het trainings script.  U hoeft dus geen aanvullende gegevens op te geven. Als u R-pakketten gebruikt die niet standaard zijn opgenomen, gebruikt u de `cran_packages` para meter Estimator om extra krans pakketten toe te voegen. Zie de [`estimator()`](https://azure.github.io/azureml-sdk-for-r/reference/estimator.html) Naslag informatie voor de volledige set Configureer bare opties.

```R
est <- estimator(source_directory = ".",
                 entry_script = "accidents.R",
                 script_params = list("--data_folder" = ds$path(target_path)),
                 compute_target = compute_target
                 )
```

### <a name="submit-the-job-on-the-remote-cluster"></a>De taak verzenden op het externe cluster

Dien de taak ten slotte in om uit te voeren op uw cluster. `submit_experiment()`retourneert een run-object dat u vervolgens gebruikt voor het uitvoeren van de uitvoering. In totaal duurt de eerste run **ongeveer tien minuten**. Maar voor latere uitvoeringen wordt dezelfde docker-installatie kopie opnieuw gebruikt, zolang de script afhankelijkheden niet worden gewijzigd.  In dit geval wordt de installatie kopie in de cache opgeslagen en is de opstart tijd van de container veel sneller.

```R
run <- submit_experiment(exp, est)
```

U kunt de details van de uitvoering bekijken in RStudio viewer. Als u op de koppeling webweergave klikt, gaat u naar Azure Machine Learning Studio, waar u de uitvoering in de gebruikers interface kunt bewaken.

```R
view_run_details(run)
```

Model training gebeurt op de achtergrond. Wacht totdat het trainen van het model is voltooid voordat u andere code uitvoert.

```R
wait_for_run_completion(run, show_output = TRUE)
```

U--en collega's met toegang tot de werk ruimte: kunnen meerdere experimenten tegelijkertijd verzenden, en in azure ML worden de taken in het berekenings cluster gepland. U kunt zelfs het cluster configureren om automatisch naar meerdere knoop punten te schalen en terug te schalen wanneer de wachtrij geen reken taken meer bevat. Deze configuratie is een rendabele manier waarop teams reken bronnen kunnen delen.

## <a name="retrieve-training-results"></a>Trainings resultaten ophalen
Zodra uw model training is voltooid, hebt u toegang tot de artefacten van uw taak die zijn opgeslagen in de record uitvoeren, inclusief alle metrische gegevens die zijn vastgelegd en het uiteindelijke getrainde model.

### <a name="get-the-logged-metrics"></a>De metrische gegevens van het logboek ophalen
In het trainings script `accidents.R`hebt u een metriek vastgelegd in uw model: de nauw keurigheid van de voor spellingen in de trainings gegevens. U kunt metrische gegevens weer geven in de [Studio](https://ml.azure.com)of als volgt uitpakken naar de lokale sessie als een R-lijst:

```R
metrics <- get_run_metrics(run)
metrics
```

Als u meerdere experimenten hebt uitgevoerd (bijvoorbeeld door verschillende variabelen, algoritmen of hyperparamers), kunt u de metrische gegevens van elke uitvoering gebruiken om te vergelijken en het model kiezen dat u in productie gaat gebruiken.

### <a name="get-the-trained-model"></a>Het getrainde model ophalen
U kunt het getrainde model ophalen en de resultaten bekijken in uw lokale R-sessie. Met de volgende code wordt de inhoud van de `./outputs` map gedownload, inclusief het model bestand.

```R
download_files_from_run(run, prefix="outputs/")
accident_model <- readRDS("outputs/model.rds")
summary(accident_model)
```

U ziet enkele factoren die bijdragen aan een toename van de geschatte waarschijnlijkheid van de dood:

* hogere snelheid 
* mannelijk-stuur programma
* oudere inzittende
* interieur

U ziet een lagere waarschijnlijkheid van overlijden met:

* aanwezigheid van airbags
* aanwezigheids seatbelts
* Front-bots 

Het jaar van de productie van het Voer tuig heeft geen significant effect.

U kunt dit model gebruiken om nieuwe voor spellingen te maken:

```R
newdata <- data.frame( # valid values shown below
 dvcat="10-24",        # "1-9km/h" "10-24"   "25-39"   "40-54"   "55+"  
 seatbelt="none",      # "none"   "belted"  
 frontal="frontal",    # "notfrontal" "frontal"
 sex="f",              # "f" "m"
 ageOFocc=16,          # age in years, 16-97
 yearVeh=2002,         # year of vehicle, 1955-2003
 airbag="none",        # "none"   "airbag"   
 occRole="pass"        # "driver" "pass"
 )

## predicted probability of death for these variables, as a percentage
as.numeric(predict(accident_model,newdata, type="response")*100)
```

## <a name="deploy-as-a-web-service"></a>Als webservice implementeren

Met uw model kunt u het risico van de dood van een botsing voors pellen. Gebruik Azure ML om uw model te implementeren als een Voorspellings service. In deze zelf studie gaat u de webservice in [Azure container instances](https://docs.microsoft.com/azure/container-instances/) (ACI) implementeren.

### <a name="register-the-model"></a>Het model registreren

Registreer eerst het model dat u naar uw werk ruimte hebt [`register_model()`](https://azure.github.io/azureml-sdk-for-r/reference/register_model.html)gedownload met. Een geregistreerd model kan elk verzameling bestanden zijn, maar in dit geval is het R-model object voldoende. Azure ML gebruikt het geregistreerde model voor de implementatie.

```R
model <- register_model(ws, 
                        model_path = "outputs/model.rds", 
                        model_name = "accidents_model",
                        description = "Predict probablity of auto accident")
```

### <a name="define-the-inference-dependencies"></a>De Afleidings afhankelijkheden definiëren
Als u een webservice voor uw model wilt maken, moet u eerst een score script (`entry_script`), een R-script maken dat als invoer variabele waarden (in JSON-indeling) gaat en een voor spelling van het model uitvoeren. Voor deze zelf studie gebruikt u het gegeven score `accident_predict.R`bestand. Het Score script moet een `init()` methode bevatten die uw model laadt en een functie retourneert die het model gebruikt om een voor spelling te maken op basis van de invoer gegevens. Raadpleeg de [documentatie](https://azure.github.io/azureml-sdk-for-r/reference/inference_config.html#details) voor meer informatie.

Definieer vervolgens een Azure ML- **omgeving** voor de pakket afhankelijkheden van uw script. Met een omgeving geeft u R-pakketten (van KRAN of elders) op die nodig zijn om uw script uit te voeren. U kunt ook de waarden van omgevings variabelen opgeven waarnaar het script kan verwijzen om het gedrag te wijzigen. Standaard bouwt Azure ML dezelfde standaard docker-installatie kopie die wordt gebruikt met de Estimator for training. Omdat de zelf studie geen speciale vereisten heeft, kunt u een omgeving maken zonder speciale kenmerken.

```R
r_env <- r_environment(name = "basic_env")
```

Als u in plaats daarvan uw eigen docker-installatie kopie wilt gebruiken voor implementatie `custom_docker_image` , geeft u de para meter op. Zie de [`r_environment()`](https://azure.github.io/azureml-sdk-for-r/reference/r_environment.html) Naslag informatie voor de volledige set Configureer bare opties voor het definiëren van een omgeving.

Nu hebt u alles wat u nodig hebt om een configuratie voor het afwijzen van een **interferentie** te maken voor het inkapselen van uw score script en omgevings afhankelijkheden.

```R
inference_config <- inference_config(
  entry_script = "accident_predict.R",
  environment = r_env)
```

### <a name="deploy-to-aci"></a>Implementeren naar ACI
In deze zelf studie implementeert u uw service naar ACI. Met deze code wordt één container ingericht om te reageren op binnenkomende aanvragen, die geschikt is voor testen en lichte belasting. Zie [`aci_webservice_deployment_config()`](https://azure.github.io/azureml-sdk-for-r/reference/aci_webservice_deployment_config.html) voor aanvullende Configureer bare opties. (Voor implementaties op productie schaal kunt u ook [implementeren naar Azure Kubernetes service](https://azure.github.io/azureml-sdk-for-r/articles/deploy-to-aks/deploy-to-aks.html).)

``` R
aci_config <- aci_webservice_deployment_config(cpu_cores = 1, memory_gb = 0.5)
```

U implementeert uw model nu als een webservice. De implementatie **kan enkele minuten duren**. 

```R
aci_service <- deploy_model(ws, 
                            'accident-pred', 
                            list(model), 
                            inference_config, 
                            aci_config)

wait_for_deployment(aci_service, show_output = TRUE)
```

## <a name="test-the-deployed-service"></a>De geïmplementeerde service testen

Nu uw model als een service is geïmplementeerd, kunt u de service testen vanuit R met [`invoke_webservice()`](https://azure.github.io/azureml-sdk-for-r/reference/invoke_webservice.html).  Geef een nieuwe set gegevens op om te voors pellen, converteer deze naar JSON en verzend deze naar de service.

```R
library(jsonlite)

newdata <- data.frame( # valid values shown below
 dvcat="10-24",        # "1-9km/h" "10-24"   "25-39"   "40-54"   "55+"  
 seatbelt="none",      # "none"   "belted"  
 frontal="frontal",    # "notfrontal" "frontal"
 sex="f",              # "f" "m"
 ageOFocc=22,          # age in years, 16-97
 yearVeh=2002,         # year of vehicle, 1955-2003
 airbag="none",        # "none"   "airbag"   
 occRole="pass"        # "driver" "pass"
 )

prob <- invoke_webservice(aci_service, toJSON(newdata))
prob
```

U kunt ook het HTTP-eind punt van de webservice ophalen, waarmee REST-client aanroepen worden geaccepteerd. Dit eindpunt kan worden gedeeld met iedereen die de webservice wil testen of wil integreren in een toepassing.

```R
aci_service$scoring_uri
```

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder de resources wanneer u deze niet meer nodig hebt. Verwijder geen resources die u nog steeds wilt gebruiken. 

De webservice verwijderen:
```R
delete_webservice(aci_service)
```

Het geregistreerde model verwijderen:
```R
delete_model(model)
```

Het berekenings cluster verwijderen:
```R
delete_compute(compute)
```

### <a name="delete-everything"></a>Alles verwijderen

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

U kunt de resourcegroep ook bewaren en slechts één werkruimte verwijderen. Geef de werk ruimte-eigenschappen weer en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

* Nu u uw eerste Azure Machine Learning experiment in R hebt voltooid, kunt u meer informatie vinden over de [Azure machine learning SDK voor r](https://azure.github.io/azureml-sdk-for-r/index.html).

* Meer informatie over Azure Machine Learning met R in de voor beelden in de andere *vignettes* -mappen.
