---
title: Python-toepassingen bewaken met Azure Monitor (preview) | Microsoft Docs
description: Biedt instructies om opentellingen python te bekabelen met Azure Monitor
ms.topic: conceptual
author: reyang
ms.author: reyang
ms.date: 10/11/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 6b8343d08962d8ce749e1160b0226b68571571f8
ms.sourcegitcommit: fc0431755effdc4da9a716f908298e34530b1238
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/24/2020
ms.locfileid: "83815720"
---
# <a name="set-up-azure-monitor-for-your-python-application"></a>Azure Monitor instellen voor uw python-toepassing

Azure Monitor ondersteunt gedistribueerde tracering, metrische verzameling en logboek registratie van python-toepassingen via integratie met [Opentellingen](https://opencensus.io). Dit artikel begeleidt u stapsgewijs door het proces voor het instellen van opentellingen voor python en het verzenden van uw bewakings gegevens naar Azure Monitor.

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u nog geen abonnement voor Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
- Installatie van python. In dit artikel wordt [python 3.7.0](https://www.python.org/downloads/)gebruikt, hoewel eerdere versies waarschijnlijk met kleine wijzigingen werken.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-an-application-insights-resource-in-azure-monitor"></a>Een Application Insights resource in Azure Monitor maken

Eerst moet u een Application Insights-resource in Azure Monitor maken, waarmee een instrumentatie sleutel (Ikey) wordt gegenereerd. De iKey wordt vervolgens gebruikt voor het configureren van de opentellings-SDK voor het verzenden van telemetriegegevens naar Azure Monitor.

1. Selecteer **een resource maken**  >  **hulp programma voor ontwikkel aars**  >  **Application Insights**.

   ![Een Application Insights resource toevoegen](./media/opencensus-python/0001-create-resource.png)

1. Er wordt een configuratie venster weer gegeven. Gebruik de volgende tabel om de invoer velden in te vullen.

   | Instelling        | Waarde           | Beschrijving  |
   | ------------- |:-------------|:-----|
   | **Naam**      | Wereld wijd unieke waarde | Naam waarmee de app wordt geïdentificeerd die u bewaken |
   | **Resource Group**     | myResourceGroup      | Naam voor de nieuwe resource groep voor het hosten van Application Insights gegevens |
   | **Locatie** | VS - oost | Een locatie bij u in de buurt of in de buurt van waar de app wordt gehost |

1. Selecteer **Maken**.

## <a name="instrument-with-opencensus-python-sdk-for-azure-monitor"></a>Instrument met opentellingen python SDK voor Azure Monitor

Installeer de Azure Monitor Exporters van opentellingen:

```console
python -m pip install opencensus-ext-azure
```

Zie [pakket met Opentellingen](https://docs.microsoft.com/azure/azure-monitor/app/nuget#common-packages-for-python-using-opencensus)voor een volledige lijst met pakketten en integraties.

> [!NOTE]
> De `python -m pip install opencensus-ext-azure` opdracht gaat ervan uit dat u een `PATH` omgevings variabele hebt ingesteld voor uw python-installatie. Als u deze variabele niet hebt geconfigureerd, moet u het volledige mappad opgeven waar uw python-uitvoerbaar bestand zich bevindt. Het resultaat is een opdracht die er als volgt uitziet: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure` .

De SDK gebruikt drie Azure Monitor Exporters voor het verzenden van verschillende soorten telemetrie naar Azure Monitor: tracering, metrische gegevens en Logboeken. Zie [het overzicht van het gegevens platform](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform)voor meer informatie over deze typen telemetrie. Gebruik de volgende instructies om deze typen telemetrie via de drie Exporters te verzenden.

## <a name="telemetry-type-mappings"></a>Toewijzingen van het type telemetrie

Dit zijn de Exporters die opentellingen bieden die zijn gekoppeld aan de typen telemetrie die u ziet in Azure Monitor.

![Scherm opname van de toewijzing van telemetrie-typen van opentelling naar Azure Monitor](./media/opencensus-python/0012-telemetry-types.png)

### <a name="trace"></a>Tracering

> [!NOTE]
> `Trace`in opentelling verwijst naar [gedistribueerde tracering](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing). De `AzureExporter` verzonden `requests` en `dependency` telemetrie naar Azure monitor.

1. Eerst gaan we een aantal tracerings gegevens lokaal genereren. Voer de volgende code in python niet-actief of uw editor van keuze in.

    ```python
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer

    tracer = Tracer(sampler=ProbabilitySampler(1.0))

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2. Als u de code uitvoert, wordt u herhaaldelijk gevraagd om een waarde in te voeren. Bij elke vermelding wordt de waarde afgedrukt op de shell en wordt door de module opentellingen python een bijbehorend onderdeel gegenereerd `SpanData` . Het project opentelling definieert een [tracering als een boom structuur van reeksen](https://opencensus.io/core-concepts/tracing/).
    
    ```
    Enter a value: 4
    4
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='15ac5123ac1f6847', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:22.805429Z', end_time='2019-06-27T18:21:44.933405Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 25
    25
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='2e512f846ba342de', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:44.933405Z', end_time='2019-06-27T18:21:46.156787Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 100
    100
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='f3f9f9ee6db4740a', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:46.157732Z', end_time='2019-06-27T18:21:47.269583Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    ```

3. Hoewel het invoeren van waarden nuttig is voor demonstratie doeleinden, willen we uiteindelijk de `SpanData` Azure monitor verzenden. Geef uw connection string rechtstreeks door aan de exporteur of u kunt dit opgeven in een omgevings variabele `APPLICATIONINSIGHTS_CONNECTION_STRING` . Wijzig uw code uit de vorige stap op basis van het volgende code voorbeeld:

    ```python
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    tracer = Tracer(
        exporter=AzureExporter(
            connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000'),
        sampler=ProbabilitySampler(1.0),
    )

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

4. Wanneer u nu het python-script uitvoert, moet u nog steeds worden gevraagd om waarden in te voeren, maar alleen de waarde wordt afgedrukt in de shell. Het gemaakte `SpanData` wordt verzonden naar Azure monitor. U vindt de verzonden gegevens over het bereik onder `dependencies` . Zie voor meer informatie over uitgaande aanvragen opentellingen python- [afhankelijkheden](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python-dependency).
Zie voor meer informatie over inkomende aanvragen opentellingen python- [aanvragen](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python-request).

#### <a name="sampling"></a>Steekproeven

Voor informatie over de bemonstering van opentellingen bekijkt u de [bemonstering in Opentellingen](sampling.md#configuring-fixed-rate-sampling-for-opencensus-python-applications).

#### <a name="trace-correlation"></a>Correlatie traceren

Voor meer informatie over de correlatie van de telemetrie in uw traceer gegevens, Bekijk dan de relatie met de python- [telemetrie](https://docs.microsoft.com/azure/azure-monitor/app/correlation#telemetry-correlation-in-opencensus-python)van opentellingen.

#### <a name="modify-telemetry"></a>Telemetrie wijzigen

Zie voor meer informatie over het wijzigen van bijgehouden telemetrie voordat deze wordt verzonden naar Azure Monitor, de python- [telemetrie-processors](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors)van opentellingen.

### <a name="metrics"></a>Metrische gegevens

1. Eerst gaan we een aantal lokale metrische gegevens genereren. We maken een eenvoudige metriek om het aantal keren dat de gebruiker op ENTER drukt, te volgen.

    ```python
    from datetime import datetime
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```
2. Als u de code uitvoert, wordt u herhaaldelijk gevraagd om op ENTER te drukken. Er wordt een metriek gemaakt om het aantal keren dat de invoer wordt ingedrukt, bij te houden. Bij elke vermelding wordt de waarde verhoogd en wordt de metrische informatie weer gegeven in de-console. De informatie bevat de huidige waarde en het huidige tijds tempel waarop de metriek is bijgewerkt.

    ```
    Press enter.
    Point(value=ValueLong(5), timestamp=2019-10-09 20:58:04.930426)
    Press enter.
    Point(value=ValueLong(6), timestamp=2019-10-09 20:58:06.570167)
    Press enter.
    Point(value=ValueLong(7), timestamp=2019-10-09 20:58:07.138614)
    ```

3. Hoewel het invoeren van waarden nuttig is voor demonstratie doeleinden, willen we uiteindelijk de metrische gegevens naar Azure Monitor verzenden. Geef uw connection string rechtstreeks door aan de exporteur of u kunt dit opgeven in een omgevings variabele `APPLICATIONINSIGHTS_CONNECTION_STRING` . Wijzig uw code uit de vorige stap op basis van het volgende code voorbeeld:

    ```python
    from datetime import datetime
    from opencensus.ext.azure import metrics_exporter
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    # TODO: replace the all-zero GUID with your instrumentation key.
    exporter = metrics_exporter.new_metrics_exporter(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')

    view_manager.register_exporter(exporter)

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```

4. De export functie verzendt metrische gegevens naar Azure Monitor met een vast interval. De standaard waarde is elke 15 seconden. Er wordt één metrische waarde bijgehouden, dus deze metrische gegevens, met een wille keurige vermeldings-en tijds tempel, worden elk interval verzonden. U kunt de gegevens vinden onder `customMetrics` .

#### <a name="standard-metrics"></a>Standaard metrische gegevens

Standaard wordt door de export functie voor metrische gegevens een aantal standaard metrische gegevens naar Azure Monitor verzonden. U kunt dit uitschakelen door de `enable_standard_metrics` vlag in te stellen op `False` de constructor van de metrische gegevens Exporter.

```python
...
exporter = metrics_exporter.new_metrics_exporter(
  enable_standard_metrics=False,
  connection_string='InstrumentationKey=<your-instrumentation-key-here>')
...
```
Hieronder ziet u een lijst met standaard metrische gegevens die momenteel worden verzonden:

- Beschikbaar geheugen (bytes)
- Processor tijd CPU (percentage)
- Binnenkomende aanvraag frequentie (per seconde)
- Gemiddelde uitvoerings tijd inkomende aanvragen (milliseconden)
- Uitgaande aanvraag frequentie (per seconde)
- CPU-gebruik verwerken (percentage)
- Privé bytes verwerken (bytes)

U kunt deze metrische gegevens weer geven in `performanceCounters` . De frequentie van binnenkomende aanvragen is onder `customMetrics` . Zie [prestatie meter items](https://docs.microsoft.com/azure/azure-monitor/app/performance-counters)voor meer informatie.

#### <a name="modify-telemetry"></a>Telemetrie wijzigen

Zie voor meer informatie over het wijzigen van bijgehouden telemetrie voordat deze wordt verzonden naar Azure Monitor, de python- [telemetrie-processors](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors)van opentellingen.

### <a name="logs"></a>Logboeken

1. Eerst gaan we een aantal lokale logboek gegevens genereren.

    ```python
    import logging

    logger = logging.getLogger(__name__)

    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2.  De code vraagt voortdurend naar een waarde die moet worden ingevoerd. Voor elke ingevoerde waarde wordt een logboek vermelding verzonden.

    ```
    Enter a value: 24
    24
    Enter a value: 55
    55
    Enter a value: 123
    123
    Enter a value: 90
    90
    ```

3. Hoewel het invoeren van waarden nuttig is voor demonstratie doeleinden, willen we uiteindelijk de logboek gegevens naar Azure Monitor verzenden. Geef uw connection string rechtstreeks door aan de exporteur of u kunt dit opgeven in een omgevings variabele `APPLICATIONINSIGHTS_CONNECTION_STRING` . Wijzig uw code uit de vorige stap op basis van het volgende code voorbeeld:

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

4. De export functie stuurt logboek gegevens naar Azure Monitor. U kunt de gegevens vinden onder `traces` . 

    > [!NOTE]
    > `traces`in deze context is niet hetzelfde als `Tracing` . `traces`verwijst naar het type telemetrie dat u in Azure Monitor kunt zien wanneer u de gebruikt `AzureLogHandler` . `Tracing`verwijst naar een concept in opentellingen en is gekoppeld aan [gedistribueerde tracering](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing).

5. Als u uw logboek berichten wilt Format teren, kunt u gebruiken `formatters` in de ingebouwde python- [logboek registratie-API](https://docs.python.org/3/library/logging.html#formatter-objects).

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    format_str = '%(asctime)s - %(levelname)-8s - %(message)s'
    date_format = '%Y-%m-%d %H:%M:%S'
    formatter = logging.Formatter(format_str, date_format)
    # TODO: replace the all-zero GUID with your instrumentation key.
    handler = AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    handler.setFormatter(formatter)
    logger.addHandler(handler)
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

6. U kunt ook aangepaste eigenschappen toevoegen aan uw logboek berichten in het argument *extra* tref woord met behulp van het veld custom_dimensions. Deze worden weer gegeven als sleutel-waardeparen in `customDimensions` in azure monitor.
    > [!NOTE]
    > Als u deze functie wilt gebruiken, moet u een woorden lijst door geven aan het veld custom_dimensions. Als u argumenten van elk ander type doorgeeft, worden deze door de logboeken genegeerd.

    ```python
    import logging
    
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )

    properties = {'custom_dimensions': {'key_1': 'value_1', 'key_2': 'value_2'}}

    # Use properties in logging statements
    logger.warning('action', extra=properties)
    ```

#### <a name="sending-exceptions"></a>Uitzonde ringen verzenden

Met opentellingen python worden telemetrie niet automatisch bijgehouden en verzonden `exception` . Ze worden via de `AzureLogHandler` logboek bibliotheek van python verzonden met behulp van uitzonde ringen. U kunt aangepaste eigenschappen toevoegen, net als bij normale logboek registratie.

```python
import logging

from opencensus.ext.azure.log_exporter import AzureLogHandler

logger = logging.getLogger(__name__)
# TODO: replace the all-zero GUID with your instrumentation key.
logger.addHandler(AzureLogHandler(
    connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
)

properties = {'custom_dimensions': {'key_1': 'value_1', 'key_2': 'value_2'}}

# Use properties in exception logs
try:
    result = 1 / 0  # generate a ZeroDivisionError
except Exception:
    logger.exception('Captured an exception.', extra=properties)
```
Aangezien u uitzonde ringen expliciet moet vastleggen, is het aan de gebruiker te weten hoe ze niet-verwerkte uitzonde ringen willen vastleggen. Opentelling heeft geen beperkingen voor de manier waarop een gebruiker dit wil doen, zolang een telemetrie van een uitzonde ring expliciet wordt vastgelegd.

#### <a name="sampling"></a>Steekproeven

Voor informatie over de bemonstering van opentellingen bekijkt u de [bemonstering in Opentellingen](sampling.md#configuring-fixed-rate-sampling-for-opencensus-python-applications).

#### <a name="log-correlation"></a>Logboekcorrelatie

Zie voor meer informatie over het verrijken van uw logboeken met tracerings context gegevens integratie met opentellingen python [Logboeken](https://docs.microsoft.com/azure/azure-monitor/app/correlation#log-correlation).

#### <a name="modify-telemetry"></a>Telemetrie wijzigen

Zie voor meer informatie over het wijzigen van bijgehouden telemetrie voordat deze wordt verzonden naar Azure Monitor, de python- [telemetrie-processors](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors)van opentellingen.

## <a name="view-your-data-with-queries"></a>Uw gegevens weer geven met query's

U kunt de telemetriegegevens weer geven die vanuit uw toepassing zijn verzonden via het tabblad **Logboeken (analyse)** .

![Scherm opname van het deel venster overzicht met de melding ' logboeken (analyse) ' geselecteerd in een rood vak](./media/opencensus-python/0010-logs-query.png)

In de lijst onder **actief**:

- Voor telemetrie die wordt verzonden met de Azure Monitor Trace-export functie worden binnenkomende aanvragen weer gegeven onder `requests` . Uitgaand of in behandeling zijnde aanvragen worden weer gegeven onder `dependencies` .
- Voor telemetrie die met de export functie voor metrische gegevens van Azure Monitor worden verzonden, worden verzonden metrische gegevens weer gegeven onder `customMetrics` .
- Voor telemetrie die wordt verzonden met de export functie van de Azure Monitor logboeken, worden logboeken weer gegeven onder `traces` . Uitzonde ringen worden weer gegeven onder `exceptions` .

Zie [Logboeken in azure monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs)voor meer gedetailleerde informatie over het gebruik van query's en Logboeken.

## <a name="learn-more-about-opencensus-for-python"></a>Meer informatie over opentellingen voor python

* [Opentellingen python op GitHub](https://github.com/census-instrumentation/opencensus-python)
* [Aanpassing](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [Azure Monitor-exporteurs op GitHub](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-azure)
* [Integratie van opentellingen](https://github.com/census-instrumentation/opencensus-python#extensions)
* [Voorbeeld toepassingen Azure Monitor](https://github.com/Azure-Samples/azure-monitor-opencensus-python)

## <a name="next-steps"></a>Volgende stappen

* [Binnenkomende aanvragen bijhouden](./../../azure-monitor/app/opencensus-python-dependency.md)
* [Lopende aanvragen bijhouden](./../../azure-monitor/app/opencensus-python-request.md)
* [Toepassings overzicht](./../../azure-monitor/app/app-map.md)
* [End-to-end prestatie bewaking](./../../azure-monitor/learn/tutorial-performance.md)

### <a name="alerts"></a>Waarschuwingen

* [Beschikbaarheidstests](../../azure-monitor/app/monitor-web-app-availability.md): maak tests om ervoor te zorgen dat uw site zichtbaar is op internet.
* [Slimme diagnostische gegevens](../../azure-monitor/app/proactive-diagnostics.md): deze tests worden automatisch uitgevoerd, zodat u niets hoeft te doen om ze in te stellen. Deze geeft aan of een app een ongebruikelijk aantal mislukte aanvragen heeft.
* [Metrische waarschuwingen](../../azure-monitor/platform/alerts-log.md): Stel waarschuwingen in om u te waarschuwen als een metriek een drempel waarde overschrijdt. U kunt deze instellen op aangepaste metrische gegevens die u in uw app codeert.
