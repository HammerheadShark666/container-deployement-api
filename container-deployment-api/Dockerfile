# ----------------------------
# Base stage
# ----------------------------
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

# ----------------------------
# Build stage
# ----------------------------
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy the project file and restore
COPY *.csproj ./
RUN dotnet restore

# Copy all source files
COPY . ./

# Build the app
RUN dotnet build -c $BUILD_CONFIGURATION -o /app/build

# ----------------------------
# Publish stage
# ----------------------------
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# ----------------------------
# Final stage
# ----------------------------
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

ENV ASPNETCORE_URLS=http://+:80

ENTRYPOINT ["dotnet", "container-deployment-api.dll"]
