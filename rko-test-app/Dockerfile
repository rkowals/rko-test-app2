#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["rko-test-app/rko-test-app.csproj", "rko-test-app/"]
RUN dotnet restore "rko-test-app/rko-test-app.csproj"
COPY . .
WORKDIR "/src/rko-test-app"
RUN dotnet build "rko-test-app.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "rko-test-app.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "rko-test-app.dll"]