FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

COPY ["TradingJournal.Web/TradingJournal.Web.csproj", "TradingJournal.Web/"]
COPY ["TradingJournal.Shared/TradingJournal.Shared.csproj", "TradingJournal.Shared/"]
RUN dotnet restore "TradingJournal.Web/TradingJournal.Web.csproj"

COPY . .
WORKDIR "/src/TradingJournal.Web"
RUN dotnet publish "TradingJournal.Web.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM nginx:alpine AS final
WORKDIR /usr/share/nginx/html
COPY TradingJournal.Web/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/publish/wwwroot .

EXPOSE 80
