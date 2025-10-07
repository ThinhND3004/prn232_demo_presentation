FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["PRN232_Presentation_API/PRN232_Presentation_API.csproj", "PRN232_Presentation_API/"]
RUN dotnet restore "PRN232_Presentation_API/PRN232_Presentation_API.csproj"
COPY . .
WORKDIR "/src/PRN232_Presentation_API"
RUN dotnet build "./PRN232_Presentation_API.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./PRN232_Presentation_API.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PRN232_Presentation_API.dll"]
