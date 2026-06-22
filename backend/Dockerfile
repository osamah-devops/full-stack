# Build stage
FROM mcr.microsoft.com/dotnet/sdk:10.0-preview AS build
WORKDIR /src

# Copy project files first for layer caching
COPY *.sln .
COPY Backend/*.csproj ./Backend/
RUN dotnet restore ./Backend/Backend.csproj

# Copy source
COPY . .

# Publish
RUN dotnet publish ./Backend/Backend.csproj \
    -c Release \
    -o /app/publish \
    --no-restore

# Runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:10.0-preview
WORKDIR /app
COPY --from=build /app/publish .
EXPOSE 80
ENV ASPNETCORE_URLS=http://+:80
ENTRYPOINT ["dotnet", "Backend.dll"]