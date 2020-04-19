#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/azure-functions/dotnet:2.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:2.1-stretch AS build
WORKDIR /src
COPY ["TimerFunctionAppContainer/TimerFunctionAppContainer.csproj", "TimerFunctionAppContainer/"]
RUN dotnet restore "TimerFunctionAppContainer/TimerFunctionAppContainer.csproj"
COPY . .
WORKDIR "/src/TimerFunctionAppContainer"
RUN dotnet build "TimerFunctionAppContainer.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TimerFunctionAppContainer.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV AzureWebJobsScriptRoot=/app