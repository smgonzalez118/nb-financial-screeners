# SISTEMA CON FUNCIONALIDADES DE TIPO SCREENER DE ACTIVOS FINANCIEROS.

# MÓDULOS:

## ACTIVOS CASTIGADOS:
Permite determinar activos de renta variable o sectores (vía ETF) que se encuentren más castigados o bajos de precio respecto de sus máximos en una ventana de tiempo n parametrizable. Facilita, así, generar el "margen de seguridad".

## ACTIVOS CASTIGADOS - INDICE DE FUNCIONES:

 - getCastigados: Toma argumento "lideres" (acciones líderes ARG), "general" (acciones del panel general ARG), "cedears" (todos los cedears de ARG), "adrs"(los adrs argentinos) o "sectores" (los etfs de los distintos sectores); y una cantidad de n días hacia atrás como ventana para tomar el máximo histórico, y devuelve un listado de activos ordenados del más al menos castigado con los % de caída desde el último ATH de la ventana.
 
## FILTRO DE TIPO DE TENDENCIA: ADX + DMI

Funcionalidad que permite determinar la existencia de tendencia y la fortaleza de la misma. Si hay tendencia se recomienda usar indicadores de tipo trend following (si hay resistencias - máximos anteriores - informarlo con alertas). Si no hay tendencia, se recomienda usar osciladores contrarian. El ADX (Average Directional Index) es un indicador de análisis técnico, utilizado para conocer si los precios se encuentran en tendencia o en rango y para medir la fuerza de la tendencia. Cuando el ADX es mayor que 30, el mercado se encuentra en una tendencia fuerte, cuando está entre 20 y 30 no está bien definido y cuando es menor a 20 indica que el mercado está en rango (sin tendencia)

## FILTRO DE TIPO DE TENDENCIA - INDICE DE FUNCIONES:

- filterTrend(activos, timeframe, report = False):
    Esta función utiliza el indicador técnico ADX para determinar la fortaleza de la tendencia. Adicionalmente se informa el sentido
    de la tendencia según el sentido de los cruces de los DMI- y DMI+. Si DMI+ > DMI - => Tendencia Alcista, sino Bajista.
    Se utilizan los siguientes rangos: 
        - Sin tendencia:                    0  >= ADX < 25
        - tendencia Fuerte:                 25 >= ADX < 50
        - tendencia Muy Fuerte              50 >= ADX < 75
        - tendencia Extremadamente fuerte   75 >= ADX < 100
- getDMISignals(activos, timeframe):
    Esta función devuelve los cruces vigentes de los indicadores DMI+ y DMI- del listado de activos
    solicitado, así como el sentido del cruce, la fecha de la señal, el precio de la señal, el rendimiento acumulado a la fecha
    y el rendimiento acumulado, todo ordenado en forma descendente por cantidad de días transcurridos desde la señal.


## FILTRO DE EXTREMOS: Sobrecompra y Sobreventa + Posición Bull/Bear (RSI 50)

Funcionalidad que utiliza el indicador técnico RSI para determinar el momentum de los activos seleccionados: Sobrecompra (>= 70), medio (menor a 70 y mayor a 30), o bien Sobreventa (<= 30). Adicionalmente se informa la zona de tendencia del activo (muchas veces utilizada como filtro para sistemas de cruces de medias móviles). Si RSI >= 50: Zona Bull, sino, Zona Bear. Devuelve tablas, tablas estiladas e incluso la info en un diccionario clasificando activos por nivel de SC, SV y zona de tendencia.

## FILTRO DE EXTREMOS - INDICE DE FUNCIONES:

- filterSCSV(activos, timeframe, report = False):
    Esta función utiliza el indicador técnico RSI para determinar el momentum de los activos seleccionados: Sobrecompra (>= 70), medio 
    (menor a 70 y mayor a 30), o bien Sobreventa (<= 30). Adicionalmente se informa la zona de tendencia del activo (muchas veces utilizada 
    como filtro para sistemas de cruces de medias móviles). Si RSI >= 50: Zona Bull, sino, Zona Bear. Devuelve tablas, tablas estiladas e 
    incluso la info en un diccionario clasificando activos por nivel de SC, SV y zona de tendencia.

## FILTROS DE TENDENCIA ESTRUTURAL (STAN WEINSTEIN): Posición Stan Weinstein Inversores (MM30w)+Posición Stan Weinstein Operadores (MM10w)

Informa tendencia estructural según posición, según teorías de Stan Weinstein (autor del bestseller "Secretos para ganar dinero en los mercados alcistas y bajistas". Permite observar estados y también cambios de estado.

## FILTROS DE TENDENCIA ESTRUTURAL (STAN WEINSTEIN) - INDICE DE FUNCIONES:

-filterStanW(activos, report = False):
    Esta función analiza la posición del precio respecto de las medias móviles exponenciales de 10 y 30 semanas para el listado de activos 
    solicitado, siguiendo la teoría de Stan Weinstein. El precio respecto a la MM de 10 semanas es para los "Operadores" o traders de más
    corto plazo, mientras que la MM de 30 semanas es para los "Inversores" o traders de más largo plazo.
    Evalúa dos métricas: 
     - Distancia: La posición del precio respecto de la la MM de 50 ruedas respecto de la MM (10 o 30 week) (si está por encima se considera 
     en posición "Alcista", y si está por debajo se considera posición "Bajista".
    - Fortaleza: la evolución de esa posición definiéndose que si en las últimas 3 semanas esa distancia se amplía al alza,
    la fortaleza es "Creciente" para una tendencia alcista o "Decreciente" si esa distancia se acorta. Viceversa para una
    tendencia Bajista. Nota: debe haber un crecimiento o decrecimiento consecutivo en las 3 semanas para catalogarse como
    "Creciente" o "Decreciente", sino será "No determinada".
    
   Devuelve una tabla con todas las métricas, otra tabla estilada con colores y dos listados con los activos alcistas (operadores e
    inversores).

-getStanSignals(activos, tipo):
    Esta función devuelve los cruces vigentes del precio respecto de las medias exponenciales de 10 o 30 semanas del listado de activos solicitado, así como el sentido del cruce, la fecha de la señal, el precio de la señal, el rendimiento acumulado a la fecha
y el rendimiento acumulado, todo ordenado en forma descendente por cantidad de días transcurridos desde la señal.

## Golden/Death Cross (MM50d-MM200d)

Se analiza el mercado desde la perspectiva del cruce de medias móviles de 50 y 200 sesiones (también llamado "Golden Cross", al alza, o bien "Death Cross" a la baja. Permite observar estado de situación y también últimas señales vigentes.

# Golden/Death Cross - INDICE DE FUNCIONES:

- analyzeGoldenDeath(activos, timeframe):
    Esta función analiza el estado de las medias móviles exponenciales de 50 y 200 ruedas para el listado de activos solicitado.
    Evalúa dos métricas: 
     - Distancia: La posición de la MM de 50 ruedas respecto de la MM de 200 ruedas (si está por encima se considera en
    posición "Alcista" o de "Golden Cross", y si está por debajo se considera posición "Bajista" o "Death Cross").
    - crossover: se analiza la posición de la "Distancia" (simil línea macd) respecto de una EMA de 30 sesiones de la Distancia (simil
    línea señal del macd). Si la distancia actual está por encima de ese promedio móvil, el crossover activado es de alineación "Alcista",
    sino, "Bajista".
- getGoldenDeathSignals(activos, timeframe):
    Esta función devuelve los cruces vigentes de las medias móviles exponenciales de 50 y 200 sesiones del listado de activos
    solicitado, así como el sentido del cruce, la fecha de la señal, el precio de la señal, el rendimiento acumulado a la fecha
    y el rendimiento acumulado, todo ordenado en forma descendente por cantidad de días transcurridos desde la señal.


## Trend following con Histograma MACD

Se utiliza el indicador técnico seguidor de tendencia denominado "MACD" (Moving Average Convergence Divergence) para determinar tendencia y cambios de estados (señales) en la situación de activos de renta variable o índices. Tres subsistemas según compresión temporal (diaria, semanal, mensual): MACD Mensual, MACD Semanal, MACD Diario.

## Trend following con Histograma MACD - INDICE DE FUNCIONES:

- analyzeMACD(activos, timeframe):
    Esta función analiza los activos solicitados con el indicador técnico MACD y calcula la tendencia, dominancia y fortaleza de la 
    dominancia. Definiciones:
        - "Tendencia": si la línea MACD es mayor a 0, la tendencia es alcista, y si la misma es menor a 0, es bajista.
        - "Dominancia": si la línea MACD es mayor a la línea señal, la dominancia es mayor a 0 (histograma positivo), y viceversa.
        - "Fortaleza": - Si la dominancia es alcista: si la diferencia entre las líneas MACD y señal se hace más positiva,
                       la fortaleza es Creciente, y sino, Decreciente.
                       - Si la dominancia es bajista: si la diferencia entre las líneas MACD y señal se hace más negativa,
                       la fortaleza es Creciente, y sino, Decreciente.
    Se pueden consultar sets de activos preconfigurados: "cedears", "adrs", "sectores", "lideres", "general".
    
- analyzeFullMACD(activos):
    Esta función se apoya en la función analyze MACD para generar un análisis multitemporal de los activos especificados.
    Los timeframes analizados son: mensual, semanal y diario. Se pueden consultar sets de activos preconfigurados: "cedears", "adrs", "sectores", "lideres", "general". Devuelve una tabla con estilos rojos/verdes representativos de los estados.
 
- getMACDSignals(activos, timeframe):
    Esta función descarga data financiera de los activos seleccionados, calcula el indicador técnico MACD e identifica las últimas señales vigentes de compra (long) o bien de
    venta en corto (short), devolviendo la fecha en que se produjo la señal, el precio, el rendimiento acumulado al día de la consulta y el rendimiento anualizado. Estos datos
    se devuelven en formato diccionario o dataframe (tabla) para todos los activos consultados. Las señales son en función de dos tipos de criterios:
        - Cruce de la línea MACD de la posición 0, que es equivalente a un crossover de las medias móviles exponenciales de 12 y 26 ruedas;
        - Cruce de la línea MACD a la línea Señal (crossover), siendo la línea Señal una EMA de 9 ruedas de la línea MACD.

## MONITOREO DE VOLATILIDAD

Controlar la evolución de la volatilidad. Utiliza medias móviles de volatilidad y analiza cruces al alza y a la baja. También es importante analizar la volatilidad para ver el tipo de bot a utilizar. Ej.: si estamos temerosos y necesitamos seguridad porque vemos mucha volatilidad en el mercado, asignarle más capital a un bot defensivo cuyo objetivo sea bajar el riesgo de exposición al mercado.). Entonces según eso vamos graduando la exposición al riesgo (y el capital asignado a acciones u estrategias defensivas, etc.). Para controlar la volatilidad/riesgo de los activos y para los loteros.

## MONITOREO DE VOLATILIDAD - ÍNDICE DE FUNCIONES:

 - plotPriceVolat(symbol, desde, hasta, n = 20): Esta función grafica precio y volatilidad de un ticker en particular entre dos fechas especificadas;
 - plotVolat(symbol, desde, hasta, n_fast = 10, n_slow = 20): Esta función plotea dos curvas de volatilidad, una rápida y una lenta y evalúa cruces para determinar zonas de elevada volatilidad y zonas de baja volatilidad. Permite parametrizar la ventana de n ruedas tomadas para cada curva. Las curvas funcionan como medias móviles y la diferencia entre las mismas es representada por un gráfico de barras como un oscilador. Se toma market data desde hasta parametrizable.
 - plotVolatNow(symbol, n_fast = 10, n_slow = 20, period = "1y"): Esta función plotea dos curvas de volatilidad, una rápida y una lenta y evalúa cruces para determinar zonas de elevada volatilidad y zonas de baja volatilidad. Permite parametrizar la ventana de n ruedas tomadas para cada curva. Las curvas funcionan como medias móviles y la diferencia entre las mismas es representada por un gráfico de barras como un oscilador. Se toma el último año de data, hasta hoy.
 
## MONITOREO: VOLUMEN

Funcionalidades para monitorear el volumen de los activos de renta variable. Entre ellas: Función que devuelve un gráfico con 3 paneles con los siguientes datos: 1) gráfico de velas del precio con una media móvil exponencial de 20 ruedas más el panel de volumen; 2) Un panel con dos medias móviles exponenciales de volumen parametrizables (una slow y una fast); 3) Un panel con el indicador técnico de Acumulación Distribución. Función que devuelve métricas de volumen nominal de un ticker seleccionado, para el período seleccionado y en función de indicadores técnicos tipo media móvil exponencial sobre el volumen, una rápida y una lenta, ambas parametrizables. Función sirve para analizar el flujo de dinero, es decir, los activos que más volumen (en dólares) están moviendo o han movido. Hay diferentes marcos temporales y rangos de tiempo de acumulación de volumen.

## MONITOREO DE VOLUMEN - INDICE DE FUNCIONES:

- plotVolume(ticker, period = "1y", m_fast = 10, m_slow = 30, interval = "1d"): Función que devuelve un gráfico con 3 paneles con los siguientes datos: 1) gráfico de velas del precio con una media móvil exponencial de 20 ruedas más el panel de volumen; 2) Un panel con dos medias móviles exponenciales de volumen parametrizables (una slow y una fast); 3) Un panel con el indicador técnico de Acumulación Distribución."

- analyzeVolume(ticker, period = "1y", m_fast = 10, m_slow = 30, interval = "1d"): Es una función que devuelve métricas de volumen nominal de un ticker seleccionado, para el período seleccionado y en función de indicadores técnicos tipo media móvil exponencial sobre el volumen, una rápida y una lenta, ambas parametrizables. Devuelve métricas de volumen.

- whereIsTheMoney(activos, timeframe):
    Esta función sirve para analizar el flujo de dinero, es decir, los activos que más volumen (en dólares) están moviendo o
    han movido. Hay diferentes marcos temporales y rangos de tiempo de acumulación de volumen.
        - Timeframes: "diario", "semanal" y "mensual";
        - activos: lista de activos especificada o bien las precargadas "adrs" (adrs), "cedears" (todos los activos de USA que coticen
        en Argentina), "sectors" (sectores vía etf), "lideres" (panel lider de ARG) o bien "general" (panel general de ARG).
    A su vez los resultados que retorna la función se agrupan por:
        - Si es rango diario: última rueda, últimas 5 ruedas (aprox 7d), últimas 21 ruedas (aprox 30d) y últimas 63 ruedas (aprox 90d)
        - Si es rango semanal: última semana, últimas 4 semanas (1m) y últimas 12 semanas (3m)
        - Si es rango mensual: último mes, últimos 3 meses y últimos 6 meses.
    A la vez que se ordena por volumen, también se visualizan otras dos importantes columnas: promedios para cada uno de los rangos
    temporales mencionados y cómo se ubica el monto operado de cada rango respecto al promedio (esto último resaltado en escala
    de colores para facilitar la identificación de entradas de dinero respecto de cada activo en particular / volumen anómalo).