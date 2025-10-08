# ---------- base runtime ----------
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app

# optionally define a default non-root UID (can override at build time)
ARG APP_UID=1000
ARG APP_GID=1000

# Create group & user (Debian-based images use addgroup/adduser or groupadd/useradd)
RUN groupadd -g ${APP_GID} appgroup \
 && useradd -u ${APP_UID} -g appgroup -m -s /bin/bash appuser

# Let ASP.NET listen on port 8080 inside container
ENV ASPNETCORE_URLS=http://+:8080

EXPOSE 8080
# EXPOSE 8081   # remove if not used

# ---------- build stage ----------
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# copy csproj and restore (cache-friendly)
COPY ["PRN232_Presentation_API/PRN232_Presentation_API.csproj", "PRN232_Presentation_API/"]
RUN dotnet restore "PRN232_Presentation_API/PRN232_Presentation_API.csproj"

# copy rest of sources and build
COPY . .
WORKDIR "/src/PRN232_Presentation_API"
RUN dotnet build "PRN232_Presentation_API.csproj" -c ${BUILD_CONFIGURATION} -o /app/build

# ---------- publish stage ----------
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
WORKDIR "/src/PRN232_Presentation_API"
RUN dotnet publish "PRN232_Presentation_API.csproj" -c ${BUILD_CONFIGURATION} -o /app/publish /p:UseAppHost=false

# ---------- final image ----------
FROM base AS final
WORKDIR /app

# copy published app from publish stage
COPY --from=publish /app/publish .

# switch to non-root user (created in base)
USER appuser

# default entrypoint
ENTRYPOINT ["dotnet", "PRN232_Presentation_API.dll"]
