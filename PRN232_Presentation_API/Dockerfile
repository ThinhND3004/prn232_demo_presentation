# =========================
# 1️⃣ Build stage
# =========================
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src

COPY ["PRN232_Presentation_API/PRN232_Presentation_API.csproj", "PRN232_Presentation_API/"]
RUN dotnet restore "PRN232_Presentation_API/PRN232_Presentation_API.csproj"

COPY . .
WORKDIR /src/PRN232_Presentation_API
RUN dotnet build -c Release -o /app/build

# =========================
# 2️⃣ Publish stage
# =========================
FROM build AS publish
RUN dotnet publish -c Release -o /app/publish /p:UseAppHost=false

# =========================
# 3️⃣ Runtime stage
# =========================
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS final
WORKDIR /app
COPY --from=publish /app/publish .
EXPOSE 8080
ENTRYPOINT ["dotnet", "PRN232_Presentation_API.dll"]
