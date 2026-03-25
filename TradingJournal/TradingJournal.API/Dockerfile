FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

COPY ["TradingJournal.API/TradingJournal.API.csproj", "TradingJournal.API/"]
COPY ["TradingJournal.Shared/TradingJournal.Shared.csproj", "TradingJournal.Shared/"]
RUN dotnet restore "TradingJournal.API/TradingJournal.API.csproj"

COPY . .
WORKDIR "/src/TradingJournal.API"
RUN dotnet publish "TradingJournal.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=build /app/publish .

ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

ENTRYPOINT ["dotnet", "TradingJournal.API.dll"]
