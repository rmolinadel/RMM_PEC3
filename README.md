# Visualización de la PEC3
Se procede a adjuntar todos los archivos utilizados para realizar las visualizaciones del diagrama de dispersión o scatterplot y diagrama de velas o candlestick en la presente plataforma de Github. Además, se adjuntaran los archivos adicionales utilizados como el código de Rstudio y los archivos excels para importar los datos en Tableau. 

**Código implementado en Rstudio para la extracción de datos:**
Número de reservas en el Hotel resort
```{r }
# Filtrar valor Resort Hotel en la columna de Hotel
Filter_x_resort <- x %>% filter(x$hotel != "City Hotel")
```

```{r tipo_visita}
# Muestra el gráfico de la evolución de las reservas en el Hotel "Resort Hotel"
Filter_x_resort$dia=as_date(paste0(Filter_x_resort$arrival_date_year,'-',Filter_x_resort$arrival_date_month,'-',Filter_x_resort$arrival_date_day_of_month))
ggplot(data=Filter_x_resort,aes(x=dia,group=arrival_date_year,color=arrival_date_year)) + 
  geom_bar() + 
  theme_light() 
```
```{r}
# Montar un dataframe para extraer en excel
table_resort <- data.frame(Filter_x_resort$hotel,Filter_x_resort$arrival_date_year,Filter_x_resort$arrival_date_month,Filter_x_resort$arrival_date_day_of_month) 
```

```{r}
# Cambiar el nombre de las columnas
names(table_resort) <- c('hotel','arrival_date_year','arrival_date_month','arrival_date_day_of_month')
```

```{r}
# Cargar la librería dplyr
library(dplyr)

# Agrupar por las columnas solicitadas y contar las frecuencias por días
conteo_dias_resort <- table_resort %>%
  group_by(hotel, arrival_date_year, arrival_date_month, arrival_date_day_of_month) %>%
  summarise(frecuencia = n(), .groups = 'drop')  # Contar las ocurrencias

# Poner los meses en orden
# Definir el orden de los meses como un factor
conteo_dias_order_resort <- conteo_dias_resort %>%
  mutate(arrival_date_month = factor(arrival_date_month, 
                                     levels = c("January", "February", "March", "April", 
                                                "May", "June", "July", "August", 
                                                "September", "October", "November", "December"))) %>%
  arrange(arrival_date_year, arrival_date_month, arrival_date_day_of_month)  # Ordenar explícitamente

# Mostrar el resultado
print(conteo_dias_order_resort)
```

```{r}
# Cargar la librería dplyr
library(dplyr)

# Agrupar por las columnas solicitadas y contar los registros por meses
conteo_meses_resort <- table_resort %>%
  group_by(hotel, arrival_date_year, arrival_date_month) %>%
  summarise(frecuencia = n(), .groups = 'drop')  # Contar los registros

# Poner los meses en orden
# Definir el orden de los meses como un factor
conteo_meses_order_resort <- conteo_meses_resort %>%
  mutate(arrival_date_month = factor(arrival_date_month, 
                                     levels = c("January", "February", "March", "April", 
                                                "May", "June", "July", "August", 
                                                "September", "October", "November", "December"))) %>%
  arrange(arrival_date_year, arrival_date_month)  # Ordenar explícitamente

# Mostrar el resultado
print(conteo_meses_order_resort)

```


```{r}
#Extraer archivo en excel 
# Exportar el data frame 'conteo_meses' a un archivo CSV
write.csv(conteo_meses_order_resort, file = "conteo_meses_reserva.csv", row.names = FALSE)
```

................................................................................

Número de reservas en el hotel City
```{r }
# Filtrar valor City Hotel en la columna de Hotel
Filter_x_city <- x %>% filter(x$hotel != "Resort Hotel")
```

```{r}
# Muestra el gráfico de la evolución de las reservas en el Hotel "City Hotel"
Filter_x_city$dia=as_date(paste0(Filter_x_city$arrival_date_year,'-',Filter_x_city$arrival_date_month,'-',Filter_x_city$arrival_date_day_of_month))
ggplot(data=Filter_x_city,aes(x=dia,group=arrival_date_year,color=arrival_date_year)) + 
  geom_bar() + 
  theme_light() 
```

```{r}
# Montar un dataframe para extraer en excel
table_city <- data.frame(Filter_x_city$hotel,Filter_x_city$arrival_date_year,Filter_x_city$arrival_date_month,Filter_x_city$arrival_date_day_of_month) 
```

```{r}
# Cambiar el nombre de las columnas
names(table_city) <- c('hotel','arrival_date_year','arrival_date_month','arrival_date_day_of_month')
```

```{r}
# Cargar la librería dplyr
library(dplyr)

# Agrupar por las columnas solicitadas y contar los registros por meses
conteo_meses_city <- table_city %>%
  group_by(hotel, arrival_date_year, arrival_date_month) %>%
  summarise(frecuencia = n(), .groups = 'drop')  # Contar los registros

# Poner los meses en orden
# Definir el orden de los meses como un factor
conteo_meses_order_city <- conteo_meses_city %>%
  mutate(arrival_date_month = factor(arrival_date_month, 
                                     levels = c("January", "February", "March", "April", 
                                                "May", "June", "July", "August", 
                                                "September", "October", "November", "December"))) %>%
  arrange(arrival_date_year, arrival_date_month)  # Ordenar explícitamente

# Mostrar el resultado
print(conteo_meses_order_city)
```

```{r}
#Extraer archivo en excel 
# Exportar el data frame 'conteo_meses' a un archivo CSV
write.csv(conteo_meses_order_city, file = "conteo_meses_city.csv", row.names = FALSE)
```

................................................................................

Filtrar tasa media por dia en el Hotel Resort
```{r }
# Filtrar valor Resort Hotel en la columna de Hotel
Filter_x_resort <- x %>% filter(x$hotel != "City Hotel")
```

```{r}
# Montar un dataframe para extraer en excel
table_resort_1 <- data.frame(Filter_x_resort$hotel,Filter_x_resort$arrival_date_year,Filter_x_resort$arrival_date_month,Filter_x_resort$arrival_date_day_of_month,Filter_x_resort$adr) 
```

```{r}
# Cambiar el nombre de las columnas
names(table_resort_1) <- c('hotel','arrival_date_year','arrival_date_month','arrival_date_day_of_month','adr')
```

```{r}
# Calcular la media de adr por año, mes y día
medias_adr <- table_resort_1 %>%
  group_by(arrival_date_year, arrival_date_month, arrival_date_day_of_month) %>%
  summarise(media_adr = mean(adr, na.rm = TRUE)) %>%
  ungroup()

# Poner los meses en orden
# Definir el orden de los meses como un factor
medias_adr_orden_resort <- medias_adr %>%
  mutate(arrival_date_month = factor(arrival_date_month, 
                                     levels = c("January", "February", "March", "April", 
                                                "May", "June", "July", "August", 
                                                "September", "October", "November", "December"))) %>%
  arrange(arrival_date_year, arrival_date_month)  # Ordenar explícitamente

# Ver el resultado
print(medias_adr_orden_resort)
```

```{r}
#Extraer archivo en excel 
# Exportar el data frame 'conteo_meses' a un archivo CSV
write.csv(medias_adr_orden_resort, file = "adr_hotel_resort.csv", row.names = FALSE)
```
................................................................................

Filtrar tasa media por dia en el City Hotel
```{r }
# Filtrar valor City Hotel en la columna de Hotel
Filter_x_city <- x %>% filter(x$hotel != "Resort Hotel")
```

```{r}
# Montar un dataframe para extraer en excel
table_city_1 <- data.frame(Filter_x_city$hotel,Filter_x_city$arrival_date_year,Filter_x_city$arrival_date_month,Filter_x_city$arrival_date_day_of_month,Filter_x_city$adr) 
```

```{r}
# Cambiar el nombre de las columnas
names(table_city_1) <- c('hotel','arrival_date_year','arrival_date_month','arrival_date_day_of_month','adr')
```

```{r}
# Calcular la media de adr por año, mes y día
medias_adr_1 <- table_city_1 %>%
  group_by(arrival_date_year, arrival_date_month, arrival_date_day_of_month) %>%
  summarise(media_adr_1 = mean(adr, na.rm = TRUE)) %>%
  ungroup()

# Poner los meses en orden
# Definir el orden de los meses como un factor
medias_adr_orden_city <- medias_adr_1 %>%
  mutate(arrival_date_month = factor(arrival_date_month, 
                                     levels = c("January", "February", "March", "April", 
                                                "May", "June", "July", "August", 
                                                "September", "October", "November", "December"))) %>%
  arrange(arrival_date_year, arrival_date_month)  # Ordenar explícitamente

# Ver el resultado
print(medias_adr_orden_city)
```

```{r}
#Extraer archivo en excel 
# Exportar el data frame 'conteo_meses' a un archivo CSV
write.csv(medias_adr_orden_city, file = "adr_hotel_city.csv", row.names = FALSE)
```
