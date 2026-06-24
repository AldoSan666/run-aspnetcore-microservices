# Base Stage (runtime)
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

# Build Stage (SDK)
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["src/ApiGateway/APIGateway/APIGateway.csproj", "src/ApiGateway/APIGateway/"]
RUN dotnet restore "src/ApiGateway/APIGateway/APIGateway.csproj"
COPY . .
WORKDIR "/src/src/ApiGateway/APIGateway"
RUN dotnet build "APIGateway.csproj" -c Release -o /app/build

# Publish Stage
FROM build AS publish
RUN dotnet publish "APIGateway.csproj" -c Release -o /app/publish

# Final Stage
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "APIGateway.dll"]
