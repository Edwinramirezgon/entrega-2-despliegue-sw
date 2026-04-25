# Práctica 3 - Publicación de Aplicaciones Web en IIS y Azure App Service

## Video de evidencia

🎥 **https://www.youtube.com/watch?v=omu750GkSMM**

## Integrantes

1. Edwin Ramirez Gonzalez
2. Juan Jose Rua David
3. Felipe Olaya Beneitez
4. Julian Andres Ramirez Bedoya
5. Argenis Alejandro Ruiz Cotes

## Fechas de publicación

| Entorno | Fecha |
|---|---|
| IIS (On-Premises) | 25/04/2026 |
| Azure App Service | 25/04/2026 |

## URLs del proyecto

| Servicio | URL |
|---|---|
| API Local (IIS) | `http://localhost:8080` |
| Frontend Local (IIS) | `http://localhost:8081` |
| API Azure | `https://tradingjournal-api-bsa5cfdggthmdgfr.brazilsouth-01.azurewebsites.net` |
| Frontend Azure | `https://tradingjournal-web-cxg5hhcxhue6g3a8.brazilsouth-01.azurewebsites.net` |

---

## Parte 1 — Publicación en IIS (On-Premises)

### Paso 1 — Verificar que IIS está habilitado en Windows
Se verificó que el rol de IIS estaba habilitado en Windows desde "Activar o desactivar características de Windows".

![Revisión IIS](Evidencias/1.Revision%20iis.png)

---

### Paso 2 — Construir redistribuible del Backend (API)
Se ejecutó el comando `dotnet publish` para compilar la API en modo Release y publicarla directamente en la carpeta de IIS:

```powershell
dotnet publish "TradingJournal\TradingJournal.API\TradingJournal.API.csproj" -c Release -o "C:\inetpub\wwwroot\TradingJournalAPI"
```

![Construir redistribuible backend](Evidencias/2.constuir%20redistribuible%20backend.png)

---

### Paso 3 — Construir redistribuible del Frontend
Se ejecutó el comando `dotnet publish` para compilar el frontend Blazor WASM en modo Release:

```powershell
dotnet publish "TradingJournal\TradingJournal.Web\TradingJournal.Web.csproj" -c Release -o "C:\inetpub\wwwroot\TradingJournalWeb"
```

![Construir redistribuible frontend](Evidencias/3.constuir%20redistribuible%20frontend.png)

---

### Paso 4 — Crear grupo de aplicaciones para el Backend en IIS
Se creó un App Pool dedicado para la API en el Administrador de IIS (`inetmgr`):
- **Nombre:** `TradingJournalAPI`
- **Versión .NET CLR:** Sin código administrado
- **Modo de canalización:** Integrado

![Crear grupo de aplicaciones backend](Evidencias/4.Crear%20grupo%20de%20aplicaciones%20backend.png)

---

### Paso 5 — Backend creado en el grupo de aplicaciones
Se verificó que el App Pool `TradingJournalAPI` quedó correctamente creado en la lista de grupos de aplicaciones.

![Backend creado en grupo de aplicaciones](Evidencias/5.backend%20ya%20creado%20en%20el%20grupo%20de%20aplicaciones.png)

---

### Paso 6 — Crear sitio web del Backend en IIS
Se creó el sitio web para la API:
- **Nombre:** `TradingJournalAPI`
- **Grupo de aplicaciones:** `TradingJournalAPI`
- **Ruta física:** `C:\inetpub\wwwroot\TradingJournalAPI`
- **Puerto:** `8080`

![Crear sitio web backend](Evidencias/6.crear%20sitio%20web%20backend.png)

---

### Paso 7 — Lista de sitios incluyendo el Backend
Se verificó que el sitio `TradingJournalAPI` aparece en la lista de sitios de IIS.

![Lista de sitios incluyendo backend](Evidencias/7.lista%20de%20stios%20incluyendo%20backend.png)

---

### Paso 8 — Instalar ASP.NET Core Hosting Bundle
Se instaló el Hosting Bundle de .NET 8 necesario para que IIS pueda ejecutar aplicaciones ASP.NET Core:

```powershell
winget install Microsoft.DotNet.HostingBundle.8
```

![Instalar Hosting Bundle](Evidencias/8.%20intalar%20micros.ne%20hostbundle.png)

---

### Paso 9 — Verificar que el Backend levanta con Swagger
Se verificó que la API responde correctamente en `http://localhost:8080/swagger`.

![Backend levanta con Swagger](Evidencias/9.%20revisando%20que%20el%20backend%20levante%20con%20swagger.png)

---

### Paso 10 — Verificar endpoint /health del Backend
Se verificó que el endpoint `/health` responde `OK` en `http://localhost:8080/health`.

![Backend levanta con health](Evidencias/10.%20Revisando%20que%20el%20backend%20levante%20con%20health.png)

---

### Paso 11 — Crear grupo de aplicaciones para el Frontend en IIS
Se creó un App Pool dedicado para el frontend:
- **Nombre:** `TradingJournalWeb`
- **Versión .NET CLR:** Sin código administrado
- **Modo de canalización:** Integrado

![Crear grupo de aplicaciones frontend](Evidencias/11.%20Crear%20grupo%20de%20aplicaciones%20frontend.png)

---

### Paso 12 — Frontend creado en el grupo de aplicaciones
Se verificó que el App Pool `TradingJournalWeb` quedó correctamente creado.

![Frontend creado en grupo de aplicaciones](Evidencias/12.%20Frontend%20ya%20creado%20en%20el%20grupo%20de%20aplicaciones.png)

---

### Paso 13 — Crear sitio web del Frontend en IIS
Se creó el sitio web para el frontend:
- **Nombre:** `TradingJournalWeb`
- **Grupo de aplicaciones:** `TradingJournalWeb`
- **Ruta física:** `C:\inetpub\wwwroot\TradingJournalWeb\wwwroot`
- **Puerto:** `8081`

![Crear sitio web frontend](Evidencias/13.%20crear%20sitio%20web%20frontend.png)

---

### Paso 14 — Lista de sitios incluyendo el Frontend
Se verificó que el sitio `TradingJournalWeb` aparece en la lista de sitios de IIS.

![Lista de sitios incluyendo frontend](Evidencias/14.%20lista%20de%20stios%20incluyendo%20frontend.png)

---

### Paso 15 — Verificar que el Frontend levanta y conecta al Backend
Se verificó que el frontend carga correctamente en `http://localhost:8081` y se conecta a la API local.

![Frontend levanta y conecta al backend](Evidencias/15.%20revisando%20que%20el%20frontend%20levante%20y%20conecta%20al%20backend.png)

---

## Parte 2 — Publicación en Azure App Service

### Paso 16 — Instalar Azure CLI
Se instaló Azure CLI para poder interactuar con Azure desde la terminal:

```powershell
winget install Microsoft.AzureCLI
```

![Instalando Azure CLI](Evidencias/16.%20instlando%20azure%20cli.png)

---

### Paso 17 — Comprobar instalación de Azure CLI
Se verificó que Azure CLI quedó correctamente instalado:

```powershell
az --version
```

![Comprobando Azure CLI](Evidencias/17.%20comprobando%20que%20azure%20cli%20haya%20quedado%20bien%20instalado.png)

---

### Paso 18 — Instalar extensión Azure App Service en VS Code
Se instaló la extensión **Azure App Service** de Microsoft en Visual Studio Code desde el marketplace de extensiones.

![Instalando Azure App Service en VS Code](Evidencias/18.instalando%20azure%20app%20service%20en%20visual%20studio.png)

---

### Paso 19 — Iniciar sesión en Azure desde VS Code
Se inició sesión en la cuenta de Azure desde la extensión de Azure en VS Code.

![Logueándose en Azure App Service](Evidencias/19.%20logueandose%20en%20azure%20app%20service.png)

---

### Paso 20 — Crear base de datos en Azure
Se creó una base de datos SQL en Azure Portal:
- **Servidor:** `tradingjournal-srv.database.windows.net`
- **Base de datos:** `TradingJournal`
- **Región:** Brazil South
- **Usuario:** `Sqlsa`

![Creando base de datos en Azure](Evidencias/20.%20creando%20base%20de%20datos%20en%20azure.png)

---

### Paso 21 — Habilitar firewall para comunicación con Azure
Se habilitó la opción "Permitir que los servicios de Azure accedan al servidor" en la configuración de red del SQL Server en Azure.

![Habilitando firewall Azure](Evidencias/21.%20habilitando%20el%20firewall%20para%20permitir%20comuncacion%20con%20servicios%20de%20azure.png)

---

### Paso 22 — Crear App Service del Backend en Azure
Se creó el App Service para la API desde VS Code:
- **Nombre:** `tradingjournal-api`
- **Runtime:** `.NET 8 (LTS)`
- **Región:** Brazil South
- **Plan:** Free (F1)

![Creando App Service backend](Evidencias/22.%20Creando%20app%20de%20backend%20en%20azure%20app%20service.png)

---

### Paso 23 — Desplegar el Backend en Azure App Service
Se compiló y desplegó la API en Azure usando zip deploy:

```powershell
dotnet publish "TradingJournal\TradingJournal.API\TradingJournal.API.csproj" -c Release -o "publish\api"
Compress-Archive -Path "publish\api\*" -DestinationPath "publish\api.zip" -Force
az webapp deploy --name tradingjournal-api --resource-group appsvc_windows_brazilsouth --src-path "publish\api.zip" --type zip
```

![Deployando backend en Azure](Evidencias/23.%20deployando%20web%20app%20del%20backend%20en%20azure%20app%20service.png)

---

### Paso 24 — Configurar Application Settings del Backend
Se configuró la variable de entorno `ASPNETCORE_ENVIRONMENT=Production` en el App Service del backend desde VS Code.

![Configurando Application Settings](Evidencias/24.%20corriendo%20ruta%20de%20la%20carpeta%20despliegue%20para%20la%20web%20app%20del%20backend.png)

---

### Paso 25 — Verificar endpoint /health del Backend en Azure
Se verificó que el endpoint `/health` responde correctamente en la URL pública de Azure.

```
https://tradingjournal-api-bsa5cfdggthmdgfr.brazilsouth-01.azurewebsites.net/health
```

![Comprobando health en Azure](Evidencias/25.%20comprobando%20que%20el%20backend%20quedo%20publicado%20con%20el%20endpoint%20health.png)

---

### Paso 26 — Verificar conexión del Backend con la base de datos en Azure
Se verificó que el backend en Azure conecta correctamente con la base de datos Azure SQL.

![Comprobando conexión con BD](Evidencias/26.%20comprobando%20que%20el%20backend%20quedo%20publicado%20en%20azure%20y%20tiene%20conexion%20con%20bd.png)

---

### Paso 27 — Activar HTTPS Only en el Backend
Se activó la opción **HTTPS Only** en la configuración general del App Service del backend en Azure Portal.

![Activando HTTPS backend](Evidencias/27.activando%20https%20en%20backend.png)

---

### Paso 28 — Desplegar el Frontend en Azure App Service
Se compiló y desplegó el frontend Blazor WASM en Azure:

```powershell
dotnet publish "TradingJournal\TradingJournal.Web\TradingJournal.Web.csproj" -c Release -o "publish\web"
Compress-Archive -Path "publish\web\wwwroot\*" -DestinationPath "publish\web.zip" -Force
az webapp deploy --name tradingjournal-web --resource-group appsvc_windows_brazilsouth --src-path "publish\web.zip" --type zip
```

![Deployando frontend en Azure](Evidencias/28.deployando%20web%20app%20del%20frontend%20en%20azure%20app%20service.png)

---

### Paso 29 — Verificar que el Frontend está publicado y conecta con el Backend
Se verificó que el frontend carga correctamente en la URL pública de Azure y se conecta a la API.

```
https://tradingjournal-web-cxg5hhcxhue6g3a8.brazilsouth-01.azurewebsites.net
```

![Frontend publicado en Azure](Evidencias/29.%20comprobando%20que%20el%20frontend%20quedo%20biebl%20publicado%20y%20que%20tenga%20conexion%20con%20el%20backend.png)

---

### Paso 30 — Activar HTTPS Only en el Frontend
Se activó la opción **HTTPS Only** en la configuración general del App Service del frontend en Azure Portal.

![Activando HTTPS frontend](Evidencias/30.%20activando%20https%20en%20el%20frontend.png)
